---
title: 移行操作
titleSuffix: Configuration Manager
description: データとクライアントを Configuration Manager Current Branch に移行するジョブを作成して実行します。
ms.date: 12/30/2016
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: c28e3492-851a-40fc-ba13-67ebc2d8b41a
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: a93269fc50c7bb39cd47f10e448d30742fc8ab22
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81704570"
---
# <a name="operations-for-migrating-to-configuration-manager-current-branch"></a>Configuration Manager Current Branch に移行するための操作

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager での移行の場合、サポートされるソース階層のソース サイトからデータを適切に収集した後で、データとクライアントを移行できます。 次のセクションの情報を使用して、データとクライアントを移行する移行ジョブを作成して実行した後、移行プロセスを終了します。  

-   [移行ジョブの作成および編集](#Create_Edit_migration_Jobs)  

-   [移行ジョブの実行](#Run_Migration_Jobs)  

-   [共有配布ポイントのアップグレードまたは再割り当て](#BKMK_ProcUpgrdSS)  

-   [[移行] ワークスペースでの移行アクティビティを監視する](#Monitor_MIgration)  

-   [クライアントの移行](#BKMK_MigrateClients)  

-   [移行の完了](#Complete_Migration)  

##  <a name="create-and-edit-migration-jobs"></a><a name="Create_Edit_migration_Jobs"></a> 移行ジョブの作成および編集  
 データの移行ジョブの作成、コレクションに基づく移行ジョブの除外リストの編集、共有配布ポイントの設定、および移行ジョブのスケジュールの編集を行うには、次の手順に従います。  

> [!NOTE]  
>  コレクションごとに移行する移行ジョブを作成するための次の手順は、Configuration Manager 2007 のサポートされるバージョンを実行するソース階層にのみ適用されます。 コレクション ベースの移行ジョブの種類は、System Center 2012 Configuration Manager または Configuration Manager Current Branch のソース階層から移行するときは使用できません。  

#### <a name="create-a-migration-job-to-migrate-by-collections"></a>コレクション別に移行するための移行ジョブを作成する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  **[ホーム]** タブの **[作成]** グループで、 **[移行ジョブの作成]** を選びます。  

4.  移行ジョブの作成ウィザードの **[全般]** ページで、次のように設定してから、 **[OK]** を選びます。  

    -   移行ジョブの名前を指定します。  

    -   **[ジョブの種類]** ドロップダウン リストで、 **[コレクションの移行]** をクリックします。  

5.  **[コレクションの選択]** ページで、次のように設定してから、 **[次へ]** を選びます。  

    -   移行するコレクションを選択します。  

    -   コレクションのみを移行し、それらに関連付けられたオブジェクトは移行しない場合は、 **[指定したコレクションに関連付けられているオブジェクトを移行する]** をオフにします。 このオプションをオフにした場合は、関連付けられたオブジェクトはこのジョブでは移行されないため、手順 6 および 7 に進みます。  

6.  **[オブジェクトの選択]** ページで、オブジェクトの種類または利用可能な個別のオブジェクトのうち、移行しないものをすべてオフにします。 既定では、関連付けられたオブジェクトの種類と利用可能なオブジェクトがすべて選択されます。 **[次へ]** を選択します。  

7.  **[コンテンツの所有権]** ページで、一覧表示された各ソース サイトのコンテンツの所有権を移行先階層のサイトに割り当てて、 **[次へ]** を選びます。  

8.  **[セキュリティ スコープ]** ページで、この移行ジョブで移行するためにオブジェクトに割り当てる、役割に基づいた管理のセキュリティ スコープを 1 つ以上選択してから、 **[次へ]** を選びます。  

9. **[コレクションの限定]** ページで、一覧表示される各コレクションの範囲を限定するように移行先階層のコレクションを設定して、 **[次へ]** を選びます。 コレクションが 1 つも表示されていない場合は、 **[次へ]** を選びます。  

10. **[サイト コードの置き換え]** ページで、一覧表示される各コレクションの Configuration Manager 2007 サイト コードを移行先階層のサイト コードに置き換えて、 **[次へ]** を選びます。 コレクションが 1 つも表示されていない場合は、 **[次へ]** を選びます。  

11. **[情報の確認]** ページで、 **[ファイルに保存する]** を選び、後で閲覧するために表示された情報を保存します。 操作を続行する準備ができたら **[次へ]** を選びます。  

12. **[設定]** ページで、移行ジョブの実行時間を設定し、この移行ジョブに必要な追加設定を選んでから、 **[次へ]** を選びます。  

13. 設定を確認しウィザードを終了します。  

#### <a name="create-a-migration-job-to-migrate-by-objects"></a>オブジェクト別に移行するための移行ジョブを作成する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  **[ホーム]** タブの **[作成]** グループで、 **[移行ジョブの作成]** を選びます。  

4.  移行ジョブの作成ウィザードの **[全般]** ページで、次のように設定してから、 **[次へ]** を選びます。  

    -   移行ジョブの名前を指定します。  

    -   **[ジョブの種類]** ドロップダウン リストで、 **[オブジェクトの移行]** をクリックします。  

5.  **[オブジェクトの選択]** ページで、移行するオブジェクトの種類を選択します。 既定では、選択したオブジェクトの各種類を対象に、利用可能なすべてのオブジェクトが選択されます。  

6.  **[コンテンツの所有権]** ページで、一覧表示された各ソース サイトのコンテンツの所有権を移行先階層のサイトに割り当てて、 **[次へ]** を選びます。 ソース サイトが 1 つも表示されていない場合は、 **[次へ]** を選びます。  

7.  **[セキュリティ スコープ]** ページで、この移行ジョブ内のオブジェクトに割り当てる、役割に基づいた管理のセキュリティ スコープを 1 つ以上選んでから、 **[次へ]** を選びます。  

8.  **[情報の確認]** ページで、 **[ファイルに保存する]** を選び、後で閲覧するために表示された情報を保存します。 操作を続行する準備ができたら **[次へ]** を選びます。  

9. **[設定]** ページで、移行ジョブの実行時間を設定し、この移行ジョブに必要な追加設定を選びます。 **[次へ]** を選びます。  

10. 設定を確認しウィザードを終了します。  

#### <a name="create-a-migration-job-to-migrate-changed-objects"></a>変更されたオブジェクトを移行するための移行ジョブを作成する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  **[ホーム]** タブの **[作成]** グループで、 **[移行ジョブの作成]** を選びます。  

4.  移行ジョブの作成ウィザードの **[全般]** ページで、次のように設定してから、 **[次へ]** を選びます。  

    -   移行ジョブの名前を指定します。  

    -   **[ジョブの種類]** ドロップダウン リストで、 **[移行後に変更されたオブジェクト]** を選びます。  

5.  **[オブジェクトの選択]** ページで、移行するオブジェクトの種類を選択します。 既定では、選択したオブジェクトの各種類を対象に、利用可能なすべてのオブジェクトが選択されます。  

6.  **[コンテンツの所有権]** ページで、一覧表示された各ソース サイトのコンテンツの所有権を移行先階層のサイトに割り当てて、 **[次へ]** を選びます。 ソース サイトが 1 つも表示されていない場合は、 **[次へ]** を選びます。  

7.  **[セキュリティ スコープ]** ページで、この移行ジョブ内のオブジェクトに割り当てる、役割に基づいた管理のセキュリティ スコープを 1 つ以上選んでから、 **[次へ]** を選びます。  

8.  **[情報の確認]** ページで、 **[ファイルに保存する]** を選び、後で閲覧するために表示された情報を保存します。 操作を続行する準備ができたら **[次へ]** を選びます。  

9. **[設定]** ページで、移行ジョブの実行時間を設定し、この移行ジョブに必要な追加設定を選びます。 その他の移行ジョブの種類と異なり、この移行ジョブでは、Configuration Manager データベースに以前に移行済みのオブジェクトを上書きする必要があります。 **[次へ]** を選択します。  

10. 設定を確認して、ウィザードを終了します。  

###  <a name="modify-the-exclusion-list-for-migration"></a><a name="BKMK_Modify_Exclusion_List"></a> 移行の除外リストを変更する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を選んで除外リストにアクセスできるようにします。 除外リストには、 **[ソース階層]** ノード、または、 **[移行ジョブ]** ノードからアクセスすることもできます。  

3.  **[ホーム]** タブの **[移行]** グループで、 **[除外リストの編集]** を選びます。  

4.  **[除外リストの編集]** ダイアログ ボックスで、除外リストから削除する除外オブジェクトを選択してから、 **[削除]** を選びます。  

5.  **[OK]** を選んで変更を保存し、編集を終了します。 現在の変更をキャンセルし、削除したすべてのオブジェクトを復元するには、 **[キャンセル]** 、 **[いいえ]** の順に選びます。 これでオブジェクトの削除が取り消され、 **[除外リストの編集]** ダイアログ ボックスが閉じます。  

#### <a name="share-distribution-points-from-the-source-hierarchy"></a>ソース階層の配布ポイントを共有する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースの **[移行]** を展開し、 **[ソース階層]** を選んで、設定するソース サイトを選びます。  

3.  **[ホーム]** タブの **[ソース サイト]** グループで、 **[構成]** を選びます。  

4.  **[ソース サイトの資格情報]** ダイアログ ボックスで、 **[ソース サイト サーバーの配布ポイント共有を有効にする]** をオンにして **[OK]** を選びます。  

5.  データ収集が終了したら **[閉じる]** を選びます。  

#### <a name="change-the-schedule-of-a-migration-job"></a>移行ジョブのスケジュールを変更する  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  変更する移行ジョブを選びます。 **[ホーム]** タブの **[プロパティ]** グループで、 **[プロパティ]** を選択します。  

4.  移行ジョブのプロパティで、 **[設定]** タブを選び、移行ジョブの実行時間を変更してから、 **[OK]** を選びます。  

##  <a name="run-migration-jobs"></a><a name="Run_Migration_Jobs"></a> 移行ジョブの実行  
 次の手順は、まだ開始されていない移行ジョブの実行に使用します。  


1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  実行する移行ジョブを選びます。 **[ホーム]** タブの **[移行ジョブ]** グループで、 **[開始]** を選びます。  

4.  **[はい]** を選び、移行ジョブを開始します。  

##  <a name="upgrade-or-reassign-a-shared-distribution-point"></a><a name="BKMK_ProcUpgrdSS"></a> 共有配布ポイントのアップグレードまたは再割り当て  
 Configuration Manager 2007 ソース サイトから共有されているサポート対象の配布ポイントをアップグレードするか、Configuration Manager ソース サイトから共有されているサポート対象の配布ポイントを再割り当てすることで、移行先階層内の配布ポイントにすることができます。  

> [!IMPORTANT]  
>  Configuration Manager 2007 ブランチ配布ポイントをアップグレードする前に、ブランチ配布ポイント コンピューターから Configuration Manager 2007 クライアント ソフトウェアをアンインストールする必要があります。 配布ポイントをアップグレードするときに Configuration Manager 2007 クライアント ソフトウェアをインストールすると、アップグレードは失敗し、以前にブランチ配布ポイントに展開されたコンテンツは、コンピューターから削除されます。  

> [!CAUTION]  
>  共有配布ポイントをアップグレードまたは再割り当てすると、配布ポイント サイト システムの役割とサイト システム コンピューターはソース サイトから削除され、選択した移行先階層内のサイトに配布ポイントとして追加されます。  

#### <a name="upgrade-or-reassign-a-shared-distribution-point"></a>共有配布ポイントのアップグレードまたは再割り当て  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開して、 **[ソース階層]** を選びます。  

3.  アップグレードする配布ポイントを所有しているサイトを選び、 **[共有配布ポイント]** タブを選んで、アップグレードまたは再割り当て対象の配布ポイントを選びます。  

4.  **[配布ポイント]** タブの **[配布ポイント]** グループで、 **[再割り当て]** を選びます。  

5.  共有配布ポイントの再割り当てウィザードで、移行先階層の新しい配布ポイントをインストールする場合と同様の設定を指定し、次の項目を追加指定します。  

    -   **[コンテンツの変換]** ページで、既存のコンテンツの変換に必要な容量に関するガイドを確認します。 次に、ウィザードの **[ドライブ設定]** ページで、選択した配布ポイント コンピューターのドライブに、必要ディスク空き容量があることを確認します。  

6.  設定を確認して、ウィザードを終了します。  

##  <a name="monitor-migration-activity-in-the-migration-workspace"></a><a name="Monitor_MIgration"></a> [移行] ワークスペースでの移行アクティビティを監視する  
 Configuration Manager コンソールを使って移行を監視します。  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開し、 **[移行ジョブ]** を選びます。  

3.  監視する移行ジョブを選びます。  

4.  **[概要]** と **[ジョブ内のオブジェクト]** の各タブに、選択された移行ジョブに関する内容と状態を表示します。  

##  <a name="migrate-clients"></a><a name="BKMK_MigrateClients"></a> クライアントの移行  
 階層間でクライアントのデータを移行した後、移行を終了する前に、クライアントを移行先階層に移行する計画を立てます。 階層間のクライアントの移行には、ソース階層に割り当てられているコンピューターから Configuration Manager クライアント ソフトウェアを削除し、その後で、移行先階層から Configuration Manager クライアント ソフトウェアをインストールする手順が含まれます。 移行先階層からクライアントをインストールするときに、その階層のプライマリ サイトへのクライアントの割り当ても行います。 クライアントの移行の詳細については、「[クライアント移行戦略の計画](../../core/migration/planning-a-client-migration-strategy.md)」をご覧ください。  

##  <a name="finish-migration"></a><a name="Complete_Migration"></a>移行の完了  
 ソース階層からの移行を終了するには、次の手順に従います。  

1.  Configuration Manager コンソールで、 **[管理]** を選択します。  

2.  **[管理]** ワークスペースで、 **[移行]** を展開して、 **[ソース階層]** を選びます。  

3.  Configuration Manager 2007 ソース階層の場合、ソース階層の最下位レベルにあるソース サイトを選択します。 System Center 2012 Configuration Manager または Configuration Manager Current Branch のソース階層の場合は、使用可能なソース サイトを選択します。  

4.  **[ホーム]** タブの **[クリーンアップ]** グループで、 **[データ収集の停止]** を選びます。  

5.  **[はい]** を選んで操作を確定します。  

6.  Configuration Manager 2007 ソース階層の場合、次の手順に進む前に、手順 3、4、5 を繰り返します。 最下位の階層から最上位の階層まで、階層内の各サイトでこれらの手順を実行します。 System Center 2012 Configuration Manager または Configuration Manager Current Branch のソース階層の場合は、次の手順に進んでください。  

7.  **[ホーム]** タブの **[クリーンアップ]** グループで、 **[移行データのクリーンアップ]** を選びます。  

8.  **[移行データのクリーンアップ]** ダイアログ ボックスの **[ソース階層]** ドロップダウン リストで、ソース階層の最上位レベルのサイト コードとサイト サーバーを選んでから、 **[OK]** を選びます。  

9. **[はい]** を選んで、ソース階層の移行処理を終了します。  