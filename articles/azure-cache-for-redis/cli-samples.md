---
title: Azure CLI-Beispiele für Azure Cache for Redis
description: 'Azure CLI-Beispiele für Azure Cache for Redis: Erstellen eines Caches, Löschen eines Caches, Abrufen von Cachedetails, Hostname, Ports und Schlüssel, Verbinden einer Web-App.'
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/14/2017
ms.openlocfilehash: 43a986f884d621f257de2e9c305a0bcc59092d3d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87033664"
---
# <a name="azure-cli-samples-for-azure-cache-for-redis"></a>Azure CLI-Beispiele für Azure Cache for Redis

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| Cache erstellen | BESCHREIBUNG |
| ------------ | ----------- |
| [Erstellen eines Caches](./scripts/create-cache.md) | Erstellt eine Ressourcengruppe und einen Azure Cache for Redis im Basic-Tarif. |
| [Erstellen eines Premium-Caches mit Clustering](./scripts/create-premium-cache-cluster.md) | Erstellt eine Ressourcengruppe und einen Cache im Premium-Tarif mit aktiviertem Clustering.|
| [Abrufen von Cachedetails](./scripts/show-cache.md) | Ruft die Details einer Azure Cache for Redis-Instanz samt Bereitstellungsstatus ab. |
| [Abrufen von Hostname, Ports und Schlüsseln](./scripts/cache-keys-ports.md) | Ruft den Hostnamen, die Ports und Schlüssel einer Azure Cache for Redis-Instanz ab. |
|**Web-App plus Cache**| **Beschreibung**|
| [Herstellen einer Verbindung zwischen einer Web-App und einer Azure Cache for Redis-Instanz](./../app-service/scripts/cli-connect-to-redis.md) | Erstellt eine Azure-Web-App und einen Azure Cache for Redis und fügt dann die Redis-Verbindungsdetails zu den App-Einstellungen hinzu. |
|**Löschen eines Caches**| **Beschreibung** |
| [Löschen eines Caches](./scripts/delete-cache.md) | Löscht eine Azure Cache for Redis-Instanz.  |

Weitere Informationen zur Azure CLI finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) und [Erste Schritte mit der Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
