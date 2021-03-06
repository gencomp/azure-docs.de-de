---
title: PowerShell-Module
titleSuffix: ML Studio (classic) - Azure
description: Sie können PowerShell verwenden, um Arbeitsbereiche, Experimente, Webdienste und vieles mehr in Azure Machine Learning Studio (klassisch) zu erstellen und zu verwalten.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 04/25/2019
ms.openlocfilehash: 21bb8253d94ca91ee1baffb6de1f47ca34e9a7d0
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117560"
---
# <a name="powershell-modules-for-azure-machine-learning-studio-classic"></a>PowerShell-Module für Azure Machine Learning Studio (klassisch)

Mithilfe von PowerShell-Modulen können Sie Ihre Ressourcen und Objekte für Studio (klassisch) wie Arbeitsbereiche, Datasets und Webdienste programmgesteuert verwalten.

Sie können mithilfe von drei PowerShell-Modulen mit Studio-Ressourcen (Classic) interagieren:

* [Azure PowerShell Az](#az-rm) (veröffentlicht 2018) umfasst die gesamte Funktionalität von AzureRM, allerdings mit anderen Cmdlet-Namen
* [AzureRM](#az-rm) (veröffentlicht 2016) wurde ersetzt durch PowerShell Az.
* [Azure Machine Learning PowerShell – klassisch](#classic), veröffentlicht 2016

Trotz einiger Ähnlichkeiten ist jedes dieser PowerShell-Module für bestimmte Szenarien konzipiert. In diesem Artikel werden die Unterschiede zwischen den PowerShell-Modulen beschrieben, und Sie erhalten Unterstützung bei der Modulauswahl.  

Überprüfen Sie die nachstehende [Unterstützungstabelle](#support-table), um zu sehen, welche Ressourcen von jedem Modul unterstützt werden. 

## <a name="azure-powershell-az-and-azurerm"></a><a name="az-rm"></a> Azure PowerShell Az und AzureRM

Az ist nun das vorgesehene PowerShell-Modul für die Interaktion mit Azure und umfasst die gesamte Funktionalität von AzureRM. Für AzureRM werden weiterhin Bugfixes bereitgestellt, jedoch keine neuen Cmdlets oder Features.  Az und AzureRM verwalten beide Lösungen, die mit dem **Azure Resource Manager**-Bereitstellungsmodell bereitgestellt werden. Zu diesen Ressourcen gehören Studio-Arbeitsbereiche (klassisch) und neue Studio-Webdienste (klassisch). 

PowerShell (klassisch) kann zusammen mit Az oder AzureRM installiert werden, um sowohl neue als auch klassische Ressourcentypen abzudecken. Eine gleichzeitige Installation von Az und AzureRM wird jedoch nicht empfohlen. Bei der Auswahl zwischen Az und AzureRM empfiehlt Microsoft, Az für alle zukünftigen Bereitstellungen zu verwenden.  Weitere Informationen zu Az und AzureRM sowie zum Migrationspfad finden Sie in der [Einführung zu Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

Befolgen Sie zum Einstieg in Az die [Installationsanweisungen für Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="powershell-classic"></a><a name="classic"></a> PowerShell – klassisch

Das [klassische PowerShell-Modul](https://aka.ms/amlps) von Studio (klassisch) ermöglicht Ihnen das Verwalten von Ressourcen, die mit dem **klassischen Bereitstellungsmodell** bereitgestellt wurden. Zu diesen Ressourcen zählen Studio-Benutzerobjekte (klassisch), „klassische“ Webdienste und „klassische“ Webdienstendpunkte.

Microsoft empfiehlt jedoch, für alle künftigen Ressourcen das Resource Manager-Bereitstellungsmodell zu verwenden, um die Bereitstellung und Verwaltung der Ressourcen zu vereinfachen. Weitere Informationen über die Bereitstellungsmodelle finden Sie im Artikel zum [Vergleich der Azure Resource Manager-Bereitstellung und der klassischen Bereitstellung](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Laden Sie zum Einstieg in PowerShell (klassisch) das [Versionspaket](https://github.com/hning86/azuremlps/releases) von GitHub herunter, und befolgen Sie die [Anweisungen für die Installation](https://github.com/hning86/azuremlps/blob/master/README.md). In den Anweisungen wird erklärt, wie Sie die Blockierung der heruntergeladenen/entzippten DLL aufheben und sie dann in Ihre PowerShell-Umgebung importieren.

PowerShell (klassisch) kann zusammen mit Az oder AzureRM installiert werden, um sowohl neue als auch klassische Ressourcentypen abzudecken.

## <a name="powershell-support-table"></a><a name="support-table"></a> PowerShell-Unterstützungstabelle


| | **Az** |  **PowerShell – klassisch** |
| --- | --- | --- |
| Erstellen/Löschen von Arbeitsbereichen | [Resource Manager-Vorlagen](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Verwalten von Vertragsplänen für Arbeitsbereiche | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | |
| Verwalten von Arbeitsbereichsbenutzern |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Verwalten von Webdiensten | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br>(„Neue“ Webdienste)|| [New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br>(„Klassische“ Webdienste) |
| Verwalten von Webdienst-Endpunkten/-schlüsseln |  [Get-AzMlWebServiceKey](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekey)|  [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint)|
| Verwalten von Benutzerdatasets/trainierten Modellen| | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Verwalten von Benutzerexperimenten |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Verwalten von benutzerdefinierten Modulen | | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die vollständige Dokumentation zu diesem PowerShell-Modul:
* [PowerShell – klassisch](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
