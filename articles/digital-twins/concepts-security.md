---
title: Sicherheit für Azure Digital Twins-Lösungen
titleSuffix: Azure Digital Twins
description: Grundlegendes zu bewährten Sicherheitsmethoden mit Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 0a1447e64b606170601e6df6a443f53e3132294d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86522260"
---
# <a name="secure-azure-digital-twins-with-role-based-access-control"></a>Schützen von Azure Digital Twins mit rollenbasierter Zugriffssteuerung

Zur Gewährleistung der Sicherheit ermöglicht Azure Digital Twins eine exakte Zugriffssteuerung für bestimmte Daten, Ressourcen und Aktionen in Ihrer Bereitstellung. Hierzu wird eine präzise Rollen- und Rechteverwaltungsstrategie namens **rollenbasierte Zugriffssteuerung** (Role-Based Access Control, RBAC) verwendet. Informationen zu den allgemeinen RBAC-Prinzipien für Azure finden Sie [hier](../role-based-access-control/overview.md).

## <a name="rbac-through-azure-ad"></a>RBAC über Azure AD

Die RBAC wird in Azure Digital Twins über die Integration mit [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) bereitgestellt.

Mit der RBAC können einem *Sicherheitsprinzipal* Berechtigungen erteilt werden. Bei einem Sicherheitsprinzipal kann es sich um einen Benutzer, um eine Gruppe oder um einen Anwendungsdienstprinzipal handeln. Der Sicherheitsprinzipal wird durch Azure AD authentifiziert und erhält im Gegenzug ein OAuth 2.0-Token. Dieses Token kann verwendet werden, um eine an eine Azure Digital Twins-Instanz gerichtete Zugriffsanforderung zu autorisieren.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Der Zugriff mit Azure AD ist ein zweistufiger Prozess. Wenn ein Sicherheitsprinzipal (ein Benutzer, eine Gruppe oder eine Anwendung) versucht, auf Azure Digital Twins zuzugreifen, muss die Anforderung *authentifiziert* und *autorisiert* werden. 

1. Zunächst wird die Identität des Sicherheitsprinzipals *authentifiziert* und ein OAuth 2.0-Token zurückgegeben.
2. Anschließend wird das Token als Teil einer Anforderung an den Azure Digital Twins-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu *autorisieren*.

Für den Authentifizierungsschritt muss jede Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthalten. Wird eine Anwendung in einer Azure-Entität ausgeführt (beispielsweise in einer [Azure Functions](../azure-functions/functions-overview.md)-App), kann für den Zugriff auf die Ressourcen eine **verwaltete Identität** verwendet werden. Weitere Informationen zu verwalteten Identitäten finden Sie im nächsten Abschnitt.

Für den Autorisierungsschritt muss dem Sicherheitsprinzipal eine RBAC-Rolle zugewiesen werden. Die möglichen Berechtigungen eines Sicherheitsprinzipals sind durch die Rollen vorgegeben, die dem Prinzipal zugewiesen sind. Von Azure Digital Twins werden RBAC-Rollen bereitgestellt, die Berechtigungssätze für Azure Digital Twins-Ressourcen enthalten. Diese Rollen werden weiter unten in diesem Artikel beschrieben.

Weitere Informationen zu in Azure unterstützten Rollen und Rollenzuweisungen finden Sie in der Azure RBAC-Dokumentation unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../role-based-access-control/rbac-and-directory-admin-roles.md).

### <a name="authentication-with-managed-identities"></a>Authentifizierung mit verwalteten Identitäten

[Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) ist ein Azure-übergreifendes Feature, mit dem Sie eine sichere Identität für die Bereitstellung erstellen können, in der Ihr Anwendungscode ausgeführt wird. Dieser Identität können dann Zugriffssteuerungsrollen zugeordnet werden, um benutzerdefinierte Berechtigungen für den Zugriff auf bestimmte Azure-Ressourcen zu erteilen, die Ihre Anwendung benötigt.

