---
title: 'Problembehandlung von Windows Virtual Desktop: Übersicht – Azure'
description: Eine Übersicht zur Problembehandlung bei der Einrichtung einer Windows Virtual Desktop-Umgebung.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 06/05/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 07857feb2e5552429c445b22c4c4b2f121f29a93
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84976488"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Problembehandlung: Übersicht, Feedback und Support

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/troubleshoot-set-up-overview-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieser Artikel gibt einen Überblick über die Probleme, die bei der Einrichtung einer Windows Virtual Desktop-Umgebung auftreten können, und beschreibt Möglichkeiten, diese Probleme zu lösen.

## <a name="report-issues-during-public-preview"></a>Melden von Problemen während der öffentlichen Vorschauphase

Über die [Tech Community für Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie während der öffentlichen Vorschauphase der Version vom Frühjahr 2020 Probleme melden oder Features vorschlagen. Außerdem können Sie in der Tech Community bewährte Methoden diskutieren oder neue Features vorschlagen sowie für diese abstimmen. Wenn Sie ein Problem im Zusammenhang mit der öffentlichen Vorschauversion melden, achten Sie darauf, den Problemtyp **Spring 2020 Update (Preview)** (Update vom Frühjahr 2020 (Vorschauversion)) anzugeben.

Wenn Sie einen Beitrag posten, in dem Sie um Hilfe bitten oder ein neues Feature vorschlagen, sollten Sie Ihr Anliegen so detailliert wie möglich beschreiben. Dies hilft anderen Benutzern dabei, Ihre Fragen zu beantworten oder sich ein genaueres Bild von dem vorgeschlagenen Feature zu machen.

## <a name="escalation-tracks"></a>Eskalationspfade

Als Erstes sollten Sie die [Azure-Statusseite](https://status.azure.com/status) und [Azure Service Health](https://azure.microsoft.com/features/service-health/) prüfen, um sich zu vergewissern, dass Ihr Azure-Dienst ordnungsgemäß funktioniert.

Orientieren Sie sich an der folgenden Tabelle, um Probleme zu identifizieren und zu beheben, die beim Einrichten einer Umgebung mit dem Remotedesktopclient auftreten können. Nachdem Sie Ihre Umgebung eingerichtet haben, können Sie den neuen [Diagnosedienst](diagnostics-role-service.md) verwenden, um Probleme in häufigen Szenarien zu identifizieren.

| **Problem**                                                            | **Vorgeschlagene Lösung**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Einstellungen für den Sitzungshostpool von Azure Virtual Network (VNET) und Express Route               | [Öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie den entsprechenden Dienst (in der Kategorie „Netzwerk“) aus. |
| Sitzungshostpool-VM-Erstellung, wenn keine mit Windows Virtual Desktop bereitgestellten Azure Resource Manager-Vorlagen verwendet werden | [Erstellen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie als Dienst **Windows Virtual Desktop** aus. <br> <br> Informationen zu Problemen mit den Vorlagen von Azure Resource Manager, die mit Windows Virtual Desktop bereitgestellt werden, finden Sie im Abschnitt „Fehler mit der Azure Resource Manager-Vorlage“ der Seite [Hostpoolerstellung](troubleshoot-set-up-issues.md). |
| Verwalten der Windows Virtual Desktop-Sitzungshostumgebung über das Azure-Portal    | [Öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/). <br> <br> Informationen zu Verwaltungsproblemen bei Verwendung von Remotedesktopdiensten/Windows Virtual Desktop PowerShell finden Sie unter [Windows Virtual Desktop PowerShell](troubleshoot-powershell.md). Alternativ [erstellen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie **Windows Virtual Desktop** als Dienst und **Konfiguration und Verwaltung** als Problemtyp aus. Wählen Sie anschließend **Probleme beim Konfigurieren einer Umgebung über PowerShell** als Untertyp des Problems aus. |
| Verwalten einer Windows Virtual Desktop-Konfiguration, die an Hostpools und Anwendungsgruppen (App-Gruppen) gebunden ist      | Weitere Informationen finden Sie unter [Windows Virtual Desktop PowerShell](troubleshoot-powershell.md). Alternativ [öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie **Windows Virtual Desktop** als Dienst sowie den jeweiligen Problemtyp aus.|
| Bereitstellen und Verwalten von FSLogix-Profilcontainern | Weitere Informationen finden Sie unter [Leitfaden zur Problembehandlung für FSLogix-Produkte](/fslogix/fslogix-trouble-shooting-ht/). Wenn sich das Problem dadurch nicht beheben lässt, [öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie **Windows Virtual Desktop** als Dienst und **FSLogix** als Problemtyp aus. Wählen Sie anschließend den entsprechenden Untertyp des Problems aus. |
| Fehlfunktion von Remotedesktopclients beim Start                                                 | Weitere Informationen finden Sie unter [Problembehandlung beim Remotedesktopclient](troubleshoot-client.md). Wenn sich das Problem dadurch nicht beheben lässt, [öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/), und wählen Sie **Windows Virtual Desktop** als Dienst und **Remotedesktopclients** als Problemtyp aus.  <br> <br> Wenn es sich um ein Netzwerkproblem handelt, müssen Ihre Benutzer sich an ihren Netzwerkadministrator wenden. |
| Verbunden, aber kein Feed                                                                 | Führen Sie die Problembehandlung anhand der im Artikel [Windows Virtual Desktop-Dienstverbindungen](troubleshoot-service-connection.md) im Abschnitt [Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed)](troubleshoot-service-connection.md#user-connects-but-nothing-is-displayed-no-feed) beschriebenen Schritte aus. <br> <br> Wenn Ihre Benutzer einer App-Gruppe zugewiesen wurden, [öffnen Sie eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/) und wählen Sie **Windows Virtual Desktop** als Dienst und **Remotedesktopclients** als Problemtyp aus. |
| Feedermittlungsprobleme aufgrund des Netzwerks                                            | Ihre Benutzer müssen sich an ihren Netzwerkadministrator wenden. |
| Verbinden von Clients                                                                    | Weitere Informationen finden Sie unter [Windows Virtual Desktop-Dienstverbindungen](troubleshoot-service-connection.md). Wenn das Ihr Problem nicht löst, finden Sie weitere Informationen unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md). |
| Reaktionsfähigkeit von Remoteanwendungen oder Desktop                                      | Wenn Probleme mit einer bestimmten Anwendung oder einem Produkt verknüpft sind, wenden Sie sich an das Team, das für das betreffende Produkt zuständig ist. |
| Lizenzierungsmeldungen oder -fehler                                                          | Wenn Probleme mit einer bestimmten Anwendung oder einem Produkt verknüpft sind, wenden Sie sich an das Team, das für das betreffende Produkt zuständig ist. |
| Probleme mit Authentifizierungsmethoden von Drittanbietern | Überprüfen Sie, ob Ihr Drittanbieter Windows Virtual Desktop-Szenarios unterstützt, und kontaktieren Sie ihn bezüglich bekannter Probleme. |
| Probleme beim Verwenden von Log Analytics für Windows Virtual Desktop | Erstellen Sie bei Problemen mit dem Diagnoseschema [eine Azure-Supportanfrage](https://azure.microsoft.com/support/create-ticket/).<br><br>Wählen Sie für Abfragen, Visualisierungen oder andere Probleme in Log Analytics unter „Log Analytics“ den entsprechenden Problemtyp aus. |
| Probleme beim Verwendung von M365-Apps | Wenden Sie sich über eine der [M365 Admin Center-Hilfeoptionen](/microsoft-365/admin/contact-support-for-business-products/) an das M365 Admin Center. |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Problembehandlung beim Erstellen eines Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Behebung von Problemen bei Windows Virtual Desktop-Clientverbindungen finden Sie unter [Windows Virtual Desktop – Clientverbindungen](troubleshoot-service-connection.md).
- Informationen zur Behebung von Problemen bei Remotedesktop-Clients finden Sie unter [Problembehandlung für den Remotedesktop-Client](troubleshoot-client.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Weitere Informationen zum Dienst finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../azure-resource-manager/management/view-activity-logs.md).
- Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen](../azure-resource-manager/templates/deployment-history.md).
