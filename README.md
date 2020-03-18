# Apache2.4 for Windows Server 2016

## Trademarks

- Linuxは、Linus Torvalds氏の米国およびその他の国における登録商標または商標です。
- RedHat、RHEL、CentOSは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
- Windows、PowerShellは、Microsoft Corporation の米国およびその他の国における登録商標または商標です。
- Ansibleは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
- pythonは、Python Software Foundationの登録商標または商標です。
- Apache、Tomcatは、Apache Software Foundationの登録商標または商標です。
- Java およびすべてのJava 関連の商標は、Oracle Corporation およびその関連会社の登録商標です。
- NECは、日本電気株式会社の登録商標または商標です。
- その他、本ロールのコード、ファイルに記載されている会社名および製品名は、各社の登録商標または商標です。

## 概要
本プロジェクトのリポジトリでApache HTTP Serverのインストール、セットアップRoleを公開しています。  
OSSの対象バージョンは以下のバージョンとなります。  
・Apache2.4  
　  
対応OSは以下となります。  
・Windows Server 2016  

各ロールの説明はREADME_role.mdをご参照ください。

## Supports

- 管理マシン(Ansibleサーバ)
  * Linux系OS（RHEL/CentOS）
  * Ansible バージョン 2.3 以上 (動作確認済みバージョン：2.4)
  * Python バージョン 2.6 または2.7


- 管理対象マシン(構築対象マシン)
  * Windows Server 2016

## roles

 - Apache_WIN_install  
    Apache2.4のパッケージのインストールを行うroleです。  
    提供している機能は以下です。

   * Apache2.4関連のパッケージのインストール
   * OSへのサービスの登録  


 - Apache_WIN_setup  
  Apache2.4のconfファイルへの設定を行うroleです。  
  提供している機能は以下です。

   * 以下のconfファイルへの設定  
      * httpd.conf
      * httpd-default.conf
      * httpd-info.conf
      * httpd-mpm.conf
      * proxy.conf


 - Apache_WIN_ossetup  
    Apache2.4のサービスの状態の設定／変更、再起動を行うroleです。  
    提供している機能は以下です。

     * サービスの自動起動設定の変更（auto/manual）
     * Apache2.4の再起動   


## Exastro IT Automationでの利用時について
Exastro IT Automation(以下、ITA)利用時に必要なITA独自ファイル「ita_readme_role名.yml」 を公開しています。  
ITA独自ファイルのないroleに関してはそのままITAで利用することができます。  
ITAの操作方法、ITA独自ファイルの利用方法についてはITAの利用マニュアルをご確認ください。  

### ITA利用時の注意事項

  * 本roleをITAで利用する場合には、ITAに登録するロールパッケージ（zipファイル）内にApache2.4のインストールパッケージを登録しておく必要があります。ロールパッケージ作成手順を参考にして作成してください。また、一部の変数についても以下の通りITA代入値管理で指定していただく必要があります。

    - VAR_Apache24_WIN_localpkg_src変数に、".(ドット)"を必ず指定


  * ITAの代入値管理において以下の変数のデフォルト値が"1"で表示されますが、role内では、default値として"on"が定義されています。ITAの代入値管理で設定する場合には、"on"または"off"を指定してください。

    - VAR_Apache24_WIN_VC_INSTALL
    - VAR_Apache24_WIN_localpkg_upload
    - VAR_Apache24_WIN_Http


  * ITAの代入値管理において以下の変数のデフォルト値が空欄で表示されますが、role内では、default値として"off"が定義されています。 これは、ITAの仕様によるものです。必要に応じて代入値管理で"on"または"off"を指定してください。

    - VAR_Apache24_WIN_Default
    - VAR_Apache24_WIN_Info
    - VAR_Apache24_WIN_MPM
    - VAR_Apache24_WIN_Proxy
    - VAR_Apache24_WIN_Enable
    - VAR_Apache24_WIN_Start


### ITAで使用するロールパッケージファイルの作成手順

    1. 本roleをダウンロードし、解凍します。

    2. 解凍したroles配下にApache2.4のインストールに必要なパッケージを格納するfilesディレクトリをApache24_WIN_installディレクトリ直下に作成します。
         - 作成するディレクトリ： \roles\Apache24_WIN_install\files

    3. Apache2.4のインストールに必要な以下のパッケージを作成したfiles配下に格納します。
         - Apache2.4インストールパッケージ（Win64-VC15.zip形式)
         - VC++再頒布パッケージ(x64.exe形式)
         パッケージの入手については、Apache24_WIN_install配下のReadme_role.mdをご参照ください。

    4. rolesディレクトリ以下をzip形式で圧縮し、ITAロールパッケージファイルを作成します。


## 備考
動作確認バージョン以外のAnsibleでは、Roleが一部正常に動作しない可能性があります。

# Copyright
Copyright (c) 2017 NEC Corporation

# Author Information
NEC Corporation
