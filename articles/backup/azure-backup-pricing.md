---
title: Preise für Azure Backup
description: Erfahren Sie, wie Sie mithilfe einer Preiskalkulation Ihre Kosten für Azure Backup abschätzen können.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 274a61ff5a98fa1291f9d8917af9ab1d1b3da2fd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391110"
---
# <a name="azure-backup-pricing"></a>Preise für Azure Backup

Informationen zu den Preisen für Azure Backup finden Sie auf der Seite [Azure Backup – Preise](https://azure.microsoft.com/pricing/details/backup/).

## <a name="download-detailed-estimates-for-azure-backup-pricing"></a>Herunterladen detaillierter Kostenschätzungen für Azure Backup

Wenn Sie Ihre Kosten zu Zwecken der Budgetierung oder des Kostenvergleichs schätzen möchten, laden Sie die detaillierte [Azure Backup-Kostenschätzung](https://aka.ms/AzureBackupCostEstimates) herunter.  

### <a name="what-does-the-estimator-contain"></a>Was enthält die Kostenschätzung?

Das Blatt zur Kostenschätzung von Azure Backup bietet Ihnen die Möglichkeit, die Kosten aller möglichen Workloads zu schätzen, die Sie mit Azure Backup sichern möchten. Beispiele für Workloads dieser Art sind:

- Virtuelle Azure-Computer
- Lokale Server
- SQL in Azure-VMs
- SAP HANA in Azure-VMs
- Azure-Dateifreigaben

## <a name="estimate-costs-for-backing-up-azure-vms-or-on-premises-servers"></a>Schätzen der Kosten für das Sichern von Azure-VMs oder lokalen Servern

Um die Kosten für die Sicherung von Azure-VMs oder lokalen Servern mit Azure Backup zu schätzen, benötigen Sie die folgenden Parameter:

- Größe der VMs oder lokalen Server, die Sie sichern möchten
  - Geben Sie die „verwendete Größe“ der zu sichernden Datenträger oder Server ein.

- Anzahl der Server dieser Größe

- Wie ist die erwartete Datenänderungsrate auf diesen Servern?<br>
  Churn refers to the amount of change in data. Wenn Sie z. B. eine VM mit 200 GB zu sichernden Daten haben, von denen sich 10 GB täglich ändern, beträgt die tägliche Änderungsrate 5 %.

  - Eine höhere Änderungsrate bedeutet, dass Sie mehr Daten sichern.

  - Wählen Sie für Dateiserver **Niedrig** oder **Mittel** und bei Einsatz von Datenbanken **Hoch**.

  - Wenn Sie Ihre **Änderungsrate( %)** kennen, können Sie **Ihre eigene Rate (%) eingeben**.

- Wählen der Sicherungsrichtlinie

  - Wie lange sollen „tägliche“ Sicherungen aufbewahrt werden? (in Tagen)

  - Wie lange sollen „wöchentliche“ Sicherungen aufbewahrt werden? (in Wochen)

  - Wie lange sollen „monatliche“ Sicherungen aufbewahrt werden? (in Monaten)

  - Wie lange sollen „jährliche“ Sicherungen aufbewahrt werden? (in Jahren)

  - Wie lange sollen „Momentaufnahmen für die sofortige Wiederherstellung“ aufbewahrt werden? (1-5 Tage)

    - Mit dieser Option können Sie auf Datenträgern gespeicherte Momentaufnahmen bis zu sieben Tage schnell rückwirkend wiederherstellen.

- **Optional**: selektive Datenträgersicherung

  - Wenn Sie beim Sichern von Azure-VMs die Option **Selektive Datenträgersicherung** nutzen, wählen Sie die Option **Datenträger ausschließen**, und geben Sie den Prozentsatz der von der Sicherung ausgeschlossenen Datenträger nach ihrer Größe ein. Wenn Sie beispielsweise eine VM an drei Datenträger mit jeweils 200 GB angeschlossen haben und zwei davon von der Sicherung ausschließen möchten, geben Sie 66,7 % ein.

- **Optional**: Redundanz für Sicherungsspeicher

  - Dies zeigt die Redundanz des Speicherkontos an, in dem Ihre Sicherungsdaten gespeichert werden. Für höchstmögliche Verfügbarkeit empfehlen wir die Verwendung von **GRS**. Da dadurch sichergestellt ist, dass eine Kopie Ihrer Sicherungsdaten in einer anderen Region aufbewahrt wird, können Sie mehrere Compliancestandards einfach einhalten. Ändern Sie die Redundanz in **LRS**, wenn Sie Entwicklungs- oder Testumgebungen sichern, die keine Sicherung auf Unternehmensebene benötigen. Wählen Sie auf dem Blatt die Option **RAGRS** aus, wenn Sie die Kosten verstehen möchten, die bei Aktivierung von [Regionsübergreifende Wiederherstellung](backup-azure-arm-restore-vms.md#cross-region-restore) für Ihre Sicherungen anfallen.

- **Optional**: Ändern regionaler Preise oder Anwenden ermäßigter Tarife

  - Wenn Sie Ihre Schätzungen für eine andere Region oder ermäßigte Tarife prüfen möchten, wählen Sie **Ja** für die Option **Schätzungen für eine andere Region versuchen?** . Geben Sie dann die Tarife ein, mit denen Sie die Schätzungen durchführen möchten.

## <a name="estimate-costs-for-backing-up-sql-servers-in-azure-vms"></a>Schätzen der Kosten für das Sichern von SQL Server-Instanzen in Azure-VMs

Um die Kosten für die Sicherung von in Azure-VMs ausgeführten SQL Server-Instanzen mit Azure Backup zu schätzen, benötigen Sie die folgenden Parameter:

- Größe der SQL Server-Instanzen, die Sie sichern möchten

- Anzahl der SQL Server-Instanzen dieser Größe

- Wie hoch ist die erwartete Komprimierung der Sicherungsdaten Ihrer SQL Server-Instanzen?

  - Die meisten Azure Backup-Kunden stellen fest, dass bei **Aktivierung** der SQL Server-Komprimierung die Sicherungsdaten im Vergleich zur Größe der SQL Server-Instanz um 80 % komprimiert werden.

  - Wenn Sie eine andere Komprimierung erwarten, geben Sie den Wert in dieses Feld ein.

- Was ist das erwartete Größe von Protokollsicherungen?

  - Der Prozentwert gibt die tägliche Protokollgröße in Prozent der Größe der SQL Server-Instanz an.

- Wie ist die zu erwartende tägliche Datenänderungsrate auf diesen Servern?

  - In der Regel weisen Datenbanken eine „hohe“ Änderungsrate auf.

  - Wenn Sie Ihre **Änderungsrate( %)** kennen, können Sie **Ihre eigene Rate (%) eingeben**.

- Wählen der Sicherungsrichtlinie

  - Sicherungstyp

    - Die effektivste Richtlinie, die Sie wählen können, ist **Tägliche differenzielle Sicherungen** mit wöchentlichen/monatlichen/jährlichen vollständigen Sicherungen. Azure Backup ermöglicht auch die Wiederherstellung differenzieller Sicherungen per Einzelklick.

    - Sie können sich auch für eine Richtlinie mit täglichen/wöchentlichen/monatlichen/jährlichen vollständigen Sicherungen entscheiden. Diese Option belegt etwas mehr Speicherplatz als die erste Option.

  - Wie lange sollen „Protokollsicherungen“ aufbewahrt werden? (in Tagen) [7-35]

  - Wie lange sollen „tägliche“ Sicherungen aufbewahrt werden? (in Tagen)

  - Wie lange sollen „wöchentliche“ Sicherungen aufbewahrt werden? (in Wochen)

  - Wie lange sollen „monatliche“ Sicherungen aufbewahrt werden? (in Monaten)

  - Wie lange sollen „jährliche“ Sicherungen aufbewahrt werden? (in Jahren)

- **Optional**: Redundanz für Sicherungsspeicher

  - Dies zeigt die Redundanz des Speicherkontos an, in dem Ihre Sicherungsdaten gespeichert werden. Für höchstmögliche Verfügbarkeit empfehlen wir die Verwendung von **GRS**. Da dadurch sichergestellt ist, dass eine Kopie Ihrer Sicherungsdaten in einer anderen Region aufbewahrt wird, können Sie mehrere Compliancestandards einfach einhalten. Ändern Sie die Redundanz in **LRS**, wenn Sie Entwicklungs- oder Testumgebungen sichern, die keine Sicherung auf Unternehmensebene benötigen.

- **Optional**: Ändern regionaler Preise oder Anwenden ermäßigter Tarife

  - Wenn Sie Ihre Schätzungen für eine andere Region oder ermäßigte Tarife prüfen möchten, wählen Sie **Ja** für die Option **Schätzungen für eine andere Region versuchen?** . Geben Sie dann die Tarife ein, mit denen Sie die Schätzungen durchführen möchten.

## <a name="estimate-costs-for-backing-up-sap-hana-servers-in-azure-vms"></a>Schätzen der Kosten für das Sichern von SAP HANA-Serverinstanzen in Azure-VMs

Um die Kosten für die Sicherung von in Azure-VMs ausgeführten SAP HANA-Serverinstanzen mit Azure Backup zu schätzen, benötigen Sie die folgenden Parameter:

- Gesamtgröße der SAP HANA-Datenbanken, die Sie sichern möchten. Dies entspricht der Summe der Größe der vollständigen Sicherungen der einzelnen Datenbanken, die von SAP HANA gemeldet wird.
- Anzahl der SAP HANA-Serverinstanzen dieser Größe
- Was ist das erwartete Größe von Protokollsicherungen?
  - Der Prozentwert gibt die durchschnittliche tägliche Protokollgröße als Prozentwert der Gesamtgröße der SAP HANA-Datenbanken an, die auf den SAP HANA-Serverinstanzen gesichert werden.
- Wie ist die zu erwartende tägliche Datenänderungsrate auf diesen Servern?
  - Der Prozentwert zeigt die durchschnittliche tägliche Änderungsrate als Prozentwert der Gesamtgröße der SAP HANA-Datenbanken an, die auf der SAP HANA-Serverinstanz gesichert werden.
  - In der Regel weisen Datenbanken eine „hohe“ Änderungsrate auf.
  - Wenn Sie Ihre **Änderungsrate( %)** kennen, können Sie **Ihre eigene Rate (%) eingeben**.
- Wählen der Sicherungsrichtlinie
  - Sicherungstyp
    - Die effektivste Richtlinie, die Sie wählen können, ist **Tägliche differenzielle Sicherungen** mit **wöchentlichen/monatlichen/jährlichen** vollständigen Sicherungen. Azure Backup ermöglicht auch die Wiederherstellung differenzieller Sicherungen per Einzelklick.
    - Sie können sich auch für eine Richtlinie mit **täglichen/wöchentlichen/monatlichen/jährlichen** vollständigen Sicherungen entscheiden. Diese Option belegt etwas mehr Speicherplatz als die erste Option.
  - Wie lange sollen „Protokollsicherungen“ aufbewahrt werden? (in Tagen) [7-35]
  - Wie lange sollen „tägliche“ Sicherungen aufbewahrt werden? (in Tagen)
  - Wie lange sollen „wöchentliche“ Sicherungen aufbewahrt werden? (in Wochen)
  - Wie lange sollen „monatliche“ Sicherungen aufbewahrt werden? (in Monaten)
  - Wie lange sollen „jährliche“ Sicherungen aufbewahrt werden? (in Jahren)
- **Optional**: Redundanz für Sicherungsspeicher
  - Dies zeigt die Redundanz des Speicherkontos an, in dem Ihre Sicherungsdaten gespeichert werden. Für höchstmögliche Verfügbarkeit empfehlen wir die Verwendung von **GRS**. Da dadurch sichergestellt ist, dass eine Kopie Ihrer Sicherungsdaten in einer anderen Region aufbewahrt wird, können Sie mehrere Compliancestandards einfach einhalten. Ändern Sie die Redundanz in **LRS**, wenn Sie Entwicklungs- oder Testumgebungen sichern, die keine Sicherung auf Unternehmensebene benötigen.
- **Optional**: Ändern regionaler Preise oder Anwenden ermäßigter Tarife
  - Wenn Sie Ihre Schätzungen für eine andere Region oder ermäßigte Tarife prüfen möchten, wählen Sie **Ja** für die Option **Schätzungen für eine andere Region versuchen?** . Geben Sie dann die Tarife ein, mit denen Sie die Schätzungen durchführen möchten.

## <a name="next-steps"></a>Nächste Schritte

[Worum handelt es sich beim Azure Backup-Dienst?](backup-overview.md)
