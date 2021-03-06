---
title: Verwenden benutzerdefinierter Rollen für Azure-Ressourcen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie benutzerdefinierte Rollen für Azure-Ressourcen in Azure AD Privileged Identity Management (PIM) verwenden.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa51508746d0024be0a5acfaeeac62e86db67d3f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743744"
---
# <a name="use-custom-roles-for-azure-resources-in-privileged-identity-management"></a>Verwenden benutzerdefinierter Rollen für Azure-Ressourcen in Privileged Identity Management

Möglicherweise müssen Sie auf einige Benutzer in einer privilegierten Rolle in Ihrer Azure Active Directory-Organisation (Azure AD) strikte Privileged Identity Management-Einstellungen (PIM) anwenden, während anderen Benutzern eine größere Autonomie eingeräumt werden kann. Stellen Sie sich z. B. ein Szenario vor, bei dem Ihre Organisation auf mehrere Vertragspartner zurückgreift, die Unterstützung bei der Entwicklung einer Anwendung leisten, die in einem Azure-Abonnement ausgeführt wird.

Als Ressourcenadministrator wünschen Sie sich, dass Mitarbeiter ohne Genehmigung für den Zugriff berechtigt sind. Allerdings muss für alle Vertragspartner eine Genehmigung vorhanden sein, wenn sie Zugriff auf die Ressourcen eines Unternehmens anfordern.

Führen Sie die im nächsten Abschnitt angegebenen Schritte aus, um Privileged Identity Management-Zieleinstellungen für Azure-Ressourcenrollen einzurichten.

## <a name="create-the-custom-role"></a>Erstellen der benutzerdefinierten Rolle

Um eine benutzerdefinierte Rolle für eine Ressource zu erstellen, führen Sie die Schritte unter [Erstellen benutzerdefinierter Rollen für rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control-custom-roles.md) aus.

Wenn Sie benutzerdefinierte Rollen erstellen, fügen Sie einen beschreibenden Namen hinzu, damit Sie sich leicht merken können, welche integrierte Rolle dupliziert werden soll.

> [!NOTE]
> Stellen Sie sicher, dass die benutzerdefinierte Rolle ein Duplikat der integrierten Rolle ist, die Sie duplizieren möchten, und dass ihr Bereich mit der integrierten Rolle übereinstimmt.

## <a name="apply-pim-settings"></a>Anwenden von PIM-Einstellungen

Nachdem die Rolle in Ihrer Azure AD-Organisation erstellt wurde, navigieren Sie im Azure-Portal zur Seite **Privileged Identity Management – Azure-Ressourcen**. Wählen Sie die Ressource aus, für die die Rolle gelten soll.

![Bereich „Privileged Identity Management - Azure-Ressourcen“](media/pim-resource-roles-custom-role-policy/aadpim-manage-azure-resource-some-there.png)

[Konfigurieren Sie die Privileged Identity Management-Rolleneinstellungen](pim-resource-roles-configure-role-settings.md), die für diese Mitglieder der Rolle gelten sollen.

Führen Sie abschließend das [Zuweisen von Rollen](pim-resource-roles-assign-roles.md) für einzelne Gruppen mit Mitgliedern aus, für die diese Einstellungen gelten sollen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Benutzerdefinierte Rollen in Azure](../../role-based-access-control/custom-roles.md)
