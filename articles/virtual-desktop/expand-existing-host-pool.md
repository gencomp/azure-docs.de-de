---
title: 'Erweitern eines vorhandenen Hostpools um neue Sitzungshosts, Frühjahr 2020: Azure'
description: Hier finden Sie Informationen dazu, wie Sie einen vorhandenen Hostpool in Windows Virtual Desktop um neue Sitzungshosts erweitern.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b23cee72af7f8262fdd552c2d4c26c608b68a1e4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86527607"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts"></a>Erweitern eines vorhandenen Hostpools um neue Sitzungshosts

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/expand-existing-host-pool-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie die Nutzung in Ihrem Hostpool erhöhen, müssen Sie den vorhandenen Pool möglicherweise um neue Sitzungshosts erweitern, um die neue Last zu verarbeiten.

In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Hostpool um neue Sitzungshosts erweitern.

## <a name="what-you-need-to-expand-the-host-pool"></a>Voraussetzungen für die Erweiterung des Hostpools

Bevor Sie beginnen, stellen Sie sicher, dass der Hostpool und die Sitzungshost-VMs mit einer der folgenden Methoden erstellt wurden:

- [Azure portal](./create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit PowerShell](./create-host-pools-powershell.md)

Sie benötigen außerdem folgende Informationen aus dem Vorgang der ersten Erstellung des Hostpools und der Sitzungshost-VMs:

- Größe, Image und Namenspräfix der VM
- Administratoranmeldeinformationen des Domänenbeitritts
- Name des virtuellen Netzwerks und des Subnetzes

## <a name="add-virtual-machines-with-the-azure-portal"></a>Hinzufügen von virtuellen Computern mit dem Azure-Portal

So erweitern Sie den Hostpool durch Hinzufügen von virtuellen Computern:

1. Melden Sie sich beim Azure-Portal an.

2. Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.

3. Wählen Sie im Menü auf der linken Seite des Bildschirms **Hostpools**aus, und wählen Sie dann den Namen des Hostpools aus, dem Sie virtuelle Computer hinzufügen möchten.

4. Wählen Sie im Menü auf der linken Seite des Bildschirms die Option **Sitzungshosts** aus.

5. Wählen Sie **+Hinzufügen** aus, um mit dem Erstellen des Hostpools zu beginnen.

6. Ignorieren Sie die Registerkarte mit den Grundeinstellungen, und wählen Sie stattdessen die Registerkarte **VM-Details** aus. Hier können Sie die Details des virtuellen Computers (VM) anzeigen und bearbeiten, den Sie dem Hostpool hinzufügen möchten.

7. Wählen Sie die Ressourcengruppe aus, unter der Sie die VMs erstellen möchten, und wählen Sie dann die Region aus. Sie können die aktuelle Region auswählen, die Sie verwenden, oder eine neue Region.

8. Geben Sie die Anzahl der Sitzungshosts, die Sie zu Ihrem Hostpool hinzufügen möchten, in **Anzahl von VMs** ein. Wenn Sie z. B. Ihren Hostpool um fünf Hosts erweitern möchten, geben Sie **5** ein.

    >[!NOTE]
    >Sie können die Größe oder das Image der VMs nicht bearbeiten, da es wichtig ist, sicherzustellen, dass alle VMs im Hostpool die gleiche Größe besitzen.

9. Wählen Sie als **Informationen zum virtuellen Netzwerk** das virtuelle Netzwerk und das Subnetz aus, mit dem die virtuellen Computer verknüpft werden sollen. Sie können dasselbe virtuelle Netzwerk auswählen, das zurzeit bereits von Ihren vorhandenen Computern verwendet wird, oder ein anderes virtuelles Netzwerk auswählen, das besser für die Region geeignet ist, die Sie in Schritt 7 ausgewählt haben.

10. Geben Sie als **Administratorkonto** den Active Directory-Domänenbenutzernamen und das Kennwort ein, der bzw. das dem ausgewählten virtuellen Netzwerk zugeordnet ist. Diese Anmeldeinformationen werden verwendet, um die virtuellen Computer mit dem virtuellen Netzwerk zu verknüpfen.

      >[!NOTE]
      >Stellen Sie sicher, dass Ihre Administratornamen den hier angegebenen Informationen entsprechen. Vergewissern Sie sich außerdem, dass keine mehrstufige Authentifizierung (MFA) für das Konto aktiviert ist.

11. Wählen Sie die Registerkarte **Tag** aus, wenn Sie über Tags verfügen, mit denen Sie die virtuellen Computer gruppieren möchten. Überspringen Sie diese Registerkarte andernfalls.

12. Wählen Sie die Registerkarte **Überprüfen und erstellen** aus. Überprüfen Sie Ihre Auswahl, und wählen Sie **Erstellen** aus, wenn alles in Ordnung ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren vorhandenen Hostpool erweitert haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um die Einstellungen im Rahmen einer Benutzersitzung zu testen. Sie können jeden der folgenden Clients verwenden, um eine Verbindung mit einer Sitzung herzustellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](./connect-windows-7-10.md)
- [Herstellen einer Verbindung mit dem Webclient](./connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](./connect-android.md)
- [Herstellen einer Verbindung mit dem macOS-Client](./connect-macos.md)
- [Herstellen einer Verbindung mit dem iOS-Client](./connect-ios.md)
