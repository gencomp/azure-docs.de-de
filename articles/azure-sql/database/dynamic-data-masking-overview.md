---
title: Dynamische Datenmaskierung
description: Die dynamische Datenmaskierung schränkt bei Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics die Offenlegung vertraulicher Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 04/28/2020
tags: azure-synpase
ms.openlocfilehash: 4a6de0b2067cdc847b830fba7607be8cb8f265ab
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85981389"
---
# <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung 
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics unterstützen die dynamische Datenmaskierung. Die dynamische Datenmaskierung beschränkt die Offenlegung vertraulicher Daten, indem sie für nicht berechtigte Benutzer maskiert werden. 

Die dynamische Datenmaskierung hilft beim Verhindern des unbefugten Zugriffs auf sensible Daten, indem Kunden festlegen dürfen, welcher Anteil der sensiblen Daten mit minimalen Auswirkungen auf die Anwendungsschicht offengelegt wird. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, während die Daten in der Datenbank nicht geändert werden.

Ein Servicemitarbeiter in einem Callcenter kann Anrufer beispielsweise anhand mehrerer Ziffern ihrer Kreditkartennummer identifizieren, wobei diese Datenelemente dem Servicemitarbeiter jedoch nicht vollständig angezeigt werden sollen. Es kann eine Maskierungsregel definiert werden, mit der die Kreditkartennummer im Resultset einer Abfrage bis auf die letzten vier Ziffern ausgeblendet wird. In einem weiteren Beispiel kann eine entsprechende Datenmaske zum Schutz personenbezogener Daten definiert werden, damit ein Entwickler Produktionsumgebungen zu Problembehandlungszwecken abfragen kann, ohne gegen Vorschriften zu verstoßen.

## <a name="dynamic-data-masking-basics"></a>Grundlegendes zur dynamischen Datenmaskierung

Sie richten eine Richtlinie für die dynamische Datenmaskierung im Azure-Portal durch Auswählen des Blatts **Dynamische Datenmaskierung** unter **Sicherheit** im Konfigurationsbereich Ihrer SQL-Datenbank-Instanz ein. Dieses Feature kann nicht über das Portal für Azure Synapse oder SQL Managed Instance festgelegt werden (verwenden Sie stattdessen PowerShell oder die REST-API). Weitere Informationen finden Sie unter [Dynamic Data Masking](/sql/relational-databases/security/dynamic-data-masking).

### <a name="dynamic-data-masking-permissions"></a>Berechtigungen für die dynamische Datenmaskierung

Die dynamische Datenmaskierung kann von den Rollen „Azure SQL-Datenbank-Administrator“, „Serveradministrator“ oder [SQL-Sicherheits-Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) konfiguriert werden.

### <a name="dynamic-data-masking-policy"></a>Richtlinie für die dynamische Datenmaskierung

* **Von der Maskierung ausgeschlossene SQL-Benutzer:** Eine Gruppe von SQL-Benutzern oder Azure AD-Identitäten, die in den Ergebnissen von SQL-Abfragen Daten ohne Maskierung erhalten. Benutzer mit Administratorrechten sind immer von der Maskierung ausgeschlossen und bekommen Originaldaten ohne Maskierung angezeigt.
* **Maskierungsregeln:** Eine Gruppe von Regeln, die die zu maskierenden Felder und verwendete Maskierungsfunktion definieren. Mithilfe eines Datenbankschemanamens, Tabellennamens und Spaltennamens können die vorgesehenen Felder bestimmt werden.
* **Maskierungsfunktionen:** Eine Reihe von Methoden, die die Anzeige von Daten in verschiedenen Szenarios steuern.

