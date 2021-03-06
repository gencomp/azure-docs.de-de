---
title: Unterstützung der Hyper-V-Migration in Azure Migrate
description: Hier finden Sie Informationen zur Unterstützung der Hyper-V-Migration mit Azure Migrate.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 1ea7d139b3d3cc8c14e43ccfb7c233fcbe4c564c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122063"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Unterstützungsmatrix für die Hyper-V-Migration

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Migration virtueller Hyper-V-Computer mithilfe von [Azure Migrate: Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool). Informationen zur Bewertung von virtuellen Hyper-V-Computern bei der Migration zu Azure finden Sie in der [Unterstützungsmatrix für die Bewertung](migrate-support-matrix-hyper-v.md).

## <a name="migration-limitations"></a>Einschränkungen bei der Migration

Sie können bis zu 10 VMs gleichzeitig für die Replikation auswählen. Wenn Sie weitere Computer migrieren möchten, replizieren Sie sie in Gruppen von 10.


## <a name="hyper-v-host-requirements"></a>Anforderungen für Hyper-V-Hosts

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Bereitstellung**       | Der Hyper-V-Host kann eigenständig oder in einem Cluster bereitgestellt werden. <br/>Azure Migrate-Replikationssoftware (Hyper-V-Replikationsanbieter) ist auf den Hyper-V-Hosts installiert.|
| **Berechtigungen**           | Sie benötigen Administratorrechte auf dem Hyper-V-Host. |
| **Betriebssystem des Hosts** | Windows Server 2019, Windows Server 2016 oder Windows Server 2012 R2 |
| **Portzugriff** |  Ausgehende Verbindungen über HTTPS-Port 443 zum Senden von VM-Replikationsdaten.


## <a name="hyper-v-vms"></a>Virtuelle Hyper-V-Computer

