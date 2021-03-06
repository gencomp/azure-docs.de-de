---
title: Preismodell für Azure Cosmos DB
description: In diesem Artikel werden das Preismodell für Azure Cosmos DB und die Möglichkeiten zur Vereinfachung von Kostenmanagement und Kostenplanung beschrieben.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: d36b4fd433af716ebd97d88d05922d94bd74c309
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523535"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Preismodell in Azure Cosmos DB

Das Preismodell für Azure Cosmos DB vereinfacht das Kostenmanagement und die Kostenplanung. Bei Azure Cosmos DB zahlen Sie für den bereitgestellten Durchsatz und den verwendeten Speicher.

* **Bereitgestellter Durchsatz**: Der [bereitgestellte Durchsatz](how-to-choose-offer.md) (auch als reservierter Durchsatz bezeichnet) garantiert eine hohe Leistung in jeder Größenordnung. Sie geben den benötigten Durchsatz (Anforderungseinheiten pro Sekunde, RUs/Sek.) an, und Azure Cosmos DB reserviert die Ressourcen, die erforderlich sind, um den konfigurierten Durchsatz zu gewährleisten. Die Abrechnung erfolgt auf Stundenbasis für den für die jeweilige Stunde bereitgestellten maximalen Durchsatz. Sie können den Durchsatz manuell bereitstellen oder die [Autoskalierung](provision-throughput-autoscale.md) verwenden.

   > [!NOTE]
   > Da das Modell mit bereitgestelltem Durchsatz Ressourcen dediziert für Ihren Container oder Ihre Datenbank bereitstellt, wird Ihnen der bereitgestellte Durchsatz auch dann in Rechnung gestellt, wenn Sie keine Workloads ausführen.

* **Speichernutzung**: Für die Gesamtmenge des Speichers (GB), der für Daten und Indizes für eine bestimmte Stunde genutzt wird, wird eine Pauschale berechnet.

Der bereitgestellte Durchsatz, der in [Anforderungseinheiten](request-units.md) pro Sekunde (RUs/Sek.) angegeben wird, ermöglicht das Lesen oder Schreiben von Daten in Containern oder Datenbanken. Sie können [Durchsatz entweder für eine Datenbank oder einen Container bereitstellen](set-throughput.md). Basierend auf Ihren Workloadanforderungen können Sie den Durchsatz jederzeit zentral hoch- oder herunterskalieren. Die Preisgestaltung für Azure Cosmos DB ist elastisch und verhält sich proportional zu dem Durchsatz, den Sie für eine Datenbank oder einen Container konfigurieren. Die Mindestwerte für Durchsatz und Speicher sowie die Skalierungseinheiten bilden ein vollständiges Preis-/Elastizitätsspektrum für alle Kundensegmente und von kleinen bis zu umfangreichen Containern ab. Der bereitgestellte Durchsatz wird für jede Datenbank bzw. jeden Container auf Stundenbasis in Einheiten von 100 RUs/Sek. (Mindestbereitstellungsmenge 400 RUs/Sek.) und der genutzte Speicher in GB in Rechnung gestellt. Im Gegensatz zum bereitgestellten Durchsatz wird der Speicher auf Nutzungsbasis abgerechnet. Das bedeutet, dass Sie keinerlei Speicher im Voraus reservieren müssen. Die Berechnung erfolgt nur für den Speicher, den Sie nutzen.

Weitere Informationen finden Sie auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/) und unter [Erläuterungen zu Ihrer Azure Cosmos DB-Rechnung](understand-your-bill.md).

Das Preismodell in Azure Cosmos DB ist für alle APIs einheitlich. Weitere Informationen finden Sie unter [Warum ist das Preismodell von Azure Cosmos DB kosteneffizient für den Kunden?](total-cost-ownership.md). Für die SLA-Garantie ist ein Mindestdurchsatz für eine Datenbank oder einen Container erforderlich, und Sie können den bereitgestellten Durchsatz pro 100 RUs/Sek. erhöhen oder verringern.

