---
title: azcopy logout | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy logout“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 18add90ace11038fbba662b13400ccc7ddd8e763
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84219937"
---
# <a name="azcopy-logout"></a>azcopy logout

Meldet den Benutzer ab und beendet den Zugriff auf Azure Storage-Ressourcen.

## <a name="synopsis"></a>Zusammenfassung

Mit diesem Befehl werden alle zwischengespeicherten Anmeldeinformationen für den aktuellen Benutzer entfernt.

```azcopy
azcopy logout [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blobspeicher](storage-use-azcopy-blobs.md)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Tastatur

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „logout“.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|
|--trusted-microsoft-suffixes-Zeichenfolge   |Gibt zusätzliche Domänensuffixe an, an die Azure Active Directory-Anmeldetoken gesendet werden können.  Der Standardwert ist ' *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net'. Alle hier aufgelisteten Werte werden zum Standardwert hinzugefügt. Aus Sicherheitsgründen sollten Sie hier nur Microsoft Azure-Domänen platzieren. Trennen Sie mehrere E-Mail-Adressen durch Semikolons voneinander.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
