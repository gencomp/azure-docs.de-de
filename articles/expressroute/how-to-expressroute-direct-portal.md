---
title: 'Azure ExpressRoute: Konfigurieren von ExpressRoute Direct: Portal'
description: Diese Seite unterstützt Sie beim Konfigurieren von ExpressRoute Direct über das Portal.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 06/12/2020
ms.author: cherylmc
ms.openlocfilehash: f22f81d69d746764af43d9167874aabedbd56f51
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84765323"
---
# <a name="create-expressroute-direct-using-the-azure-portal"></a>Erstellen von ExpressRoute Direct mit dem Portal

In diesem Artikel wird gezeigt, wie Sie ExpressRoute Direct mit dem Azure-Portal erstellen.
ExpressRoute Direct ermöglicht es, sich direkt mit dem globalen Netzwerk von Microsoft zu verbinden, und zwar an strategisch über die ganze Welt verteilten Peeringstandorten. Weitere Informationen über ExpressRoute finden Sie unter [Informationen zu ExpressRoute Direct](expressroute-erdirect-about.md).

## <a name="1-create-expressroute-direct"></a><a name="create-erdir"></a>1. Erstellenvon ExpressRoute Direct

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Homepage** die Option **Ressource erstellen** aus.

1. Geben Sie auf der Seite **Neu** im Feld ***Marketplace durchsuchen*** den Suchbegriff **ExpressRoute Direct** ein, und drücken Sie anschließend die **EINGABETASTE**, um zu den Suchergebnissen zu gelangen.

1. Wählen Sie in den Ergebnissen **ExpressRoute Direct** aus.

1. Wählen Sie auf der Seite **ExpressRoute Direct** die Option **Erstellen** aus, um die Seite **ExpressRoute Direct erstellen** zu öffnen.

1. Beginnen Sie, indem Sie die Felder auf der Seite **Grundlagen** ausfüllen.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/basics.png" alt-text="Seite „Grundlagen“":::

    * **Abonnement**: Das Azure-Abonnement, das Sie zum Erstellen einer neuen ExpressRoute Direct-Ressource verwenden möchten. Die ExpressRoute Direct-Ressource und ExpressRoute-Leitungen müssen sich im selben Abonnement befinden.
    * **Ressourcengruppe**: Die Azure-Ressourcengruppe, in der die neue ExpressRoute Direct-Ressource erstellt wird. Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist.
    * **Region**: Die öffentliche Azure-Region, in der die Ressource erstellt wird.
    * **Name**: Der Name der neuen ExpressRoute Direct-Ressource.

