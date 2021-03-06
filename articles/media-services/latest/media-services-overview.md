---
title: Azure Media Services v3 の概要 | Microsoft Docs
description: この記事では、Media Services の概要を説明し、詳細についての記事へのリンクを示します。
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: Azure Media Services, ストリーム, ブロードキャスト, ライブ, オフライン
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/14/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 018392db2ffb510d41385d8e0af19635c35678e6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58003407"
---
# <a name="what-is-azure-media-services-v3"></a>Azure Media Services v3 の概要

クラウドベースのプラットフォームである Azure Media Services では、ブロードキャスト品質のビデオ ストリーミングを実現し、アクセス性と配信を強化し、コンテンツを分析するソリューションを構築できます。 アプリケーション開発者、コール センター、政府機関、エンターテイメント会社のいずれであっても、Media Services を利用すると、今日の最も一般的なモバイル デバイスとブラウザーの多くのユーザーに、優れた品質のメディア エクスペリエンスを提供するアプリケーションを作成できます。 

## <a name="what-can-i-do-with-media-services"></a>Media Services の機能

Media Services を使うと、クラウドでさまざまなメディア ワークフローを構築できます。Media Services で実現できることの例を次に示します。  

* 広範なブラウザーやデバイスで再生できるように、さまざまな形式のビデオを提供します。 さまざまなクライアント (モバイル デバイス、TV、PC など) にオンデマンドとライブ ストリーミングの両方を提供するには、ビデオ コンテンツとオーディオ コンテンツを適切にエンコードしてパッケージ化する必要があります。 このようなコンテンツを配信およびストリーミングする方法については、[ファイルのエンコードとストリームに関するクイック スタート](stream-files-dotnet-quickstart.md)を参照してください。
* サッカー、野球、大学や高校のスポーツなど、多数のオンライン視聴者にライブ スポーツ イベントをストリーム配信します。 
* タウンホール ミーティング、市議会、立法機関などの公開の会合やイベントをブロードキャストします。
* 録画されたビデオやオーディオ コンテンツを分析します。 たとえば、より高い顧客満足度を実現するため、音声からテキストを抽出して、検索インデックスやダッシュボードを作成できます。 これにより、一般的な苦情、苦情の原因、その他の関連データに関する知見を引き出すことができます。
* 顧客 (たとえば映画スタジオ) が独自の著作権所有作品のアクセスや使用を制限する必要がある場合は、サブスクリプション ビデオ サービスを作成して、DRM で保護されたコンテンツをストリーム配信します。
* 飛行機、列車、自動車で再生するためのオフライン コンテンツを提供します。 顧客は、ネットワークから切断される可能性があるときは、携帯電話やタブレットにコンテンツをダウンロードして再生する必要があります。
* 音声からのテキスト キャプション作成や、多言語への翻訳などのために、Azure Media Services と [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) で教育用 E ラーニング ビデオ プラットフォームを実装します。 
* Azure Media Services を [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) と共に使用して、より広範な視聴者 (たとえば、聴覚障碍を持つ人や、別の言語で読みたい人など) に対応できるよう、ビデオに字幕とキャプションを追加します。
* 瞬間的高負荷 (製品発表イベントの開始時など) を処理しやすくする大規模なスケーリングを Azure CDN が実現できるようにします。 

## <a name="v3-capabilities"></a>v3 の機能

v3 は、Azure Resource Manager 上に構築された管理と操作の両方の機能を公開する統一された API サーフェスに基づいています。 

このバージョンでは次の機能が提供されます。  