Die Azure-Plattform verwaltet diese Laufzeitidentität mit verwalteten Identitäten. Sie müssen keine Zugriffsschlüssel in Ihrem Anwendungscode speichern und schützen – weder für die Identität selbst noch für die Ressourcen, auf die zugegriffen werden muss. Von einer Azure Digital Twins-Client-App, die innerhalb einer Azure App Service-Anwendung ausgeführt wird, müssen keine SAS-Regeln und -Schlüssel oder andere Zugriffstoken verarbeitet werden. Die Client-App benötigt nur die Endpunktadresse des Azure Digital Twins-Namespace. Wenn die App eine Verbindung herstellt, bindet Azure Digital Twins den Kontext der verwalteten Entität an den Client. Nach der Zuordnung zu einer verwalteten Identität können von Ihrem Azure Digital Twins-Client sämtliche autorisierten Vorgänge ausgeführt werden. Zur Autorisierung wird dann eine verwaltete Entität einer Azure Digital Twins-RBAC-Rolle zugeordnet, wie im Anschluss beschrieben.

### <a name="authorization-rbac-roles-for-azure-digital-twins"></a>Autorisierung: RBAC-Rollen für Azure Digital Twins

Für die Autorisierung des Zugriffs auf eine Azure Digital Twins-Ressource werden von Azure die folgenden integrierten RBAC-Rollen bereitgestellt:
* Azure Digital Twins-Besitzer (Vorschau): Verwenden Sie diese Rolle, um Vollzugriff auf Azure Digital Twins-Ressourcen zu erteilen.
* Azure Digital Twins-Leser (Vorschau): Verwenden Sie diese Rolle, um Lesezugriff auf Azure Digital Twins-Ressourcen zu erteilen.

> [!TIP]
> Von der Rolle „Azure Digital Twins-Leser (Vorschau)“ wird nun auch das Durchsuchen von Beziehungen unterstützt.

Weitere Informationen zur Definition integrierter Rollen finden Sie in der Azure RBAC-Dokumentation unter [Grundlegendes zu Azure-Rollendefinitionen](../role-based-access-control/role-definitions.md). Informationen zur Erstellung benutzerdefinierter RBAC-Rollen finden Sie unter [Benutzerdefinierte Azure-Rollen](../role-based-access-control/custom-roles.md).

Rollen können auf zwei Arten zugewiesen werden:
* Über den Bereich „Zugriffssteuerung (IAM)“ für Azure Digital Twins im Azure-Portal (siehe [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md))
* Mithilfe von CLI-Befehlen zum Hinzufügen oder Entfernen einer Rolle

Ausführlichere Schritte hierzu finden Sie im [Azure Digital Twins-Tutorial: *Verbinden einer End-to-End-Lösung*](tutorial-end-to-end.md).

## <a name="permission-scopes"></a>Berechtigungsbereiche

Bevor Sie einem Sicherheitsprinzipal eine RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es empfiehlt sich, immer nur den kleinstmöglichen Umfang zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure Digital Twins-Ressourcen einschränken können:
* Modelle: Die Aktionen für diese Ressource geben die Kontrolle über [Modelle](concepts-models.md) vor, die in Azure Digital Twins hochgeladen werden.
* Abfragen des Digital Twins-Graphen: Die Aktionen für diese Ressource bestimmen die Fähigkeit, [Abfragevorgänge](concepts-query-language.md) für digitale Zwillinge innerhalb des Azure Digital Twins-Graphen auszuführen.
* Digitaler Zwilling: Die Aktionen für diese Ressource ermöglichen die Steuerung von CRUD-Vorgängen für [digitale Zwillinge](concepts-twins-graph.md) im Zwillingsgraphen.
* Digitale Zwillingsbeziehung: Die Aktionen für diese Ressource definieren die Steuerung von CRUD-Vorgängen für [Beziehungen](concepts-twins-graph.md) zwischen digitalen Zwillingen im Zwillingsgraphen.
* Ereignisroute: Die Aktionen für diese Ressource bestimmen Berechtigungen zum [Weiterleiten von Ereignissen](concepts-route-events.md) aus Azure Digital Twins an einen Endpunktdienst wie [Event Hub](../event-hubs/event-hubs-about.md), [Event Grid](../event-grid/overview.md) oder [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Durchlaufen dieser Schritte mit einer exemplarischen Clientanwendung finden Sie unter [*Gewusst wie: Authentifizieren einer Clientanwendung*](how-to-authenticate-client.md).

* Erfahren Sie mehr über [RBAC für Azure](../role-based-access-control/overview.md).
