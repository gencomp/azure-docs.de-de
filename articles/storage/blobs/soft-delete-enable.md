---
title: Aktivieren und Verwalten des vorläufigen Löschens für Blobs
titleSuffix: Azure Storage
description: Aktivieren Sie vorläufiges Löschen für Blobobjekte, um Ihre Daten leichter wiederherstellen zu können, wenn sie irrtümlich geändert oder gelöscht wurden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/15/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 2914dfed14360c114476025c74f3dc0c03d82e25
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224892"
---
# <a name="enable-and-manage-soft-delete-for-blobs"></a>Aktivieren und Verwalten des vorläufigen Löschens für Blobs

Vorläufiges Löschen schützt Blobdaten vor versehentlichem oder irrtümlichem Ändern oder Löschen. Wenn vorläufiges Löschen für ein Speicherkonto aktiviert ist, können Blobs, Blobversionen (Vorschau) und Momentaufnahmen in diesem Speicherkonto innerhalb eines von Ihnen angegebenen Beibehaltungszeitraums wiederhergestellt werden, nachdem sie gelöscht wurden.

Wenn die Möglichkeit besteht, dass Ihre Daten von einer Anwendung oder einem anderen Benutzer des Speicherkontos versehentlich geändert oder gelöscht werden, empfiehlt Microsoft, vorläufiges Löschen zu aktivieren.

Dieser Artikel zeigt die ersten Schritte mit dem vorläufigen Löschen.

## <a name="enable-soft-delete"></a>Aktivieren von „Vorläufiges Löschen“

# <a name="portal"></a>[Portal](#tab/azure-portal)

Aktivieren Sie das vorläufige Löschen für Blobs in Ihrem Speicherkonto über das Azure-Portal:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Speicherkonto aus. 

2. Navigieren Sie unter **Blob-Dienst** zur Option **Datenschutz**.

3. Klicken Sie unter **Vorläufiges Löschen von Blobs** auf **Aktiviert**.

4. Geben Sie unter **Aufbewahrungsrichtlinien** die Anzahl der Tage für die *Aufbewahrung* ein.

5. Wählen Sie die Schaltfläche **Speichern** aus, um Ihre Datenschutzeinstellungen zu bestätigen.

![Screenshot des Azure-Portals mit ausgewähltem Datenschutzblobdienst.](media/soft-delete-enable/storage-blob-soft-delete-portal-configuration.png)

Aktivieren Sie zum Anzeigen von vorläufig gelöschten Blobs das Kontrollkästchen **Gelöschte Blobs anzeigen**.

![Screenshot der Seite mit dem Datenschutzblobdienst mit hervorgehobener Option „Gelöschte Blobs anzeigen“.](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted.png)

Um vorläufig gelöschte Momentaufnahmen für ein bestimmtes Blob anzuzeigen, wählen Sie das Blob aus, und klicken Sie dann auf **Momentaufnahmen anzeigen**.

