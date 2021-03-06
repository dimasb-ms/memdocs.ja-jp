---
title: Microsoft Intune で macOS デバイス用の Wi-Fi 設定を構成する - Azure | Microsoft Docs
titleSuffix: ''
description: macOS デバイス用の Wi-Fi デバイス構成プロファイルを作成または追加します。 さまざまな設定を確認し、証明書を追加し、EAP の種類を選択し、Microsoft Intune で認証方法を選択します。
keywords: ''
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 06/10/2020
ms.topic: reference
ms.service: microsoft-intune
ms.subservice: configuration
ms.localizationpriority: medium
ms.technology: ''
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1cd60b8a9a8612034972e8357d2e346d194ca3a
ms.sourcegitcommit: 387706b2304451e548d6d9c68f18e4764a466a2b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2020
ms.locfileid: "85092897"
---
# <a name="add-wi-fi-settings-for-macos-devices-in-microsoft-intune"></a>Microsoft Intune で macOS デバイス向けの Wi-Fi 設定を追加する

特定の Wi-Fi 設定でプロファイルを作成し、macOS デバイスにこのプロファイルを展開することができます。 Microsoft Intune では、ネットワークに対する認証や、PKCS または SCEP 証明書の追加など、多くの機能が提供されています。

これらの Wi-Fi 設定は、次の 2 つのカテゴリに分かれています:基本設定とエンタープライズ設定。

この記事では、これらの設定について説明します。

## <a name="before-you-begin"></a>始める前に

[デバイス プロファイルを作成します](wi-fi-settings-configure.md)。

> [!NOTE]
> これらの設定は、すべての登録の種類で使用できます。 登録の種類の詳細については、[macOS の登録](../enrollment/macos-enroll.md)に関する記事をご覧ください。

## <a name="basic-profiles"></a>Basic プロファイル

基本プロファイルまたは個人プロファイルでは WPA/WPA2 を使用し、デバイスの Wi-Fi 接続をセキュリティで保護します。 通常、WPA/WPA2 はホーム ネットワークや個人ネットワークで使用されます。 接続を認証するため、事前共有キーを追加することもできます。

- **[Wi-Fi の種類]** : **[基本]** を選択します。
- **[ネットワーク名]** :この Wi-Fi 接続の名前を入力します。 この値は、ユーザーがデバイスで使用可能な接続の一覧を参照しているときに表示される名前です。
- **[SSID]** :**サービス セット識別子**の短縮形です。 このプロパティは、デバイスの接続先のワイヤレス ネットワークの実際の名前です。 ただし、ユーザーが接続を選択したときにユーザーに表示されるのは、構成されたネットワーク名のみです。
- **[自動的に接続する]** : **[有効]** を選択すると、デバイスが範囲内に入るとこのネットワークに自動的に接続します。 **[無効]** を選択すると、デバイスは自動的に接続されません。
- **[非公開のネットワーク]** : **[有効]** を選択すると、デバイス上で使用可能なネットワークのリストにこのネットワークが含まれなくなります。 SSID はブロードキャストされません。 **[無効]** を選択すると、デバイスで使用可能なネットワークのリストにこのネットワークが含まれるようになります。
- **[セキュリティの種類]** :Wi-Fi ネットワークへの認証に使用するセキュリティ プロトコルを選択します。 次のようなオプションがあります。

  - **[オープン (認証なし)]** :このオプションは、ネットワークがセキュリティで保護されていない場合にのみ使用します。
  - **[WPA/WPA2 - Personal]/(WPA/WPA2 - パーソナル/)** : **[事前共有キー]** にパスワードを入力します。 組織のネットワークがセットアップまたは構成されるときに、パスワードまたはネットワーク キーも構成されます。 PSK の値に対して、このパスワードまたはネットワーク キーを入力します。
  - **4**

- **[プロキシ設定]** :次のようなオプションがあります。
  - **なし**: プロキシ設定は構成されません。
  - **手動**:IP アドレスとしての **[プロキシ サーバーのアドレス]** およびその **[ポート番号]** を入力します。
  - **自動**:ファイルを使用してプロキシ サーバーを構成します。 構成ファイルが格納されている **[プロキシ サーバーの URL]** を入力します (例: `http://proxy.contoso.com`)。

## <a name="enterprise-profiles"></a>Enterprise プロファイル

エンタープライズ プロファイルでは拡張認証プロトコル (EAP) を使用し、Wi-Fi 接続を認証します。 EAP の場合、証明書を利用して接続を認証し、セキュリティで保護し、構成するセキュリティ オプションの数が多いため、企業で使用されることが多いです。

- **[Wi-Fi の種類]** : **[Enterprise]** を選択します。
- **[SSID]** :**サービス セット識別子**の短縮形です。 このプロパティは、デバイスの接続先のワイヤレス ネットワークの実際の名前です。 ただし、ユーザーが接続を選択したときにユーザーに表示されるのは、構成されたネットワーク名のみです。
- **[自動的に接続する]** : **[有効]** を選択すると、デバイスが範囲内に入るとこのネットワークに自動的に接続します。 **[無効]** を選択すると、デバイスは自動的に接続されません。
- **[非公開のネットワーク]** : **[有効]** を選択すると、デバイス上で使用可能なネットワークのリストにこのネットワークが含まれなくなります。 SSID はブロードキャストされません。 **[無効]** を選択すると、デバイスで使用可能なネットワークのリストにこのネットワークが含まれるようになります。

