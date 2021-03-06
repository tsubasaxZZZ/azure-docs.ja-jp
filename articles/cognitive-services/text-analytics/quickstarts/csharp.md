---
title: クイック スタート:C# を使用して Text Analytics API を呼び出す
titleSuffix: Azure Cognitive Services
description: Text Analytics API をすぐに使い始めるのに役立つ情報とコード サンプルを提供します。
services: cognitive-services
author: ashmaka
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: assafi
ms.openlocfilehash: bc4553df239dbb8b62a31414539b10998cd74f02
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189650"
---
# <a name="quickstart-using-c-to-call-the-text-analytics-cognitive-service"></a>クイック スタート:C# を使用して Text Analytics Cognitive Service を呼び出す
<a name="HOLTop"></a>

このチュートリアルでは、 [Text Analytics API シリーズ](//go.microsoft.com/fwlink/?LinkID=759711) を C# で使用して、言語の検出、センチメントの分析、およびキー フレーズの抽出を行う方法について説明します。 このコードは、.NET Core アプリケーションで動作するように作成されており、外部ライブラリへの参照は最小限なので、Linux または MacOS で実行することもできます。

API の技術ドキュメントについては、[API の定義](//go.microsoft.com/fwlink/?LinkID=759346)に関するページを参照してください。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

また、サインアップ中に生成された[エンドポイントとアクセス キー](../How-tos/text-analytics-how-to-access-key.md)も必要です。

## <a name="install-the-nuget-sdk-package"></a>NuGet SDK パッケージのインストール
1. Visual Studio で新しいコンソール ソリューションを作成します。
1. ソリューションを右クリックし、**[ソリューションの NuGet パッケージの管理]** をクリックします
1. **[プレリリースを含める]** チェック ボックスをオンにします。
1. **[参照]** タブを選択し、**Microsoft.Azure.CognitiveServices.Language.TextAnalytics** を検索します。
1. NuGet パッケージを選択して、インストールします。 当面 (2019 年 3 月 18 日現在) は、ソフトウェアのバグが修正されるまで v3.0.0 ではなく v2.8.0 が必要となります。

> [!Tip]
>  [HTTP エンドポイント](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)は C# から直接呼び出すことができますが、Microsoft.Azure.CognitiveServices.Language SDK を使用すると、JSON のシリアル化とシリアル化解除について心配する必要がなく、サービスがかなり呼び出しやすくなります。
>
> 便利なリンク:
> - [SDK Nuget ページ](<https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics>)
> - [SDK コード](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)

## <a name="call-the-text-analytics-api-using-the-sdk"></a>SDK を使用した Text Analytics API の呼び出し

1. Program.cs を以下のコードで置き換えます。 このプログラムは、3 つのセクション (言語の抽出、キー フレーズの抽出、感情分析) で Text Analytics API の機能を示しています。
1. `Ocp-Apim-Subscription-Key` ヘッダー値を、お使いのサブスクリプションで有効なアクセス キーで置き換えます。
1. `Endpoint` のリージョンは、実際の値に置き換えてください。 [Azure portal](<https://ms.portal.azure.com>) から Text Analytics リソースの概要セクションで、該当するエンドポイントを確認できます。 エンドポイントの中の "https://[region].api.cognitive.microsoft.com" の部分だけを含めるようにしてください。
1. プログラムを実行します。

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        private const string SubscriptionKey = ""; //Insert your Text Anaytics subscription key

        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            }; //Replace 'westus' with the correct region for your Text Analytics subscription

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result = client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
            }

            // Getting key-phrases
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing keyphrases
            foreach (var document in result2.Documents)
            {
                Console.WriteLine($"Document ID: {document.Id} ");

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine($"\t\t{keyphrase}");
                }
            }

            // Extracting sentiment
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results
            foreach (var document in result3.Documents)
            {
                Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
            }


            // Identify entities
            Console.WriteLine("\n\n===== ENTITIES ======");

            EntitiesBatchResultV2dot1 result4 = client.EntitiesAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%.")
                        })).Result;

            // Printing entities results
            foreach (var document in result4.Documents)
            {
                Console.WriteLine($"Document ID: {document.Id} ");

                Console.WriteLine("\t Entities:");

                foreach (EntityRecordV2dot1 entity in document.Entities)
                {
                    Console.WriteLine($"\t\t{entity.Name}\t\t{entity.WikipediaUrl}\t\t{entity.Type}\t\t{entity.SubType}");
                }
            }

            Console.ReadLine();
        }
    }
}
```

## <a name="application-output"></a>アプリケーションの出力

このアプリケーションには次の情報が表示されます。

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified


===== KEY-PHRASE EXTRACTION ======
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol

===== SENTIMENT ANALYSIS ======
Document ID: 0 , Sentiment Score: 0.87
Document ID: 1 , Sentiment Score: 0.11
Document ID: 2 , Sentiment Score: 0.44
Document ID: 3 , Sentiment Score: 1.00
```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Text Analytics と Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>関連項目

 [Text Analytics の概要](../overview.md) [よく寄せられる質問 (FAQ)](../text-analytics-resource-faq.md)

