---
title: 'Tutorial: Verwalten von Computeressourcen mit Azure Functions'
description: Sie erfahren, wie Sie Azure Functions zum Verwalten der Computevorgänge in Ihrem SQL-Pool in Azure Synapse Analytics verwenden.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/27/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 9d680283250cc323c833f388f6b20d7fe6fa132d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85211050"
---
# <a name="use-azure-functions-to-manage-compute-resources-in-azure-synapse-analytics-sql-pool"></a>Verwalten von Computeressourcen mit Azure Functions im SQL-Pool von Azure Synapse Analytics

In diesem Tutorial werden Computeressourcen für einen SQL-Pool mit Azure Functions in Azure Synapse Analytics verwaltet.

Zum Nutzen einer Azure-Funktions-App mit SQL-Pool müssen Sie ein [Dienstprinzipalkonto](../../active-directory/develop/howto-create-service-principal-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) mit Zugriff vom Typ „Mitwirkender“ unter demselben Abonnement wie Ihre SQL-Pool-Instanz erstellen.

## <a name="deploy-timer-based-scaling-with-an-azure-resource-manager-template"></a>Bereitstellen einer zeitgeberbasierten Skalierungsfunktion mit einer Azure Resource Manager-Vorlage

Sie benötigen die folgenden Informationen, um die Vorlage bereitzustellen:

- Name der Ressourcengruppe, in der sich Ihre SQL-Pool-Instanz befindet
- Name des Servers, auf dem sich Ihre SQL-Pool-Instanz befindet
- Name Ihrer SQL-Pool-Instanz
- Mandanten-ID (Verzeichnis-ID) Ihrer Azure Active Directory-Instanz
- Abonnement-ID
- Dienstprinzipal-Anwendungs-ID
- Geheimer Schlüssel des Dienstprinzipals

Stellen Sie diese Vorlage bereit, nachdem Sie die zuvor angegebenen Informationen ermittelt haben:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Nach dem Bereitstellen der Vorlage sollten drei neue Ressourcen vorhanden sein: ein kostenloser Azure App Service-Plan, ein nutzungsbasierter Funktionen-App-Plan und ein Speicherkonto für die Verarbeitung der Protokollierung und der Warteschlange für die Vorgänge. Fahren Sie mit dem Lesen der anderen Abschnitte fort, um zu erfahren, wie Sie die bereitgestellten Funktionen an Ihre Anforderungen anpassen.

## <a name="change-the-compute-level"></a>Ändern der Computeebene

1. Navigieren Sie zu Ihrem Funktionen-App-Dienst. Wenn Sie die Vorlage mit den Standardwerten bereitgestellt haben, sollte dieser Dienst den Namen *DWOperations* haben. Nach dem Öffnen Ihrer Funktionen-App sollte zu sehen sein, dass für Ihren Funktionen-App-Dienst fünf Funktionen bereitgestellt wurden.

   ![Funktionen, die mit der Vorlage bereitgestellt werden](./media/manage-compute-with-azure-functions/five-functions.png)

2. Wählen Sie in Abhängigkeit davon, ob Sie die Zeit für das Hoch- oder Herunterskalieren ändern möchten, die Option *DWScaleDownTrigger* oder *DWScaleUpTrigger*. Wählen Sie im Dropdownmenü „Integrieren“ aus.

   ![Auswählen von „Integrieren“ als Funktion](./media/manage-compute-with-azure-functions/select-integrate.png)

3. Derzeit sollte der angezeigte Wert *%ScaleDownTime%* oder *%ScaleUpTime%* lauten. Diese Werte geben an, dass der Zeitplan auf Werten basiert, die in Ihren [Anwendungseinstellungen](../../azure-functions/functions-how-to-use-azure-function-app-settings.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) definiert sind. Sie können diesen Wert vorerst ignorieren und den Zeitplan basierend auf den nächsten Schritten auf Ihre bevorzugte Zeit festlegen.