Wenn Sie Ihr Azure Cosmos DB-Konto in einer Region in den USA bereitstellen und es sich dabei nicht um eine Government-Region handelt, beträgt der Mindestpreis für datenbank- und containerbasierten Durchsatz derzeit ungefähr 24 USD pro Monat. Die Preise variieren je nach verwendeter Region. Aktuelle Preisinformationen finden Sie auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/). Wenn für Ihre Workload mehrere Container verwendet werden, lassen sich die Kosten anhand von Durchsatz auf Datenbankebene optimieren, mit dem beliebig viele Container in einer Datenbank den Durchsatz gemeinsam nutzen können. Die folgende Tabelle enthält eine Übersicht über den bereitgestellten Durchsatz und die Kosten für unterschiedliche Entitäten:

|**Entität**  | **Mindestdurchsatz** |**Skalierungseinheiten** |**Bereitstellungsumfang** |
|---------|---------|---------|-------|
|Datenbank    | 400 RUs/Sek.    | 100 RUs/Sek.   |Der Durchsatz ist für die Datenbank reserviert und wird von Containern in der Datenbank gemeinsam genutzt |
|Container     | 400 RUs/Sek.   | 100 RUs/Sek.  |Der Durchsatz ist für einen bestimmten Container reserviert |

Wie in der obigen Tabelle zu sehen, beginnt der Preis für den Mindestdurchsatz in Azure Cosmos DB bei etwa 24 USD/Monat. Wenn Sie mit dem Mindestdurchsatz beginnen und den Durchsatz mit der Zeit hochskalieren, um Ihre Workloads in der Produktion zu unterstützen, erhöhen sich die Kosten nach und nach in Schritten von etwa 6 USD/Monat. Die Preise variieren je nach verwendeter Region. Aktuelle Preisinformationen finden Sie auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/). Das Preismodell für Azure Cosmos DB ist elastisch, die Preiserhöhung (Preissenkung) für das Hochskalieren (Herunterskalieren) ist moderat.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB kostenlos testen

Azure Cosmos DB bietet viele Optionen für Entwickler kostenlos an. Die Optionen umfassen:

