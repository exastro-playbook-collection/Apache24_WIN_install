# Ansible Role: Apache24_WIN\_install

## Description

本Ansible RoleはWebサーバソフトウェアである"Apache HTTP Server"のインストールを行います。
対象バージョンは以下のバージョンです。

- Apache2.4 (Apache Launge版)

Apacheインストール時に、"Microsoft Visual C++ 2017 再頒布可能パッケージ"の
インストールを行うこともできます。対象となるパッケージのバージョンは、"14.1x.x"です。

## Supports

- 管理マシン(Ansibleサーバ)
  - Linux系OS（RHEL/CentOS）
  - Ansible バージョン 2.3 以上 (動作確認済みバージョン：2.4、2.9)
  - Python バージョン 2.x、3.x  (動作確認済みバージョン：2.6、2.7、3.6)


- 管理対象マシン(インストール対象マシン)
  - Windows Server 2016
  - PowerShell 5.0

## Requirements
- 管理マシン(Ansibleサーバ)
  - 管理対象マシンへ	WinRM接続できること
  - WinRM接続設定は次のサイトを参考にすること
    - http://docs.ansible.com/ansible/intro_windows.html#windows-system-prep
- 管理対象マシン(インストール対象マシン)
  - PowerShell 5.0を利用できること
  - ファイアフォールが適切に設定されていること

## Role Variables
### Mandatory Variables

以下の変数は必ず指定します。

- WinRM設定（group_varsもしくはhost_varsに設定）
  * ''ansible_user'': ユーザ名(string)
  * ''ansible_password'': パスワード(string)
  * ''ansible_port'': ポート(number)
  * ''ansible_connection'': 接続方式(string)
  * ''ansible_winrm_server_cert_validation'': 暗号化有無(string)


- バイナリの配置元(管理マシン)
  * ''VAR_Apache24_WIN_localpkg_Apache_file'': Apacheバイナリのファイル名 (string)

### Optional variables

以下の変数は任意で指定します。

- バイナリの配置元(管理マシン)
  * ''VAR_Apache24_WIN_localpkg_src'': バイナリの配置ディレクトリ(string, default:"/tmp")
  * ''VAR_Apache24_WIN_VC_INSTALL'': Microsoft Visual C++ 2017 再頒布可能パッケージのインストール(on|off, default:"on")
    + on を指定した場合は、Microsoft Visual C++ 2017 再頒布可能パッケージをインストールします。
    　ファイルをダウンロードして、'VAR_Apache24_WIN_localpkg_src'に配置します。
    　'VAR_Apache24_WIN_localpkg_VC_file'の指定が必要です。
    + off を指定した場合は、Microsoft Visual C++ 2017 再頒布可能パッケージをインストールしません。
      'VAR_Apache24_WIN_localpkg_VC_file'の指定も不要です。
  * ''VAR_Apache24_WIN_localpkg_VC_file'': Microsoft Visual C++ 2017 再頒布可能パッケージのファイル名 (string,default:"")
  * ''VAR_Apache24_WIN_localpkg_upload'': バイナリファイルをアップロードするかどうか (on|off, default:"on")
    + off に指定した場合は、ApacheおよびMicrosoft Visual C++ 2017 再頒布可能パッケージのアップロードを行いません。
      予めアップロード先のフォルダにバイナリを配置してください。


- Apacheのインストール先(管理対象マシン)
  * ''VAR_Apache24_WIN_path'': Apacheインストール先フォルダ (string, default:"'C:\Apache24'")
    + 空白を含むパスを指定する場合は、'' で囲むこと
  * ''VAR_Apache24_WIN_ServiceName'': Apacheサービス名 (string, default:"Apache2.4")
  * ''VAR_Apache24_WIN_localpkg_dst'': ApacheおよびMicrosoft Visual C++ 2017 再頒布可能パッケージの一時保存先 (string, default:"'C:\temp'")
    + 空白を含むパスを指定する場合は、'' で囲むこと
    + 一時保存先フォルダが存在しない場合はロール実行時に作成されます。ロール実行後はフォルダ自体は削除されません。

## Dependencies

特にありません。

## Usage

1. 本Roleを用いたPlaybookを作成します。
2. Microsoft Visual C++ 2017 再頒布可能パッケージをダウンロードします。
  - Apache Loungeサイト「http://www.apachelounge.com/download/ 」から、「Be sure !! that you have installed the latest C++ Redistributable Visual Studio 2017 : vc_redist_x64 or vc_redist_x86.」のリンク「vc_redist_x64」をクリックする
  - ファイル名は変数「VAR_Apache24_WIN_localpkg_VC_file」で指定します。
3. Apacheバイナリをダウンロードします。
  - Apache Loungeサイト「http://www.apachelounge.com/download/ 」から、[Apache 2.4 binaries VC15]→[httpd-2.4.x-Win64-VC15.zip]を選択し、zip形式のApacheバイナリをダウンロードする。
  - ファイル名は変数「VAR_Apache24_WIN_localpkg_Apache_file」で指定します。
4. Microsoft Visual C++ 2017 再頒布可能パッケージおよびApacheバイナリを、管理マシン上に配置する。
  - Ansibleサーバ上の配置先ディレクトリは変数「VAR_Apache24_WIN_localpkg_src」です。（デフォルトは/tmp）
5. 必須変数を指定します。
6. 任意変数を必要に応じて指定します。
7. Playbookを実行します。


## Example Playbook

インストールとすべての設定をする場合は、提供した以下のRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - group_vars/
    ・ server1
    ・ server2
  - host_vars/
    ・ host1
    ・ host2
  - roles/
    ・ Apache_install/
    ・ Apache_setup/
  - Apache24_WIN_install.yml
  - Apache24_WIN_ossetup.yml
  - Apache24_WIN_setup.yml
  - conf.yml
  - hosts
  - site.yml
~~~

- マスターPlaybook サンプル「Apache24_WIN_install.yml」
~~~
# Apache24_WIN_install.yml
 - name: install Apache2.4
   hosts: all
   gather_facts: yes
   become: no
   tags:
    - install
   roles:
     - Apache24_WIN_install
~~~


## Running Playbook
- extra-vars を利用する場合の実行例
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml"

- group_vars を利用する場合の実行例  
group_vars で指定したグループ名が webserver1 の場合
> ansible-playbook site.yml  -i hosts -l webserver1

- host_vars を利用する場合の実行例  
 host_vars で指定したグループ名が server1 の場合
> ansible-playbook site.yml  -i hosts -l server1

- 本Roleのみを実行する場合は、 --tags "install" を付け加える
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml" --tags "install"

# Copyright
Copyright (c) 2017 NEC Corporation

# Author Information
NEC Corporation