1. Vervollständigen Sie dann die Felder auf der Seite **Konfiguration**.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration.png" alt-text="Seite „Konfiguration“":::

    * **Peeringstandort**: Der Peeringstandort, an dem Sie eine Verbindung mit der ExpressRoute Direct-Ressource herstellen. Weitere Informationen zu Peeringstandorten finden Sie unter [ExpressRoute-Standorte](expressroute-locations-providers.md).
   * **Bandbreite**: Die Portpaarbandbreite, die Sie reservieren möchten. ExpressRoute Direct unterstützt die Bandbreitenoptionen 10 GB und 100 GB. Wenn die gewünschte Bandbreite am angegebenen Peeringstandort nicht verfügbar ist, [öffnen Sie eine Supportanfrage im Azure-Portal](https://aka.ms/azsupt).
   * **Kapselung**: ExpressRoute Direct unterstützt die Kapselungen QinQ und Dot1Q.
     * Bei Wahl von QinQ wird jeder ExpressRoute-Leitung dynamisch ein S-Tag zugewiesen, das in der gesamten ExpressRoute Direct-Ressource eindeutig ist.
     *  Jedes C-Tag für die Leitung muss innerhalb der Leitung eindeutig sein, jedoch nicht innerhalb von ExpressRoute Direct.
     * Wenn die Kapselung Dot1Q ausgewählt wird, müssen Sie die Eindeutigkeit des C-Tags (VLAN) für die gesamte ExpressRoute Direct-Ressource sicherstellen.
     >[!IMPORTANT]
     >Für ExpressRoute Direct kann nur ein Kapselungstyp festgelegt werden. Nach der ExpressRoute Direct-Erstellung kann die Kapselung nicht geändert werden.
     >

1. Geben Sie beliebige Ressourcentags an, und wählen Sie dann **Überprüfen und erstellen** aus, um die ExpressRoute Direct-Ressourceneinstellungen zu überprüfen.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/validate.png" alt-text="Überprüfen und erstellen":::

1. Klicken Sie auf **Erstellen**. Daraufhin wird eine Meldung mit dem Hinweis angezeigt, dass Ihre Bereitstellung ausgeführt wird. Der Status der Ressourcenerstellung wird auf dieser Seite angezeigt. 

## <a name="2-change-admin-state-of-links"></a><a name="state"></a>2. Ändern des Verwaltungsstatus von Verknüpfungen

Dieser Prozess sollte verwendet werden, um einen Layer-1-Test durchzuführen, der sicherstellt, dass jede Kreuzverbindung ordnungsgemäß in jeden Router für primäre und sekundäre Verbindungen eingebunden ist.

1. Wählen Sie auf der Seite **Übersicht** der ExpressRoute Direct-Ressource im Abschnitt **Links** die Angabe **Link1** aus.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/link.png" alt-text="Link 1" lightbox="./media/how-to-expressroute-direct-portal/link-expand.png":::

1. Schalten Sie die Einstellung **Administratorstatus** auf **Aktiviert** um, und wählen Sie dann **Speichern** aus.

    :::image type="content" source="./media/how-to-expressroute-direct-portal/state.png" alt-text="Administratorstatus":::

    >[!IMPORTANT]
    >Die Abrechnung beginnt, wenn der Administratorstatus für einen der beiden Links aktiviert ist.
    >

1. Wiederholen Sie den gleichen Vorgang für **link2**.

## <a name="3-create-a-circuit"></a><a name="circuit"></a>3. Erstellen einer Leitung

Sie können standardmäßig 10 Leitungen im Abonnement erstellen, in dem sich die ExpressRoute Direct-Ressource befindet. Dieser Wert kann vom Support erhöht werden. Sie sind für die Nachverfolgung der Werte für die bereitgestellte und belegte Bandbreite zuständig. Die bereitgestellte Bandbreite ist die Summe der Bandbreiten aller Verbindungen der ExpressRoute Direct-Ressource. Die genutzte Bandbreite steht für die physische Nutzung der zugrunde liegenden physischen Schnittstellen.

* Es gibt zusätzliche Leitungsbandbreiten, die für ExpressRoute Direct nur zur Unterstützung der oben beschriebenen Szenarien genutzt werden können. Dies sind: 40 GBit/s und 100 GBit/s.

* SkuTier kann „Local“, „Standard“ oder „Premium“ sein.

* SkuFamily kann nur „MeteredData“ sein. „Unlimited“ wird für ExpressRoute Direct nicht unterstützt.

Die folgenden Schritte unterstützen Sie beim Erstellen einer ExpressRoute-Verbindung aus dem ExpressRoute Direct-Workflow. Wenn Sie es vorziehen, können Sie eine Verbindung auch über den regulären Verbindungsworkflow erstellen, obwohl es für diese Konfiguration keinen Vorteil mit sich bringt, die Workflowschritte des regulären Verbindungsworkflows zu verwenden. Weitere Informationen finden Sie unter [Erstellen und Ändern einer ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md).

1. Wählen Sie im Abschnitt **ExpressRoute Direct-Einstellungen** die Option **Verbindungen** aus, und wählen Sie dann **+Hinzufügen** aus. 

    :::image type="content" source="./media/how-to-expressroute-direct-portal/add.png" alt-text="Add (Hinzufügen)" lightbox="./media/how-to-expressroute-direct-portal/add-expand.png":::

1. Konfigurieren Sie die Einstellungen auf der Seite **Konfiguration**.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/configuration2.png" alt-text="Seite „Konfiguration“":::

1. Geben Sie beliebige Ressourcen Tags an, und wählen Sie **Überprüfen und erstellen** aus, um die Werte vor dem Erstellen der Ressource zu validieren.

   :::image type="content" source="./media/how-to-expressroute-direct-portal/review.png" alt-text="Überprüfen und erstellen":::

1. Klicken Sie auf **Erstellen**. Daraufhin wird eine Meldung mit dem Hinweis angezeigt, dass Ihre Bereitstellung ausgeführt wird. Der Status der Ressourcenerstellung wird auf dieser Seite angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute Direct finden Sie in der [Übersicht](expressroute-erdirect-about.md).
