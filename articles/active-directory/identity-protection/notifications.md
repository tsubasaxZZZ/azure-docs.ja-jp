---
title: Azure Active Directory Identity Protection の通知 | Microsoft Docs
description: 通知が調査作業にどのように役立つのかを説明します。
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, アプリケーションの管理, セキュリティ, リスク, リスク レベル, 脆弱性, セキュリティ ポリシー
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 482b69752cc889ff99c3d9082d3bc20a7caa6d76
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522181"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection の通知

Azure AD Identity Protection では、ユーザーのリスクとリスク イベントの管理に役立つ 2 種類の自動通知電子メールが送信されます。

- 危険な状態のユーザーが検出された電子メール
- 週間ダイジェスト電子メール

この記事では、両方の通知メールの概要を説明します。


## <a name="users-at-risk-detected-email"></a>危険な状態のユーザーが検出された電子メール

危険な状態のアカウントが検出されると、Azure AD Identity Protection は **[Users at risk detected]\(危険な状態のユーザーが検出されました\)** という件名のアラート メールを生成します。 このメールには、**[リスクのフラグ付きユーザー](../reports-monitoring/concept-user-at-risk.md)** レポートへのリンクが含まれます。 ベスト プラクティスとして、危険な状態のユーザーをすぐに調べる必要があります。

![危険な状態のユーザーが検出された電子メール](./media/notifications/01.png)


### <a name="configuration"></a>構成

管理者は以下を設定することができます。

- **このメールの生成をトリガーするリスク レベル** - 既定では、リスク レベルは "高" リスクに設定されます。
- **このメールの受信者** - 既定では、受信者にはすべてのグローバル管理者が含まれます。 グローバル管理者は、受信者として他のグローバル管理者、セキュリティ管理者、セキュリティ閲覧者を追加することもできます。  


関連するダイアログを開くには、**[Identity Protection]** ページの **[設定]** セクションの **[アラート]** をクリックします。

![危険な状態のユーザーが検出された電子メール](./media/notifications/05.png)


## <a name="weekly-digest-email"></a>週間ダイジェスト電子メール

週間ダイジェスト電子メールには、新しいリスク イベントの概要が含まれます。  
次の情報が含まれます。

- リスクのあるユーザー

- 不審なアクティビティ

- 検出された脆弱性

- Identity Protection の関連するレポートへのリンク

    ![修復](./media/notifications/400.png "Remediation")

### <a name="configuration"></a>構成

管理者は、週間ダイジェスト電子メールの送信を無効にできます。

![ユーザーのリスク](./media/notifications/62.png "User risks")

関連するダイアログを開くには、**[Identity Protection]** ページの **[設定]** セクションの **[週間ダイジェスト]** をクリックします。

![危険な状態のユーザーが検出された電子メール](./media/notifications/04.png)


## <a name="see-also"></a>関連項目

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
