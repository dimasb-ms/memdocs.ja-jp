---
title: 'クイック スタート: Microsoft Intune を無料で試す'
titleSuffix: ''
description: このクイック スタートでは、無料試用版サブスクリプションを作成し、サポートされている構成とネットワーク要件を理解し、必要に応じてドメイン名を構成します。
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 08/13/2020
ms.topic: quickstart
ms.service: microsoft-intune
ms.subservice: fundamentals
ms.localizationpriority: high
ms.technology: ''
ms.assetid: 195931c0-8208-43bd-b0af-b1f8e469a32c
ms.reviewer: ''
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6ce7aeb5d7a64d98ec27fe3882c256120358d3c
ms.sourcegitcommit: fde92731a7e27c892d32c63f515cf19545e02ceb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2020
ms.locfileid: "88994159"
---
# <a name="quickstart-try-microsoft-intune-for-free"></a>クイック スタート: Microsoft Intune を無料で試す

Microsoft Intune でデバイスやアプリケーションを管理すると、従業員の会社データを保護できます。 このクイック スタートでは、Intune をテスト環境で試用する無料のサブスクリプションを作成します。

Intune には、モバイル デバイス管理 (MDM) 機能とモバイル アプリケーション管理 (MAM) 機能があります。これらの機能は、Microsoft Endpoint Manager admin center を使用して管理されているセキュリティで保護されたクラウドベースのサービスで利用できます。 Intune を使用すると、企業のコンプライアンス ポリシーや要件を満たしながら、従業員の会社リソース (データ、デバイス、アプリ) の構成、アクセス、更新を適切に行うことができるようになります。

## <a name="prerequisites"></a>[前提条件]
Microsoft Intune を設定する前に、次の要件を確認してください。

- [サポートされるオペレーティング システムとブラウザー](supported-devices-browsers.md)
- [ネットワーク構成の要件と帯域幅](network-bandwidth-use.md)

## <a name="sign-up-for-a-microsoft-intune-free-trial"></a>Microsoft Intune の無料試用版にサインアップ

Intune は 30 日間無料で試用できます。 既に職場または学校アカウントがある場合は、そのアカウントで**サインイン**し、Intune をサブスクリプションに追加します。 それ以外の場合は、新しいアカウントに**サインアップ**して、組織で Intune を使うことができます。

> [!IMPORTANT]
> 新しいアカウントにサインアップした後で、既存の職場または学校アカウントを組み合わせることはできません。

