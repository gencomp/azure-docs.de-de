---
title: Skalieren von Azure Cosmos DB nach einem Zeitplan mithilfe von Azure Functions-Timern
description: Erfahren Sie, wie Sie bei Änderungen am Durchsatz in Azure Cosmos DB mithilfe von PowerShell und Azure Functions eine Skalierung durchführen.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 01/13/2020
ms.author: mjbrown
ms.openlocfilehash: 9f538b02e81d885e22a6417d7c1f139c22635b0d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85262479"
---
# <a name="scale-azure-cosmos-db-throughput-by-using-azure-functions-timer-trigger"></a>Skalieren des Azure Cosmos DB-Durchsatzes mithilfe von Azure Functions-Triggern

Die Leistung eines Azure Cosmos-Kontos basiert auf der Menge des bereitgestellten Durchsatzes, der in Anforderungseinheiten pro Sekunde (Request Units, RU/s) angegeben wird. Die Bereitstellung erfolgt mit einer zweiten Granularität und wird basierend auf den höchsten RU/s pro Stunde abgerechnet. Dieses bereitgestellte Kapazitätsmodell ermöglicht dem Dienst, einen vorhersagbaren und konsistenten Durchsatz, garantiert niedrige Latenz und Hochverfügbarkeit zu bieten. Die meisten Produktionsworkloads nutzen diese Features. In Entwicklungs- und Testumgebungen, in denen Azure Cosmos DB nur während der Arbeitszeit verwendet wird, können Sie den Durchsatz jedoch am Morgen hochskalieren und am Abend nach der Arbeitszeit wieder herunterskalieren.

Sie können den Durchsatz über [Azure Resource Manager-Vorlagen](resource-manager-samples.md), die [Azure-Befehlszeilenschnittstelle](cli-samples.md) und [PowerShell-](powershell-samples-sql.md) für Core-API-Konten (SQL) oder mithilfe der sprachspezifischen Azure Cosmos DB-SDKs festlegen. Der Vorteil der Verwendung von Resource Manager-Vorlagen, der Azure-Befehlszeilenschnittstelle oder PowerShell besteht darin, dass sie alle Azure Cosmos DB-Modell-APIs unterstützen.

## <a name="throughput-scheduler-sample-project"></a>Beispielprojekt für den Durchsatzscheduler

Um den Prozess der Skalierung von Azure Cosmos DB nach einem Zeitplan zu vereinfachen, haben wir das Beispielprojekt [ Azure Cosmos throughput scheduler](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler) (Azure Cosmos-Durchsatzscheduler) erstellt. Dieses Projekt ist eine Azure Functions-App mit zwei Triggern: „ScaleUpTrigger“ und „ScaleDownTrigger“. Die Trigger führen ein PowerShell-Skript aus, mit dem der Durchsatz für jede Ressource wie in der Datei `resources.json` für jeden Trigger definiert festgelegt wird. ScaleUpTrigger ist für die Ausführung um 8:00 Uhr UTC konfiguriert, und ScaleDownTrigger ist für die Ausführung um 18:00 Uhr UTC konfiguriert. Diese Zeiten können in der Datei `function.json` für den jeweiligen Trigger problemlos geändert werden.

Sie können dieses Projekt lokal klonen, um die Azure Cosmos DB Ressourcen anzugeben, die hoch- und herunterskaliert werden sollen, und um den Zeitplan für die Ausführung anzupassen. Später können Sie sie in einem Azure-Abonnement bereitstellen und mithilfe der verwalteten Dienstidentität mit den Berechtigungen der [rollenbasierten Zugriffssteuerung](role-based-access-control.md) (Role-based Access Control, RBAC) mit der Rolle „Azure Cosmos DB-Operator“ schützen, um den Durchsatz für Ihre Azure Cosmos-Konten festzulegen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr, und laden Sie das Beispiel für den [Azure Cosmos DB-Durchsatzscheduler](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler) herunter.
