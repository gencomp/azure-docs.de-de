---
title: Einrichten einer Verbindung mit einem Cosmos DB-Konto mithilfe einer verwalteten Identität (Vorschauversion)
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie mithilfe einer verwalteten Identität eine Indexerverbindung mit einem Cosmos DB-Konto einrichten (Vorschauversion).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 107cd113645a2cbd4b452f9350fa67d734ee6df8
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86143652"
---
# <a name="set-up-an-indexer-connection-to-a-cosmos-db-database-using-a-managed-identity-preview"></a>Einrichten einer Indexerverbindung mit einer Cosmos DB-Datenbank mithilfe einer verwalteten Identität (Vorschauversion)

> [!IMPORTANT] 
> Das Einrichten einer Verbindung mit einer Datenquelle mithilfe einer verwalteten Identität wird derzeit in einer geschlossenen öffentlichen Vorschauversion unterstützt. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/mi-preview-request) aus, wenn Sie Zugriff auf die Vorschauversion anfordern möchten.

Auf dieser Seite wird beschrieben, wie Sie eine Indexerverbindung mit einer Cosmos DB-Datenbank mithilfe einer verwalteten Identität einrichten, anstatt Anmeldeinformationen in der Verbindungszeichenfolge des Datenquellenobjekts anzugeben.

Bevor Sie mehr über diese Funktion erfahren, sollten Sie wissen, was ein Indexer ist, und wie Sie einen Indexer für Ihre Datenquelle einrichten. Weitere Informationen finden Sie unter den folgenden Links:
* [Indexer (Übersicht)](search-indexer-overview.md)
* [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md)

## <a name="set-up-a-connection-using-a-managed-identity"></a>Einrichten einer Verbindung mithilfe einer verwalteten Identität

### <a name="1---turn-on-system-assigned-managed-identity"></a>1\. Aktivieren einer systemseitig zugewiesenen verwalteten Identität

Wenn eine systemseitig zugewiesene verwaltete Identität aktiviert ist, wird in Azure eine Identität für den Suchdienst erstellt, die für die Authentifizierung bei anderen Azure-Diensten innerhalb desselben Mandanten und desselben Abonnements verwendet werden kann. Anschließend können Sie diese Identität in RBAC-Zuweisungen (rollenbasierte Zugriffssteuerung) verwenden, die den Zugriff auf Daten während der Indizierung ermöglichen.

![Aktivieren einer systemseitig zugewiesenen verwalteten Identität](./media/search-managed-identities/turn-on-system-assigned-identity.png "Aktivieren einer systemseitig zugewiesenen verwalteten Identität")

Nach dem Auswählen von **Speichern** wird eine Objekt-ID angezeigt, die dem Suchdienst zugewiesen wurde.

![Objekt-ID](./media/search-managed-identities/system-assigned-identity-object-id.png "ObjectID")
 
### <a name="2---add-a-role-assignment"></a>2\. Hinzufügen einer Rollenzuweisung

In diesem Schritt erteilen Sie dem Azure Cognitive Search-Dienst die Berechtigung, Daten aus Ihrer Cosmos DB-Datenbank zu lesen.

1. Navigieren Sie im Azure-Portal zu dem Cosmos DB-Konto, in dem die Daten gespeichert werden, die indiziert werden sollen.
2. Wählen Sie **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie **Hinzufügen** und dann **Rollenzuweisung hinzufügen** aus.

    ![Hinzufügen der Rollenzuweisung](./media/search-managed-identities/add-role-assignment-cosmos-db.png "Rollenzuweisung hinzufügen")

4. Wählen Sie die Cosmos DB-Rolle **Kontoleser** aus.
5. Behalten Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** bei.
6. Suchen Sie nach Ihrem Suchdienst, wählen Sie ihn aus, und wählen Sie dann **Speichern** aus.

    ![Hinzufügen der Rollenzuweisung „Lese- und Datenzugriff“](./media/search-managed-identities/add-role-assignment-cosmos-db-account-reader-role.png "Hinzufügen der Rollenzuweisung „Lese- und Datenzugriff“")

### <a name="3---create-the-data-source"></a>3\. Erstellen der Datenquelle

Eine **Datenquelle** gibt die zu indizierenden Daten, Anmeldeinformationen und Richtlinien für das Bestimmen von Änderungen in den Daten an (z.B. geänderte oder gelöschte Dokumente in Ihrer Sammlung). Die Datenquelle wird als unabhängige Ressource definiert, sodass sie von mehreren Indexern verwendet werden kann.

