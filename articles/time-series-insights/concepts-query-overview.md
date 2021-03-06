---
title: Abfragen von Daten – Azure Time Series Insights Gen2 | Microsoft-Dokumentation
description: Konzepte für die Datenabfrage und Übersicht über die REST-API in Azure Time Series Insights Gen2.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: seodec18
ms.openlocfilehash: a4b969ecbc92df45021b4a9ec711960171d77d4e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86495294"
---
# <a name="querying-data-from-azure-time-series-insights-gen2"></a>Abfragen von Daten in Azure Time Series Insights Gen2

Azure Time Series Insights Gen2 ermöglicht das Abfragen von Daten für Ereignisse und Metadaten, die in der Umgebung gespeichert sind, über öffentliche Oberflächen-APIs. Diese APIs werden auch im [Azure Time Series Insights Gen2-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer) verwendet.

Drei primäre API-Kategorien sind in Azure Time Series Insights Gen2 verfügbar:

* **Umgebungs-APIs**: Diese APIs ermöglichen Abfragen der Azure Time Series Insights Gen2-Umgebung selbst. Sie ermöglichen das Zusammenstellen der Liste der Umgebungen, auf die der Aufrufer Zugriff hat, sowie von Umgebungsmetadaten.
* **Zeitreihenmodellabfrage (TSM-Q)-APIs**: Ermöglicht die Vorgänge Erstellen, Lesen, Aktualisieren und Löschen (Create, Read, Update, Delete, CRUD) mit Metadaten, die im Zeitreihenmodell der Umgebung gespeichert sind. Diese können verwendet werden, um auf die Instanzen, Typen und Hierarchien zuzugreifen und sie zu bearbeiten.
* **Zeitreihenabfrage (TSQ)-APIs**: Ermöglicht das Abrufen von Telemetrie- oder Ereignisdaten, wie sie vom Quellanbieter aufgezeichnet werden, sowie leistungsstarke Berechnungen und Aggregationen der Daten mithilfe von erweiterten Skalar- und Aggregatfunktionen.

Azure Time Series Insights Gen2 verwendet eine umfassende zeichenfolgenbasierte Ausdruckssprache, [Time Series Expression, TSX (Zeitreihenausdrucksprache)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax), um Berechnungen in [Zeitreihenvariablen](./concepts-variables.md) auszudrücken.

## <a name="azure-time-series-insights-gen2-apis-overview"></a>Übersicht über Azure Time Series Insights Gen2-APIs

Die folgenden Kern-APIs werden unterstützt.

[![Zeitreihenabfrage (TSQ): Übersicht](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Umgebungs-APIs

* [„Umgebungen abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/management/environments/get): Gibt die Liste der Umgebungen zurück, auf die der Benutzer zum Zugriff autorisiert ist.
* [„Umgebungsverfügbarkeit abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/getavailability): Gibt die Verteilung der Anzahl von Ereignissen über den Ereigniszeitstempel `$ts` zurück. Diese API hilft dabei, zu ermitteln, ob Ereignisse in der Umgebung vorhanden sind, indem die Anzahl von Ereignissen (sofern vorhanden) unterteilt in Zeitintervalle zurückgegeben wird.
* [„Ereignisschema abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/geteventschema): Gibt die Ereignisschema-Metadaten für einen bestimmten Suchzeitraum zurück. Diese API hilft beim Abrufen aller Metadaten und Eigenschaften, die im Schema für den angegebenen Suchzeitraum zur Verfügung stehen.

## <a name="time-series-model-query-tsm-q-apis"></a>Zeitreihenmodellabfrage (TSM-Q)-APIs

Die meisten dieser APIs unterstützen die Batchausführung, um Batch-CRUD-Vorgänge für mehrere Entitäten von Zeitreihenmodellen zu ermöglichen:

* [„Modelleinstellungen“-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#model-settings-api): Ermöglicht *GET* und *PATCH* für den Standardtyp und den Modellnamen der Umgebung.
* [Typen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#types-api): Ermöglicht CRUD mit Zeitreihentypen und deren zugeordneten Variablen.
* [Hierarchien-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#hierarchies-api): Ermöglicht CRUD mit Zeitreihenhierarchien und deren zugeordneten Feldpfaden.
* [Instanzen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#instances-api): Ermöglicht CRUD mit Zeitreiheninstanzen und deren zugeordneten Instanzenfeldern. Zusätzlich unterstützt die Instanzen-API die folgenden Vorgänge:
  * [Suchen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/search): Ruft eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen ab.
  * [Vorschlagen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/suggest): Sucht eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen und schlägt sie vor.

## <a name="time-series-query-tsq-apis"></a>Zeitreihenabfrage (TSQ)-APIs

Diese APIs sind in beiden Speichern (warm und kalt) unserer mehrschichtigen Speicherlösung verfügbar. Parameter zum Abfragen der URL werden verwendet, um den [Speichertyp](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#uri-parameters) anzugeben, für den die Abfrage ausgeführt werden soll:

* [„Ereignisse abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents): Ermöglicht das Abfragen und Abrufen von unformatierten Ereignissen und den zugehörigen Zeitstempeln, wie sie in Azure Time Series Insights Gen2 vom Quellanbieter aufgezeichnet werden. Diese API ermöglicht das Abrufen von Rohereignissen für eine bestimmte Zeitreihen-ID und Suchzeitspanne. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 

  > [!IMPORTANT]
  > * Im Rahmen der [bevorstehenden Änderungen für JSON-Vereinfachungs- und -Escaperegeln](https://docs.microsoft.com/azure/time-series-insights/ingestion-rules-update) werden Arrays mit dem Typ **Dynamic** gespeichert. Nutzlasteigenschaften, die mit diesem Typ gespeichert werden, sind **NUR über die Ereignisabruf-API zugänglich**.

* [„Reihe abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries): Ermöglicht das Abfragen und Abrufen berechneter Werte und der zugehörigen Ereigniszeitstempel durch Anwenden von Berechnungen, die durch Variablen in unformatierten Ereignissen definiert werden. Diese Variablen können im Zeitreihenmodell definiert oder in der Abfrage bereitgestellt werden. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 

* [„Reihe aggregieren“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries): Ermöglicht das Abfragen und Abrufen aggregierter Werte und der zugehörigen Intervallzeitstempel durch Anwenden von Berechnungen, die durch Variablen in unformatierten Ereignissen definiert werden. Diese Variablen können im Zeitreihenmodell definiert oder in der Abfrage bereitgestellt werden. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 
  
  Diese API gibt für eine angegebene Suchzeitspanne und ein angegebenes Intervall eine aggregierte Antwort pro Intervall pro Variable für eine Zeitreihen-ID zurück. Die Anzahl der Intervalle im Antwortdataset wird durch Zählen der Epochenticks (Anzahl der Millisekunden seit Beginn der Unix-Epoche am 1. Januar 1970) und die Division der Ticks durch die in der Abfrage angegebene Intervallzeitspanne berechnet.

  Die im Antwortdataset zurückgegebenen Zeitstempel stellen die linken Intervallgrenzen dar, nicht die Stichprobenereignisse im Intervall. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über verschiedene Variablen, die im [Zeitreihenmodell](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm) definiert werden können.
- Erfahren Sie mehr über das Abfragen von Daten im [Azure Time Series Insights Gen2-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer).
