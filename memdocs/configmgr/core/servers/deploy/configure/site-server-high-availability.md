---
title: サイト サーバーの高可用性
titleSuffix: Configuration Manager
description: パッシブ モードのサイト サーバーを追加することによって、Configuration Manager サイト サーバーの高可用性を設定する方法です。
ms.date: 02/07/2020
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: 6dcef836-c0d1-40af-ad30-cd8d864b09a9
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 395504f5ccc3635f580bca739f1c4b7273861123
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81700900"
---
# <a name="site-server-high-availability-in-configuration-manager"></a>Configuration Manager でのサイト サーバーの高可用性

*適用対象:Configuration Manager (Current Branch)*

<!--1128774-->

これまで、環境内に各役割のインスタンスを複数含めることで、Configuration Manager の役割のほとんどに冗長性を追加できました。 サイト サーバー自体を除きます。 サイト サーバーの役割の高可用性は、追加のサイト サーバーを*パッシブ* モードでインストールするための Configuration Manager ベースのソリューションです。 中央管理サイトと子プライマリ サイトでは、パッシブ モードで追加のサイト サーバーを使用できます。 Azure では、パッシブ モードのサイト サーバーは、オンプレミスまたはクラウド ベースにすることができます。

この機能には、次のベネフィットがあります

- サイト サーバーの役割の冗長性と高可用性  
- サイト サーバーのハードウェアまたは OS を変更しやすくなる  
- サイト サーバーを Azure IaaS に移動しやすくなる  

パッシブ モードのサイト サーバーは、*アクティブ* モードになっている既存のサイト サーバーに加えられます。 パッシブ モードのサイト サーバーは、必要なときにすぐに使用できます。 Configuration Manager サービスの[高可用性](high-availability-options.md)を実現するために、この追加のサイト サーバーを全体的なデザインの一部として含めます。  

パッシブ モードのサイト サーバー

- アクティブ サイトのサイト サーバーと同じサイト データベースを使用します。
- パッシブ モードのときに、サイト データベースにデータを書き込むことはありません。
- アクティブ モードのサイト サーバーと同じコンテンツ ライブラリを使用します。

パッシブ モードのサイト サーバーをアクティブにするには、手動で*昇格*させます。 この操作によって、アクティブ モードのサイト サーバーをパッシブ モードのサイト サーバーに切り替えます。 元のアクティブ モード サーバーで使用可能なサイト システムの役割は、そのコンピューターにアクセスできる間は引き続き使用できます。 サイト サーバーの役割のみが、アクティブ モードとパッシブ モードの間で切り替えられます。