4. Fügen Sie im Zeitplanbereich die Zeit per CRON-Ausdruck hinzu, um anzugeben, wie oft für SQL Data Warehouse das zentrale Hochskalieren durchgeführt werden soll.

   ![Ändern des Funktionszeitplans](./media/manage-compute-with-azure-functions/change-schedule.png)

   Der Wert von `schedule` ist ein [CRON-Ausdruck](https://en.wikipedia.org/wiki/Cron#CRON_expression) mit diesen sechs Feldern:

   ```json
   {second} {minute} {hour} {day} {month} {day-of-week}
   ```

   *"0 30 9 * * 1-5"* steht beispielsweise für eine Auslösung an jedem Werktag um 9:30 Uhr. Weitere Informationen finden Sie im Artikel mit den [Zeitplanbeispielen](../../azure-functions/functions-bindings-timer.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#example) für Azure Functions.

## <a name="change-the-time-of-the-scale-operation"></a>Ändern des Zeitpunkts des Skalierungsvorgangs

1. Navigieren Sie zu Ihrem Funktionen-App-Dienst. Wenn Sie die Vorlage mit den Standardwerten bereitgestellt haben, sollte dieser Dienst den Namen *DWOperations* haben. Nach dem Öffnen Ihrer Funktionen-App sollte zu sehen sein, dass für Ihren Funktionen-App-Dienst fünf Funktionen bereitgestellt wurden.

2. Wählen Sie in Abhängigkeit davon, ob Sie den Computewert für das Hoch- oder Herunterskalieren ändern möchten, die Option *DWScaleDownTrigger* oder *DWScaleUpTrigger*. Nach dem Auswählen der Funktionen sollte in Ihrem Bereich die Datei *index.js* angezeigt werden.

   ![Ändern der Funktionstrigger-Computeebene](././media/manage-compute-with-azure-functions/index-js.png)

3. Ändern Sie den Wert von *ServiceLevelObjective* in die gewünschte Ebene, und wählen Sie „Speichern“. Dieser Wert ist die Computeebene, auf die Ihre Data Warehouse-Instanz basierend auf dem im Abschnitt „Integrieren“ definierten Zeitplan skaliert wird.

## <a name="use-pause-or-resume-instead-of-scale"></a>Verwenden des Anhaltens/Fortsetzens anstelle der Skalierung

Derzeit sind standardmäßig die Funktionen *DWScaleDownTrigger* und *DWScaleUpTrigger* aktiviert. Wenn Sie stattdessen die Funktion zum Anhalten und Fortsetzen verwenden möchten, können Sie *DWPauseTrigger* oder *DWResumeTrigger* aktivieren.

1. Navigieren Sie zum Functions-Bereich.

   ![Functions-Bereich](./media/manage-compute-with-azure-functions/functions-pane.png)

2. Klicken Sie jeweils auf die Umschaltfläche der entsprechenden Trigger, die Sie aktivieren möchten.

3. Navigieren Sie für die Trigger jeweils zur Registerkarte *Integrieren*, um den Zeitplan zu ändern.

   > [!NOTE]
   > Die funktionelle Unterschied zwischen den Skalierungstriggern und den Triggern zum Anhalten/Fortsetzen ist die Nachricht, die an die Warteschlange gesendet wird. Weitere Informationen finden Sie unter [Hinzufügen einer neuen Triggerfunktion](manage-compute-with-azure-functions.md#add-a-new-trigger-function).

## <a name="add-a-new-trigger-function"></a>Hinzufügen einer neuen Triggerfunktion

Derzeit sind nur zwei Skalierungsfunktionen in der Vorlage enthalten. Mit diesen Funktionen können Sie im Laufe eines Tags nur einmal herunter- und hochskalieren. Sie müssen einen weiteren Trigger hinzufügen, um eine präzisere Steuerung zu ermöglichen, z.B. mehrfaches zentrales Herunterskalieren an einem Tag oder Nutzung von unterschiedlichen Skalierungsverhalten am Wochenende.

1. Erstellen Sie eine neue leere Funktion. Wählen Sie die Schaltfläche *+* neben „Functions“, um den Bereich mit der Funktionsvorlage anzuzeigen.

   ![Erstellen einer neuen Funktion](./media/manage-compute-with-azure-functions/create-new-function.png)

2. Wählen Sie als Sprache *JavaScript* und dann *TimerTrigger* aus.

   ![Erstellen einer neuen Funktion](./media/manage-compute-with-azure-functions/timertrigger-js.png)

3. Benennen Sie die Funktion, und legen Sie Ihren Zeitplan fest. In der Abbildung ist dargestellt, wie Sie die Funktion jeden Samstag um Mitternacht (also in der Nacht von Freitag auf Samstag) auslösen können.

   ![Zentrales Herunterskalieren am Samstag](./media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Kopieren Sie den Inhalt von *index.js* aus einer der Triggerfunktionen.

   ![Kopieren von „index.js“](././media/manage-compute-with-azure-functions/index-js.png)

5. Legen Sie Ihre Vorgangsvariable wie folgt auf das gewünschte Verhalten fest:

   ```JavaScript
   // Resume the SQL pool instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the SQL pool instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the SQL pool instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```

## <a name="complex-scheduling"></a>Komplexe Zeitpläne

In diesem Abschnitt wird kurz demonstriert, wie Sie eine komplexere Zeitplanung für das Anhalten, Fortsetzen und Skalieren erreichen.

### <a name="example-1"></a>Beispiel 1

Tägliches Hochskalieren um 8:00 Uhr auf DW600 und Herunterskalieren um 20:00 Uhr auf DW200

| Funktion  | Zeitplan     | Vorgang                                |
| :-------- | :----------- | :--------------------------------------- |
| Function1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW600"}` |
| Function2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-2"></a>Beispiel 2

Tägliches Hochskalieren um 8:00 Uhr auf DW1000, Herunterskalieren auf DW600 um 16:00 Uhr und auf DW200 um 22:00 Uhr

| Funktion  | Zeitplan     | Vorgang                                |
| :-------- | :----------- | :--------------------------------------- |
| Function1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| Function2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Function3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-3"></a>Beispiel 3

Hochskalieren um 8:00 Uhr auf DW1000, Herunterskalieren auf DW600 um 16:00 Uhr an Wochentagen; Anhalten am Freitag um 23:00 Uhr, Fortsetzen am Montag um 7:00 Uhr

| Funktion  | Zeitplan       | Vorgang                                |
| :-------- | :------------- | :--------------------------------------- |
| Function1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| Function2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Function3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| Function4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Trigger mit Timer](../../azure-functions/functions-create-scheduled-function.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) für Azure-Funktionen.

Sehen Sie sich das [Beispielrepository](https://github.com/Microsoft/sql-data-warehouse-samples) für den SQL-Pool an.