| **Unterstützung**                  | **Details**               
| :----------------------------- | :------------------- |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](../virtual-machines/linux/endorsed-distros.md)-Betriebssysteme, die von Azure unterstützt werden |
**Windows Server 2003** | Bei virtuellen Computern mit Windows Server 2003 müssen Sie [vor der Migration die Hyper-V-Integrationsdienste](prepare-windows-server-2003-migration.md) installieren. | 
**Linux-VMs in Azure** | Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können.<br/><br/> Bei diesen Linux-Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> – Debian 7, 8. Bei anderen Betriebssystemen nehmen Sie die [erforderlichen Änderungen](prepare-for-migration.md#linux-machines) manuell vor.
| **Erforderliche Änderungen für Azure** | Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können. Nehmen Sie vor der Migration Anpassungen manuell vor. Die entsprechenden Artikel enthalten Anweisungen zur Vorgehensweise. |
| **Linux-Start**                 | Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.<br/> Wenn „/boot“ Teil der Stammpartition („/“) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und nicht auf andere Datenträger erstrecken. |
| **UEFI-Start**                  | Der migrierte virtuelle Computer in Azure wird automatisch in einen virtuellen Computer mit BIOS-Start konvertiert. Auf dem virtuellen Computer sollte nur Windows Server 2012 oder höher ausgeführt werden. Der Betriebssystemdatenträger sollte maximal fünf Partitionen aufweisen, und die Größe des Betriebssystemdatenträgers sollte weniger als 300 GB betragen.|
| **Datenträgergröße**                  | 2 TB für den Betriebssystemdatenträger, 4 TB für Datenträger|
| **Anzahl der Datenträger** | Maximal 16 Datenträger pro virtuellem Computer|
| **Verschlüsselte Datenträger/Volumes**    | Nicht unterstützt für die Migration.|
| **RDM-Datenträger/Pass-Through-Datenträger**      | Nicht unterstützt für die Migration.|
| **Freigegebener Datenträger** | Virtuelle Computer mit freigegebenen Datenträgern werden für die Migration nicht unterstützt.|
| **NFS**                        | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.|
| **ISCSI**                      | Virtuelle Computer mit iSCSI-Zielen werden für die Migration nicht unterstützt.
| **Zieldatenträger**                | Sie können nur mit verwalteten Datenträgern zu Azure-VMs migrieren. |
| **IPv6** | Wird nicht unterstützt.|
| **NIC-Teaming** | Wird nicht unterstützt.|
| **Azure Site Recovery** | Wenn der virtuelle Computer für die Replikation mit Azure Site Recovery aktiviert ist, können Sie keine Replikation mit der Azure Migrate-Servermigration durchführen.|
| **Ports** | Ausgehende Verbindungen über HTTPS-Port 443 zum Senden von VM-Replikationsdaten.|

### <a name="url-access-public-cloud"></a>URL-Zugriff (öffentliche Cloud)

Die Software des Replikationsanbieters auf den Hyper-V-Hosts benötigt Zugriff auf die folgenden URLs.

**URL** | **Details**
--- | ---
login.microsoftonline.com | Zugriffssteuerung und Identitätsverwaltung mit Active Directory
backup.windowsazure.com | Für die Übertragung und Koordinierung von Replikationsdaten
*.hypervrecoverymanager.windowsazure.com | Wird für die Replikationsverwaltung verwendet.
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten 
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
time.windows.com | Überprüft die Zeitsynchronisierung zwischen Systemzeit und globaler Zeit

### <a name="url-access-azure-government"></a>URL-Zugriff (Azure Government)

Die Software des Replikationsanbieters auf den Hyper-V-Hosts benötigt Zugriff auf die folgenden URLs.

**URL** | **Details**
--- | ---
login.microsoftonline.us | Zugriffssteuerung und Identitätsverwaltung mit Active Directory
backup.windowsazure.us | Für die Übertragung und Koordinierung von Replikationsdaten
*.hypervrecoverymanager.windowsazure.us | Wird für die Replikationsverwaltung verwendet.
*.blob.core.usgovcloudapi.net | Hochladen von Daten in Speicherkonten
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
time.nist.gov | Überprüft die Zeitsynchronisierung zwischen Systemzeit und globaler Zeit

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Alle lokalen VMs, die in Azure repliziert werden, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Größe des Betriebssystem-Datenträgers | Bis zu 2.048 GB. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 16. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Datenträgergröße | Bis zu 4.095 GB | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Freigegebene VHD | Wird nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Wird nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
BitLocker | Wird nicht unterstützt. | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen Computer aktivieren.
Name des virtuellen Computers | 1 bis 63 Zeichen.<br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt.<br/><br/> Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. |  Aktualisieren Sie den Wert in den Computereigenschaften in Site Recovery.
Herstellen einer Verbindung nach der Migration: Windows | So stellen Sie nach der Migration eine Verbindung mit Azure-VMs unter Windows her:<br/><br/> – Aktivieren Sie vor der Migration RDP auf dem lokalen virtuellen Computer. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/><br/> – Aktivieren Sie bei Site-to-Site-VPN-Zugriff das RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private Netzwerke und Domänennetzwerke**. Achten Sie außerdem darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](prepare-for-migration.md) |
Herstellen einer Verbindung nach der Migration: Linux | So stellen Sie nach der Migration per SSH eine Verbindung mit Azure-VMs her:<br/><br/> – Überprüfen Sie vor der Migration auf dem lokalen Computer, ob der Secure Shell-Dienst auf „Start“ festgelegt ist und ob die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/> – Lassen Sie nach der Migration auf der Azure-VM eingehende Verbindungen am SSH-Port für die Netzwerksicherheitsgruppen-Regeln auf der VM, für die ein Failover ausgeführt wurde, sowie für das Azure-Subnetz zu, mit dem die VM verbunden ist. Fügen Sie außerdem der VM eine öffentliche IP-Adresse hinzu. |  

## <a name="next-steps"></a>Nächste Schritte

[Migrieren von virtuellen Hyper-V-Computern](tutorial-migrate-hyper-v.md) für die Migration.
