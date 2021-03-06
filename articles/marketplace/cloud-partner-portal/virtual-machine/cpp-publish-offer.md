---
title: Azure Marketplace で仮想マシンのオファーを発行する | Microsoft Docs
description: 仮想マシンの既存のオファーを Azure Marketplace に発行するために必要な手順を示します。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 3cf6a3d9bcb9470fd3a6bd4fef964c1966adfa1a
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639099"
---
# <a name="publish-a-virtual-machine-offer"></a>仮想マシンのオファーを発行する

 ポータルでオファーを定義し、関連する技術資産を作成した後の最後のステップでは、オファーを発行するために送信します。 次の図では、"稼働" させるための発行プロセスの主な手順を示します。

![仮想マシンのオファーの発行手順](./media/publishvm_013.png)

次の表では、これらのステップについて説明し、完了するのに要する最大推定時間を示します。 <!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **発行ステップ**           | **Time**    | **説明**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| 前提条件の検証         | 15 分   | オファーの情報とオファーの設定の妥当性を確認します。                        |
| 体験版の検証 (省略可能) | 2 時間 | 体験版を有効にすることを選択した場合、Microsoft は、体験版の構成、その展開、および選択されたリージョンでのレプリケーションを検証します。 |
| 認定                  | 3 日 | オファーが Azure 認定チームによって分析されます。 この手順では、ウイルス、マルウェア、安全性のコンプライアンス、およびセキュリティ問題のスキャンを実行します。 問題が見つかった場合、フィードバックが提供されます。 |
| プロビジョニング                   | 4 日   | VM オファーがマーケットプレースの運用システムにレプリケートされます。               |
| パッケージ化とリード生成の登録 | 1 時間以内  | オファーの技術資産が顧客の使用のためにパッケージ化され、リード システムが構成されて設定されます。 |
|  発行元のサインオフ             |  -        | オファーが稼働状態になる前に、最終的な発行元のレビューと確認が行われます。 (オファー情報の手順で) 選択されたサブスクリプション内にオファーをデプロイして、すべての要件を満たしていることを確認できます。  |
| プロビジョニング                   | 4 日 | マーケットプレースの運用システムとリージョンに、完成した VM オファーがレプリケートされます。 | 
| ライブ                           | 4 日 | VM オファーが、必要なリージョンにリリース、レプリケートされて、一般公開されます。 |
|  |  |

このプロセスが完了するには、最大で 16 日にかかります。  発行手順が済むと、VM オファーは [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) にリストされます。 

