---
title: Häufig gestellte Fragen zu Azure Spring Cloud | Microsoft-Dokumentation
description: In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: f1871871fa3a191c636a965977dba485d2c77f1f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037507"
---
# <a name="azure-spring-cloud-faq"></a>Häufig gestellte Fragen zu Azure Spring Cloud

In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Spring Cloud.

## <a name="general"></a>Allgemein

### <a name="why-azure-spring-cloud"></a>Vorteile von Azure Spring Cloud

Azure Spring Cloud bietet Spring Cloud-Entwicklern eine PaaS-Lösung (Platform-as-a-Service). Azure Spring Cloud verwaltet Ihre Anwendungsinfrastruktur, damit Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können. Zu den wichtigsten in Azure Spring Cloud integrierten Features gehören unter anderem Eureka, Config Server, Dienstregistrierungsserver, Pivotal Build Service, Blau/Grün-Bereitstellung und andere. Mit diesem Dienst können Entwickler außerdem Ihre Anwendungen an andere Azure-Dienste wie Azure Cosmos DB, Azure Database for MySQL und Azure Cache for Redis binden.

Azure Spring Cloud verbessert durch die Integration von Azure Monitor, Application Insights und Log Analytics die Anwendungsdiagnose für Entwickler und Operatoren.

### <a name="how-secure-is-azure-spring-cloud"></a>Wie sicher ist Azure Spring Cloud?

Sicherheit und Datenschutz gehören zu den wichtigsten Prioritäten für Azure- und Azure Spring Cloud-Kunden. Azure gewährleistet durch eine sichere Datenverschlüsselung, dass nur Kunden Zugriff auf Anwendungsdaten, Protokolle oder Konfigurationen haben. 

* In Azure Spring Cloud sind alle Dienstinstanzen voneinander isoliert.
* Azure Spring Cloud bietet eine umfassende TLS/SSL- und Zertifikatverwaltung.
* Wichtige Sicherheitspatches für OpenJDK- und Spring Cloud-Runtimes werden so bald wie möglich auf Azure Spring Cloud angewendet.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>In welchen Regionen ist Azure Spring Cloud verfügbar?

„USA, Osten“, „USA, Westen 2“, „Europa, Westen“ und „Asien, Südosten“.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Welche Einschränkungen sind für Azure Spring Cloud bekannt?

Während dem Vorschaurelease wies Azure Spring Cloud die folgenden Einschränkungen auf:

* `spring.application.name` wird durch den Anwendungsnamen überschrieben, der zum Erstellen der einzelnen Anwendungen verwendet wird.
* Für `server.port` werden standardmäßig die Ports 80/443 verwendet. Wenn ein anderer Wert angewendet wird, wird er in den Wert 80/443 geändert.
* Das Hochladen von Anwendungspaketen wird vom Azure-Portal und von Azure Resource Manager-Vorlagen nicht unterstützt. Sie können Anwendungspakete nur hochladen, indem Sie die Anwendung über die Azure CLI bereitstellen.