| Maskierungsfunktion | Maskierungslogik |
| --- | --- |
| **Standard** |**Vollständige Maskierung anhand der Datentypen der festgelegten Felder**<br/><br/>• XXXX oder weniger X-Zeichen verwenden, wenn die Größe des Felds weniger als vier Zeichen für Zeichenfolgendatentypen (nchar, ntext, nvarchar) beträgt.<br/>• Für numerische Datentypen (bigint, bit, decimal, int, money, numeric, smallint, smallmoney, tinyint, float, real) einen Nullwert verwenden.<br/>• Für Datum/Uhrzeit-Datentypen (date, datetime2, datetime, datetimeoffset, smalldatetime, time) 01-01-1900 verwenden.<br/>• Für SQL-Varianten wird der Standardwert des aktuellen Typs verwendet.<br/>• Für XML wird das Dokument \<masked/> verwendet.<br/>• Für spezielle Datentypen (timestamp table, hierarchyid, GUID, binary, image, räumliche varbinary-Typen) einen leeren Wert verwenden. |
| **Kreditkarte** |**Maskierungsmethode, die die letzten vier Ziffern der festgelegten Felder anzeigt** und eine Konstantenzeichenfolge als Präfix in Form einer Kreditkarte hinzufügt.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-Mail** |**Maskierungsmethode, die den ersten Buchstaben und die Domäne durch „XXX.com“ ersetzt** und dafür eine Konstantenzeichenfolge als Präfix in Form einer E-Mail-Adresse verwendet.<br/><br/>aXX@XXXX.com |
| **Zufallszahl** |**Maskierungsmethode, die eine Zufallszahl** entsprechend den ausgewählten Grenzen und den tatsächlichen Datentypen generiert. Wenn die festgelegten Grenzen gleich sind, ist die Maskierungsfunktion eine konstante Zahl.<br/><br/>![Navigationsbereich](./media/dynamic-data-masking-overview/1_DDM_Random_number.png) |
| **Benutzerdefinierter Text** |**Maskierungsmethode, die die ersten und letzten Zeichen anzeigt** und in der Mitte eine benutzerdefinierte Auffüllzeichenfolge hinzufügt. Wenn die ursprüngliche Zeichenfolge kürzer als das verfügbar gemachte Präfix und Suffix ist, wird nur die Auffüllzeichenfolge verwendet. <br/>prefix[padding]suffix<br/><br/>![Navigationsbereich](./media/dynamic-data-masking-overview/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Empfohlene Felder für die Maskierung

Von der DDM-Empfehlungs-Engine werden bestimmte Felder Ihrer Datenbank als potenzielle Felder mit vertraulichen Daten angegeben, bei denen es sich um gute Kandidaten für die Maskierung handelt. Auf dem Blatt „Dynamische Datenmaskierung“ im Portal werden die empfohlenen Spalten für Ihre Datenbank angezeigt. Sie können einfach für eine oder mehrere Spalten auf **Maske hinzufügen** und dann auf **Speichern** klicken, um eine Maskierung auf diese Felder anzuwenden.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Einrichten der dynamischen Datenmaskierung für Ihre Datenbank mithilfe von PowerShell-Cmdlets

### <a name="data-masking-policies"></a>Datenmaskierungsrichtlinien

- [Get-AzSqlDatabaseDataMaskingPolicy](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseDataMaskingPolicy)
- [Set-AzSqlDatabaseDataMaskingPolicy](https://docs.microsoft.com/powershell/module/az.sql/Set-AzSqlDatabaseDataMaskingPolicy)

### <a name="data-masking-rules"></a>Datenmaskierungsregeln

- [Get-AzSqlDatabaseDataMaskingRule](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseDataMaskingRule)
- [New-AzSqlDatabaseDataMaskingRule](https://docs.microsoft.com/powershell/module/az.sql/New-AzSqlDatabaseDataMaskingRule)
- [Remove-AzSqlDatabaseDataMaskingRule](https://docs.microsoft.com/powershell/module/az.sql/Remove-AzSqlDatabaseDataMaskingRule)
- [Set-AzSqlDatabaseDataMaskingRule](https://docs.microsoft.com/powershell/module/az.sql/Set-AzSqlDatabaseDataMaskingRule)

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-rest-api"></a>Einrichten der dynamischen Datenmaskierung für Ihre Datenbank mithilfe der REST-API

Sie können die REST-API verwenden, um Datenmaskierungsrichtlinien und -regeln programmgesteuert zu verwalten. Die veröffentlichte REST-API unterstützt die folgenden Vorgänge:

### <a name="data-masking-policies"></a>Datenmaskierungsrichtlinien

- [Erstellen oder aktualisieren:](https://docs.microsoft.com/rest/api/sql/datamaskingpolicies/createorupdate) Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung der angegebenen Spalte.
- [Get](https://docs.microsoft.com/rest/api/sql/datamaskingpolicies/get) (Abrufen): Ruft eine Datenbank-Datenmaskierungsrichtlinie ab. 

### <a name="data-masking-rules"></a>Datenmaskierungsregeln

- [Erstellen oder aktualisieren:](https://docs.microsoft.com/rest/api/sql/datamaskingrules/createorupdate) Erstellt oder aktualisiert eine Datenbank-Datenmaskierungsregel.
- [Nach Datenbank auflisten](https://docs.microsoft.com/rest/api/sql/datamaskingrules/listbydatabase): Ruft eine Liste von Datenbank-Datenmaskierungsregeln ab.