* **Free-Tarif von Azure Cosmos DB**: Der Free-Tarif von Azure Cosmos DB erleichtert Ihnen den Einstieg, die Entwicklung und die Durchführung von Tests für Ihre Anwendungen. Sie können sogar kleinere Produktionsworkloads kostenlos ausführen. Wenn für ein Konto der Free-Tarif aktiviert ist, sind die ersten 400 RU/s und 5 GB Speicher während der gesamten Lebensdauer des Kontos kostenlos. Sie können pro Azure-Abonnement maximal ein Konto im Free-Tarif einrichten und müssen sich beim Erstellen des Kontos registrieren. [Erstellen Sie zunächst ein neues Konto im Azure-Portal mit aktiviertem Free-Tarif](create-cosmosdb-resources-portal.md), oder verwenden Sie eine [Resource Manager-Vorlage](manage-sql-with-resource-manager.md#free-tier).

* **Kostenloses Azure-Konto**: Azure bietet einen [Free-Tarif](https://azure.microsoft.com/free/) an, mit dem Sie ein Azure-Guthaben von 200 USD für die ersten 30 Tage und 12 Monate lang eine begrenzte Anzahl kostenloser Dienste erhalten. Weitere Informationen finden Sie unter [Kostenloses Azure-Konto](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB ist Bestandteil des kostenlosen Azure-Kontos. Speziell für Azure Cosmos DB bietet dieses kostenlose Konto einen Speicher von 5 GB und einen bereitgestellten Durchsatz von 400 RU/s für das ganze Jahr.

* **Azure Cosmos DB kostenlos testen**: Azure Cosmos DB bietet eine zeitlich begrenzte Benutzererfahrung, indem Sie Azure Cosmos DB für kostenlose Konten testen. Mithilfe der Schnellstartanleitungen und den Tutorials können Sie ein Azure Cosmos DB-Konto sowie eine Datenbank und Sammlungen erstellen und eine Beispielanwendung ausführen. Sie können die Beispielanwendung ausführen, ohne sich für ein Azure-Konto registrieren oder Ihre Kreditkarte verwenden zu müssen. [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/) stellt Azure Cosmos DB einen Monat lang mit der Möglichkeit bereit, Ihr Konto beliebig oft zu erneuern.

* **Azure Cosmos DB-Emulator**: Der Azure Cosmos DB-Emulator stellt eine lokale Umgebung bereit, die den Azure Cosmos DB-Dienst zu Entwicklungszwecken emuliert. Der Emulator wird kostenlos und mit hoher Wiedergabetreue für den Clouddienst angeboten. Bei Verwendung des Azure Cosmos DB-Emulators können Sie die Anwendung lokal entwickeln und testen, ohne ein Azure-Abonnement zu erstellen oder Kosten zu verursachen. Sie können Ihre Anwendungen mit dem Emulator lokal entwickeln, bevor sie in die Produktion gehen. Nachdem Sie mit der Funktionalität der Anwendung im Emulator zufrieden sind, können Sie zum Azure Cosmos DB-Konto in der Cloud wechseln und so erheblich Kosten sparen. Ausführlichere Informationen zum Emulator finden Sie im Artikel [Verwenden von Azure Cosmos DB für Entwicklungs- und Testzwecke](local-emulator.md).

## <a name="pricing-with-reserved-capacity"></a>Preise mit reservierter Kapazität

Die [reservierte Kapazität](cosmos-db-reserved-capacity.md) von Azure Cosmos DB hilft Ihnen, Geld zu sparen, indem Sie die Ressourcen von Azure Cosmos DB entweder für ein Jahr oder für drei Jahre im Voraus bezahlen. Mit der Vorabfestlegung und Bezahlung für ein Jahr oder drei Jahre können Sie Ihre Kosten deutlich senken und gegenüber der regulären Preisgestaltung zwischen 20 % und 65 % sparen. Mit der reservierten Kapazität von Azure Cosmos DB können Sie durch Zahlung des bereitgestellten Durchsatzes (RUs/Sek.) für ein Jahr oder drei Jahre im Voraus die Kosten senken und auf den bereitgestellten Umsatz einen Rabatt erhalten. 

Die reservierte Kapazität bezieht sich auf einen Rechnungsrabatt und wirkt sich nicht auf den Status Ihrer Azure Cosmos DB-Ressourcen zur Laufzeit aus. Die reservierte Kapazität steht einheitlich für alle APIs (einschließlich MongoDB, Cassandra, SQL, Gremlin und Azure Tables ) sowie alle Regionen weltweit zur Verfügung. Erfahren Sie mehr über reservierte Kapazität im Artikel [Vorauszahlen für Azure Cosmos DB-Ressourcen mit reservierter Kapazität](cosmos-db-reserved-capacity.md), und erwerben Sie reservierte Kapazität im [Azure-Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Optimierung der Kosten für Ihre Azure Cosmos DB-Ressourcen in den folgenden Artikeln:

* Weitere Informationen unter [Optimale Entwicklungs- und Testmöglichkeiten mit Azure Cosmos DB](optimize-dev-test.md)
* Weitere Informationen unter [Erläuterungen zu Ihrer Azure Cosmos DB-Rechnung](understand-your-bill.md)
* Weitere Informationen unter [Optimieren der Durchsatzkosten](optimize-cost-throughput.md)
* Weitere Informationen unter [Optimieren der Speicherkosten](optimize-cost-storage.md)
* Weitere Informationen unter [Optimieren der Kosten für Lese- und Schreibvorgänge](optimize-cost-reads-writes.md)
* Weitere Informationen unter [Optimieren der Kosten von Abfragen](optimize-cost-queries.md)
* Weitere Informationen unter [Optimieren der Kosten bei Cosmos-Konten mit mehreren Regionen](optimize-cost-regions.md)
* Weitere Informationen zur [Reservierte Azure Cosmos DB-Kapazität](cosmos-db-reserved-capacity.md)
* Weitere Informationen zum [Azure Cosmos DB-Emulator](local-emulator.md)
