---
title: 'Schnellstart: Einrichten des einmaligen Anmeldens (Single Sign-On, SSO) für eine Anwendung in Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten)'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung in Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten) einrichten.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2f8f19e6b98143bb48430decdd51f5626e72d422
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387284"
---
# <a name="quickstart-set-up-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Schnellstart: Einrichten des einmaligen Anmeldens (Single Sign-On, SSO) für eine Anwendung in Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten)

Beginnen Sie mit vereinfachten Benutzeranmeldungen, indem Sie einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung einrichten, die Sie Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten) hinzugefügt haben. Nachdem Sie SSO eingerichtet haben, können sich Ihre Benutzer mit ihren Azure AD-Anmeldeinformationen bei einer Anwendung anmelden. SSO ist in der kostenlosen Edition von Azure AD enthalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um SSO für eine Anwendung einzurichten, die Sie Ihrem Azure AD-Mandanten hinzugefügt haben:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine der folgenden Rollen: Globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.
- Eine Anwendung, die SSO unterstützt und bereits vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurde. Bei den meisten Apps kann Azure AD für SSO verwendet werden. Die Apps im Azure AD-Katalog sind vorkonfiguriert. Falls Ihre App nicht aufgelistet oder eine benutzerdefinierte App ist, können Sie sie trotzdem mit Azure AD verwenden. Sehen Sie sich die Tutorials und die restliche Dokumentation im Inhaltsverzeichnis an. In dieser Schnellstartanleitung liegt der Schwerpunkt auf Apps, die von den App-Entwicklern für SSO vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurden.
- Optional: Gehen Sie den Schnellstart [Anzeigen Ihrer Apps](view-applications-portal.md) durch.
- Optional: Gehen Sie den Schnellstart [Hinzufügen einer App](add-application-portal.md) durch.
- Optional: Gehen Sie den Schnellstart [Konfigurieren einer App](add-application-portal-configure.md) durch.


>[!IMPORTANT]
>Verwenden Sie zum Testen der in dieser Schnellstartanleitung aufgeführten Schritte keine Produktionsumgebung.


## <a name="enable-single-sign-on-for-an-app"></a>Aktivieren des einmaligen Anmeldens für eine App

Nachdem Sie Ihrem Azure AD-Mandanten eine Anwendung hinzugefügt haben, wird die Übersichtsseite angezeigt. Wenn Sie eine Anwendung konfigurieren, die bereits hinzugefügt wurde, lesen Sie die erste Schnellstartanleitung. Darin wird beschrieben, wie Sie die Ihrem Mandanten hinzugefügten Anwendungen anzeigen können. 

Richten Sie einmaliges Anmelden wie folgt für eine Anwendung ein:

1. Wählen Sie im Azure AD-Portal die Option **Unternehmensanwendungen** aus. Suchen Sie dann nach der Anwendung, für die Sie einmaliges Anmelden einrichten möchten, und wählen Sie diese aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus, um den Bereich **Einmaliges Anmelden** zur Bearbeitung zu öffnen.

    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Screenshot der Seite zum Konfigurieren des einmaligen Anmeldens im Azure AD-Portal":::

1. Wählen Sie **SAML** aus, um die Seite für die SSO-Konfiguration zu öffnen. In diesem Beispiel ist GitHub die Anwendung, für die wir SSO konfigurieren. Nachdem Sie GitHub eingerichtet haben, können sich Ihre Benutzer mit ihren Anmeldeinformationen von Ihrem Azure AD-Mandanten bei GitHub anmelden.

    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Screenshot der Seite zum Konfigurieren des einmaligen Anmeldens für GitHub":::

1. Der Prozess zum Konfigurieren einer Anwendung für die Verwendung von Azure AD für SAML-basiertes SSO variiert je nach Anwendung. Es ist ein Link zur Anleitung für GitHub vorhanden. Anleitungen für andere Apps finden Sie unter [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/).
1. Befolgen Sie die Anleitung zum Einrichten von SSO für die Anwendung. Viele Anwendungen verfügen über spezifische Abonnementanforderungen für die SSO-Funktionen. Beispielsweise ist für GitHub ein Enterprise-Abonnement erforderlich.
    > [!TIP]
    > Weitere Informationen zu den SAML-Konfigurationsoptionen finden Sie unter [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-saml-single-sign-on.md).

    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="Screenshot der GitHub-Preisseite mit der Option für einmaliges Anmelden im Enterprise-Abonnement":::


## <a name="next-step"></a>Nächster Schritt

- [Löschen einer App](delete-application-portal.md)
