---
title: 'Self-Service-Registrierung für externe Identitäten: Azure AD'
description: Hier erfahren Sie, wie Sie es durch Aktivierung der Self-Service-Registrierung externen Benutzern ermöglichen, sich selbst für Ihre Anwendungen zu registrieren. Erstellen Sie eine personalisierte Registrierungsumgebung, indem Sie den Benutzerflow für die Self-Service-Registrierung anpassen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34b08e2e530843dd98c87e424812706247388228
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85551329"
---
# <a name="self-service-sign-up-preview"></a>Self-Service-Registrierung (Vorschauversion)

> [!NOTE]
> Die Self-Service-Registrierung ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie eine Anwendung für externe Benutzer freigeben, wissen Sie möglicherweise nicht immer im Voraus, wer Zugriff auf die Anwendung benötigt. Statt Einladungen direkt an Einzelpersonen zu senden, können Sie es externen Benutzern ermöglichen, sich selbst für bestimmte Anwendungen zu registrieren, indem Sie die Self-Service-Registrierung aktivieren. Sie können eine personalisierte Registrierungsumgebung erstellen, indem Sie den Benutzerflow für die Self-Service-Registrierung anpassen. Beispielsweise können Sie Optionen für die Registrierung bei Azure AD oder Social Media-Identitätsanbietern bereitstellen und während des Registrierungsvorgangs Informationen zu dem Benutzer sammeln.

> [!NOTE]
> Sie können Benutzerflows mit Apps verknüpfen, die von Ihrer Organisation erstellt wurden. Benutzerflows können nicht für Microsoft-Apps wie SharePoint oder Teams verwendet werden.

## <a name="user-flow-for-self-service-sign-up"></a>Benutzerflow für die Self-Service-Registrierung

Ein Benutzerflow für die Self-Service-Registrierung generiert über die freizugebende Anwendung eine Registrierungsumgebung für die externen Benutzer. Der Benutzerflow kann mehreren Anwendungen zugeordnet werden. Zunächst aktivieren Sie die Self-Service-Registrierung für Ihren Mandanten und richten einen Verbund mit den Identitätsanbietern ein, die externe Benutzer für die Anmeldung verwenden dürfen. Anschließend erstellen Sie den Benutzerflow für die Registrierung, passen ihn an und weisen ihm Ihre Anwendungen zu.
Sie können Benutzerfloweinstellungen konfigurieren, um zu steuern, wie sich Benutzer für die Anwendung registriert:

- Für die Anmeldung verwendete Kontotypen (z. B. Konten für soziale Netzwerke wie Facebook) oder Azure AD-Konten
- Attribute, die vom Benutzer bei der Registrierung gesammelt werden, z. B. Vorname, Postleitzahl oder Land/Region des Wohnsitzes

Wenn sich ein Benutzer bei Ihrer Anwendung anmelden möchte, bei der es sich gleichermaßen um eine Web-, Mobil-, Desktop- oder Single-Page-Webanwendung (SPA) handeln kann, initiiert die Anwendung eine Autorisierungsanforderung an den vom Benutzerflow bereitgestellten Endpunkt. Der Benutzerflow definiert und steuert die Funktionalität für die Benutzer. Wenn der Benutzer den Benutzerflow für die Registrierung abschließt, generiert Azure AD ein Token und leitet den Benutzer wieder zurück zur Anwendung. Nach Abschluss der Registrierung wird ein Gastkonto für den Benutzer im Verzeichnis bereitgestellt. Mehrere Anwendungen können denselben Benutzerflow verwenden.

## <a name="example-of-self-service-sign-up"></a>Beispiel für die Self-Service-Registrierung

Im folgenden Beispiel wird veranschaulicht, wie Sie Social Media-Identitätsanbieter in Azure AD mit Self-Service-Registrierungsfunktionen für Gastbenutzer einbinden.  
Ein Partner von Woodgrove öffnet die Woodgrove-App. Er möchte sich für ein Lieferantenkonto registrieren und wählt daher die Option „Request your supplier account“ (Lieferantenkonto anfordern) aus. Dadurch wird der Flow für die Self-Service-Registrierung initiiert.

![Beispiel einer Startseite für die Self-Service-Registrierung](media/self-service-sign-up-overview/example-start-sign-up-flow.png)

Der Benutzer verwendet für die Registrierung eine E-Mail-Adresse seiner Wahl.

![Beispiel für die Auswahl von Facebook für die Anmeldung](media/self-service-sign-up-overview/example-sign-in-with-facebook.png)

Azure AD erstellt mithilfe des Facebook-Kontos des Partners eine Verknüpfung mit Woodgrove und erstellt nach der Registrierung ein neues Gastkonto für den Benutzer.

Woodgrove möchte mehr über den Benutzer erfahren, etwa Name, Unternehmensname, Firmenregistrierungscode und Telefonnummer.

![Beispiel für die Benutzerregistrierungsattribute](media/self-service-sign-up-overview/example-enter-user-attributes.png)

Der Benutzer gibt die Informationen ein, setzt den Registrierungsflow fort und erhält Zugriff auf die benötigten Ressourcen.

![Beispiel für den angemeldeten Benutzer](media/self-service-sign-up-overview/example-signed-in.png)

## <a name="next-steps"></a>Nächste Schritte

 Lesen Sie die ausführlichen Informationen zum [Hinzufügen der Self-Service-Registrierung zu einer App](self-service-sign-up-user-flow.md).