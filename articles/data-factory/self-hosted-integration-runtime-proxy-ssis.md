---
title: Konfigurieren einer selbstgehosteten Integration Runtime als Proxy für SSIS
description: Hier erfahren Sie, wie Sie eine selbstgehostete Integration Runtime als Proxy für eine Azure-SSIS Integration Runtime konfigurieren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 07/09/2020
ms.openlocfilehash: 1eac86e856840d5cb78313fb4d61751066d6886b
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86184003"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>Konfigurieren einer selbstgehosteten IR als Proxy für eine Azure-SSIS IR in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird beschrieben, wie Sie SSIS-Pakete (SQL Server Integration Services) in einer Azure-SSIS Integration Runtime (Azure-SSIS IR) in Azure Data Factory mit einer selbstgehosteten Integration Runtime (selbstgehostete IR) ausführen, die als Proxy konfiguriert wurde. 

Mit diesem Feature können Sie auf lokale Daten zugreifen, ohne [Ihre Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) zu müssen. Dieses Feature ist nützlich, wenn Ihr Unternehmensnetzwerk eine zu komplexe Konfiguration aufweist oder wenn eine Richtlinie zu restriktiv für Sie ist, um Ihre Azure-SSIS IR darin einfügen zu können.

Dieses Feature teilt SSIS-Datenflusstasks mit einer lokalen Datenquelle in zwei Stagingtasks auf: 
* Der erste Task, der in Ihrer selbstgehosteten IR ausgeführt wird, verschiebt zuerst Daten aus der lokalen Datenquelle in einen Stagingbereich in Ihrem Azure Blob Storage.
* Mit dem zweiten Task, der in Ihrer Azure-SSIS IR ausgeführt wird, werden dann Daten aus dem Stagingbereich in das vorgesehene Datenziel verschoben.

Weitere Vorteile und Fähigkeiten dieses Features ermöglichen es Ihnen, Ihre selbstgehostete IR in Regionen einzurichten, die noch nicht von einer Azure-SSIS IR unterstützt werden, und die öffentliche statische IP-Adresse Ihrer selbstgehosteten IR auf der Firewall Ihrer Datenquellen zuzulassen.

## <a name="prepare-the-self-hosted-ir"></a>Vorbereiten der selbstgehosteten IR

Wenn Sie dieses Feature verwenden möchten, erstellen Sie zuerst eine Data Factory und richten darin eine Azure-SSIS IR ein. Falls dies noch nicht geschehen ist, führen Sie die Anleitungen unter [Einrichten einer Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) aus.

