---
title: Netzwerksicherheit für Azure Relay
description: In diesem Artikel wird beschrieben, wie Sie Zugriff über private Endpunkte konfigurieren.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: a1ade21df39890b7f1c31a81fca1fffafe2acaa0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85322166"
---
# <a name="network-security-for-azure-relay"></a>Netzwerksicherheit für Azure Relay 
In diesem Artikel wird beschrieben, wie Sie die folgenden Sicherheitsfunktionen mit Azure Relay verwenden: 

- IP-Firewallregeln (Vorschau)
- Private Endpunkte (Vorschau)

> [!NOTE]
> Azure Relay unterstützt keine Netzwerkdienstendpunkte. 


## <a name="ip-firewall"></a>IP-Firewall 
Standardmäßig kann über das Internet auf Relaynamespaces zugegriffen werden, solange die Anforderung eine gültige Authentifizierung und Autorisierung aufweist. Mit der IP-Firewall können Sie den Zugriff auf eine Gruppe von IPv4-Adressen oder IPv4-Adressbereichen in der [CIDR-Notation (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) weiter einschränken.

Diese Funktion ist in Szenarien hilfreich, in denen Azure Relay nur von bestimmten bekannten Websites aus zugänglich sein soll. Mithilfe von Firewallregeln können Sie Regeln konfigurieren, um Datenverkehr von bestimmten IPv4-Adressen zuzulassen. Wenn Sie z. B. Relay mit [Azure Express Route](/azure/expressroute/expressroute-faqs#supported-services) verwenden, können Sie eine **Firewallregel** erstellen, um Datenverkehr nur von den IP-Adressen Ihrer lokalen Infrastruktur zuzulassen. 

Die IP-Firewallregeln werden auf der Relaynamespaceebene angewendet. Daher gelten die Regeln für alle Clientverbindungen mit einem beliebigen unterstützten Protokoll. Jeder Verbindungsversuch von einer IP-Adresse, die nicht mit einer zulässigen IP-Regel im Relaynamespace übereinstimmt, wird als nicht autorisiert abgelehnt. In der Antwort wird die IP-Regel nicht erwähnt. IP-Filterregeln werden der Reihe nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Weitere Informationen finden Sie unter [Konfigurieren einer IP-Firewall für einen Relaynamespace](ip-firewall-virtual-networks.md).

## <a name="private-endpoints"></a>Private Endpunkte

Mit **Azure Private Link** können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-Dienste wie Azure Relay, Azure Service Bus, Azure Event Hubs, Azure Storage und Azure Cosmos DB sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen. Weitere Informationen finden Sie unter [Was ist Azure Private Link? (Vorschau)](../private-link/private-link-overview.md).

Ein **privater Endpunkt** ist eine Netzwerkschnittstelle, mit der Ihre Workloads, die in einem virtuellen Netzwerk ausgeführt werden, eine private und sichere Verbindung mit einem Dienst herstellen können, der über eine **Private Link-Ressource** verfügt (z. B. über einen Relaynamespace). Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem VNET und bindet den Dienst dadurch in Ihr VNET ein. Der gesamte für den Dienst bestimmte Datenverkehr kann über den privaten Endpunkt geleitet werden. Es sind also keine Gateways, NAT-Geräte, ExpressRoute-Verbindungen, VPN-Verbindungen oder öffentlichen IP-Adressen erforderlich. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbonenetzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können eine gewünschte Granularitätsebene für die Zugriffssteuerung festlegen, indem Sie Verbindungen mit bestimmten Azure Relay-Namespaces zulassen.

> [!NOTE]
> Diese Funktion steht derzeit als **Vorschau** zur Verfügung. 

Weitere Informationen finden Sie unter [Konfigurieren privater Endpunkte](private-link-service.md).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Konfigurieren der IP-Firewall](ip-firewall-virtual-networks.md)
- [Konfigurieren privater Endpunkte](private-link-service.md)