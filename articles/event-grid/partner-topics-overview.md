---
title: Azure Event Grid-Partnerthemen
description: Mit Azure Event Grid können Sie Ereignisse von SaaS- und PaaS-Drittanbieterpartnern direkt an Azure-Dienste senden.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 97829f262a00e2353624b2a3fa9df210518ad3a0
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86115994"
---
# <a name="partner-topics-in-azure-event-grid-preview"></a>Partnerthemen in Azure Event Grid (Vorschau)
Mit Partnerthemen können Sie Ereignisquellen von Drittanbietern direkt mit Azure Event Grid verbinden. Diese Integration ermöglicht Ihnen, Ereignisse von Partnern auf dieselbe Weise wie Ereignisse von Azure-Diensten zu abonnieren. 

## <a name="available-partners"></a>Verfügbare Partner
Der erste Partner, der über Event Grid-Partnerthemen verfügbar ist, ist Auth0. Sie können mit dem [Partnerthema Auth0](auth0-overview.md) eine Verbindung mit Ihren Auth0- und Azure-Konten herstellen. Die Integration ermöglicht Ihnen, in Echtzeit auf Auth0-Ereignisse zu reagieren, diese zu protokollieren und zu überwachen.

[Um es auszuprobieren](auth0-how-to.md), melden Sie sich bei Ihrem Auth0-Konto an und erstellen eine Event Grid-Integration. Nachdem Sie in Auth0 **Erstellen** ausgewählt haben, sehen Sie in Ihrem Azure-Konto ein ausstehendes Auth0-Thema. Wählen Sie **Aktivieren** aus. Anschließend können Sie Abonnements für Event Grid erstellen, um Ihre Ereignisse wie jede andere Ereignisquelle weiterzuleiten, zu filtern und zu übermitteln.

## <a name="pricing"></a>Preise
Partnerthemen werden zum gleichen Tarif wie Systemthemen in Rechnung gestellt.

## <a name="limits"></a>Einschränkungen
Partnerthemen sind in der Public Preview-Phase. In dieser Phase gelten für Partnerthemen die [ gleichen Grenzwerte](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#event-grid-limits) wie für System- und benutzerdefinierte Themen.

## <a name="how-do-i-become-an-event-grid-partner"></a>Wie werde ich Event Grid-Partner?
Die zur Unterstützung dieser Produkteinführung geschaffene Infrastruktur ermöglicht neuen Partnern einfach und schnell, ihre Ereignisverarbeitungskapazitäten in Event Grid zu integrieren. Weitere Informationen finden Sie in der [Dokumentation zum Onboarding von Partnern](partner-onboarding-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- [Auth0-Partnerthema](auth0-overview.md)
- [Integrieren von Azure Event Grid mit Auth0](auth0-how-to.md)
- [Registrieren als Event Grid-Partner](partner-onboarding-overview.md)