Microsoft Core Services Engineering and Operations では、その中央管理サイトを Microsoft Azure に移行するためにこの機能を使いました。 詳細については、[Microsoft IT Showcase の記事](https://www.microsoft.com/itshowcase/Article/Content/1065/Migrating-System-Center-Configuration-Manager-onpremises-infrastructure-to-Microsoft-Azure)をご覧ください。

## <a name="prerequisites"></a>[前提条件]

- サイト コンテンツ ライブラリは、リモート ネットワーク共有上にある必要があります。 両方のサイト サーバーには、共有とそのコンテンツに対するフル コントロールのアクセス許可が必要です。 詳細については、[コンテンツ ライブラリの管理](../../../plan-design/hierarchy/the-content-library.md#bkmk_remote)に関するページを参照してください。<!--1357525-->  

  - サイト サーバー コンピューター アカウントには、コンテンツ ライブラリの移動先であるネットワーク パスに対して **[フル コントロール]** アクセス許可が必要です。 このアクセス許可は、共有とファイル システムの両方に適用されます。 リモート システムにコンポーネントはインストールされません。

  - サイト サーバーは配布ポイントの役割を持つことができません。 配布ポイントでもコンテンツ ライブラリを使用し、この役割ではリモート コンテンツ ライブラリをサポートしません。 コンテンツ ライブラリを移動した後は、サイト サーバーに配布ポイントの役割を追加することはできません。  

- Azure では、パッシブ モードのサイト サーバーは、オンプレミスまたはクラウド ベースにすることができます。  

    > [!NOTE]
    > クラウド ベースのパッシブ モードのサイト サーバーでは、Azure のサービスとしてのインフラストラクチャ (IaaS) を使用します。 詳細については、以下の記事を参照してください。
    >
    >   - [Azure 仮想マシン (クラウドベースのインフラストラクチャ用)](../../../understand/use-cloud-services.md#azure-virtual-machines-for-cloud-based-infrastructure)
    >   - [Azure での Configuration Manager に関する FAQ](../../../understand/configuration-manager-on-azure.md)  

- 両方のサイト サーバーが、同じ Active Directory ドメインに参加している必要があります。  

- Configuration Manager では階層内のパッシブ モードのサイト サーバーがサポートされています。 中央管理サイトと子プライマリ サイトでは、パッシブ モードで追加のサイト サーバーを使用できます。<!-- 3607755 -->  

- 両方のサイト サーバーで同じサイト データベースを使う必要があります。  

  - データベースは各サイト サーバーからリモートであってもかまいません。 バージョン 1810 以降では、Configuration Manager セットアップ プロセスで、フェールオーバー クラスタリング用の Windows の役割でコンピューターにサイト サーバーの役割がインストールされるのがブロックされなくなりました。 SQL Always On では、このロールが必要なため、以前はサイト サーバーにサイト データベースを同時に配置できませんでした。 この変更により、SQL Always On とパッシブ モードのサイト サーバーを使用して、少ないサーバー数で高可用性サイトを作成できます。<!-- SCCMDocs issue 1074 -->  

  - サイト データベースをホストする SQL Server では、既定のインスタンス、名前付きインスタンス、[SQL Server クラスター](use-a-sql-server-cluster-for-the-site-database.md)、または [SQL Server Always On 可用性グループ](sql-server-alwayson-for-a-highly-available-site-database.md)を使用できます。  

  - 両方のサイト サーバーに、サイト データベースをホストする SQL Server インスタンスの **sysadmin** セキュリティ ロールが必要です。 元のサイト サーバーは既にこれらの役割を持っているはずなので、新しいサイト サーバーにそれらを追加します。 たとえば、次の SQL スクリプトでは、Contoso ドメインの新しいサイト サーバー **VM2** にこれらの役割を追加します。  

    ```SQL
    USE [master]
    GO
    CREATE LOGIN [contoso\vm2$] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english]
    GO
    ALTER SERVER ROLE [sysadmin] ADD MEMBER [contoso\vm2$]
    GO
    ```

  - 両方のサイト サーバーで、SQL Server のインスタンス上のサイト データベースにアクセスする必要があります。 元のサイト サーバーは既にこのアクセス権を持っているはずなので、新しいサイト サーバーにそれを追加します。 たとえば、次の SQL スクリプトでは、Contoso ドメインの新しいサイト サーバー **VM2** に、**CM_ABC** データベースへのログインを追加します。  

    ```SQL
    USE [CM_ABC]
    GO
    CREATE USER [contoso\vm2$] FOR LOGIN [contoso\vm2$] WITH DEFAULT_SCHEMA=[dbo]
    GO
    ```

  - パッシブ モードのサイト サーバーは、アクティブ モードのサイト サーバーと同じサイト データベースを使用するように構成されます。 パッシブ モードのサイト サーバーでは、そのデータベースからのみ読み取りが行われます。 アクティブ モードに昇格されるまで、データベースに書き込みを行うことはありません。  

- パッシブ モードのサイト サーバー  

  - プライマリ サイトをインストールするための前提条件を満たす必要があります。 たとえば、.NET Framework、Remote Differential Compression、Windows ADK です。 完全なリストについては、[サイトとサイト システムの前提条件](../../../plan-design/configs/site-and-site-system-prerequisites.md)に関するページをご覧ください。<!-- SCCMDocs issue 765 -->  

    > [!NOTE]
    > 必ず SQL Server Native Client をインストールしてください。 インストールしない場合は、Configuration Manager セットアップ時に前提条件チェッカーによって、不足している SQL Server のアクセス許可に関するエラーが報告されます。<!-- SCCMDocs#2290 -->

  - アクティブ モードのサイト サーバー上のローカルの管理者グループに、そのコンピューター アカウントがある必要があります。<!--516036-->

  - アクティブ モードのサイト サーバーのバージョンと一致するソース ファイルを使ってインストールする必要があります。  

  - パッシブ モードのサイト サーバーの役割をインストールするまでは、それにインストールされているいずれのサイトからのサイト システムの役割も持つことはできません。  

- 両方のサイト サーバーでは、[Configuration Manager によってサポートされている](../../../plan-design/configs/supported-operating-systems-for-site-system-servers.md)限り、異なる OS またはサービス パックのバージョンを実行できます。  

- 高可用性のために構成されているいずれかのサイト サーバー上でサービス コネクション ポイント役割をホストしないでください。 それが現在元のサイト サーバー上にある場合は、それを削除して、別のサイト システム サーバーにインストールします。 詳細については、「[About the service connection point](about-the-service-connection-point.md)」 (サービスの接続ポイントについて) を参照してください。  

- [サイト システム インストール アカウント](../../../plan-design/hierarchy/accounts.md#site-system-installation-account)のアクセス許可  

  - 既定では、多くのお客様はサイト サーバーのコンピューター アカウントを使って新しいサイト システムをインストールします。 そのため、要件は、サイト サーバーのコンピューター アカウントを、リモート サイト システム上のローカルの **[管理者]** グループに追加することです。 ご自身の環境でこの構成を使う場合は、新しいサイト サーバーのコンピューター アカウントを、すべてのリモート サイト システム上のこのローカル グループに必ず追加します。 たとえば、すべてのリモート配布ポイントです。  

  - より安全かつ推奨される構成は、サイト システムをインストールするためにサービス アカウントを使うことです。 最も安全な構成は、ローカル サービス アカウントを使うことです。 ご自身の環境でこの構成を使う場合、変更は必要ありません。  

## <a name="limitations"></a>制限事項

- サイトごとにパッシブ モードのサイト サーバーが 1 つだけサポートされます。  

- パッシブ モードのサイト サーバーは、セカンダリ サイトではサポートされません。<!--SCCMDocs issue 680-->  

    > [!NOTE]  
    > セカンダリ サイトは、高可用性のサイト サーバーを備えたプライマリ サイトの下で引き続きサポートされます。

- パッシブ モードのサイト サーバーのアクティブ モード サーバーへの昇格は、手動で行います。 自動フェールオーバーはありません。  

- サイト システムの役割は、パッシブ モードのサイト サーバーを追加する前に、新しいサーバー上にインストールすることはできません。  

    > [!NOTE]
    > パッシブ モードのサイト サーバーをインストールした後、必要に応じて、さらに役割を追加できます。 たとえば、プライマリ サイトの管理ポイントです。

- データベースを使用するレポート ポイントなどの役割については、両方のサイト サーバーからリモートにあるサーバー上にデータベースをホストします。  

- Configuration Manager コンソールは、パッシブ モードのサイト サーバー上に自動的にインストールされません。  

## <a name="add-a-site-server-in-passive-mode"></a>プライマリ サイト サーバーをパッシブ モードで追加する

役割を追加する一般的なプロセスの詳細については、「[サイト システムの役割のインストール](install-site-system-roles.md)」を参照してください。

1. Configuration Manager コンソールで、 **[管理]** ワークスペースに移動し、 **[サイトの構成]** を展開して **[サイト]** ノードを選択し、リボンの **[サイト システム サーバーの作成]** をクリックします。

2. サイト システム サーバーの作成ウィザードの **[全般]** ページで、パッシブ モードのサイト サーバーをホストするサーバーを指定します。 指定したサーバーは、パッシブ モードでサイト サーバーをインストールするまで、サイト システムの役割をホストすることはできません。  

3. **[システムの役割の選択]** ページで、 **[パッシブ モードのサイト サーバー]** のみを選択します。  

    > [!NOTE]  
    > ウィザードでは、このページ上で次の初期の前提条件の確認を実行します。
    >
    > - 選択したサーバーは、セカンダリ サイト サーバーではない
    > - 選択したサーバーは、まだパッシブ モードのサイト サーバーではない
    > - サイトのコンテンツ ライブラリは、リモートの場所にある  
    >  
    > これらの初期の前提条件の確認が失敗した場合、ウィザードのこのページを継続することはできません。  

4. **[パッシブ モードのサイト サーバー]** ページで、次の情報を提供する必要があります。この情報は、セットアップを実行して、指定したサーバーにサイト サーバーの役割をインストールするのに使用されます。

     - 次のいずれかのオプションを選択します。  

         - **ネットワーク経由でサイト サーバーからアクティブ モードでインストール ソース ファイルをコピーする**:このオプションでは圧縮されたパッケージが作成され、新しいサイト サーバーに送信します。  

         - **パッシブ モードのサイト サーバーでは、次の場所にあるソース ファイルを使用する**:たとえば、ソース ファイルを既にコピーしているローカル パス。 このコンテンツは、アクティブ モードのサイト サーバーと同じバージョンであることを確認します。  

         - (*推奨*) **次のネットワークの場所にあるソース ファイルを使用する**:アクティブ モードのサイト サーバーから **CD.Latest** フォルダーのコンテンツへの直接パスを指定します。 たとえば、`\\Server\SMS_ABC\CD.Latest` ここでは、"*Server*" がアクティブ モードのサイト サーバーの名前で、"*ABC*" はサイト コードです。  

     - 新しいサイト サーバー上の Configuration Manager をインストールするローカル パスを指定します。 例: `C:\Program Files\Configuration Manager`  

5. ウィザードを完了します。 Configuration Manager が、指定されたサーバーにパッシブ モードでサイト サーバーをインストールします。

インストールの状態の詳細については、コンソールで **[監視]** ワークスペースに移動して、 **[サイト サーバーの状態]** ノードを選択します。 パッシブ モードのサイト サーバーの状態は、 **[インストール中]** として表示されます。 詳細な情報については、サーバーを選択して **[状態の表示]** をクリックします。 このアクションによって、[サイト サーバーのインストールの状態] ウィンドウが開きます。 プロセスが完了すると、両方のサーバーの状態に **[OK]** が表示されます。

セットアップ プロセスの詳細については、「[Flowchart - Set up a site server in passive mode](passive-site-server-flowchart.md)」 (フローチャート - パッシブ モードのサイト サーバーのセットアップ) を参照してください。

パッシブ モードのサイト サーバーを追加した後、コンソールの **[サイト]** ノードの **[ノード]** タブにある両方のサイト サーバーを確認します。

Configuration Manager サイト サーバーのコンポーネントはすべて、パッシブ モードのサイト サーバー上でスタンバイ状態です。 Windows サービスは、実行され続けます。

## <a name="site-server-promotion"></a>サイト サーバーの昇格  

バックアップや回復と同様に、サイト サーバーを変更するためのプロセスを計画して実施します。 昇格計画で次の点を検討してください。  

- 両方のサイト サーバーがオンラインのときに、計画済みの昇格を実施します。 また、アクティブ モードのサイト サーバーを強制的に切断するか、シャットダウンすることで、計画外のフェールオーバーも実施します。  

- フェールオーバー中の操作プロセス、およびその他の Configuration Manager 管理者と通信する内容を決定します。  

- 計画済みの昇格前  

  - サイトとサイト コンポーネントにおける全体の状態を確認します。 すべてがご利用の環境でいつものように正常であることを確認します。  

  - サイト間でアクティブにレプリケートする任意のパッケージにおけるコンテンツの状態を確認します。  

  - セカンダリ サイトのステータスとサイトのレプリケーションを確認します。

  - 子またはセカンダリ サイト サーバー上では、新しいコンテンツ配布ジョブやメンテナンスを開始しないでください。

    > [!NOTE]
    > サイト間のファイルまたはデータベースのレプリケーションがフェールオーバー中に進行中である場合は、新しいサイト サーバーでレプリケートされたコンテンツを受信しない可能性があります。 これが発生した場合は、新しいサイト サーバーがアクティブになった後にソフトウェア コンテンツを再配布します。<!--515436--> データベースのレプリケーションの場合、フェールオーバー後にセカンダリ サイトを再初期化する必要があることがあります。<!-- SCCMDocs issue 808 -->

  - 同時にスケジュールされている他のアクティビティを少なくするか、削除します。 たとえば、サイトを新しいバージョンに更新した直後に、サイト サーバーの昇格を計画しないでください。 サイトの更新には、サイト サーバーの昇格と競合する可能性があるその他のタスクが含まれています。

    > [!TIP]
    > 他のアクティビティがサイト サーバーの昇格とどのように競合する可能性があるのかの例を次に示します。
    >
    > - 月曜日: サイトを最新バージョンに更新します。 クライアントのパイロット運用により自動クライアント アップグレードを有効にします。
    > - 火曜日: パッシブ モードのサイト サーバーをアクティブなサイト サーバーに昇格します。
    >
    > このアクションにより、水曜日または木曜日までに、パイロット コレクションだけでなく、*すべての*クライアントがアップグレードされる可能性があります。 この動作により、ネットワークの使用率が高くなり、配布ポイントで予期しない負荷が発生する可能性があります。<!-- SCCMDocs-pr#4794 -->

### <a name="process-to-promote-the-site-server-in-passive-mode-to-active-mode"></a>パッシブ モードのサイト サーバーをアクティブ モードに昇格するプロセス

このセクションでは、パッシブ モードのサイト サーバーをアクティブ オードに変更する方法について説明します。 サイトにアクセスしてこの変更を行うには、SMS プロバイダーのインスタンスにアクセスできる必要があります。 詳細については、「[複数の SMS プロバイダーの使用](../../../plan-design/hierarchy/plan-for-the-sms-provider.md#BKMK_MultiSMSProv)」を参照してください。  

> [!IMPORTANT]  
> SMS プロバイダーのすべてのインスタンスがオフラインの場合は、プロバイダーを利用できないサイトに接続することはできません。 パッシブ モードでサイト サーバーを追加すると、セットアップによって、このサーバーに SMS プロバイダーのインスタンスがインストールされます。<!-- SCCMDocs#1613 -->
>
> Configuration Manager コンソールでは、サイト サーバー上の WMI から利用可能な SMS プロバイダーの一覧を要求します。 サイトに複数の SMS プロバイダーをインストールした場合は、インストール済みの SMS プロバイダーを使用するために、サイトによって新しい接続要求がランダムに割り当てられます。 特定の接続セッションで、特定の場所にある SMS プロバイダーを使用するように指定することはできません。 現在のサイト サーバーがオフラインのため、ご利用のコンソールでサイトに接続できない場合、サイト接続ウィンドウで他のサイト サーバーを指定します。  

1. Configuration Manager コンソールで、 **[管理]** ワークスペースに移動し、 **[サイトの構成]** を展開して **[サイト]** ノードを選択します。 サイトを選択して、 **[ノード]** タブに切り替えます。パッシブ モードのサイト サーバーを選択して、リボンの **[アクティブに昇格]** をクリックします。 **[はい]** をクリックして確認して続けます。
  
2. コンソール ノードを更新します。 昇格中のサーバーの **[状態]** 列が、 **[ノード]** タブに **[昇格中]** として表示されます。  

3. 昇格が完了すると、 **[状態]** 列には、新しいアクティブ モードのサイト サーバーと新しいパッシブ モードのサイト サーバーの両方に **[OK]** が表示されます。 現在、サイトの **[サーバー名]** 列には、アクティブ モードの新しいサイト サーバーの名前が表示されています。

詳細な状態については、 **[監視]** ワークスペースに移動し、 **[サイト サーバーの状態]** ノードを選択します。 **[モード]** 列でどのサーバーが*アクティブ*または*パッシブ*かを識別できます。 サーバーをパッシブ モードからアクティブ モードに昇格する場合、アクティブに昇格中のサイト サーバーを選択し、リボンから **[状態を表示]** を選びます。 このアクションにより、プロセスに関する追加の詳細が表示される [Site Server Promotion Status]\(サイト サーバーの昇格の状態\) ウィンドウが開きます。

アクティブ モードのサイト サーバーがパッシブ モードに切り替わると、サイト システムの役割のみがパッシブになります。 そのコンピューターにインストールされている他のすべてのサイト システムの役割はアクティブのままで、クライアントからアクセスできます。

*計画済み*の昇格プロセスの詳細については、「[フローチャート - サイト サーバーの昇格 (計画済み)](promote-site-server-flowchart.md)」を参照してください。

### <a name="unplanned-failover"></a>計画外のフェールオーバー

現在のアクティブ モードのサイト サーバーがオフラインの場合、昇格するサイト サーバーでは、30 分間、現在のアクティブ モードのサイト サーバーへの接続を試みます。 それまでにオフラインのサーバーが元に戻った場合、正常に通知され、変更は正常に続行されます。 それ以外の場合は、昇格するサイト サーバーによって、アクティブになるようにサイトの構成が強制的に更新されます。 この期間の後にオフライン サーバーが元に戻ると、まずサイト データベース内の現在の状態が確認されます。 次に、サーバー自体によるパッシブ モードのサイト サーバーへの降格が続けられます。

この 30 分の待機時間中は、サイトにアクティブ モードのサイト サーバーはありません。 クライアントは、管理ポイント、ソフトウェア更新プログラムのポイント、配布ポイントなどのクライアント向けの役割と通信し続けます。 ユーザーは既に展開されているソフトウェアをインストールできます。 この期間は、サイト管理を実行できません。 詳細については、「[サイト障害の影響](../../manage/site-failure-impacts.md)」を参照してください。  

オフライン サーバーを復帰できないなど、サーバーが破損している場合は、コンソールからこのサイト サーバーを削除します。 次に、新しいパッシブ モードのサイト サーバーを作成して、高可用性サービスを復元します。

*計画外*のフェールオーバー プロセスの詳細については、「[フローチャート - サイト サーバーの昇格 (計画外)](promote-site-server-unplanned-flowchart.md)」を参照してください。

### <a name="additional-tasks-after-site-server-promotion"></a>サイト サーバーの昇格後の追加タスク  

サイト サーバーを切り替えた後は、[サイトの回復](../../manage/recover-sites.md#post-recovery-tasks)時に必要なその他のタスクのほとんどは実行する必要がありません。 たとえば、パスワードをリセットしたり、Microsoft Intune サブスクリプションに再接続したりする必要はありません。

ご利用の環境で必要な場合は、次の手順が必要になる可能性があります。  

- 配布ポイントに対する PKI 証明書をインポートする場合は、有効なサーバーの証明書をインポートし直します。 詳細については、「[Regenerate the certificates for distribution points](../../manage/recover-sites.md#regenerate-the-certificates-for-distribution-points)」 (配布ポイント用の証明書の再生成) を参照してください。  

- Configuration Manager をビジネス向け Microsoft Store と統合する場合、その接続を再構成します。 詳細については、[ビジネス向け Microsoft Store からのアプリの管理](../../../../apps/deploy-use/manage-apps-from-the-windows-store-for-business.md)に関するページを参照してください。  

## <a name="daily-monitoring"></a>日常的な監視

パッシブ モードのサイト サーバーを所有している場合、毎日監視します。 その状態が OK のままで、使用する準備ができていることを確認します。 Configuration Manager コンソールで、 **[監視]** ワークスペースに移動して、 **[サイト サーバーの状態]** ノードを選択します。 サイト サーバーとその現在の状態の両方を表示します。 **[管理]** ワークスペースでも状態を表示します。 **[サイトの構成]** を展開し、 **[サイト]** ノードを選択します。 サイトを選択して、 **[ノード]** タブに切り替えます。

> [!NOTE]
> サイトを新しいバージョンの Configuration Manager に更新すると、パッシブ モードのサイト サーバーも更新されます。 <!-- SCCMDocs-pr#4293 -->