Wenn Sie verwaltete Identitäten für die Authentifizierung bei der Datenquelle verwenden, enthalten die **Anmeldeinformationen** keinen Kontoschlüssel.

Beispiel zum Erstellen eines Cosmos DB-Datenquellenobjekts mithilfe der [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-data-source):

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [Search service admin key]

{
    "name": "cosmos-db-datasource",
    "type": "cosmosdb",
    "credentials": {
        "connectionString": "Database=sql-test-db;ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/cosmos-db-resource-group/providers/Microsoft.DocumentDB/databaseAccounts/my-cosmos-db-account/;"
    },
    "container": { "name": "myCollection", "query": null },
    "dataChangeDetectionPolicy": {
        "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName": "_ts"
    }
}
```    

Der Anforderungstext umfasst die Datenquellendefinition, welche die folgenden Felder enthalten sollte:

| Feld   | BESCHREIBUNG |
|---------|-------------|
| **name** | Erforderlich. Wählen Sie für Ihr Datenquellenobjekt einen beliebigen Namen aus. |
|**type**| Erforderlich. Muss `cosmosdb`lauten. |
|**credentials** | Erforderlich. <br/><br/>Beim Herstellen einer Verbindung mit einer verwalteten Identität sollten die **Anmeldeinformationen** das folgende Format aufweisen: *Database=[database-name];ResourceId=[resource-id-string];(ApiKind=[api-kind];)*<br/> <br/>Format von ResourceId: *ResourceId=/subscriptions/**Abonnement-ID**/resourceGroups/**Name der Ressourcengruppe**/providers/Microsoft.DocumentDB/databaseAccounts/**Name des Cosmos DB-Kontos**/;*<br/><br/>Für SQL-Sammlungen ist für die Verbindungszeichenfolge keine ApiKind erforderlich.<br/><br/>Bei MongoDB-Sammlungen fügen Sie der Verbindungszeichenfolge **ApiKind=MongoDb** hinzu. <br/><br/>Registrieren Sie sich bei Verwendung von Gremlin-Graphen und Cassandra-Tabellen für die [geschlossene Indexervorschau](https://aka.ms/azure-cognitive-search/indexer-preview), um Zugriff auf die Vorschauversion sowie Informationen zur Formatierung der Anmeldeinformationen zu erhalten.<br/>|
| **container** | Enthält die folgenden Elemente: <br/>**name:** Erforderlich. Geben Sie die ID der zu indizierenden Datenbanksammlung an.<br/>**Abfrage**: Optional. Sie können eine Abfrage angeben, um ein beliebiges JSON-Dokument in einem Flatfile-Schema zu vereinfachen, das in der kognitiven Azure-Suche indiziert werden kann.<br/>Für die MongoDB-API, die Gremlin-API und die Cassandra-API werden keine Abfragen unterstützt. |
| **dataChangeDetectionPolicy** | Empfohlen |
|**dataDeletionDetectionPolicy** | Optional |

Die Verbindungszeichenfolge für verwaltete Identitäten wird auch über das Azure-Portal und das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) unterstützt. Im Azure-Portal ist ein Featureflag erforderlich, das Sie bei der Registrierung für die Vorschauversion über den Link oben auf dieser Seite erhalten. 

### <a name="4---create-the-index"></a>4\. Erstellen des Index

Mit dem Index werden die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben.

So erstellen Sie einen Index mit einem durchsuchbaren `booktitle`-Feld

```
POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "my-target-index",
    "fields": [
    { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    { "name": "booktitle", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
    ]
}
```

Weitere Informationen zum Erstellen von Indizes finden Sie unter [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index erstellen).

### <a name="5---create-the-indexer"></a>5\. Erstellen des Indexers

Ein Indexer verbindet eine Datenquelle mit einem Zielsuchindex und stellt einen Zeitplan zur Automatisierung der Datenaktualisierung bereit.

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen.

Beispiel für die Indexerdefinition:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "cosmos-db-indexer",
      "dataSourceName" : "cosmos-db-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall wird auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall beträgt fünf Minuten. Der Zeitplan ist optional. Ohne Zeitplan wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Weitere Informationen zum Definieren von Indexerzeitplänen finden Sie unter [Festlegen eines Zeitplans für Indexer in der kognitiven Azure-Suche](search-howto-schedule-indexers.md).

## <a name="see-also"></a>Weitere Informationen

Weitere Informationen zu Cosmos DB-Indexern:
* [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md)
