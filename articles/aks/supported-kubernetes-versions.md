---
title: Unterstützte Kubernetes-Versionen in Azure Kubernetes Service
description: Grundlegendes zur Richtlinie zur Unterstützung der Kubernetes-Version und zum Lebenszyklus von Clustern in Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 07/08/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: 019ae80020dafb54f2c06dd504797f21069914ae
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507062"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Unterstützte Kubernetes-Versionen in Azure Kubernetes Service (AKS)

Die Kubernetes-Community veröffentlicht etwa alle drei Monate Nebenversionen. Diese Releases enthalten neue Features und Verbesserungen. Patchreleases kommen häufiger vor (manchmal wöchentlich) und sind nur für wichtige Fehlerbehebungen in einer Nebenversion gedacht. Diese Patchreleases enthalten Fixes für Sicherheitsrisiken oder schwer wiegende Fehler.

## <a name="kubernetes-versions"></a>Kubernetes-Versionen

Kubernetes verwendet das standardmäßige Schema der [semantischen Versionierung](https://semver.org/), sodass jede Kubernetes-Version diesem Nummerierungsformat folgt:

```
[major].[minor].[patch]

Example:
  1.17.7
  1.17.8
```

Für jede Zahl in der Version gilt, dass allgemeine Kompatibilität mit der vorherigen Version besteht:

* Die Hauptversionen ändern sich, wenn nicht kompatible API-Änderungen vorgenommen werden oder möglicherweise keine Abwärtskompatibilität mehr gewährleistet wird.
* Nebenversionen ändern sich, wenn Änderungen an der Funktionalität vorgenommen werden, die zu den anderen Nebenreleases abwärtskompatibel sind.
* Patchversionen ändern sich, wenn abwärtskompatible Fehlerbehebungen vorgenommen werden.

Benutzer sollten nach Möglichkeit immer das neueste Patchrelease der aktuell verwendeten Nebenversion nutzen. Wenn Ihr Produktionscluster beispielsweise die Version **`1.17.7`** verwendet und **`1.17.8`** die neueste verfügbare Patchversion für *1.17* ist, sollten Sie ein Upgrade auf Version **`1.17.8`** durchführen, sobald Sie sicher sind, dass Ihr Cluster vollständig gepatcht ist und unterstützt wird.

## <a name="kubernetes-version-support-policy"></a>Richtlinie zur Unterstützung der Kubernetes-Version

In AKS ist eine allgemein verfügbare Version als eine Version definiert, die in allen SLO- oder SLA-Messungen aktiviert und in allen Regionen verfügbar ist. AKS unterstützt drei allgemein verfügbare Nebenversionen von Kubernetes:

* Die neueste allgemein verfügbare Nebenversion, die in AKS veröffentlicht wurde (diese wird als „N“ bezeichnet). 
* Die beiden vorherigen Nebenversionen. 
* Jede unterstützte Nebenversion unterstützt auch maximal zwei stabile Patches.
* AKS unterstützt möglicherweise auch Vorschauversionen, die explizit als solche gekennzeichnet sind und den [Nutzungsbedingungen für Vorschauversionen][preview-terms] unterliegen.

> [!NOTE]
> AKS verwendet sichere Bereitstellungsmethoden einschließlich einer graduellen Bereitstellung in Regionen. Das bedeutet, dass es bis zu zehn Werktage dauern kann, bis ein neues Release oder eine neue Version in allen Regionen verfügbar ist.

Das unterstützte Fenster für Kubernetes-Versionen in AKS wird als „N-2“ bezeichnet: („N“ für das neueste Release – 2 für die Anzahl der Nebenversionen).

Wenn AKS beispielsweise die Version *1.17.a* einführt, werden die folgenden Versionen unterstützt:

Neue Nebenversion    |    Liste mit unterstützten Versionen
-----------------    |    ----------------------
1.17.a               |    1.17.a, 1.17.b, 1.16.c, 1.16.d, 1.15.e, 1.15.f

Dabei steht der Buchstabe für die jeweiligen Patchversionen.

Wenn eine neue Nebenversion eingeführt wird, werden die älteste Nebenversion und die unterstützten Patchreleases nicht mehr unterstützt und entfernt. Angenommen, die aktuelle Liste mit den unterstützten Versionen sieht wie folgt aus:

```
1.17.a
1.17.b
1.16.c
1.16.d
1.15.e
1.15.f
```

Wenn AKS dann Version 1.18.\* veröffentlicht, bedeutet das, dass alle 1.15.\*-Versionen entfernt und nach 30 Tagen nicht mehr unterstützt werden.

> [!NOTE]
> Wenn Kunden eine nicht unterstützte Kubernetes-Version verwenden, werden sie aufgefordert, ein Upgrade vorzunehmen, sobald sie Support für den Cluster anfordern. Die [AKS-Supportrichtlinien](./support-policies.md) gelten nicht für Cluster, auf denen nicht unterstützte Kubernetes-Releases ausgeführt werden.

Darüber hinaus unterstützt AKS maximal zwei **Patchreleases** für eine bestimmte Nebenversion. Angenommen, folgende Versionen werden unterstützt:

```
Current Supported Version List
------------------------------
1.17.8, 1.17.7, 1.16.10, 1.16.9
```

Wenn AKS `1.17.9` und `1.16.11` veröffentlicht, werden die ältesten Patchversionen als veraltet markiert und entfernt. Die Liste mit den unterstützten Versionen ändert sich dann wie folgt:

```
New Supported Version List
----------------------
1.17.*9*, 1.17.*8*, 1.16.*11*, 1.16.*10*
```

## <a name="release-and-deprecation-process"></a>Veröffentlichen und Markieren als veraltet

Sie können neue Releases und veraltete Versionen im [Releasekalender für AKS Kubernetes](#aks-kubernetes-release-calendar) einsehen.

Bei neuen **Nebenversionen** von Kubernetes:
1. AKS veröffentlicht mindestens 30 Tage vor dem Entfernen einer Version in den [AKS-Versionshinweisen](https://aka.ms/aks/releasenotes) eine Vorankündigung mit dem geplanten Datum der neuen Version und der Entfernung der entsprechenden alten Version.
2. AKS veröffentlicht eine [Benachrichtigung zur Dienstintegrität](../service-health/service-health-overview.md), die für alle Benutzer mit Zugriff auf AKS und das Portal verfügbar ist. Außerdem sendet AKS eine E-Mail an die Abonnementadministratoren mit den geplanten Daten für die Entfernung von Versionen.
3. Benutzer haben nach der Entfernung einer Version **30** Tage lang Zeit, ein Upgrade auf eine unterstützte Nebenversion durchzuführen, um sicherzustellen, dass sie weiterhin Support erhalten.

Bei neuen **Patchversionen** von Kubernetes:
  * Aufgrund der dringenden Natur von Patchversionen können diese in den Dienst eingefügt werden, sobald sie verfügbar sind.
  * Im Allgemeinen teilt AKS die Veröffentlichung neuer Patchversionen nicht flächendeckend mit. AKS überwacht und überprüft allerdings verfügbare CVE-Patches, um diese möglichst schnell in AKS zu unterstützen. Wenn ein kritischer Patch gefunden wird oder eine Benutzeraktion erforderlich ist, benachrichtigt AKS die Benutzer, damit diese ein Upgrade auf den neu verfügbaren Patch ausführen.
  * Benutzer haben ab dem Zeitpunkt, zu dem ein Patchrelease von AKS entfernt wurde, **30 Tage** lang Zeit, ein Upgrade auf einen unterstützten Patch auszuführen, um weiterhin Support zu erhalten.

### <a name="supported-versions-policy-exceptions"></a>Richtlinienausnahmen für unterstützte Versionen

AKS behält sich das Recht vor, ohne Vorankündigung neue oder bestehende Versionen hinzuzufügen oder zu entfernen, wenn in diesen kritische, die Produktion einschränkende Fehler oder Sicherheitsprobleme gefunden werden.

Bestimmte Patchreleases können übersprungen oder Rollouts je nach Schweregrad des Fehlers oder Sicherheitsproblems beschleunigt werden.

## <a name="azure-portal-and-cli-versions"></a>Versionen für Azure-Portal and CLI

Wenn Sie einen AKS-Cluster im Portal oder mit der Azure-Befehlszeilenschnittstelle bereitstellen, wird der Cluster standardmäßig auf die Nebenversion N-1 und den neuesten Patch festgelegt. Wenn AKS beispielsweise *1.17.a*, *1.17.b*, *1.16.c*, *1.16.d*, *1.15.e* und *1.15.f* unterstützt, ist die standardmäßig ausgewählte Version *1.16.c*.

Um herauszufinden, welche Versionen für Ihr Abonnement und Ihre Region derzeit verfügbar sind, verwenden Sie den Befehl [az aks get-versions][az-aks-get-versions]. Im folgenden Beispiel sind die verfügbaren Kubernetes-Versionen für die Region *EastUS* aufgelistet:

```azurecli-interactive
az aks get-versions --location eastus --output table
```


## <a name="aks-kubernetes-release-calendar"></a>Releasekalender für AKS Kubernetes

Den Verlauf der letzten Versionen finden Sie [hier](https://en.wikipedia.org/wiki/Kubernetes#History).

|  Kubernetes-Version | Upstreamrelease  | AKS – Vorschau  | AKS – allgemeine Verfügbarkeit (GA)  | Ende der Lebensdauer |
|--------------|-------------------|--------------|---------|-------------|
| 1.17  | 09. Dezember 2019  | Januar 2019   | Juli 2020  | 1.20 GA | 
| 1.18  | 23. März 2020  | Mai 2020   | August 2020  | 1.21 GA | 
| 1.19  | 04. August 2020  | August 2020   | November 2020  | 1.22 GA | 
| 1.20  | *November 2020    | *Dezember 2020   | *Januar 2021  | 1.23 GA | 

\* Vorbehaltlich der Bestätigung des Upstreamreleasedatums.

## <a name="faq"></a>Häufig gestellte Fragen

**Was geschieht, wenn ein Benutzer ein Upgrade eines Kubernetes-Clusters mit einer Nebenversion durchführt, die nicht unterstützt wird?**

Wenn Sie die Version *N-3* oder älter verwenden, wird diese Version nicht mehr unterstützt, und Sie werden zu einem Upgrade aufgefordert. Wenn das Upgrade von Version N-3 auf N-2 erfolgreich ist, greifen die Supportrichtlinien wieder, und die Version wird wieder unterstützt. Beispiel:

- Wenn die älteste unterstützte AKS-Version *1.15.a* ist und Sie *1.14.b* oder eine ältere Version verwenden, wird diese Version nicht mehr unterstützt.
- Wenn das Upgrade von *1.14.b* auf *1.15.a* oder höher erfolgreich ist, greifen die Supportrichtlinien wieder, und die Version wird wieder unterstützt.

Downgrades werden nicht unterstützt.

**Was bedeutet „nicht unterstützt“?**

„Nicht unterstützt“ bedeutet, dass die verwendete Version nicht in der Liste mit den unterstützten Versionen enthalten ist. Wenn Sie Support anfordern, werden Sie aufgefordert, für den Cluster ein Upgrade auf eine unterstützte Version vorzunehmen, sofern Sie sich nicht mehr innerhalb des 30-Tage-Zeitraums befinden, der beginnt, nachdem eine Version als veraltet gekennzeichnet wurde. Des Weiteren stellt AKS für Cluster mit Versionen, die nicht in der Liste der unterstützten Versionen aufgeführt sind, weder Laufzeitgarantien noch sonstige Garantien bereit.

**Was geschieht, wenn ein Benutzer einen Kubernetes-Clusters mit einer Nebenversion skaliert, die nicht unterstützt wird?**

Bei Nebenversionen, die nicht von AKS unterstützt werden, sollte das Hoch- oder Herunterskalieren weiterhin funktionieren. Es gibt jedoch keine Garantien in Bezug auf die Servicequalität (Quality of Service, QoS), daher wird dringend empfohlen, ein Upgrade durchzuführen, um wieder Support für den Cluster zu erhalten.

**Kann ein Benutzer eine Kubernetes-Version für immer verwenden?**

Wenn ein Cluster für mehr als drei Nebenversionen keinen Supportanspruch mehr hat und Sicherheitsrisiken für ihn festgestellt wurden, setzt Azure sich bezüglich eines proaktiven Upgrades Ihres Cluster mit Ihnen in Verbindung. Wenn Sie keine weiteren Maßnahmen ergreifen, behält Azure sich das Recht vor, automatisch ein Upgrade Ihres Clusters in Ihrem Namen auszuführen.

**Welche Version wird von der Steuerungsebene unterstützt, wenn der Knotenpool keine der unterstützten AKS-Versionen verwendet?**

Die Steuerungsebene muss sich innerhalb eines Fensters von Versionen aller Knotenpools befinden. Ausführliche Informationen zum Aktualisieren der Steuerungsebene oder der Knotenpools finden Sie in der Dokumentation zum [Aktualisieren von Knotenpools](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools).

**Kann ich beim Upgrade eine Version überspringen?**

Nein, gemäß den Best Practices von Kubernetes erlaubt AKS nur Upgrades auf die direkt folgende Patch- oder Nebenversion, die unterstützt wird. Im Azure-Portal werden nur die Versionen angezeigt, die Sie für ein Upgrade verwenden können. In der CLI können Sie `az aks get-upgrades -n MyAKSCluster -g MyResourceGroup` ausführen, um die verfügbaren Upgrades für Ihre aktuelle Version anzuzeigen.

**Wie führe ich ein Upgrade auf eine unterstützte Version aus, wenn mehrere Versionen zwischen der letzten unterstützten Version und meiner derzeitigen Version liegen?**

Um jederzeit Support zu erhalten, sollten Sie vermeiden, dass mehrere Versionen zwischen Ihrer derzeit verwendeten Version und der Liste der aktuell unterstützten Versionen liegen. Wenn Sie sich jedoch in dieser Situation befinden, ermöglicht AKS immer ein Upgrade auf die unterstützte Mindestversion.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Upgrade Ihres Clusters finden Sie unter [Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
