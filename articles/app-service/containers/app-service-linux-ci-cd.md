---
title: CI/CD für benutzerdefinierte Linux-Container
description: Erfahren Sie, wie Sie Continuous Deployment für einen benutzerdefinierten Linux-Container in Azure App Service einrichten. Continuous Deployment wird für Docker Hub und ACR unterstützt.
keywords: Azure App Service, Linux, Docker, ACR, OSS
author: msangapu-msft
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.topic: article
ms.date: 11/08/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: d43491de7500204ed470757a1b744017a8180b57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687637"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Continuous Deployment mit Web-App für Container

In diesem Tutorial konfigurieren Sie Continuous Deployment für ein benutzerdefiniertes Containerimage aus verwalteten [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)-Repositorys oder aus [Docker Hub](https://hub.docker.com).

## <a name="enable-continuous-deployment-with-acr"></a>Aktivieren von Continuous Deployment mit ACR

![Screenshot des ACR-Webhooks](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-02.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der Seite links die Option **App Service** .
3. Klicken Sie auf den Namen der App, für die Sie Continuous Deployment konfigurieren möchten.
4. Wählen Sie auf der Seite **Containereinstellungen** die Option **Einzelner Container** aus.
5. Wählen Sie **Azure Container Registry** aus.
6. Wählen Sie **Continuous Deployment > Ein**.
7. Wählen Sie **Speichern**, um Continuous Deployment zu aktivieren.

## <a name="use-the-acr-webhook"></a>Verwenden des ACR-Webhooks

Nachdem Continuous Deployment aktiviert wurde, können Sie den neu erstellten Webhook auf Ihrer Webhooks-Seite in Azure Container Registry anzeigen.

![Screenshot des ACR-Webhooks](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-03.png)

Klicken Sie in Ihrer Container Registry-Instanz auf „Webhooks“, um die aktuellen Webhooks anzuzeigen.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Aktivieren von Continuous Deployment mit Docker Hub (optional)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der Seite links die Option **App Service** .
3. Klicken Sie auf den Namen der App, für die Sie Continuous Deployment konfigurieren möchten.
4. Wählen Sie auf der Seite **Containereinstellungen** die Option **Einzelner Container** aus.
5. Wählen Sie **Docker Hub**.
6. Wählen Sie **Continuous Deployment > Ein**.
7. Wählen Sie **Speichern**, um Continuous Deployment zu aktivieren.

![Screenshot der App-Einstellung](./media/app-service-webapp-service-linux-ci-cd/ci-cd-docker-02.png)

Kopieren Sie die Webhook-URL. Zum Hinzufügen eines Webhooks für Docker Hub gehen Sie gemäß der Anleitung unter <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">Webhooks for Docker Hub</a> (Webhooks für Docker Hub) vor.

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure App Service unter Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Erstellen einer .NET Core-Web-App in App Service unter Linux](quickstart-dotnetcore.md)
* [Erstellen einer Ruby-App in App Service unter Linux](quickstart-ruby.md)
* [Bereitstellen einer Docker/Go-Web-App über die Web-App für Container](quickstart-docker-go.md)
* [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](./app-service-linux-faq.md)
* [Verwalten von Web-App für Container mithilfe der Azure CLI](./app-service-linux-cli.md)
