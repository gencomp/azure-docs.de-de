---
title: Azure CLI-Beispiele für virtuelle Netzwerke
description: Azure CLI-Beispiele für virtuelle Netzwerke.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: 03041f7c0172312828487d934ab540972dc5e2c9
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232099"
---
# <a name="azure-cli-samples-for-virtual-network"></a>Azure CLI-Beispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Bashskripts mit Azure CLI-Befehlen:

| Skript | BESCHREIBUNG |
|----|----|
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr am Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr zum Back-End-Subnetz auf MySQL und Port 3306 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP, HTTPS und SSH beschränkt. Aus dem Back-End-Subnetz an das Internet ausgehender Datenverkehr ist nicht zulässig. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6)mit Load Balancer im Tarif „Basic“](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 + IPv6) mit zwei virtuellen Computern und einem Azure Load Balancer Basic mit öffentlichen IPv4- und IPv6-Adressen bereit. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6) mit Load Balancer Standard](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 und IPv6) mit zwei virtuellen Computern und einer Azure Load Balancer Standard-Instanz mit öffentlichen IPv4- und IPv6-Adressen bereit. |
|[Tutorial: Erstellen eines NAT-Gateways mit der Azure CLI und Testen des NAT-Diensts](../virtual-network/tutorial-create-validate-nat-gateway-cli.md)|Hier erfahren Sie, wie Sie ein NAT-Gateway unter Verwendung eines virtuellen Quell- und Zielcomputers erstellen und überprüfen. |