- **[EAP の種類]** :次の中から、セキュリティで保護されたワイヤレス接続の認証に使用される拡張認証プロトコル (EAP) の種類を選択します。 次のようなオプションがあります。

  - **[EAP-FAST]** : **[Protected Access Credential (PAC) の設定]** を入力します。 このオプションでは、Protected Access Credential を使用してクライアントと認証サーバーの間に認証済みトンネルが作成されます。 次のようなオプションがあります。
    - **[使用不可 (PAC)]**
    - **[使用 (PAC)]** :既存の PAC ファイルが存在する場合は、それを使用します。
    - **[PAC の使用とプロビジョニング]** :PAC ファイルを作成してデバイスに追加します。
    - **[匿名による PAC の使用とプロビジョニング]** :サーバーに対する認証を行わずに、PAC ファイルを作成してデバイスに追加します。

  - **EAP-SIM**

  - **[EAP-TLS]** :次の項目も入力します。

    - **[サーバー信頼]**  -  **[証明書サーバー名]** :信頼された証明機関 (CA) によって発行された証明書で使用される 1 つ以上の共通名を**追加**します。 この情報を入力すると、この Wi-Fi ネットワークに接続するときに、ユーザーのデバイスに表示される動的な信頼ウィンドウをバイパスできます。
    - **[サーバー検証のためのルート証明書]** :既存の信頼されたルート証明書プロファイルを 1 つまたは複数選択します。 クライアントがネットワーク接続されると、これらの証明書がサーバーに提示されます。 証明書によって接続が認証されます。

    - **[クライアント認証]**  -  **[クライアント認証に使用するクライアント証明書 (ID 証明書)]** :やはりデバイスに展開される SCEP または PKCS クライアント証明書プロファイルを選択します。 この証明書は、接続の認証のためにデバイスによってサーバーに提示される ID です。

  - **[EAP-TTLS]** :次の項目も入力します。

    - **[サーバー信頼]**  -  **[証明書サーバー名]** :信頼された証明機関 (CA) によって発行された証明書で使用される 1 つ以上の共通名を**追加**します。 この情報を入力すると、この Wi-Fi ネットワークに接続するときに、ユーザーのデバイスに表示される動的な信頼ウィンドウをバイパスできます。
    - **[サーバー検証のためのルート証明書]** :既存の信頼されたルート証明書プロファイルを 1 つまたは複数選択します。 クライアントがネットワーク接続されると、これらの証明書がサーバーに提示されます。 証明書によって接続が認証されます。

    - **[クライアント認証]** - **[認証方法]** を選択します。 次のようなオプションがあります。

      - **[ユーザー名とパスワード]** :接続の認証のためにユーザーにユーザー名とパスワードを要求します。 次の項目も入力します。
        - **[EAP 以外の方法 (内部 ID)]** :接続を認証する方法を選択します。 Wi-Fi ネットワークで構成されているものと同じプロトコルを選択する必要があります。

          次のようなオプションがあります。 **[暗号化されていないパスワード (PAP)]** 、 **[チャレンジ ハンドシェイク認証プロトコル (CHAP)]** 、 **[Microsoft CHAP (MS-CHAP)]** 、 **[Microsoft CHAP Version 2 (MS-CHAP v2)]**

      - **証明書**:やはりデバイスに展開される SCEP または PKCS クライアント証明書プロファイルを選択します。 この証明書は、接続の認証のためにデバイスによってサーバーに提示される ID です。

      - **[ID プライバシー (外部 ID)]** :EAP ID 要求への応答で送信されるテキストを入力します。 このテキストには任意の値を指定できます (例: `anonymous`)。 認証時に、この匿名の ID が最初に送信され、その後、セキュリティで保護されたトンネルで実際の ID が送信されます。

  - **LEAP**

  - **[PEAP]** :次の項目も入力します。

    - **[サーバー信頼]**  -  **[証明書サーバー名]** :信頼された証明機関 (CA) によって発行された証明書で使用される 1 つ以上の共通名を**追加**します。 この情報を入力すると、この Wi-Fi ネットワークに接続するときに、ユーザーのデバイスに表示される動的な信頼ウィンドウをバイパスできます。
    - **[サーバー検証のためのルート証明書]** :既存の信頼されたルート証明書プロファイルを 1 つまたは複数選択します。 クライアントがネットワーク接続されると、これらの証明書がサーバーに提示されます。 証明書によって接続が認証されます。

    - **[クライアント認証]** - **[認証方法]** を選択します。 次のようなオプションがあります。

      - **[ユーザー名とパスワード]** :接続の認証のためにユーザーにユーザー名とパスワードを要求します。 

      - **証明書**:やはりデバイスに展開される SCEP または PKCS クライアント証明書プロファイルを選択します。 この証明書は、接続の認証のためにデバイスによってサーバーに提示される ID です。

      - **[ID プライバシー (外部 ID)]** :EAP ID 要求への応答で送信されるテキストを入力します。 このテキストには任意の値を指定できます (例: `anonymous`)。 認証時に、この匿名の ID が最初に送信され、その後、セキュリティで保護されたトンネルで実際の ID が送信されます。

- **[プロキシ設定]** :次のようなオプションがあります。
  - **なし**: プロキシ設定は構成されません。
  - **手動**:IP アドレスとしての **[プロキシ サーバーのアドレス]** およびその **[ポート番号]** を入力します。
  - **自動**:ファイルを使用してプロキシ サーバーを構成します。 構成ファイルが格納されている **[プロキシ サーバーの URL]** を入力します (例: `http://proxy.contoso.com`)。

## <a name="next-steps"></a>次のステップ

プロファイルは作成されますが、何も実行されません。 次に、[このプロファイルを割り当て](device-profile-assign.md)、[その状態を監視](device-profile-monitor.md)します。

[Android](wi-fi-settings-android.md)、[Android エンタープライズ](wi-fi-settings-android-enterprise.md)、[iOS/iPadOS](wi-fi-settings-ios.md)、および [Windows 10](wi-fi-settings-windows.md) デバイスに対して Wi-Fi 設定を構成します。