![Screenshot der Seite mit dem Datenschutzblobdienst mit hervorgehobener Option „Momentaufnahmen anzeigen“.](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Stellen Sie sicher, dass das Kontrollkästchen **Gelöschte Momentaufnahmen anzeigen** aktiviert ist.

![Screenshot der Seite „Momentaufnahmen anzeigen“ mit hervorgehobener Option „Gelöschte Blobs anzeigen“.](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Beachten Sie beim Klicken auf ein vorläufig gelöschtes Blob oder eine Momentaufnahme die neuen Blobeigenschaften. Sie geben an, wann das Objekt gelöscht wurde und wie viele Tage verbleiben, bis das Blob oder die Blobmomentaufnahme dauerhaft gelöscht wird. Wenn das vorläufig gelöschte Objekt keine Momentaufnahme ist, ist auch eine Option verfügbar, den Löschvorgang rückgängig zu machen.

![Screenshot der Details eines vorläufig gelöschten Objekts.](media/soft-delete-enable/storage-blob-soft-delete-portal-properties.png)

Denken Sie daran, dass bei der Wiederherstellung eines Blob auch alle zugehörige Momentaufnahmen wiederhergestellt werden. Um vorläufig gelöschte Momentaufnahmen für ein aktives Blob wiederherzustellen, klicken Sie auf das Blob, und wählen Sie dann **Alle Momentaufnahmen wiederherstellen** aus.

![Screenshot der Details eines vorläufig gelöschten Blobs.](media/soft-delete-enable/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Sobald Sie die Momentaufnahmen eines Blobs wiederhergestellt haben, können Sie auf **Höher stufen** klicken, um eine Momentaufnahme über das Stammblob zu kopieren und so das Blob in der Momentaufnahme wiederherzustellen.

![Screenshot der Seite „Momentaufnahmen anzeigen“ mit hervorgehobener Option „Höherstufen“.](media/soft-delete-enable/storage-blob-soft-delete-portal-promote-snapshot.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aktualisieren Sie zum Aktivieren des vorläufigen Löschens die Diensteigenschaften eines Blobclients. Im folgenden Beispiel wird das vorläufige Löschen für eine Teilmenge der Konten in einem Abonnement aktiviert:

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```

Mithilfe des folgenden Befehls können Sie überprüfen, ob vorläufiges Löschen aktiviert wurde:

```powershell
$MatchingAccounts | $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context | Select-Object -ExpandProperty DeleteRetentionPolicy
```

Rufen Sie zum Wiederherstellen von Blobs, die versehentlich gelöscht wurden, „Undelete“ für diese Blobs auf. Denken Sie daran, dass der Aufruf von **Undelete Blob** (für aktive und vorläufig gelöschte Blobs) alle zugeordneten vorläufig gelöschten Momentaufnahmen als aktiv wiederherstellt. Im folgende Beispiel wird „Undelete“ für alle vorläufig gelöschten und aktiven Blobs in einem Container aufgerufen:

```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Mithilfe des folgenden Befehls können Sie die aktuelle Aufbewahrungsrichtlinie für vorläufiges Löschen suchen:

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-CLI)

Aktualisieren Sie zum Aktivieren des vorläufigen Löschens die Diensteigenschaften eines Blobclients:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Verwenden Sie den folgenden Befehl, um sicherzustellen, dass das vorläufige Löschen aktiviert ist: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

# <a name="python"></a>[Python](#tab/python)

Aktualisieren Sie zum Aktivieren des vorläufigen Löschens die Diensteigenschaften eines Blobclients:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(
    account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(
    delete_retention_policy=DeleteRetentionPolicy(enabled=True, days=7))
```

# <a name="net-v12-sdk"></a>[.NET v12 SDK](#tab/dotnet)

Aktualisieren Sie zum Aktivieren des vorläufigen Löschens die Diensteigenschaften eines Blobclients:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_EnableSoftDelete":::

Rufen Sie zum Wiederherstellen von Blobs, die versehentlich gelöscht wurden, „Undelete“ für diese Blobs auf. Denken Sie daran, dass der Aufruf von **Undelete** (für aktive und vorläufig gelöschte Blobs) alle zugeordneten vorläufig gelöschten Momentaufnahmen als aktiv wiederherstellt. Im folgende Beispiel wird „Undelete“ für alle vorläufig gelöschten und aktiven Blobs in einem Container aufgerufen:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverDeletedBlobs":::

Um eine bestimmte Blobversion wiederherzustellen, rufen Sie zuerst „Undelete“ für ein Blob auf, und kopieren Sie dann die gewünschte Momentaufnahme über das Blob. Im folgenden Beispiel wird die zuletzt generierte Momentaufnahme eines Blockblobs wiederhergestellt:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverSpecificBlobVersion":::

# <a name="net-v11-sdk"></a>[.NET v11 SDK](#tab/dotnet11)

Aktualisieren Sie zum Aktivieren des vorläufigen Löschens die Diensteigenschaften eines Blobclients:

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Rufen Sie zum Wiederherstellen von Blobs, die versehentlich gelöscht wurden, „Undelete“ für diese Blobs auf. Denken Sie daran, dass der Aufruf von **Undelete** (für aktive und vorläufig gelöschte Blobs) alle zugeordneten vorläufig gelöschten Momentaufnahmen als aktiv wiederherstellt. Im folgende Beispiel wird „Undelete“ für alle vorläufig gelöschten und aktiven Blobs in einem Container aufgerufen:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Um eine bestimmte Blobversion wiederherzustellen, rufen Sie zuerst „Undelete“ für ein Blob auf, und kopieren Sie dann die gewünschte Momentaufnahme über das Blob. Im folgenden Beispiel wird die zuletzt generierte Momentaufnahme eines Blockblobs wiederhergestellt:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```  

---

## <a name="next-steps"></a>Nächste Schritte

- [Vorläufiges Löschen für Blobspeicher](soft-delete-overview.md)
- [Blobversionsverwaltung (Vorschau)](versioning-overview.md)