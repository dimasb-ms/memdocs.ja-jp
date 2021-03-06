---
title: 事前設定されたメディアの作成
titleSuffix: Configuration Manager
description: いくつかのシナリオで Windows の展開を簡素化するため、Configuration Manager で事前設定されたメディアを使用します。
ms.date: 05/02/2019
ms.prod: configuration-manager
ms.technology: configmgr-osd
ms.topic: how-to
ms.assetid: ff6e7267-302a-4563-815e-cdc0d1a4b60f
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 82bb02d8154939b4b0e0ee89bcc6637e9393acff
ms.sourcegitcommit: d225ccaa67ebee444002571dc8f289624db80d10
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88125221"
---
# <a name="create-prestaged-media"></a>事前設定されたメディアの作成

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager で事前設定されたメディアは、Windows イメージ (WIM) ファイルです。 これは、製造元または Configuration Manager の運用環境に接続していない自社のステージング センターで、ベアメタル コンピューターにインストールすることができます。 事前設定されたメディアには、展開先のコンピューターを起動するのに使用するブート イメージと、展開先のコンピューターに適用される OS イメージが含まれています。 また、事前設定されたメディアの一部として含める、アプリケーション、パッケージ、およびドライバー パッケージを指定できます。 OS を展開するタスク シーケンスはメディアに含まれていません。 事前設定されたメディアは、コンピューターがエンドユーザーに渡る前に、新しいコンピューターのハード ドライブに適用されます。

事前設定されたメディアは、次の OS の展開シナリオに使用します。  

- [工場出荷時の OEM 用、または現地保管場所用のイメージの作成](create-an-image-for-an-oem-in-factory-or-a-local-depot.md)  

- [新しいコンピューター (ベア メタル) に新しいバージョンの Windows をインストールする](install-new-windows-version-new-computer-bare-metal.md)  

- [Windows to Go の展開](deploy-windows-to-go.md)  


## <a name="usage"></a>使用方法

事前設定されたメディアを適用した後、最初にコンピューターを起動すると、コンピューターは Windows PE で起動されます。 管理ポイントに接続して、OS の展開プロセスを完了するタスク シーケンスを検索します。 事前設定されたメディアを使用するタスク シーケンスを展開するときに、最初にタスク シーケンスのローカル キャッシュで有効なコンテンツがクライアントによってチェックされます。 コンテンツが見つからない場合や変更されている場合は、クライアントによって配布ポイントまたはピアからコンテンツがダウンロードされます。  


## <a name="prerequisites"></a>[前提条件]

タスク シーケンス メディアの作成ウィザードを使用して、事前設定されたメディアを作成する前に、すべての条件が満たされていることを確認してください。

### <a name="boot-image"></a>ブート イメージ

OS を展開するためにタスク シーケンスで使用するブート イメージについて、次の点を考慮してください。

- ブート イメージのアーキテクチャが、展開先のコンピューターのアーキテクチャに対して適切である必要があります。 たとえば、x64 のコンピューターでは、x86 または x64 のブート イメージを起動して実行できます。 ただし、x86 のコンピューターで起動して実行できるのは、x86 ブート イメージのみです。
- ブート イメージに、対象コンピューターのプロビジョニングに必要なネットワーク ドライバーと記憶装置ドライバーが含まれている必要があります。

### <a name="create-a-task-sequence-to-deploy-an-os"></a>OS を展開するためのタスク シーケンスの作成

事前設定されたメディアの一部として、OS を展開するタスク シーケンスを指定します。 詳しくは、[OS をインストールするタスク シーケンスの作成](create-a-task-sequence-to-install-an-operating-system.md)に関するページをご覧ください。

### <a name="distribute-all-content-associated-with-the-task-sequence"></a>タスク シーケンスに関連付けられているすべてのコンテンツを配布する

