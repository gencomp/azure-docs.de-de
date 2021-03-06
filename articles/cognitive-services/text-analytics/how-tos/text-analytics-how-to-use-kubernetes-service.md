---
title: Ausführen von Azure Kubernetes Service – Textanalyse
titleSuffix: Azure Cognitive Services
description: Stellen Sie das Textanalysecontainerimage beim Azure Kubernetes Service bereit, und testen Sie sie in einem Webbrowser.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 155f32ee76f69fe0f16e7698123381fdc12efd0e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877823"
---
# <a name="deploy-a-text-analytics-container-to-azure-kubernetes-service"></a>Bereitstellen eines Textanalysecontainers in Azure Kubernetes Service

Erfahren Sie, wie Sie das [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)-Containerimage von Azure Cognitive Services für Azure Kubernetes Services (AKS) bereitstellen. Dieses Verfahren zeigt das Erstellen einer Textanalyseressource, das Erstellen eines zugehörigen Standpunktanalysebilds und die Orchestrierung der beiden über einen Browser. Die Verwendung von Containern kann Ihre Aufmerksamkeit von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Verfahren müssen mehrere Tools lokal installiert und ausgeführt werden. Verwenden Sie nicht Azure Cloud Shell. Sie benötigen Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Einen Text-Editor, z.B. [Visual Studio Code](https://code.visualstudio.com/download).
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) muss installiert sein.
* Die [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) muss installiert sein.
* Eine Azure-Ressource mit dem korrekten Tarif. Nicht alle Tarife können mit diesem Container verwendet werden:
    * Eine **Azure-Textanalyse**-Ressource nur in den Tarifen „F0“ oder „Standard“.
    * Eine **Azure Cognitive Services**-Ressource im S0-Tarif.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

#### <a name="key-phrase-extraction"></a>[Schlüsselbegriffserkennung](#tab/keyphrase)

[!INCLUDE [Key Phrase Extraction Kubernetes config and deploy steps](../includes/key-phrase-extraction-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Sprachenerkennung](#tab/language)

[!INCLUDE [Language Detection Kubernetes config and deploy steps](../includes/language-detection-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Standpunktanalyse](#tab/sentiment)

[!INCLUDE [Sentiment Analysis Kubernetes config and deploy steps](../includes/sentiment-analysis-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Nächste Schritte

* Verwenden weiterer [Cognitive Services-Container](../../cognitive-services-container-support.md)
* Verwenden des [Text Analytics Connected Service](../vs-text-connected-service.md)
