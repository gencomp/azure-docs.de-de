---
title: 'Ereignisquellen zur Streamingerfassung: Azure Time Series Insights | Microsoft-Dokumentation'
description: Hier erfahren Sie mehr über das Streamen von Daten in Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 602f5a0df6cbd7c308d45d02795e7404c46c73a7
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049389"
---
# <a name="time-series-insights-event-sources"></a>Time Series Insights-Ereignisquellen

 Ihre TSI-Umgebung kann bis zu zwei Streamingereignisquellen enthalten. Zwei Arten von Azure-Ressourcen werden als Eingaben unterstützt:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Ereignisse müssen als UTF-8-codierte JSON-Dateien gesendet werden.

## <a name="create-or-edit-event-sources"></a>Erstellen oder Bearbeiten von Ereignisquellen

Die Ressource(n) Ihrer Ereignisquelle kann bzw. können sich im selben Azure-Abonnement wie Ihre Time Series Insights-Umgebung oder in einem anderen Abonnement befinden. Mithilfe des [Azure-Portals](time-series-insights-update-create-environment.md#create-a-preview-payg-environment), der [Azure CLI](https://github.com/Azure/azure-cli-extensions/tree/master/src/timeseriesinsights), von [ARM-Vorlagen](time-series-insights-manage-resources-using-azure-resource-manager-template.md) und der [REST-API](https://docs.microsoft.com/rest/api/time-series-insights/management/eventsources) können Sie die Ereignisquelle Ihrer Umgebung erstellen, bearbeiten oder entfernen.

Wenn Sie eine Ereignisquelle verbinden, liest Ihre TSI-Umgebung alle Ereignisse, die aktuell in Ihrem IoT Hub oder Event Hub gespeichert sind. Dabei wird mit dem ältesten Ereignis begonnen.

> [!IMPORTANT]
>
> * Beim Anfügen einer Ereignisquelle an die Vorschau-Umgebung tritt zu Anfang möglicherweise eine hohe Latenz auf.
> Die Latenz der Ereignisquelle hängt von der Anzahl der Ereignisse ab, die sich aktuell in Ihrem IoT Hub oder Event Hub befinden.
> * Eine hohe Latenz lässt nach der erstmaligen Erfassung der Ereignisquelldaten nach. Senden Sie ein Supportticket über das Azure-Portal, falls es bei Ihnen zu einer dauerhaft hohen Latenz kommt.

## <a name="streaming-ingestion-best-practices"></a>Bewährte Methoden für die Streamingerfassung

* Erstellen Sie immer eine eindeutige Consumergruppe für Ihre TSI-Umgebung, um Daten aus Ihrer Ereignisquelle zu nutzen. Die erneute Verwendung von Consumergruppen kann zufällige Verbindungsunterbrechungen verursachen und zu Datenverlusten führen.

* Konfigurieren Sie Ihre TSI-Umgebung und IoT Hub und/oder Event Hubs in derselben Azure-Region. Obwohl eine Ereignisquelle in einer anderen Region konfiguriert werden kann, wird dieses Szenario nicht unterstützt, und es kann keine Hochverfügbarkeit garantiert werden.

* Überschreiten Sie nicht den [Grenzwert Ihrer Umgebung für die Durchsatzrate](concepts-streaming-throughput-limitations.md) oder den Durchsatz pro Partition.

* Konfigurieren Sie eine [Verzögerungswarnung](https://review.docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency?branch=pr-en-us-117938#monitor-latency-and-throttling-with-alerts), damit Sie benachrichtigt werden, wenn in Ihrer Umgebung Probleme beim Verarbeiten von Daten auftreten.

* Verwenden Sie die Streamingerfassung nur für Daten in Quasi-Echtzeit und für aktuelle Daten. Das Streamen historischer Daten wird nicht unterstützt.

* Informieren Sie sich, wie Eigenschaften mit Escapezeichen versehen werden und wie JSON-[Daten vereinfacht und gespeichert](./concepts-json-flattening-escaping-rules.md) werden.

* Befolgen Sie das Prinzip der geringsten Rechte, wenn Sie Verbindungszeichenfolgen für Ereignisquellen angeben. Konfigurieren Sie für Event Hubs eine SAS-Richtlinie, die ausschließlich den *send*-Anspruch umfasst, und verwenden Sie für IoT Hub nur die *service connect*-Berechtigung.

### <a name="historical-data-ingestion"></a>Erfassung historischer Daten

Die Streamingpipeline kann in der Vorschauversion von Azure Time Series Insights aktuell nicht zum Importieren historischer Daten verwendet werden. Wenn Sie ältere Daten in Ihre Umgebung importieren möchten, beachten Sie die folgenden Richtlinien:

* Streamen Sie Livedaten und historische Daten nicht parallel. Die Erfassung unsortierter Daten wirkt sich nachteilig auf die Abfrageleistung aus.
* Erfassen Sie historische Daten in zeitlicher Reihenfolge, um die bestmögliche Leistung zu erzielen.
* Bleiben Sie innerhalb der unten angegebenen Ratengrenzwerte für den Erfassungsdurchsatz.
* Deaktivieren Sie den warmen Speicher, wenn das Alter der Daten den Aufbewahrungszeitraum Ihres warmen Speichers übersteigt.

## <a name="event-source-timestamp"></a>Zeitstempel der Ereignisquelle

Beim Konfigurieren einer Ereignisquelle werden Sie dazu aufgefordert, eine Zeitstempel-ID-Eigenschaft anzugeben. Die Zeitstempeleigenschaft wird verwendet, um Ereignisse im Zeitverlauf zu verfolgen. Diese Zeit wird als $event.$ts in den [Time Series-Abfrage-APIs](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute) und zum Zeichnen von Reihen im TSI-Explorer verwendet. Wenn zum Zeitpunkt der Erstellung keine Eigenschaft bereitgestellt wird oder die Zeitstempeleigenschaft aus einem Ereignis fehlt, wird der Zeitpunkt der Einreihung in die IoT Hub- oder Event Hubs-Warteschlange als Standardwert verwendet. Werte der Zeitstempeleigenschaft werden in UTC gespeichert.

Im Allgemeinen entscheiden sich Benutzer dafür, die Zeitstempeleigenschaft anzupassen und den Zeitpunkt zu verwenden, zu dem der Sensor oder das Tag den Lesevorgang generiert hat, anstatt den Standardwert des Zeitpunkts der Einreihung in die Hub-Warteschlange zu verwenden. Dies ist besonders dann notwendig, wenn Geräte vorübergehenden Konnektivitätsverlust aufweisen und ein Batch verzögerter Nachrichten an TSI weitergeleitet wird.

Wenn sich der benutzerdefinierte Zeitstempel innerhalb eines geschachtelten JSON-Objekts oder eines Arrays befindet, müssen Sie den richtigen Eigenschaftennamen unter Berücksichtigung der [Namenskonventionen beim Vereinfachen und Versehen mit Escapezeichen](concepts-json-flattening-escaping-rules.md) bereitstellen. Der [hier](concepts-json-flattening-escaping-rules.md#example-a) angezeigte Zeitstempel der Ereignisquelle für die JSON-Nutzlast sollte als `"values.time"` eingegeben werden.

### <a name="time-zone-offsets"></a>Zeitzonenoffset

Zeitstempel müssen im ISO 8601-Format gesendet werden und werden im UTC-Format gespeichert. Wenn ein Zeitzonenoffset angegeben wird, wird der Offset angewendet und anschließend die Zeit im UTC-Format gespeichert und zurückgegeben. Wenn der Offset nicht ordnungsgemäß formatiert ist, wird er ignoriert. In Situationen, in denen Ihre Lösung möglicherweise keinen Kontext des ursprünglichen Offsets aufweist, können Sie die Offsetdaten in einer zusätzlichen separaten Ereigniseigenschaft senden, um sicherzustellen, dass sie beibehalten werden und Ihre Anwendung in einer Abfrageantwort verweisen kann.

Der Zeitzonenoffset sollte eines der folgenden Formate aufweisen:

±HHMMZ</br>
±HH:MM</br>
±HH:MMZ</br>

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md), um zu verstehen, wie Ereignisse gespeichert werden. 

* Informieren Sie sich über die [Durchsatzbeschränkungen](concepts-streaming-throughput-limitations.md) Ihrer Umgebung




