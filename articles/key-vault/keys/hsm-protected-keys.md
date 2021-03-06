---
title: 'Gewusst wie: Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault – Azure Key Vault| Microsoft-Dokumentation'
description: Verwenden Sie diesen Artikel zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit dem Azure-Schlüsseltresor. Wird auch als Bring Your Own Key (BYOK) bezeichnet.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: ambapat
ms.openlocfilehash: 5433d9746cd64d0e942e056cfcd1940eba35c77d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84417921"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Importieren von HSM-geschützten Schlüsseln in Key Vault

Zur Steigerung der Sicherheit können Sie bei Verwendung des Azure-Schlüsseltresors Schlüssel in HSMs (Hardwaresicherheitsmodule) importieren oder darin generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Azure Key Vault verwendet die nCipher nShield-HSM-Produktfamilie (validiert für FIPS 140-2 Level 2) zum Schützen der Schlüssel.

Diese Funktion steht für Azure China 21ViaNet nicht zur Verfügung.

> [!NOTE]
> Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../general/overview.md)  
> Ein Tutorial zu den ersten Schritten, z.B. zum Erstellen eines Schlüsseltresors für HSM-geschützte Schlüssel, finden Sie unter [Was ist Azure Key Vault?](../general/overview.md).

## <a name="supported-hsms"></a>Unterstützte HSMs

HSM-geschützte Schlüssel können abhängig vom verwendeten HSM auf zwei Arten an Key Vault übertragen werden. In der folgenden Tabelle erfahren Sie, welche Methode für Ihre HSMs verwendet werden muss, um Ihre eigenen HSM-geschützten Schlüssel zu generieren und anschließend für die Verwendung mit Azure Key Vault zu übertragen: 

|Herstellername|Herstellertyp|Unterstützte HSM-Modelle|Unterstützte Übertragungsmethode für HSM-Schlüssel|
|---|---|---|---|
|[nCipher](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|Hersteller,<br/>HSM als Dienst (aaS)|<ul><li>HSM-Produktfamilie „nShield“</li><li>nShield als Dienst</ul>|**Methode 1:** [nChiffre BYOK](hsm-protected-keys-ncipher.md) (mit starkem Nachweis für den Schlüsselimport und die HSM-Prüfung)<br/>**Methode 2:** [Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md) |
|Thales|Hersteller|<ul><li>Produktfamilie „Luna HSM 7“ mit Firmwareversion 7.3 oder neuer</li></ul>| [Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Fortanix|Hersteller,<br/>HSM als Dienst (aaS)|<ul><li>Self-Defending Key Management Service (SDKMS, selbstverteidigender Schlüsselverwaltungsdienst)</li><li>Equinix SmartKey</li></ul>|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Marvell|Hersteller|Alle Liquid Security-HSMs mit<ul><li>Firmwareversion 2.0.4 oder höher</li><li>Firmwareversion 3.2 oder höher</li></ul>|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Cryptomathic|ISV (Enterprise Key Management System)|Mehrere HSM-Marken und -Modelle, einschließlich<ul><li>nCipher</li><li>Thales</li><li>Utimaco</li></ul>Weitere Informationen finden Sie auf der [Cryptomathic-Website](https://www.cryptomathic.com/azurebyok).|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|


## <a name="next-steps"></a>Nächste Schritte

* Halten Sie sich an die [bewährten Methoden zum Verwenden von Key Vault](../general/best-practices.md), um die Sicherheit, Dauerhaftigkeit und Überwachung Ihrer Schlüssel zu gewährleisten.
* Eine vollständige Beschreibung der neuen BYOK-Methode finden Sie unter [BYOK-Spezifikation](https://docs.microsoft.com/azure/key-vault/keys/byok-specification).
