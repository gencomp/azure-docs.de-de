---
title: Zuordnungsdatenflussskript
description: Übersicht über die CodeBehind-Sprache für das Datenflussskript von Data Factory
author: kromerm
ms.author: nimoolen
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/02/2020
ms.openlocfilehash: 27de2d3926a1f03cbd9169216e8f68c8ca81f2a5
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298600"
---
# <a name="data-flow-script-dfs"></a>Datenflussskript (DFS)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Beim Datenflussskript (DFS) handelt es sich um die zugrunde liegenden Metadaten (ähnlich wie eine Programmiersprache), die zur Ausführung der in einem Zuordnungsdatenfluss enthaltenen Transformationen dienen. Jede Transformation wird durch eine Reihe von Eigenschaften dargestellt, die die erforderlichen Informationen zur ordnungsgemäßen Ausführung des Auftrags liefern. Durch Klicken auf die Schaltfläche „Skript“ im oberen Menüband der Browser-Benutzeroberfläche kann das Skript aus ADF heraus angezeigt und bearbeitet werden.

![Schaltfläche „Skript“](media/data-flow/scriptbutton.png "Schaltfläche „Skript“")

So weist beispielsweise `allowSchemaDrift: true,` in einer Quelltransformation den Dienst an, alle Spalten aus dem Quelldataset in den Datenfluss einzubeziehen – sogar, wenn sie in der Schemaprojektion nicht enthalten sind.

## <a name="use-cases"></a>Anwendungsfälle
Das DFS wird von der Benutzeroberfläche automatisch erstellt. Wenn Sie das Skript anzeigen und anpassen möchten, können Sie auf die Schaltfläche „Skript“ klicken. Sie können Skripts auch außerhalb der ADF-Benutzeroberfläche generieren und dann an das PowerShell-Cmdlet übergeben. Beim Debuggen komplexer Datenflüsse finden Sie es vielleicht einfacher, den Script-CodeBehind zu überprüfen, statt die Darstellung der Diagramm-Benutzeroberfläche für Ihre Datenflüsse überprüfen zu müssen.

Hier sind ein paar Beispiele für Anwendungsfälle:
- Programmgesteuerte Erstellung vieler Datenflüsse, die ziemlich ähnlich sind, d.h., „Ausmerzen“ von Datenflüssen.
- Komplexe Ausdrücke, die in der Benutzeroberfläche schwierig zu verwalten sind oder zu Problemen bei der Überprüfung führen.
- Debuggen von verschiedenen Fehlern, die während der Ausführung zurückgegeben werden, und ein besseres Verständnis dafür.

Wenn Sie ein Datenflussskript zur Verwendung von PowerShell oder einer API erstellen, müssen Sie den formatierten Text in eine einzige Zeile reduzieren. Sie können Tabstopps und Zeilenumbrüche als Escapezeichen beibehalten. Der Text muss aber so formatiert werden, dass er in eine JSON-Eigenschaft passt. In der Benutzeroberfläche des Skript-Editors gibt es ganz unten eine Schaltfläche, über die das Skript als eine einzige Zeile formatiert wird.

![Schaltfläche „Kopieren“](media/data-flow/copybutton.png "Schaltfläche „Kopieren“")

## <a name="how-to-add-transforms"></a>Hinzufügen von Transformationen
Zum Hinzufügen von Transformationen sind drei grundlegende Schritte erforderlich: Hinzufügen der Haupttransformationsdaten, Umleiten des Eingabestreams und Umleiten des Ausgabestreams. Dies lässt sich am einfachsten in einem Beispiel zeigen.
Angenommen, Sie beginnen mit einer einfachen Quelle zum Senken des Datenflusses wie hier:

```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Wenn Sie beschließen, eine abgeleitete Transformation hinzuzufügen, müssen Sie zuerst den Haupttransformationstext erstellen, der einen einfachen Ausdruck zum Hinzufügen der neuen Großbuchstabenspalte `upperCaseTitle` enthält:
```
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
```

Anschließend fügen Sie dem vorhandenen DFS die Transformation hinzu:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Dann leiten Sie den eingehenden Stream um, indem Sie identifizieren, nach welcher Transformation die neue Transformation erfolgen soll (in diesem Fall, `source1`), und den Namen des Streams in die neue Transformation kopieren:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Und schließlich identifizieren Sie die Transformation, die nach dieser neuen Transformation erfolgen soll, und ersetzen deren Eingabestream (in diesem Fall, `sink1`) durch den Namen des Ausgabestreams für die neue Transformation:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
deriveTransformationName sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="dfs-fundamentals"></a>DFS-Grundlagen
Der DFS besteht aus einer Reihe von verbundenen Transformationen, einschließlich Quellen, Senken und verschiedenen anderen, die neue Spalten hinzufügen, Daten filtern, Daten verknüpfen und vieles mehr erledigen können. Normalerweise startet das Skript mit einer oder mehreren Quellen, gefolgt von vielen Transformationen, und es endet mit einer oder mehreren Senken.

Alle Quellen haben denselben Standardaufbau:
```
source(
  source properties
) ~> source_name
```

Beispielsweise würde eine einfache Quelle mit drei Spalten („movieId“, „title“, „genres“) so aussehen:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
```

Alle anderen Transformationen als Quellen haben denselben Standardaufbau:
```
name_of_incoming_stream transformation_type(
  properties
) ~> new_stream_name
```