### <a name="what-pricing-tiers-are-available"></a>Welche Tarife sind verfügbar? 
Welchen Tarif sollte ich verwenden, und welche Beschränkungen gelten für die einzelnen Tarife?
* Für Azure Spring Cloud gibt es zwei Tarife: Basic und Standard. Der Tarif „Basic“ ist für Dev/Test-Prozesse und den Einstieg in Azure Spring Cloud vorgesehen. Der Tarif „Standard“ ist für anfallenden universellen Produktionsdatenverkehr optimiert. Weitere Informationen zu Beschränkungen sowie einen Vergleich der Features beider Tarife finden Sie unter [Azure Spring Cloud – Preise](https://azure.microsoft.com/pricing/details/spring-cloud/).

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Wie kann ich Feedback geben und Probleme melden?

Wenn bei der Azure Spring Cloud Probleme auftreten, erstellen Sie eine [Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Besuchen Sie [Azure-Feedback](https://feedback.azure.com/forums/34192--general-feedback), um eine Featureanforderung oder Feedback einzureichen.

## <a name="development"></a>Entwicklung

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-azure-spring-cloud-application"></a>Ich bin Spring Cloud-Entwickler, habe aber noch nicht mit Azure gearbeitet. Wie lerne ich am schnellsten, wie ich eine Azure Spring Cloud-Anwendung entwickle?

Für den schnellsten Einstieg mit Azure Spring Cloud führen Sie die Anweisungen im [Schnellstart: Starten einer Azure Spring Cloud-Anwendung über das Azure-Portal](spring-cloud-quickstart-launch-app-portal.md).

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Welche Java-Runtime unterstützt Azure Spring Cloud?

Azure Spring Cloud unterstützt Java 8 und 11. Weitere Informationen finden Sie unter [Java-Runtime und Betriebssystemversionen](#java-runtime-and-os-versions).

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>Wo kann ich meine Spring Cloud-Anwendungsprotokolle und -metriken einsehen?

Metriken finden Sie auf der Registerkarte „App Overview“ (App-Übersicht) sowie der Registerkarte [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics).

Azure Spring Cloud unterstützt das Exportieren von Spring Cloud-Anwendungsprotokollen und -metriken in Azure Storage, EventHub und [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries). Der Tabellenname im Log Analytics lautet *AppPlatformLogsforSpring*. Informationen zum Aktivieren dieser Protokollierung finden Sie unter [Diagnosedienste](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Unterstützt Azure Spring Cloud die verteilte Ablaufverfolgung?

Ja. Weitere Informationen finden Sie im [Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>Welche Ressourcentypen werden von der Dienstbindung unterstützt?

Derzeit werden drei Dienste unterstützt:
* Azure Cosmos DB
* Azure Database for MySQL
* Azure Cache for Redis.

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>Kann ich persistente Volumes innerhalb meiner Anwendungen anzeigen, hinzufügen oder verschieben?

Ja.

### <a name="when-i-deletemove-an-azure-spring-cloud-service-instance-will-its-extension-resources-be-deletedmoved-as-well"></a>Werden beim Löschen oder Verschieben einer Azure Spring Cloud-Dienstinstanz auch die Erweiterungsressourcen gelöscht bzw. verschoben?

Dies hängt von der Logik der Ressourcenanbieter ab, denen die Erweiterungsressourcen gehören. Die Erweiterungsressourcen einer `Microsoft.AppPlatform`-Instanz gehören nicht zum selben Namespace, sodass das Verhalten je nach Ressourcenanbieter variiert. Der Vorgang zum Löschen oder Verschieben wird z. B. nicht an die Ressourcen der **Diagnoseeinstellungen** weitergegeben. Wenn eine neue Azure Spring Cloud-Instanz mit derselben Ressourcen-ID wie die gelöschte Instanz bereitgestellt oder die vorherige Azure Spring Cloud-Instanz zurück verschoben wird, erweitern die vorherigen Ressourcen der **Diagnoseeinstellungen** diese weiterhin.

## <a name="java-runtime-and-os-versions"></a>Java-Runtime und Betriebssystemversionen

### <a name="which-versions-of-java-runtime-are-supported-in-azure-spring-cloud"></a>Welche Versionen der Java-Runtime werden in Azure Spring Cloud unterstützt?

Azure Spring Cloud unterstützt Java LTS-Versionen mit den neuesten Builds. Aktuell, d. h. im Juni 2020, werden Java 8 Build 252 und Java 11 Build 7 unterstützt. Weitere Informationen finden Sie unter [Installieren des JDK für Azure und Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install).

### <a name="who-built-these-java-runtimes"></a>Wer hat diese Java-Runtimeversionen erstellt?

Azul Systems. Bei Enterprise Edition-JDK-Builds von Azul Zulu für Azure handelt es sich um eine kostenlose, plattformübergreifende und produktionsbereite Distribution von OpenJDK für Azure und Azure Stack, die von Microsoft und Azul Systems unterstützt wird. Sie enthält alle Komponenten, die zum Erstellen und Ausführen von Java SE-Anwendungen benötigt werden.

### <a name="how-often-will-java-runtimes-get-updated"></a>Wie oft werden Java-Runtimes aktualisiert?

Für LTS- und MTS-JDK-Releases werden vierteljährliche Sicherheitsupdates, Fehlerbehebungen sowie wichtige Out-of-band-Updates und -Patches nach Bedarf bereitgestellt. Dieser Support umfasst Zurückportierungen von Sicherheitsupdates auf Java 7 und 8 sowie die Behebung von Fehlern, die in neueren Versionen von Java (etwa in Java 11) gemeldet wurden.

### <a name="how-long-will-java-8-and-java-11-lts-versions-be-supported"></a>Wie lange werden Java 8- und Java 11 LTS-Versionen unterstützt?

Weitere Informationen finden Sie unter [Langfristiger Java-Support für Azure und Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support).

* Java 8 LTS wird bis Dezember 2030 unterstützt.
* Java 11 LTS wird bis September 2027 unterstützt.

### <a name="how-can-i-download-a-supported-java-runtime-for-local-development"></a>Wie kann ich eine unterstützte Java-Runtime für die lokale Entwicklung herunterladen?

Weitere Informationen finden Sie unter [Installieren des JDK für Azure und Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install).

### <a name="what-is-the-retire-policy-for-older-java-runtimes"></a>Welche Außerkraftsetzungsrichtlinie gilt für ältere Java-Runtimeversionen?

Der öffentliche Hinweis wird 12 Monate vor dem Auslaufen einer alten Runtimeversion versendet. Sie haben 12 Monate Zeit, um zu einer höheren Version zu migrieren.

* Abonnementadministratoren erhalten eine E-Mail-Benachrichtigung, wenn eine Java-Version eingestellt wird.
* Die Informationen zur Einstellung werden in der Dokumentation veröffentlicht.

### <a name="how-can-i-get-support-for-issues-at-the-java-runtime-level"></a>Wie erhalte ich Support für Probleme auf der Ebene der Java-Runtime?

Sie können beim Azure-Support ein Supportticket öffnen.  Weitere Informationen finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

### <a name="what-is-the-operation-system-to-run-my-apps"></a>Welches Betriebssystem wird zum Ausführen meiner Apps verwendet?

Es wird die neueste Version von Ubuntu LTS verwendet, derzeit ist [Ubuntu 20.04 LTS (Focal Fossa)](https://releases.ubuntu.com/focal/) das Standardbetriebssystem.

### <a name="how-often-will-os-security-patches-be-applied"></a>Wie oft werden Betriebssystem-Sicherheitsupdates angewendet?

Auf Azure Spring Cloud anwendbare Sicherheitsupdates werden monatlich in der Produktionsumgebung eingeführt.
Für kritische Sicherheitsupdates (CVE-Score >= 9), die auf Azure Spring Cloud anwendbar sind, wird so bald wie möglich ein Rollout ausgeführt.

## <a name="deployment"></a>Bereitstellung

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Werden Blau/Grün-Bereitstellungen von Azure Spring Cloud unterstützt?
Ja. Weitere Informationen finden Sie unter [Einrichten einer Stagingumgebung](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Kann ich auf Kubernetes zugreifen, um meine Anwendungscontainer zu bearbeiten?

Nein.  Azure Spring Cloud trennt den Entwickler von der zugrunde liegenden Architektur, sodass Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Unterstützt Azure Spring Cloud das Erstellen von Containern aus einer Quelle?

Ja. Weitere Informationen finden Sie unter [Starten Ihrer Spring Cloud-Anwendung über den Quellcode](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Unterstützt Azure Spring Cloud die automatische Skalierung in App-Instanzen?

Nein.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>Wie lauten die bewährten Methoden für die Migration vorhandener Spring Cloud-Microservices zu Azure Spring Cloud?

Beim Migrieren vorhandener Spring Cloud-Microservices zu Azure Spring Cloud empfiehlt es sich, die folgenden bewährten Methoden zu befolgen:
* Alle Anwendungsabhängigkeiten müssen aufgelöst werden.
* Bereiten Sie Ihre Konfigurationseinträge, Umgebungsvariablen und JVM-Parameter so vor, dass Sie sie mit der Bereitstellung in Azure Spring Cloud vergleichen können.
* Wenn Sie die Dienstbindung verwenden möchten, überprüfen Sie Ihre Azure-Dienste, und stellen Sie sicher, dass Sie die entsprechenden Zugriffsberechtigungen festgelegt haben.
* Es wird empfohlen, dass Sie jegliche eingebettete Dienste, die möglicherweise mit von Azure Spring Cloud verwalteten Diensten wie dem Ermittlungsdienst, dem Konfigurationsserver usw. im Konflikt stehen können, entfernen oder deaktivieren.
* Es wird empfohlen, dass Sie offizielle, stabile Pivotal Spring-Bibliotheken verwenden. Inoffizielle oder verzweigte Versionen und Betaversionen von Pivotal Spring-Bibliotheken haben keine SLA-Unterstützung (Vereinbarung zum Servicelevel).

Überwachen Sie nach der Migration Ihre CPU-/RAM-Metriken und den Netzwerkdatenverkehr, um sicherzustellen, dass die Anwendungsinstanzen entsprechend skaliert werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Fragen haben, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung bei Azure Spring Cloud](spring-cloud-troubleshoot.md).