1. [Microsoft Intune の無料試用版](https://go.microsoft.com/fwlink/?linkid=2019088)ページにアクセスしてフォームに入力してください。

    ![Microsoft Intune 試用版アカウントのサインアップ Web ページのスクリーンショット](./media/free-trial-sign-up/account-sign-up-site-full-browser.png)

    IT 運用チームやユーザーの多くが自分と異なる場所に属している場合は、その場所を **[国または地域]** で選択することをお勧めします。 Azure は地域情報を使用して適切なサービスを提供しています。 この設定は後で変更できません。

2. 会社名に続けて「 **.onmicrosoft.com**」と入力してアカウントを作成します。 

    ![Intune 試用版アカウントの新しい資格情報の手順のスクリーンショット](./media/free-trial-sign-up/account-sign-up-site-user-id.png)

    **.onmicrosoft.com** ではない独自のカスタム ドメインを組織で使用したい場合は、この記事で後述する Microsoft 365 管理センターで変更することができます。

3. サインアップ プロセスの最後に新しいアカウント情報が表示されます。

    ![アカウント情報の画像](./media/free-trial-sign-up/intune-end-of-sign-up-process.png) 

## <a name="sign-in-to-intune-in-the-microsoft-endpoint-manager"></a>Microsoft Endpoint Manager で Intune にサインインする

まだポータルにサインインしていない場合は、次の手順を実行します。

1. 新しいブラウザー ウィンドウを開き、アドレスバーに「 **https://endpoint.microsoft.com** 」と入力します。 
2. 前の手順で指定したユーザー ID を使用して、サインインします ( *yourID@yourdomain* .onmicrosoft.com)。

    ![ポータルのサインイン ページの画像](./media/free-trial-sign-up/azure-portal-signin.png)

試用版にサインアップすると、アカウント情報の記載された電子メール メッセージが、サインアップ プロセス中に指定した電子メール アドレスに送信されます。 このメールで、試用版がアクティブになったことが確認されます。

> [!TIP]
> Microsoft Endpoint Manager を使用する場合、プライベート モードではなく、通常モードでブラウザーを使用する方が良い結果が得られる場合があります。

## <a name="confirm-the-mdm-authority-in-intune"></a>Intune で MDM 機関を確認する

既定では、無料試用版を作成するときに MDM 機関が設定されます。 MDM 機関が設定されていることを確認するには、次の手順を実行します。

1. まだサインインしていない場合は、Microsoft Endpoint Manager にサインインします。
2. **[テナント管理]** をクリックします。
3. テナントの詳細を表示します。 **[MDM 機関]** は **[Microsoft Intune]** に設定する必要があります。

Microsoft Endpoint Manager にサインインした後、まだ MDM 機関が設定されていないことを示すオレンジ色のバナーが表示される場合は、この時点で有効にすることができます。 モバイル デバイス管理 (MDM) 機関の設定によって、デバイスの管理方法が決まります。 MDM 機関を設定してからでないと、ユーザーは管理対象のデバイスを登録できません。

### <a name="to-set-the-mdm-authority-to-intune-follow-these-steps"></a>MDM 機関を Intune に設定するには、次の手順を実行します。

1. 新しいブラウザー ウィンドウを開き、アドレスバーに「 **https://portal.azure.com** 」と入力します。 
2. **[すべてのサービス]**  >  **[Microsoft Intune]** を選択します。
3. デバイスの管理を有効にしていないことを示すバナーを選択するか、すぐにバナーが表示されない場合は **[デバイスの登録]** を選択します。 デバイス管理をまだ有効にしていない場合、 **[MDM 機関の選択]** ブレードが表示されます。

    > [!NOTE]
    > MDM 機関を設定した場合に、MDM 機関の値が **[デバイスの登録]** ブレードに表示されます。 オレンジのバナーは、MDM 機関をまだ設定していない場合にのみ表示されます。 

    ![[MDM 機関の選択] ブレードの画像](./media/free-trial-sign-up/choose-mdm-authority.png) 

4. MDS 機関が設定されていない場合は、 **[MDM 機関の選択]** で、MDM 機関を **[Intune MDM 機関]** に設定します。

MDM 機関について詳しくは、「[モバイル デバイス管理機関の設定](mdm-authority-set.md)」をご覧ください。

## <a name="configure-your-custom-domain-name-optional"></a>カスタム ドメイン名を構成する (省略可能)

前述のように、 **.onmicrosoft.com** ではない独自のカスタム ドメインを組織で使用したい場合は、Microsoft 365 管理センターで変更することができます。 次の手順に従ってカスタム ドメイン名を追加、確認、および構成できます。  

> [!IMPORTANT]
> "*初期*" ドメイン名の **onmicrosoft.com** の部分は、名前変更または削除できません。 ただし、企業のアイデンティティを明確にするために、Intune で使う*カスタム* ドメイン名を追加、確認、削除することはできます。 詳細については、[カスタム ドメイン名の構成](custom-domain-name-configure.md)に関するページを参照してください。

1. [Microsoft 365 管理センター](https://admin.microsoft.com)を開き、管理者アカウントを使用してサインインします。

2. ナビゲーション ウィンドウで **[セットアップ]**  >  **[ドメイン]**  >  **[ドメイン名の追加]** の順に選択します。

3. カスタム ドメイン名を入力します。 **[次へ]** を選択します。

   ![Microsoft 365 管理センターのスクリーンショット - [ドメイン名の追加]](./media/free-trial-sign-up/domain-custom-add.png)

4. 前の手順で入力したドメインの所有者が自分であることを確認します。 
    
    **[メールでコードを送信する]** を選択すると、ドメインの登録済み連絡先にメールが送信されます。 メールを受け取ったら、コードをコピーし、 **[ここに確認コードを入力]** というフィールドに貼り付けます。 確認コードが一致すると、ドメインがテナントに追加されます。 見慣れないメール アドレスが表示されることがあります。 一部のレジストラーは、実際のメール アドレスを表示しません。 また、ドメインの登録時のものとメール アドレスが異なる場合があります。

   ![Microsoft 365 管理センターのスクリーンショット - [ドメイン名の確認]](./media/free-trial-sign-up/domain-custom-verify.png)

   > [!NOTE]
   > TXT レコードの検証の詳細については、[任意の DNS ホスティング プロバイダーで Microsoft 365 用の DNS レコードを作成する](https://support.office.com/article/Create-DNS-records-at-any-DNS-hosting-provider-for-Office-365-7B7B075D-79F9-4E37-8A9E-FB60C1D95166)方法に関するページを参照してください。

## <a name="admin-experiences"></a>管理者向けの操作性

最も頻繁に使用するポータルは 2 つあります。
- Microsoft Endpoint Manager admin center ([https://endpoint.microsoft.com/](https://endpoint.microsoft.com/)) では、[Intune の機能](what-is-intune.md)を調べることができます。 管理者はここで Intune を操作します。
- Microsoft 365 管理センター ([https://admin.microsoft.com](https://admin.microsoft.com)) では、ユーザーの追加と管理を行えます (このために Azure Active Directory を使用していない場合)。 また、課金やサポートなど、アカウントのその他の要素を管理することもできます。

## <a name="next-steps"></a>次のステップ

このクイック スタートでは、Intune をテスト環境で試用する無料のサブスクリプションを作成します。 Intune のセットアップについて詳しくは、「[Intune をセットアップする](setup-steps.md)」をご覧ください。

この一連の Intune のクイック スタートに従うには、次のクイック スタートに進んでください。

> [!div class="nextstepaction"]
> [クイック スタート: ユーザーを作成してライセンスを割り当てる](quickstart-create-user.md)
