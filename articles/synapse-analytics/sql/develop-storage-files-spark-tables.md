---
title: Synchronisieren von Apache Spark für externe Azure Synapse-Tabellendefinitionen in SQL On-Demand (Vorschau)
description: Übersicht über das Abfragen von Spark-Tabellen mithilfe von SQL On-Demand (Vorschau)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: db6b2d95bf8f38495296885d14260b9900af1d51
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247046"
---
# <a name="synchronize-apache-spark-for-azure-synapse-external-table-definitions-in-sql-on-demand-preview"></a>Synchronisieren von Apache Spark für externe Azure Synapse-Tabellendefinitionen in SQL On-Demand (Vorschau)

SQL On-Demand (Vorschau) kann Metadaten aus Apache Spark für Azure Synapse-Pools automatisch synchronisieren. Für jede in Spark-Pools vorhandene Datenbank wird eine SQL On-Demand-Datenbank erstellt (Vorschau). 

Für jede externe Spark-Tabelle, die auf Parquet basiert und sich in Azure Storage befindet, wird in der SQL On-Demand-Datenbank eine externe Tabelle erstellt. Aus diesem Grund können Sie Ihre Spark-Pools herunterfahren und dennoch externe Spark-Tabellen über SQL On-Demand abfragen.

Wenn Tabelle in Spark partitioniert ist, werden die Dateien im Speicher in Ordner organisiert. SQL On-Demand verwendet Partitionsmetadaten und bezieht nur relevante Ordner und Dateien in Ihre Abfragen ein.

Die Synchronisierung von Metadaten wird für jeden im Azure Synapse-Arbeitsbereich bereitgestellten Spark-Pool automatisch konfiguriert. Sie können mit dem Abfragen von externen Spark-Tabellen sofort beginnen.

Jede externe Spark Parquet-Tabelle in Azure Storage wird durch eine externe Tabelle in einem dbo-Schema dargestellt, das einer SQL On-Demand-Datenbank entspricht. 

Führen Sie bei externen Spark-Tabellen eine Abfrage in einer externen „[spark_table]“ aus. Bevor Sie das unten stehende Beispiel ausführen, stellen Sie sicher, dass Sie ordnungsgemäßen [Zugriff auf das Speicherkonto haben](develop-storage-files-storage-access-control.md), in dem sich die Dateien befinden.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

> [!NOTE]
> Das Hinzufügen, Löschen oder Ändern von externen Spark-Tabellenspalten wird in externen Tabellen in SQL On-Demand nicht widergespiegelt.

## <a name="apache-spark-data-types-to-sql-data-types-mapping"></a>Zuordnung von Apache Spark-Datentypen zu SQL-Datentypen

| Spark-Datentyp | SQL-Datentyp               |
| --------------- | --------------------------- |
| ByteType        | SMALLINT                    |
| ShortType       | SMALLINT                    |
| IntegerType     | INT                         |
| LongType        | BIGINT                      |
| FloatType       | real                        |
| DoubleType      | float                       |
| DecimalType     | Decimal                     |
| TimestampType   | datetime2                   |
| DateType        | date                        |
| StringType      | varchar(max)*               |
| BinaryType      | varbinary                   |
| BooleanType     | bit                         |
| ArrayType       | varchar(max)* (in JSON)** |
| MapType         | varchar(max)* (in JSON)** |
| StructType      | varchar(max)* (in JSON)** |

\* Die verwendete Sortierung ist Latin1_General_100_BIN2_UTF8.

** ArrayType, MapType und StructType werden als JSONs dargestellt.



## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Steuerung des Speicherzugriffs](develop-storage-files-storage-access-control.md) erfahren Sie mehr über die Steuerung des Speicherzugriffs.
