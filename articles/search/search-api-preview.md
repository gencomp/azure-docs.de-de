---
title: Liste der Previewfunktionen
titleSuffix: Azure Cognitive Search
description: Previewfunktionen werden veröffentlicht, damit Kunden Feedback zu ihrer Auslegung und Nutzbarkeit geben können. Dieser Artikel stellt eine umfassende Liste aller Funktionen dar, die sich derzeit in der Vorschau befinden.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: b952d6b6fec9a1ec0dcd8af1a9566e67d3301d77
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496705"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Previewfunktionen in Azure Cognitive Search

Dieser Artikel stellt eine umfassende Liste aller Funktionen dar, die sich in der öffentlichen Vorschau befinden. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Previewfunktionen, die in die allgemeine Verfügbarkeit übergehen, werden aus dieser Liste entfernt. Wenn eine Funktion im Folgenden nicht aufgeführt ist, können Sie davon ausgehen, dass sie allgemein verfügbar ist. Ankündigungen zur allgemeinen Verfügbarkeit finden Sie in den [Dienstupdates](https://azure.microsoft.com/updates/?product=search) sowie unter [Neuigkeiten](whats-new.md).

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
| [**AML-Skill (Azure Machine Learning)** ](cognitive-search-aml-skill.md) | KI-Anreicherung| Ein neuer Skilltyp zum Integrieren eines Rückschlussendpunkts aus Azure Machine Learning. Erste Schritte mit [diesem Tutorial](cognitive-search-tutorial-aml-custom-skill.md). | Verwenden Sie [Search-REST-API 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/) oder 2019-05-06-Preview. Ebenfalls im Portal im Skillsetentwurf verfügbar. Dabei wird davon ausgegangen, das die Dienste Cognitive Search und Azure ML im gleichen Abonnement bereitgestellt wurden. |
| [**featuresMode-Parameter**](https://docs.microsoft.com/rest/api/searchservice/search-documents#featuresmode) | Relevanz (Bewertung) | Erweiterung der Relevanzbewertung um Details: Ähnlichkeitsbewertung pro Feld, Ausdruckshäufigkeit pro Feld und Anzahl der zugeordneten eindeutigen Token pro Feld. Sie können diese Datenpunkte in [benutzerdefinierten Bewertungslösungen](https://github.com/Azure-Samples/search-ranking-tutorial) verwenden. | Fügen Sie diesen Abfrageparameter mithilfe von [Dokumente durchsuchen (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) in api-version=2020-06-30-Preview oder 2019-05-06-Preview hinzu. |
| [**Verwaltete Dienstidentität**](search-howto-managed-identities-data-sources.md) | Indexer, Sicherheit| Registrieren Sie einen Suchdienst bei Azure Active Directory, um ihn zu einem vertrauenswürdigen Dienst zu machen, und verwenden Sie dann RBAC-Berechtigungen für Azure-Datenquellen, um den schreibgeschützten Zugriff durch einen Indexer zuzulassen. | Greifen Sie auf diese Funktion beim Verwenden des Portals oder mit [Datenquelle erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview zu. |
| [**Debugsitzungen**](cognitive-search-debug-session.md) | Portal, KI-Anreicherung (Skillset) | Ein in die Sitzung integrierter Skillset-Editor, der zum Untersuchen und Beheben von Problemen bei einem Skillset verwendet wird. Während einer Debugsitzung angewendete Korrekturen können in einem Skillset im Dienst gespeichert werden. | Nur im Portal mithilfe von Links zur Seitenmitte zum Öffnen einer Debugsitzung. |
| [**Natives vorläufiges Löschen von Blobs**](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Indexer, Azure-Blobs| Der Azure Blob Storage-Indexer in Azure Cognitive Search erkennt Blobs, die sich im vorläufig gelöschten Zustand befinden. Das entsprechende Suchdokument wird während der Indizierung entfernt. | Fügen Sie diese Konfigurationseinstellung mithilfe von [Indexer erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview hinzu. |
| [**Skill für benutzerdefinierte Entitätssuche**](cognitive-search-skill-custom-entity-lookup.md ) | KI-Anreicherung (Skillset) | Eine kognitive Fähigkeit, die nach Text aus einer benutzerdefinierten Liste von Wörtern und Ausdrücken sucht. Mithilfe dieser Liste werden alle Dokumente mit übereinstimmenden Entitäten mit einer Bezeichnung markiert. Die Qualifikation unterstützt auch einen gewissen Grad an Fuzzyübereinstimmung, der für die Suche nach ähnlichen, aber nicht exakten Übereinstimmungen verwendet werden kann. | Verweisen Sie auf diesen Preview-Skill über den Skillset-Editor im Portal oder mithilfe von [Skillset erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview. |
| [**Skill für PII-Erkennung**](cognitive-search-skill-pii-detection.md) | KI-Anreicherung (Skillset) | Eine kognitive Fähigkeit, die bei der Indizierung eingesetzt wird. Sie extrahiert personenbezogene Informationen aus einem Eingabetext und bietet verschiedene Maskierungsmöglichkeiten für diese Informationen. | Verweisen Sie auf diesen Preview-Skill über den Skillset-Editor im Portal oder mithilfe von [Skillset erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview. |
| [**Inkrementelle Anreicherung**](cognitive-search-incremental-indexing-conceptual.md) | Indexerkonfiguration| Fügt einer Anreicherungspipeline die Zwischenspeicherung hinzu, sodass Sie vorhandene Ausgaben wiederverwenden können, wenn die Inhalte durch eine Zieländerung, z. B. eine Aktualisierung am Skillset oder einem anderen Objekt, nicht geändert werden. Die Zwischenspeicherung betrifft nur angereicherte Dokumente, die über ein Skillset erstellt werden.| Fügen Sie diese Konfigurationseinstellung mithilfe von [Indexer erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview hinzu. |
| [**Cosmos DB-Indexer: MongoDB API, Gremlin API, Cassandra API**](search-howto-index-cosmosdb.md) | Indexerdatenquelle | Für Cosmos DB ist die SQL-API allgemein verfügbar, aber die MongoDB-, Gremlin- und Cassandra-APIs befinden sich in der Vorschauphase. | Nur für die Gremlin- und die Cassandra-API müssen Sie sich [zuerst registrieren](https://aka.ms/azure-cognitive-search/indexer-preview), damit Unterstützung für Ihr Abonnement im Back-End aktiviert werden kann. MongoDB-Datenquellen können im Portal konfiguriert werden. Darüber hinaus wird die Datenquellenkonfiguration für alle drei APIs mithilfe von [Datenquelle erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview unterstützt. |
|  [**Azure Data Lake Storage Gen2-Indexer**](search-howto-index-azure-data-lake-storage.md) | Indexerdatenquelle | Indizieren von Inhalten und Metadaten aus Data Lake Storage Gen2.| [Registrierung](https://aka.ms/azure-cognitive-search/indexer-preview) ist erforderlich, damit Unterstützung für Ihr Abonnement im Back-End aktiviert werden kann. Greifen Sie auf diese Datenquelle mithilfe von [Datenquelle erstellen (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview zu. |
| [**moreLikeThis**](search-more-like-this.md) | Abfrage | Sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. | Fügen Sie diesen Abfrageparameter mithilfe von Aufrufen von [Dokumente durchsuchen (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) in api-version=2020-06-30-Preview, 2019-05-06-Preview, 2016-09-01-Preview, 2017-11-11-Preview hinzu. |

## <a name="calling-preview-rest-apis"></a>Aufrufen von Vorschau-Rest-APIs

Die kognitive Azure-Suche bietet immer zuerst Vorabversionen experimenteller Funktionen über die REST-API, dann über Vorabversionen des .NET SDK.

Vorschaufunktionen stehen für Tests und Experimente zur Verfügung, mit dem Ziel, Feedback zu Entwurf und Implementierung der Funktionen zu sammeln. Aus diesem Grund können sich Vorschaufunktionen mit der Zeit ändern – mitunter in einer Weise, die die Abwärtskompatibilität beeinträchtigt. Dies steht im Gegensatz zu Funktionen in der allgemein verfügbaren Version (GA), die stabil sind und sich bis auf kleinere abwärtskompatible Fehlerbehebungen und Verbesserungen eher nicht ändern. Außerdem werden Vorschaufunktionen nicht immer in der GA-Version übernommen.

Auch wenn einige Previewfunktionen im Portal und im .NET SDK verfügbar sein können, enthält die REST-API immer alle Previewfunktionen.

+ Für Suchvorgänge lautet die aktuelle Vorschauversion [ **`2020-06-30-Preview`** ](https://docs.microsoft.com/rest/api/searchservice/index-preview).

+ Für Verwaltungsvorgänge lautet die aktuelle Vorschauversion [ **`2019-10-01-Preview`** ](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview).

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2019-05-06-Preview`, `api-version=2016-09-01-Preview` oder `api-version=2017-11-11-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert. 

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

```HTTP
GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2020-06-30-Preview
```

Der Dienst für die kognitive Azure-Suche ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Referenzdokumentation für die Search-REST-Vorschau-API an. Wenn Probleme auftreten, können Sie sich über [Stack Overflow](https://stackoverflow.com/) an uns wenden oder [den Support kontaktieren](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referenz zur REST-API für den Azure-Suchdienst (Vorschau)](https://docs.microsoft.com/rest/api/searchservice/index-preview)