Beispielsweise würde eine einfache abgeleitete Transformation, die eine Spalte („title“) mit einer Version in Großbuchstaben überschreibt, so aussehen:
```
source1 derive(
  title = upper(title)
) ~> derive1
```

Und eine Senke ohne Schema würde so aussehen:
```
derive1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="script-snippets"></a>Skriptausschnitte

Skriptausschnitte sind zur Freigabe geeigneter Code von Datenflussskripts, den Sie Datenflüsse übergreifend verwenden können. Im folgenden Video wird erläutert, wie Sie Skriptausschnitte verwenden und mit dem Datenflussskript Teile des Skripts kopieren und hinter Ihren Datenflussdiagrammen einfügen können:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tA9b]


### <a name="aggregated-summary-stats"></a>Aggregierte Zusammenfassungsstatistiken
Fügen Sie dem Datenfluss „SummaryStats“ eine aggregierte Transformation hinzu, und fügen Sie anschließend den folgenden Code für die Aggregatfunktion in das Skript ein. Ersetzen Sie dabei die vorhandenen SummaryStats. Dadurch wird ein generisches Muster für Zusammenfassungsstatistiken zum Datenprofil bereitgestellt.

```
aggregate(each(match(true()), $$+'_NotNull' = countIf(!isNull($$)), $$ + '_Null' = countIf(isNull($$))),
        each(match(type=='double'||type=='integer'||type=='short'||type=='decimal'), $$+'_stddev' = round(stddev($$),2), $$ + '_min' = min ($$), $$ + '_max' = max($$), $$ + '_average' = round(avg($$),2), $$ + '_variance' = round(variance($$),2)),
        each(match(type=='string'), $$+'_maxLength' = max(length($$)))) ~> SummaryStats
```
Sie können auch das unten aufgeführte Beispiel verwenden, um die Anzahl der eindeutigen und die Anzahl der unterschiedlichen Zeilen in Ihren Daten zu ermitteln. Das folgende Beispiel kann in einen Datenfluss mit der Aggregattransformation ValueDistAgg eingefügt werden. In diesem Beispiel wird eine Spalte mit dem Namen „title“ verwendet. Achten Sie darauf, „title“ durch die Zeichenfolgenspalte in Ihren Daten zu ersetzen, die Sie verwenden möchten, um Werteanzahlen zu erhalten.

```
aggregate(groupBy(title),
    countunique = count()) ~> ValueDistAgg
ValueDistAgg aggregate(numofunique = countIf(countunique==1),
        numofdistinct = countDistinct(title)) ~> UniqDist
```

### <a name="include-all-columns-in-an-aggregate"></a>Einschließen aller Spalten in ein Aggregat
Dabei handelt es sich um ein allgemeines Aggregatmuster, das veranschaulicht, wie Sie beim Erstellen von Aggregaten die verbleibenden Spalten in den Ausgabemetadaten beibehalten können. In diesem Fall verwenden Sie die ```first()```-Funktion, um den ersten Wert in jeder Spalte auszuwählen, deren Name nicht „movie“ ist. Für die Verwendung erstellen Sie eine Aggregattransformation namens „DistinctRows“ und fügen diese dann über dem vorhandenen Aggregatskript „DistinctRows“ in Ihr Skript ein.

```
aggregate(groupBy(movie),
    each(match(name!='movie'), $$ = first($$))) ~> DistinctRows
```

### <a name="create-row-hash-fingerprint"></a>Erstellen von Zeilenhash-Fingerabdrücken 
Verwenden Sie diesen Code in Ihrem Datenflussskript, um eine neue abgeleitete Spalte namens ```DWhash``` zu erstellen, die einen ```sha1```-Hash von drei Spalten erzeugt.

```
derive(DWhash = sha1(Name,ProductNumber,Color))
```

Sie können das Skript unten auch zum Generieren eines Zeilenhashs mithilfe aller im Stream vorhandenen Spalten verwenden, ohne jede einzelne Spalte benennen zu müssen:

```
derive(DWhash = sha1(columns()))
```

### <a name="string_agg-equivalent"></a>String_agg equivalent
Dieser Code verhält sich wie die T-SQL-Funktion ```string_agg()``` und aggregiert Zeichenfolgenwerte in ein Array. Dieses Array können Sie dann in eine Zeichenfolge zur Verwendung bei SQL-Zielen umwandeln.

```
source1 aggregate(groupBy(year),
    string_agg = collect(title)) ~> Aggregate1
Aggregate1 derive(string_agg = toString(string_agg)) ~> DerivedColumn2
```

### <a name="count-number-of-updates-upserts-inserts-deletes"></a>Anzahl von Updates, Upserts, Einfügungen, Löschungen
Bei Verwendung einer Zeilenänderungstransformation können Sie die Anzahl von Updates, Upserts, Einfügungen und Löschvorgängen zählen, die sich aus den Richtlinien für Zeilenänderungen ergeben. Fügen Sie nach der Zeilenänderung eine Transformation für das Aggregieren hinzu, und fügen Sie das folgende Datenflussskript in die Aggregatdefinition für diese Anzahl ein:

```
aggregate(updates = countIf(isUpdate(), 1),
        inserts = countIf(isInsert(), 1),
        upserts = countIf(isUpsert(), 1),
        deletes = countIf(isDelete(),1)) ~> RowCount
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie Datenflüsse, und lesen Sie zunächst den [Artikel „data flows overview“](concepts-data-flow-overview.md) (Übersicht über Datenflüsse).