* メディア処理タスクまたは分析タスクの簡単なワークフローを定義するのに役立つ**変換**。 変換は、ビデオ ファイルとオーディオ ファイルを処理するためのレシピです。 変換にジョブを送信することで、変換を繰り返し適用して、コンテンツ ライブラリ内のすべてのファイルを処理できます。
* ビデオを処理 (エンコードまたは分析) するための**ジョブ**。 HTTPS URL、SAS URL、または Azure Blob Storage 内に存在するファイルへのパスを使って、ジョブで入力コンテンツを指定できます。 現時点では、AMS v3 には、HTTPS URL 経由でチャンク転送エンコード処理がサポートされていません。
* ジョブの進行状況や状態、またはライブ イベントの開始/停止とエラー イベントを監視する**通知**。 通知は、Azure Event Grid の通知システムに統合されています。 Azure Media Services の複数のリソースでのイベントを簡単にサブスクライブすることができます。 
* **Azure Resource Management** テンプレートを使って、変換、ストリーミング エンドポイント、ライブ イベントなどを作成して展開できます。
* **ロールベースのアクセス制御**をリソース レベルで設定でき、変換やライブ イベントなどの特定のリソースへのアクセスをロックダウンできます。
* 複数の言語 (.NET、.NET Core、Python、Go、Java、Node.js) での**クライアント SDK**。

## <a name="naming-conventions"></a>名前付け規則

Azure Media Services v3 のリソース名 (アセット、ジョブ、変換など) には、Azure Resource Manager の名前付け規則が適用されます。 Azure Resource Manager に従って、リソース名は常に一意となります。 そのため、リソース名には一意識別子の文字列 (GUID など) を使用できます。 

Media Services リソース名には、"<",">"、"%"、"&"、":"、"&#92;"、"?"、"/"、"*"、"+"、"."、一重引用符などの制御文字を使用することができません。 それ以外の文字は使用できます。 リソース名の最大文字数は 260 文字です。 

Azure Resource Manager の名前付けの詳細については、[名前付けの要件](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource)と[名前付け規則](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)に関するページを参照してください。

## <a name="v3-api-design-principles"></a>v3 API の設計原則

v3 API の主要な設計原則の 1 つは、API の安全性の向上です。 v3 API は、**Get** または **List** 操作でシークレットまたは資格情報を返しません。 キーは常に、null または空であるか、応答から削除されます。 シークレットまたは資格情報を取得するには、別のアクション メソッドを呼び出す必要があります。 別のアクションを使用すれば、シークレットが取得/表示される API もあればそうでない API もある場合に、異なる RBAC セキュリティ アクセス許可を設定できます。 RBAC を使用してアクセスを管理する方法の詳細については、[RBAC を使用したアクセスの管理](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)に関するページを参照してください。

この例には以下のようなものがあります 

* StreamingLocator の Get で ContentKey の値が返されない。 
* ContentKeyPolicy の Get で制限キーが返されない。 
* ジョブの HTTP 入力 URL の (署名を削除する) URL に含まれているクエリ文字列部分が返されない。

[.NET を使用してコンテンツ キー ポリシーを取得する方法](get-content-key-policy-dotnet-howto.md)の例を参照してください。

## <a name="how-can-i-get-started-with-v3"></a>v3 の利用を始める方法

各種のツールと SDK を使用して Media Services v3 API での開発を始める方法については、[開発の開始](developers-guide.md)に関する記事をご覧ください。

## <a name="v3-content-map"></a>v3 のコンテンツ マップ

Media Services v3 のコンテンツは、次の構造に従って編成されています (目次にも反映されています)。

|セクション| 説明|
|---|---|
| 概要 | Media Services の機能と、サービスでできることについて説明します。|
| クイック スタート | 新しいお客様が Media Services を簡単に試すことができるよう、基礎について 1 日で説明します。|
| チュートリアル | Media Services でよく行われるタスクについて、シナリオ ベースで手順を示します。|
| サンプル | コード サンプルへのリンクを示します。 |
| 概念 | [Media Services v3 の概念と機能](concepts-overview.md)についての詳しい説明が含まれます。 開発を始める前に、これらのトピックで説明されている基本的な概念を確認する必要があります。<br/><br/>* クラウドのアップロードとストレージ<br/>* エンコード<br/>* メディア分析<br/>* パッケージ化、デリバリー、保護<br/>* ライブ ストリーミング<br/>* 監視<br/>* Player クライアント<br/><br/>その他にも用途はあります。 |
| ハウツー ガイド | タスクを完了する方法を示します。|

## <a name="next-steps"></a>次の手順

ビデオ ファイルのエンコードおよびストリーミングを簡単に始める方法については、[ファイルのストリーム配信](stream-files-dotnet-quickstart.md)に関するページをご覧ください。 

