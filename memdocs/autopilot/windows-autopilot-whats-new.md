---
title: Windows 自動操縦の新機能
ms.reviewer: ''
manager: laurawi
description: 最新の更新プログラムと過去のバージョンの Windows 自動操縦に関するニュースとリソースをご覧ください。
keywords: MDM, セットアップ, Windows, Windows 10, OOBE, 管理, 展開, Autopilot, ZTD, ゼロタッチ, パートナー, MSFB, Intune
ms.technology: windows
ms.prod: w10
ms.mktglfcycl: deploy
ms.localizationpriority: medium
ms.sitesec: library
ms.pagetype: deploy
audience: itpro
author: greg-lindsay
ms.author: greglin
ms.collection: M365-modern-desktop
ms.topic: article
ms.openlocfilehash: 736e01696cf503b63762c32a9acee3cb68c1e241
ms.sourcegitcommit: 8fc1704ed0e1141f46662bdd32b52bec00fb93b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89606555"
---
# <a name="windows-autopilot-whats-new"></a>Windows 自動操縦: 新機能

**適用対象**

- Windows 10

## <a name="windows-autopilot-update-history"></a>Windows 自動操縦の更新履歴

次の [Windows 自動操縦用更新プログラム](autopilot-update.md) が利用可能です。 **注**: 更新プログラムは、Windows 自動操縦の展開プロセス中に自動的にダウンロードされて適用されます。 

まだ利用できる更新プログラムはありません。 後で詳しい情報が必要になった場合は、ここに戻って確認してください。

## <a name="new-in-windows-10-version-2004"></a>Windows 10 バージョン2004の新版

このリリースでは、Windows 自動操縦の [ユーザー主導型](user-driven.md) ハイブリッド Azure Active Directory VPN サポートで参加するように構成できます。 このサポートは、Windows 10 バージョン1909および1903にも移植されています。

自動操縦プロファイルで言語設定を構成し、デバイスがイーサネットに接続されている場合、すべてのシナリオで言語、ロケール、およびキーボードページがスキップされるようになります。 以前のバージョンでは、これは自己展開プロファイルでのみサポートされていました。

## <a name="new-in-windows-10-version-1903"></a>Windows 10 バージョン1903の新版

[ホワイトグローブ展開の Windows 自動操縦](white-glove.md) は、windows 10 バージョン1903の新バージョンです。 次のビデオを参照してください。

<br>

> [!VIDEO https://www.youtube.com/embed/nE5XSOBV0rI]

また、このバージョンの Windows で新しく追加されたものは次のとおりです。
- Intune の登録ステータスページ (ESP) で、Intune の管理拡張機能が追跡されるようになりました。
- [OOBE 中の Cortana voiceover と音声認識](windows-autopilot-scenarios.md#cortana-voiceover-and-speech-recognition-during-oobe) は、すべての Windows 10 Pro 教育および Enterprise sku では既定で無効になっています。
- [Windows 自動操縦は、OOBE 中に自己更新され](windows-autopilot-scenarios.md#windows-autopilot-is-self-updating-during-oobe)ます。 Windows 10 以降では、バージョン1903の自動操縦機能と重要な更新プログラムは、OOBE 中に自動的にダウンロードが開始されます。
- Windows 自動操縦は、Windows 10 バージョン1903以降を実行しているデバイス上の OOBE 中に、診断データレベルを Full に設定します。 

## <a name="new-in-windows-10-version-1809"></a>Windows 10 バージョン1809の新版

Windows 自動操縦 [用の自己展開モード](self-deploying.md) は、ゼロタッチデバイスのプロビジョニングプロセスです。 デバイスの電源を入れ、イーサネットに接続するだけで、自動操縦によってデバイスが自動的に構成されます。 エンドユーザーは、展開プロセス中に [次へ] ボタンを押す必要はありません。 

Windows 自動操縦の自己デプロイモードを使用して、AAD テナントにデバイスを登録し、組織の MDM プロバイダーに登録して、ポリシーとアプリケーションをプロビジョニングすることができます。 ユーザー認証またはユーザー操作は必要ありません。

>[!NOTE]
>Windows 10 version 1809 の TPM デバイス構成証明の問題により、自己展開モードを使用するには、Windows 10 バージョン1903以降が必要です。

## <a name="related-topics"></a>関連トピック

[Microsoft Intune の新機能](/intune/whats-new)<br>
[Windows 10 の新機能](/windows/whats-new/)