少なくとも 1 つの配布ポイントに対して、タスク シーケンスで必要なすべてのコンテンツを配布します。 このコンテンツには、ブート イメージ、OS イメージ、およびその他の関連するファイルが含まれます。 事前設定されたメディアの作成時に、ウィザードによって配布ポイントからコンテンツが収集されます。

ユーザー アカウントには、少なくとも配布ポイントのコンテンツ ライブラリへの**読み取り**アクセス権が必要です。 詳細については、「[コンテンツの配布](../../core/servers/deploy/configure/deploy-and-manage-content.md#bkmk_distribute)」をご覧ください。

### <a name="hard-drive-on-the-destination-computer"></a>展開先のコンピューター上のハード ディスク ドライブ

事前設定されたメディアを適用する前に、展開先のコンピューターのハード ドライブをフォーマットする必要があります。 メディアの適用時にハード ドライブがフォーマットされていないと、OS を展開するタスク シーケンスが対象コンピューターを起動しようとしたときに、このタスク シーケンスは失敗します。

> [!NOTE]  
> タスク シーケンス メディアの作成ウィザードは、次のタスク シーケンス変数条件をメディアに設定します: **_SMSTSMediaType = OEMMedia**。 これと同じ条件をタスク シーケンスで使用できます。  


## <a name="process"></a>プロセス

 > [!NOTE]  
 > PKI 環境では、ルート CA がプライマリ サイトで指定されるため、事前設定されたメディアがプライマリ サイトで作成されていることを確認してください。 CAS サイトには、事前設定されたメディアを適切に作成するためのルート CA 情報はありません。

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** ワークスペースに移動し、 **[オペレーティング システム]** を展開して **[タスク シーケンス]** ノードを選択します。  

2. リボンの **[ホーム]** タブにある **[作成]** グループで、 **[タスク シーケンス メディアの作成]** を選択します。 このアクションにより、タスク シーケンス メディアの作成ウィザードが起動します。  

3. **[メディアの種類の選択]** ページで次のオプションを指定します。  

    - **[事前設定されたメディア]** を選択します。  

    - 必要に応じて、ユーザーの入力なしに OS を展開できるようにするには、 **[オペレーティング システムの無人展開を許可する]** を選択します。  

        > [!IMPORTANT]  
        > このオプションを選択すると、ネットワーク構成情報またはオプションのタスク シーケンスのプロンプトはユーザーに表示されません。 メディアにパスワード保護を構成した場合、ユーザーは引き続きパスワードの入力を求められます。  

4. **[メディア管理]** ページで次のオプションのいずれかを指定します。  

    - **動的メディア**: サイト境界内のクライアントの場所に基づいて、ある管理ポイントから別の管理ポイントにメディアをリダイレクトできるようにします。  

    - **サイトベースのメディア**: メディアは、指定された管理ポイントにのみ接続します。  

5. **[メディアのプロパティ]** ページで、次の情報を指定します。  

    - **作成者**: メディアの作成者を指定します。  

    - **バージョン**:メディアのバージョン番号を指定します。  

    - **コメント**: メディアの用途に関する一意の説明を指定します。  

    - **メディア ファイル**: 出力ファイルの名前とパスを指定します。 この場所に出力ファイルが書き込まれます。 例: `\\servername\folder\outputfile.wim`  

    - **ステージング フォルダー**<!--1359388-->:メディアの作成プロセスでは、一時ドライブに大きな空き領域が必要になる場合があります。 既定では、この場所は次のようなパスになります。`%UserProfile%\AppData\Local\Temp` これらの一時ファイルの格納場所をより柔軟に選択できるようにするため、バージョン 1902 以降では、この値を別のドライブおよびパスに変更します。  

6. **[セキュリティ]** ページで、次のオプションを指定します。  

    - **不明なコンピューターのサポートを有効にする**: Configuration Manager によって管理されていないコンピューターに、メディアによって OS を展開できるようにします。 これらのコンピューターのレコードは Configuration Manager データベースには存在しません。 詳細については、「[不明なコンピューターの展開の準備](../get-started/prepare-for-unknown-computer-deployments.md)」を参照してください。  

    - **[メディアをパスワードで保護する]** :承認されていないアクセスからメディアを保護するために強力なパスワードを入力します。 パスワードを指定した場合、ユーザーは、事前設定メディアを使用するにはパスワードを入力する必要があります。  

        > [!IMPORTANT]  
        > セキュリティ運用方法として、事前設定メディアを保護するために、常にパスワードを指定することをお勧めします。  
 
    - HTTP 通信の場合は、 **[自己署名入りメディア証明書を作成する]** を選択します。 次に、証明書の開始日と終了日を指定します。  
    
        > [!NOTE] 
        > このオプションを選択した場合、このウィザードの **ブート イメージ** ページで HTTPS 管理ポイント を選択できなくなります。

    - HTTPS 通信の場合は、 **[PKI 証明書のインポート]** を選択します。 次に、インポートする証明書とそのパスワードを指定します。  

        ブート イメージで使用される、このクライアント証明書の詳細については、[PKI 証明書の要件](../../core/plan-design/network/pki-certificate-requirements.md)に関するページを参照してください。  

    - **ユーザーとデバイスのアフィニティ**: Configuration Manager でユーザー中心の管理をサポートするため、ユーザーと対象コンピューターをメディアによって関連付ける方法を指定します。 OS の展開でユーザーとデバイスのアフィニティをサポートする方法については、[展開先のコンピューターにユーザーを関連付ける](../get-started/associate-users-with-a-destination-computer.md)方法に関するページをご覧ください。  

        - **ユーザーとデバイスのアフィニティを自動的に承認する**: ユーザーと展開先のコンピューターがメディアによって自動的に関連付けられます。 この機能は、OS を展開するタスク シーケンスのアクションに基づきます。 このシナリオでは、タスク シーケンスで展開先のコンピューターに OS が展開されるときに、指定のユーザーと展開先のコンピューターのリレーションシップが作成されます。  

        - **ユーザーとデバイスのアフィニティを管理者の承認待ちにする**: 承認後に、ユーザーと展開先のコンピューターがメディアによって関連付けられます。 この機能は、OS を展開するタスク シーケンスのスコープに基づきます。 このシナリオでは、タスク シーケンスは、指定のユーザーと展開先のコンピューターのリレーションシップを作成しますが、OS を展開する前に管理ユーザーからの承認を待機します。  

        - **ユーザーとデバイスのアフィニティを許可しない**: ユーザーと展開先のコンピューターは、メディアによって関連付けられません。 このシナリオでは、タスク シーケンスでは OS の展開時にユーザーが展開先のコンピューターに関連付けられません。  

7. **[タスク シーケンス]** ページで、展開先のコンピューターで実行されるタスク シーケンスを選択します。 タスク シーケンスによって参照されるコンテンツの一覧を確認します。  

    - **関連付けられたアプリケーションの依存関係を検出し、このメディアに追加する**: また、アプリケーションの依存関係のメディアにコンテンツを追加します。  

        > [!TIP]  
        > 必要なアプリケーションの依存関係が表示されない場合は、このオプションを選択解除してから再度選択し、リストを更新します。  

8. **[ブート イメージ]** ページで、次のオプションを指定します。  

    > [!IMPORTANT]  
    > 配布するブート イメージのアーキテクチャが、配布先のコンピューターのアーキテクチャに適している必要があります。 たとえば、x64 のコンピューターでは、x86 または x64 のブート イメージを起動して実行できます。 ただし、x86 のコンピューターで起動して実行できるのは、x86 ブート イメージのみです。  

    - **ブート イメージ**:展開先のコンピューターを起動するブート イメージを選択します。  

    - **配布ポイント**: ブート イメージがある配布ポイントを選択します。 配布ポイントからブート イメージが取得されてメディアに書き込まれます。  

        > [!NOTE]  
        > ユーザー アカウントには、少なくとも配布ポイントのコンテンツ ライブラリへの**読み取り**アクセス権が必要です。  

    - **管理ポイント**: "*サイトベースのメディア*" に対してのみ、プライマリ サイトから管理ポイントを選択します。  

    - **関連付けられている管理ポイント**: "*動的メディア*" に対してのみ、使用するプライマリ サイトの管理ポイントと、初期通信での優先順位を選択します。  

        > [!NOTE]  
        > HTTPS が有効な管理ポイントは、このウィザードの **[セキュリティ]** ページで PKI 証明書が指定されている場合にのみ表示されます。  

9. **[イメージ]** ページで、次のオプションを指定します。  

    - **イメージ パッケージ**:使用する OS イメージを指定します。 詳しくは、[OS イメージの管理](../get-started/manage-operating-system-images.md)に関するページをご覧ください。  

    - **イメージ インデックス**:パッケージに複数の OS イメージが含まれている場合は、展開するイメージのインデックスを指定します。  

    - **配布ポイント**: OS イメージ パッケージがある配布ポイントを指定します。 ウィザードによって配布ポイントから OS イメージが取得されてメディアに書き込まれます。  

10. **[アプリケーションの選択]** ページで、事前設定されたメディア ファイルに追加するその他のアプリケーションを選択します。  

11. **[パッケージの選択]** ページで、事前設定されたメディア ファイルに追加するその他のパッケージを選択します。  

12. **[ドライバー パッケージの選択]** ページで、事前設定されたメディア ファイルに追加するその他のドライバー パッケージを選択します。  

13. **[配布ポイント]** ページで、コンテンツの取得先となる 1 つまたは複数の配布ポイントを選択します。  

    Configuration Manager には、コンテンツが含まれる配布ポイントのみが表示されます。 続行する前に、タスク シーケンスに関連付けられているすべてのコンテンツを少なくとも 1 つの配布ポイントに配布します。 コンテンツを配布した後、配布ポイントの一覧を更新します。 このページで既に選択している配布ポイントをすべて削除し、前のページに移動して **[配布ポイント]** ページに戻ります。 または、ウィザードを再起動します。 詳細については、「[タスク シーケンスによって参照されるコンテンツの配布](manage-task-sequences-to-automate-tasks.md#BKMK_DistributeTS)」と「[コンテンツとコンテンツ インフラストラクチャの管理](../../core/servers/deploy/configure/manage-content-and-content-infrastructure.md)」を参照してください。  

14. **[カスタマイズ]** ページで、次のオプションを指定します。  

    - タスク シーケンスで使用される変数を追加します。  

    - **起動前コマンドを有効にする**: タスク シーケンスを実行する前に実行する起動前コマンドを指定します。 起動前コマンドは、タスク シーケンスを実行する前に、Windows PE でユーザーと対話できるスクリプトまたは実行可能ファイルです。 詳細については、「[タスク シーケンス メディアの起動前コマンド](../understand/prestart-commands-for-task-sequence-media.md)」を参照してください。  

        > [!TIP]  
        > メディアの作成中に、タスク シーケンスによって、パッケージ ID と起動前コマンドライン (タスク シーケンスの変数の値を含む) が、Configuration Manager コンソールを実行しているコンピューターの **CreateTSMedia.log** というログファイルに書き込まれます。 このログ ファイルで、タスク シーケンスの変数の値を確認できます。  

        起動前コマンドで何らかのコンテンツが必要な場合は、 **[起動前コマンドにファイルを含める]** オプションを選択します。  

15. ウィザードを完了します。  


## <a name="next-steps"></a>次のステップ

[工場出荷時の OEM 用、または現地保管場所用のイメージの作成](create-an-image-for-an-oem-in-factory-or-a-local-depot.md)
