---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/05/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: da9388a3bd5f4d46ec34ed226e3ee23a96b2f494
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374786"
---
## <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

Als neuer LUIS-Benutzer müssen Sie wie folgt vorgehen:

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, wählen Sie Ihr Land/Ihre Region aus, und stimmen Sie den Nutzungsbedingungen zu. Wenn stattdessen **Meine Apps** angezeigt wird, ist bereits eine LUIS-Ressource vorhanden, und Sie können mit dem Erstellen einer App fortfahren. Eine Liste der unterstützten Regionen finden Sie unter [Erstellungs- und Veröffentlichungsregionen und die dazugehörigen Schlüssel](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions).

1. Wählen Sie **Azure-Ressource erstellen** und anschließend **Create an authoring resource to migrate your apps to** (Erstellen einer Erstellungsressource für die Migration Ihrer Apps) aus.

    ![Typ der Language Understanding-Erstellungsressource auswählen](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Füllen Sie die Details für Ihre Ressource aus.

    ![Erstellungsressource erstellen](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    Geben Sie beim **Erstellen einer neuen Erstellungsressource** die folgenden Informationen an:

    * **Ressourcenname**: ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird
    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist
    * **Abonnementname**: das Abonnement, unter dem die Ressource abgerechnet wird.
    * **Ressourcengruppe**: ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.
    * **Standort**: Die Auswahl des Standorts hängt von der Auswahl der **Ressourcengruppe** ab.
    * **Tarif**: Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.

1. Eine Zusammenfassung der zu erstellenden Ressource wird angezeigt. Wählen Sie **Weiter** aus.

    ![Erstellungsressource erstellen](../media/sign-in/sign-in-confirm-key-selection.png)

1. Bestätigen Sie durch Auswahl von **Fortfahren**.

    ![Erstellungsressource erstellen](../media/sign-in/sign-in-confirm-continue.png)