Anschließend richten Sie Ihre selbstgehostete IR in derselben Data Factory ein, in der Ihre Azure-SSIS IR eingerichtet wurde. Informationen dazu finden Sie unter [Erstellen einer selbstgehosteten IR](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

Zum Schluss laden Sie die neueste Version der selbstgehosteten IR sowie die zusätzlichen Treiber und die Runtime auf Ihren lokalen Computer oder virtuellen Azure-Computer (VM) wie folgt herunter und installieren sie dort:
- Laden Sie die neueste Version der [selbstgehosteten IR](https://www.microsoft.com/download/details.aspx?id=39717) herunter, und installieren Sie sie.
- Wenn Sie OLE DB-Connectors (Object Linking and Embedding Database) in Ihren Paketen verwenden, laden Sie die relevanten OLE DB-Treiber herunter, und installieren Sie sie auf demselben Computer, auf dem Ihre selbstgehostete IR installiert wurde, falls dies noch nicht geschehen ist.  

  Wenn Sie die frühere Version des OLE DB-Treibers für SQL Server (SQL Server Native Client [SQLNCLI]) verwenden, [laden Sie die 64-Bit-Version herunter](https://www.microsoft.com/download/details.aspx?id=50402).  

  Wenn Sie die neueste Version des OLE DB-Treibers für SQL Server (MSOLEDBSQL) verwenden, [laden Sie die 64-Bit-Version herunter](https://www.microsoft.com/download/details.aspx?id=56730).  
  
  Wenn Sie OLE DB-Treiber für andere Datenbanksysteme wie PostgreSQL, MySQL, Oracle usw. verwenden, können Sie die 64-Bit-Version von deren Websites herunterladen.
- Falls dies noch nicht geschehen ist, [laden Sie die 64-Bit-Version von Visual C++ (VC) Runtime herunter, und installieren Sie sie](https://www.microsoft.com/download/details.aspx?id=40784) auf demselben Computer, auf dem Ihre selbstgehostete IR installiert wurde.

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging

Falls dies noch nicht geschehen ist, erstellen Sie einen mit Azure Blob Storage verknüpften Dienst in derselben Data Factory, in der Ihre Azure-SSIS IR eingerichtet wurde. Informationen dazu finden Sie unter [Erstellen eines mit Azure Data Factory verknüpften Diensts](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service). Führen Sie unbedingt die folgenden Schritte aus:
- Wählen Sie **Azure Blob Storage** als **Datenspeicher** aus.  
- Wählen Sie für **Connect via integration runtime** (Über Integration Runtime verbinden) die Option **AutoResolveIntegrationRuntime** (nicht Ihre Azure-SSIS IR oder Ihre selbstgehostete IR) aus, da Sie die Standard-Azure IR zum Abrufen von Zugriffsanmeldeinformationen für Ihre Azure Blob Storage-Instanz verwenden.
- Wählen Sie unter **Authentifizierungsmethode** eine der Optionen **Kontoschlüssel**, **SAS-URI** oder **Dienstprinzipal** aus.  

    >[!TIP]
    >Wenn Sie die Methode **Dienstprinzipal** auswählen, gewähren Sie dem Dienstprinzipal mindestens die Rolle *Mitwirkender an Storage-Blobdaten*. Weitere Informationen finden Sie unter  [Azure Blob Storage-Connector](connector-azure-blob-storage.md#linked-service-properties).

![Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>Konfigurieren einer Azure-SSIS IR mit Ihrer selbstgehosteten IR als Proxy

Nachdem Sie Ihre selbstgehostete IR und den mit Azure Blob Storage verknüpften Dienst für das Staging vorbereitet haben, können Sie jetzt Ihre neue oder vorhandene Azure-SSIS IR mit der selbstgehosteten IR als Proxy in Ihrem Data Factory-Portal bzw. in Ihrer App konfigurieren. Bevor Sie das tun, sollten Sie jedoch –falls Ihre vorhandene Azure-SSIS IR bereits ausgeführt wird – diese beenden und dann neu starten.

1. Überspringen Sie im Bereich **Integration Runtime-Setup** die Abschnitte **Allgemeine Einstellungen** und **SQL-Einstellungen**, indem Sie **Weiter** auswählen. 

1. Führen Sie im Abschnitt **Erweiterte Einstellungen** die folgenden Schritte aus:

   1. Aktivieren Sie das Kontrollkästchen **Selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS Integration Runtime**. 

   1. Wählen Sie in der Dropdownliste **Selbstgehostete Integration Runtime** Ihre vorhandene selbstgehostete IR als Proxy für die Azure-SSIS IR aus.

   1. Wählen Sie in der Dropdownliste **Staging storage linked service** (Verknüpfter Dienst des Stagingspeichers) Ihren vorhandenen mit Azure Blob Storage verknüpften Dienst aus, oder erstellen Sie einen neuen Dienst für den Stagingprozess.

   1. Geben Sie im Feld **Stagingpfad** einen Blobcontainer in Ihrem ausgewählten Azure Blob Storage-Konto an, oder lassen Sie das Feld leer, wenn Sie einen Standardpfad für den Stagingprozess verwenden möchten.

   1. Wählen Sie **Weiter**.

   ![Erweiterte Einstellungen bei einer selbstgehosteten IR](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

Sie können auch Ihre neue oder vorhandene Azure-SSIS IR mit der selbstgehosteten IR als Proxy mithilfe von PowerShell konfigurieren.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>Aktivieren von SSIS-Paketen für die Verbindungsherstellung über den Proxy

Wenn Sie entweder die neueste Erweiterung „SSDT bei SSIS-Projekten“ für Visual Studio oder ein eigenständiges Installationsprogramm verwenden, können Sie eine neue `ConnectByProxy`-Eigenschaft finden, die in OLE DB- oder Flatfile-Verbindungs-Managern hinzugefügt wurde.
* [Herunterladen der Erweiterung „SSDT bei SSIS-Projekten“ für Visual Studio](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Herunterladen des eigenständigen Installationsprogramms](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)   

Wenn Sie neue Pakete entwerfen, die Datenflusstasks mit OLE DB- oder Flatfilequellen enthalten, mit denen Sie auf lokale Datenbanken oder Dateien zugreifen können, können Sie diese Eigenschaft aktivieren, indem Sie sie im Bereich *Eigenschaften* der relevanten Verbindungs-Manager auf **True** festlegen.

![Aktivieren der ConnectByProxy-Eigenschaft](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Sie können diese Eigenschaft auch aktivieren, wenn Sie vorhandene Pakete ausführen, ohne sie manuell einzeln ändern zu müssen.  Es gibt zwei Optionen:
- **Option A**: Öffnen, Neuerstellen und erneutes Bereitstellen des Projekts, das diese Pakete enthält, mit den neuesten SSDT, die in Ihrer Azure-SSIS IR ausgeführt werden sollen. Sie können dann die Eigenschaft aktivieren, indem Sie sie für die relevanten Verbindungs-Manager auf *True* festlegen. Wenn Sie Pakete aus SSMS ausführen, werden diese Verbindungs-Manager im Popup Fenster **Paket ausführen** auf der Registerkarte **Verbindungs-Manager** angezeigt.

  ![Aktivieren der ConnectByProxy-Eigenschaft (2)](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Sie können die Eigenschaft auch aktivieren, indem Sie sie für die entsprechenden Verbindungs-Manager auf *True* festlegen, die auf der Registerkarte **Connection Managers** (Verbindungs-Manager) der [Aktivität „SSIS-Paket ausführen“](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) angezeigt werden, wenn Pakete in Data Factory-Pipelines ausgeführt werden.
  
  ![Aktivieren der ConnectByProxy-Eigenschaft (3)](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- **Option B:** Erneutes Bereitstellen des Projekts, das diese Pakete enthält, für die Ausführung in Ihrer SSIS IR. Sie können dann die Eigenschaft aktivieren, indem Sie deren Eigenschaftspfad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, angeben und dafür *True* als Eigenschaftsüberschreibung auf der Registerkarte **Advanced** (Erweitert) des Popupfensters **Execute Package** (Paket ausführen) festlegen, wenn Sie Pakete aus SSMS ausführen.

  ![Aktivieren der ConnectByProxy-Eigenschaft (4)](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Sie können die Eigenschaft auch aktivieren, indem Sie deren Eigenschaftspfad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, angeben und dafür *True* als Eigenschaftsüberschreibung auf der Registerkarte **Property Overrides** (Eigenschaftenüberschreibungen) der [Aktivität „SSIS-Paket ausführen“](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) festlegen, wenn Sie Pakete in Data Factory-Pipelines ausführen.
  
  ![Aktivieren der ConnectByProxy-Eigenschaft (5)](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>Debuggen der ersten und zweiten Stagingtasks

In Ihrer selbstgehosteten IR können Sie die Laufzeitprotokolle im Ordner *C:\ProgramData\SSISTelemetry* und die Ausführungsprotokolle der ersten Stagingtasks im Ordner *C:\ProgramData\SSISTelemetry\ExecutionLog* finden.  Sie können die Ausführungsprotokolle der zweiten Stagingtasks in Ihrer SSISDB oder in den angegebenen Protokollierungspfaden finden – abhängig davon, ob Sie Ihre Pakete in SSISDB oder im Dateisystem, auf Dateifreigaben oder in Azure Files speichern. Die eindeutigen IDs der ersten Stagingtasks können Sie auch in den Ausführungsprotokollen der zweiten Stagingtasks finden. 

![Eindeutige ID des ersten Stagingtasks](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="use-windows-authentication-in-staging-tasks"></a>Verwenden der Windows-Authentifizierung in Stagingtasks

Wenn für die Stagingtasks in Ihrer selbstgehosteten IR die Windows-Authentifizierung erforderlich ist, [konfigurieren Sie Ihre SSIS-Pakete für die Verwendung derselben Windows-Authentifizierung](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15). 

Ihre Stagingtasks werden mit dem Dienstkonto der selbstgehosteten IR (standardmäßig *NT SERVICE\DIAHostService*) aufgerufen, und der Zugriff auf Ihre Datenspeicher erfolgt über das Windows-Authentifizierungskonto. Beiden Konten müssen bestimmte Sicherheitsrichtlinien zugewiesen werden. Wechseln Sie auf dem Computer mit der selbstgehosteten IR zu **Lokale Sicherheitsrichtlinie** > **Lokale Richtlinien** > **Zuweisen von Benutzerrechten**, und führen Sie dann die folgenden Schritte aus:

1. Weisen Sie die Richtlinien *Arbeitsspeicherkontingente für einen Prozess anpassen* und *Token auf Prozessebene ersetzen* dem Dienstkonto der selbstgehosteten IR zu. Dies sollte automatisch erfolgen, wenn Sie Ihre selbstgehostete IR mit dem Standarddienstkonto installieren. Wenn dies nicht der Fall ist, weisen Sie diese Richtlinien manuell zu. Wenn Sie ein anderes Dienstkonto verwenden, müssen Sie ihm diese Richtlinien zuweisen.

1. Weisen Sie dem Windows-Authentifizierungskonto die Richtlinie *Als Dienst anmelden* zu.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>Abrechnung der ersten und zweiten Stagingtasks

Die ersten in Ihrer selbstgehosteten IR ausgeführten Stagingtasks werden separat abgerechnet, so wie alle Datenverschiebungsaktivitäten in Rechnung gestellt werden, die in einer selbstgehosteten IR ausgeführt werden. Dies wird im Artikel [Azure Data Factory data pipeline pricing](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) (Preise für Azure Data Factory-Datenpipeline) beschrieben.

Die zweiten in Ihrer selbstgehosteten IR ausgeführten Stagingtasks werden nicht separat abgerechnet, aber Ihre Ausführung von Azure-SSIS IR wird wie im Artikel [Preise für Azure-SSIS IR](https://azure.microsoft.com/pricing/details/data-factory/ssis/) beschrieben in Rechnung gestellt.

## <a name="enabling-tls-12"></a>Aktivieren von TLS 1.2

Wenn Sie starke Kryptografie und ein sichereres Netzwerkprotokoll (TLS 1.2) verwenden müssen und ältere SSL-/TLS-Versionen auf Ihrer selbstgehosteten IR deaktivieren, können Sie das Skript *main.cmd* aus dem Ordner *CustomSetupScript/UserScenarios/TLS 1.2* des öffentlichen Vorschaucontainers herunterladen und ausführen.  Wenn Sie [Azure Storage-Explorer](https://storageexplorer.com/) verwenden, können Sie eine Verbindung mit unserem öffentlichen Vorschaucontainer herstellen, indem Sie den folgenden SAS-URI eingeben:

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Derzeit werden nur Datenflussaufgaben mit ODBC- (Open Database Connectivity), OLE DB- oder Flatfilequellen oder mit OLE DB-Zielen unterstützt. 
- Derzeit werden nur mit Azure Blob Storage verknüpfte Dienste unterstützt, die mit einer der Authentifizierungen *Kontoschlüssel*, *Shared Access Signature (SAS)-URI* oder *Dienstprinzipal* konfiguriert wurden.
- *ParameterMapping* in der OLE DB-Quelle wird noch nicht unterstützt. Verwenden Sie zur Problemumgehung die Option *SQL-Befehl aus Variable* als *AccessMode*, und verwenden Sie die Option *Ausdruck*, um die Variablen/Parameter in einen SQL-Befehl einzufügen. Eine Veranschaulichung finden Sie im Paket *ParameterMappingSample.dtsx* im Ordner *SelfHostedIRProxy/Limitations* in unserem öffentlichen Vorschaucontainer. Wenn Sie Azure Storage-Explorer verwenden, können Sie eine Verbindung mit unserem öffentlichen Vorschaucontainer herstellen, indem Sie den obigen SAS-URI eingeben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre selbstgehostete IR als Proxy für Ihre Azure-SSIS IR konfiguriert haben, können Sie Ihre Pakete bereitstellen und ausführen, um in Form von Aktivitäten vom Typ „SSIS-Paket ausführen“ in Data Factory-Pipelines auf lokale Daten zuzugreifen. Weitere Informationen finden Sie unter [Ausführen von SSIS-Paketen als Aktivitäten vom Typ „SSIS-Paket ausführen“ in Data Factory-Pipelines](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
