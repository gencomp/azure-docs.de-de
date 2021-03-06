---
title: Erstellen einer FCI mit freigegebenen Azure-Datenträgern (Vorschau)
description: Verwenden Sie freigegebene Azure-Datenträger, um eine Failoverclusterinstanz (FCI) mit SQL Server in Azure Virtual Machines zu erstellen.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2020
ms.author: mathoma
ms.openlocfilehash: 4e704a25e0c9700afbe4fa85031d7ff4d6a8d0c1
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85965357"
---
# <a name="create-an-fci-with-azure-shared-disks-sql-server-on-azure-vms"></a>Erstellen einer FCI mit freigegebenen Azure-Datenträgern (SQL Server auf Azure-VMs)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

In diesem Artikel wird erläutert, wie Sie auf virtuellen Azure-Computern (Azure Virtual Machines) eine SQL Server-Failoverclusterinstanz (FCI) mit SQL Server mithilfe freigegebener Azure-Datenträger erstellen. 

Weitere Informationen finden Sie in der Übersicht zu [FCI mit SQL Server auf Azure-VMs](failover-cluster-instance-overview.md) und über [Bewährte Methoden für Cluster](hadr-cluster-best-practices.md). 


## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie die in diesem Artikel aufgeführten Anweisungen ausführen, sollten Sie über Folgendes verfügen:

