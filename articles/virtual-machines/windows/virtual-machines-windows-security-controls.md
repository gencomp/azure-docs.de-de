---
title: Sicherheitskontrollen für virtuelle Windows-Computer in Azure
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung virtueller Windows-Computer in Azure
ms.service: virtual-machines
ms.subservice: security
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: ac1ed9ac25d65d0391175fc6d43b48048da74926
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82101585"
---
# <a name="security-controls-for-windows-virtual-machines"></a>Sicherheitskontrollen für virtuelle Windows-Computer

In diesem Artikel sind die Sicherheitskontrollen dokumentiert, die in virtuellen Windows-Computern integriert sind.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | |
| Unterstützung der VNet-Einschleusung| Ja | |
| Netzwerkisolation und Firewallunterstützung| Ja |  |
| Unterstützung der Tunnelerzwingung| Ja | Siehe [Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | [Überwachen und Aktualisieren eines virtuellen Windows-Computers in Azure](tutorial-monitoring.md). |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene | Nein |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentifizierung| Ja |  |
| Authorization| Ja |  |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Siehe [Verschlüsseln virtueller Datenträger auf einer Windows-VM](/azure/virtual-machines/windows/disk-encryption-overview). |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Azure Virtual Machines unterstützt [ExpressRoute](/azure/expressroute)- und VNET-Verschlüsselung. Siehe [Verschlüsselung während der Übertragung zwischen virtuellen Computern](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja | Von Kunden verwaltete Schlüssel werden bei der Azure-Verschlüsselung unterstützt; siehe [Übersicht über die Azure-Verschlüsselung](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Per HTTPS und TLS |



## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja |  | 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../../security/fundamentals/security-controls.md).