- Ein Azure-Abonnement. [Kostenlos](https://azure.microsoft.com/free/) einsteigen. 
- [Mindestens zwei für „USA, Westen-Mitte“ vorbereitete virtuelle Windows Azure-Computer](failover-cluster-instance-prepare-vm.md) in derselben [Verfügbarkeitsgruppe](../../../virtual-machines/linux/tutorial-availability-sets.md) und eine [Näherungsplatzierungsgruppe](../../../virtual-machines/windows/co-location.md#proximity-placement-groups), wobei die Verfügbarkeitsgruppe, die mit Fehlerdomäne und Updatedomäne erstellt wird, auf **1** festgelegt ist. 
- Ein Konto mit Berechtigungen zum Erstellen von Objekten auf virtuellen Azure-Computern und in Active Directory
- Die neueste Version von [PowerShell](/powershell/azure/install-az-ps?view=azps-4.2.0). 


## <a name="add-azure-shared-disk"></a>Hinzufügen eines freigegebenen Azure-Datenträgers
Stellen Sie einen verwalteten SSD Premium-Datenträger mit aktivierter Funktion für freigegebene Datenträger bereit. Legen Sie `maxShares` auf **2** fest, damit der Datenträger für beide FCI-Knoten freigegeben werden kann. 

Fügen Sie einen freigegebenen Azure-Datenträger hinzu, indem Sie folgendermaßen vorgehen: 


1. Speichern Sie das folgende Skript als *SharedDiskConfig.json*: 

   ```JSON
   { 
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "dataDiskName": {
         "type": "string",
         "defaultValue": "mySharedDisk"
       },
       "dataDiskSizeGB": {
         "type": "int",
         "defaultValue": 1024
       },
       "maxShares": {
         "type": "int",
         "defaultValue": 2
       }
     },
     "resources": [
       {
         "type": "Microsoft.Compute/disks",
         "name": "[parameters('dataDiskName')]",
         "location": "[resourceGroup().location]",
         "apiVersion": "2019-07-01",
         "sku": {
           "name": "Premium_LRS"
         },
         "properties": {
           "creationData": {
             "createOption": "Empty"
           },
           "diskSizeGB": "[parameters('dataDiskSizeGB')]",
           "maxShares": "[parameters('maxShares')]"
         }
       }
     ] 
   }
   ```


2. Führen Sie *SharedDiskConfig.json* mithilfe von PowerShell aus: 

   ```powershell
   $rgName = < specify your resource group name>
       $location = 'westcentralus'
       New-AzResourceGroupDeployment -ResourceGroupName $rgName `
   -TemplateFile "SharedDiskConfig.json"
   ```

3. Initialisieren Sie für jeden virtuellen Computer die angefügten freigegebenen Datenträger als GUID-Partitionstabelle (GPT), und formatieren Sie diese als NTFS (New Technology File System), indem Sie den folgenden Befehl ausführen: 

   ```powershell
   $resourceGroup = "<your resource group name>"
       $location = "<region of your shared disk>"
       $ppgName = "<your proximity placement groups name>"
       $vm = Get-AzVM -ResourceGroupName "<your resource group name>" `
           -Name "<your VM node name>"
       $dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup `
           -DiskName "<your shared disk name>"
       $vm = Add-AzVMDataDisk -VM $vm -Name "<your shared disk name>" `
           -CreateOption Attach -ManagedDiskId $dataDisk.Id `
           -Lun <available LUN  check disk setting of the VM>
    update-AzVm -VM $vm -ResourceGroupName $resourceGroup
   ```


## <a name="create-failover-cluster"></a>Erstellen des Failoverclusters

Für das Erstellen des Failoverclusters benötigen Sie Folgendes:

- Die Namen der virtuellen Computer, die zu Clusterknoten werden.
- Einen Namen für den Failovercluster.
- Eine IP-Adresse für den Failovercluster. Sie können eine IP-Adresse verwenden, die nicht in demselben virtuellen Azure-Netzwerk und Subnetz wie die Clusterknoten genutzt wird.


# <a name="windows-server-2012-2016"></a>[Windows Server 2012 bis 2016](#tab/windows2012)

Mit dem folgenden PowerShell-Skript wird ein Failovercluster erstellt. Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem virtuellen Azure-Netzwerk.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

# <a name="windows-server-2019"></a>[Windows Server 2019](#tab/windows2019)

Mit dem folgenden PowerShell-Skript wird ein Failovercluster erstellt. Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem virtuellen Azure-Netzwerk.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```

Weitere Informationen finden Sie unter [Failovercluster: Cluster Network Object](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).

---


## <a name="configure-quorum"></a>Konfigurieren des Quorums

Konfigurieren Sie die Quorumlösung, die Ihren Geschäftsanforderungen am besten entspricht. Sie können einen [Datenträgerzeugen](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum), einen [Cloudzeugen](/windows-server/failover-clustering/deploy-cloud-witness) oder einen [Dateifreigabezeugen](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum) konfigurieren. Weitere Informationen finden Sie unter [Quorum mit SQL Server-VMs](hadr-cluster-best-practices.md#quorum). 

## <a name="validate-cluster"></a>Validieren des Clusters
Führen Sie die Validierung für den Cluster auf der Benutzeroberfläche oder mit PowerShell durch.

Um den Cluster über die Benutzeroberfläche zu validieren, gehen Sie auf einem der beiden virtuellen Computer folgendermaßen vor:

1. Wählen Sie unter **Server-Manager** die Option **Tools** aus, und wählen Sie dann **Failovercluster-Manager** aus.
1. Wählen Sie unter **Failovercluster-Manager** die Option **Aktion** aus, und wählen Sie dann **Konfiguration überprüfen** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie unter **Server oder Cluster auswählen** die Namen der beiden virtuellen Computer ein.
1. Wählen Sie unter **Testoptionen** die Option **Nur ausgewählte Tests ausführen** aus. 
1. Wählen Sie **Weiter** aus.
1. Wählen Sie unter **Testauswahl** Alle Tests aus, *ausgenommen* **Direkte Speicherplätze**.

## <a name="test-cluster-failover"></a>Testen des Failovers des Clusters

Testen Sie das Failover Ihres Clusters. Klicken Sie im **Failovercluster-Manager** mit der rechten Maustaste auf den Cluster, und wählen Sie **Weitere Aktionen** > **Hauptclusterressource verschieben** > **Knoten auswählen** und dann den anderen Knoten des Clusters aus. Verschieben Sie die Hauptclusterressource auf alle Knoten des Clusters und dann zurück auf den primären Knoten. Wenn Sie den Cluster erfolgreich auf jeden Knoten verschieben können, können Sie SQL Server installieren.  

:::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/test-cluster-failover.png" alt-text="Testen des Clusterfailovers durch Verschieben der Hauptressource auf die anderen Knoten":::

## <a name="create-sql-server-fci"></a>Erstellen der SQL Server-FCI

Nachdem Sie den Failovercluster und alle Clusterkomponenten einschließlich Speicher konfiguriert haben, können Sie die SQL Server-FCI erstellen.

1. Stellen Sie mithilfe von RDP (Remotedesktopprotokoll) eine Verbindung mit dem ersten virtuellen Computer her.

1. Stellen Sie im **Failovercluster-Manager** sicher, dass sich alle Kernressourcen des Clusters auf dem ersten virtuellen Computer befinden. Verschieben Sie alle Ressourcen auf diesen virtuellen Computer, falls dies erforderlich ist.

1. Suchen Sie nach den Installationsmedien. Wenn für den virtuellen Computer eines der Azure Marketplace-Images verwendet wird, befinden sich die Medien unter `C:\SQLServer_<version number>_Full`. 

1. Wählen Sie **Setup** aus.

1. Wählen Sie im **SQL Server-Installationscenter** die Option **Installation** aus.

1. Wählen Sie **Neue SQL Server-Failoverclusterinstallation** aus. Befolgen Sie im Assistenten die Anleitung zum Installieren der SQL Server-FCI.

   Die FCI-Datenverzeichnisse müssen sich in gruppiertem Speicher befinden. Bei „Direkte Speicherplätze“handelt es sich nicht um einen freigegebenen Datenträger, sondern um einen Bereitstellungspunkt für ein Volume auf jedem Server. „Direkte Speicherplätze“ synchronisiert das Volume zwischen beiden Knoten. Das Volume wird dem Cluster als freigegebenes Clustervolume (Cluster Shared Volume, CSV) angezeigt. Verwenden Sie den CSV-Bereitstellungspunkt für die Datenverzeichnisse.

   ![Datenverzeichnisse](./media/failover-cluster-instance-storage-spaces-direct-manually-configure/20-data-dicrectories.png)

1. Nach Durchführung der Anweisungen im Assistenten wird vom Setup eine SQL Server-FCI auf dem ersten Knoten installiert.

1. Nachdem das Setup die FCI auf dem ersten Knoten installiert hat, können Sie per RDP eine Verbindung mit dem zweiten Knoten herstellen.

1. Öffnen Sie das **SQL Server-Installationscenter**, und wählen Sie **Installation** aus.

1. Wählen Sie **Knoten einem SQL Server-Failovercluster hinzufügen** aus. Befolgen Sie die Anweisungen im Assistenten, um SQL Server zu installieren und die Serverinstanz der FCI hinzuzufügen.

   >[!NOTE]
   >Wenn Sie ein Azure Marketplace-Katalogimage, das SQL Server enthält, verwendet haben, sind die SQL Server-Tools im Image enthalten. Wenn Sie keines dieser Images verwendet haben, müssen Sie die SQL Server-Tools separat installieren. Weitere Informationen finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).
   >

## <a name="register-with-the-sql-vm-rp"></a>Registrieren beim SQL-VM-RP

Um Ihre SQL Server-VM über das Portal zu verwalten, registrieren Sie sie beim SQL-VM-Ressourcenanbieter (RP) im [Verwaltungsmodus „Lightweight“](sql-vm-resource-provider-register.md#lightweight-management-mode), der derzeit als einziger Modus mit FCI und SQL Server auf Azure-VMs unterstützt wird. 


Registrieren einer SQL Server-VM im Modus „Lightweight“ mit PowerShell:  

```powershell-interactive
# Get the existing compute VM
$vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
         
# Register SQL VM with 'Lightweight' SQL IaaS agent
New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
   -LicenseType PAYG -SqlManagementType LightWeight  
```

## <a name="configure-connectivity"></a>Konfigurieren von Konnektivität 

Um Datenverkehr ordnungsgemäß an den aktuellen primären Knoten zu leiten, konfigurieren Sie die für Ihre Umgebung geeignete Konnektivitätsoption. Sie können einen [Azure-Lastenausgleich](hadr-vnn-azure-load-balancer-configure.md) erstellen oder bei Verwendung von SQL Server 2019 und Windows Server 2019 stattdessen eine Vorschau des Features für [verteilte Netzwerknamen](hadr-distributed-network-name-dnn-configure.md) anzeigen. 

## <a name="limitations"></a>Einschränkungen

- Nur SQL Server 2019 unter Windows Server 2019 wird unterstützt. 
- Nur die Registrierung beim SQL-VM-Ressourcenanbieter im [Verwaltungsmodus „Lightweight“](sql-vm-resource-provider-register.md#management-modes) wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Wenn dies noch nicht geschehen ist, konfigurieren Sie die Konnektivität mit Ihrer FCI mit einem [virtuellen Netzwerknamen und einem Azure-Lastenausgleich](hadr-vnn-azure-load-balancer-configure.md) oder einem [verteilten Netzwerknamen (DNN)](hadr-distributed-network-name-dnn-configure.md). 

Wenn freigegebene Azure-Datenträger nicht die richtige FCI-Speicherlösung für Sie sind, können Sie Ihre FCI stattdessen mithilfe von [Premium-Dateifreigaben](failover-cluster-instance-premium-file-share-manually-configure.md) oder [Direkte Speicherplätze](failover-cluster-instance-storage-spaces-direct-manually-configure.md) erstellen. 

Weitere Informationen finden Sie in der Übersicht zu [FCI mit SQL Server auf Azure-VMs](failover-cluster-instance-overview.md) und unter [Bewährte Methoden für die Clusterkonfiguration](hadr-cluster-best-practices.md).

Weitere Informationen finden Sie unter: 
- [Windows-Clustertechnologie](/windows-server/failover-clustering/failover-clustering-overview)   
- [SQL Server-Failoverclusterinstanzen](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
