---
title: 'Tutorial: Konfigurieren von Hootsuite für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD automatisch für Hootsuite bereitstellen und die Bereitstellung wieder aufheben.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fbbee24e-272f-4fa6-819c-10c548bf0215
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2020
ms.author: Zhchia
ms.openlocfilehash: 1adbb0961ab610db936107f2fff210f4acecf2ed
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83882973"
---
# <a name="tutorial-configure-hootsuite-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Hootsuitefür die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie sowohl in Hootsuite als auch in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren. Nach der Konfiguration stellt Azure AD mithilfe des Azure AD-Bereitstellungsdiensts automatisch Benutzer und Gruppen für [Hootsuite](https://hootsuite.com/) bereit bzw. hebt die Bereitstellung von Benutzern und Gruppen auf. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="capabilities-supported"></a>Unterstützte Funktionen
> [!div class="checklist"]
> * Erstellen von Benutzern in Hootsuite
> * Entfernen von Benutzern aus Hootsuite, wenn diese keinen Zugriff mehr benötigen
> * Synchronisieren von Benutzerattributen zwischen Azure AD und Hootsuite
> * Bereitstellen von Gruppen und Gruppenmitgliedschaften in Hootsuite
> * [Einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial) bei Hootsuite (empfohlen)

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* [Azure AD-Mandant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Ein Benutzerkonto in Azure AD mit der [Berechtigung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für die Konfiguration von Bereitstellungen (z.B. Anwendungsadministrator, Cloudanwendungsadministrator, Anwendungsbesitzer oder Globaler Administrator). 
* Ein Benutzerkonto bei [Hootsuite](http://www.hootsuite.com/) mit der Berechtigung **Manage Member** (Mitglied verwalten) für die Organisation.

## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) einbezogen werden soll.
3. Legen Sie fest, welche Daten [zwischen Azure AD und Hootsuite zugeordnet werden sollen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-hootsuite-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren von Hootsuite für die Unterstützung der Bereitstellung mit Azure AD

Wenden Sie sich an dev.support@hootsuite.com, um ein langfristiges geheimes Token zu erhalten, das in späteren Schritten benötigt wird. 

## <a name="step-3-add-hootsuite-from-the-azure-ad-application-gallery"></a>Schritt 3: Hinzufügen von Hootsuite aus dem Azure AD-Anwendungskatalog

Fügen Sie Hootsuite aus dem Azure AD-Anwendungskatalog hinzu, um mit dem Verwalten der Bereitstellung in Hootsuite zu beginnen. Wenn Sie Hootsuite zuvor für einmaliges Anmelden (Single Sign-On, SSO) eingerichtet haben, können Sie dieselbe Anwendung verwenden. Es ist jedoch empfehlenswert, beim erstmaligen Testen der Integration eine separate App zu erstellen. [Hier](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) erfahren Sie mehr über das Hinzufügen einer Anwendung aus dem Katalog. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Schritt 4. Definieren der Benutzer für den Bereitstellungsbereich 

Mit dem Azure AD-Bereitstellungsdienst können Sie anhand der Zuweisung zur Anwendung oder aufgrund von Attributen für den Benutzer/die Gruppe festlegen, wer in die Bereitstellung einbezogen werden soll. Wenn Sie sich dafür entscheiden, anhand der Zuweisung festzulegen, wer für Ihre App bereitgestellt werden soll, können Sie der Anwendung mithilfe der folgenden [Schritte](../manage-apps/assign-user-or-group-access-portal.md) Benutzer und Gruppen zuweisen. Wenn Sie allein anhand der Attribute des Benutzers oder der Gruppe auswählen möchten, wer bereitgestellt wird, können Sie einen [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) beschriebenen Bereichsfilter verwenden. 

* Beim Zuweisen von Benutzern und Gruppen zu Hootsuite müssen Sie eine andere Rolle als **Standardzugriff** auswählen. Benutzer mit der Rolle „Standardzugriff“ werden von der Bereitstellung ausgeschlossen und in den Bereitstellungsprotokollen als „nicht effektiv berechtigt“ gekennzeichnet. Wenn für die Anwendung nur die Rolle „Standardzugriff“ verfügbar ist, können Sie das [Anwendungsmanifest aktualisieren](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) und weitere Rollen hinzufügen. 

* Fangen Sie klein an. Testen Sie die Bereitstellung mit einer kleinen Gruppe von Benutzern und Gruppen, bevor Sie sie für alle freigeben. Wenn der Bereitstellungsbereich auf zugewiesene Benutzer und Gruppen festgelegt ist, können Sie dies durch Zuweisen von einem oder zwei Benutzern oder Gruppen zur App kontrollieren. Ist der Bereich auf alle Benutzer und Gruppen festgelegt, können Sie einen [attributbasierten Bereichsfilter](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) angeben. 


## <a name="step-5-configure-automatic-user-provisioning-to-hootsuite"></a>Schritt 5: Konfigurieren der automatischen Benutzerbereitstellung in Hootsuite 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in ServiceNow auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

### <a name="to-configure-automatic-user-provisioning-for-hootsuite-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Hootsuite in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](./media/hootsuite-provisioning-tutorial/enterprise-applications.png)

    ![Blatt „Alle Anwendungen“](./media/hootsuite-provisioning-tutorial/all-applications.png)

2. Wählen Sie in der Anwendungsliste **Hootsuite** aus.

    ![Hootsuite-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**. Klicken Sie auf **Erste Schritte**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

    ![Blatt „Erste Schritte“](./media/hootsuite-provisioning-tutorial/get-started.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld „Mandanten-URL“ `https://platform.hootsuite.com/scim/v2` ein. Geben Sie den Wert des langfristigen geheimen Tokens ein, den Sie zuvor in **Schritt 2** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Hootsuite herstellen kann. Wenn die Verbindung nicht möglich ist, müssen Sie sicherstellen, dass Ihr Hootsuite-Konto über Administratorberechtigungen verfügt. Versuchen Sie es anschließend noch mal.

    ![Bereitstellung](./media/hootsuite-provisioning-tutorial/provisioning.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer bereitstellen** aus.

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Hootsuite synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Hootsuite für Aktualisierungsvorgänge verwendet werden. Wenn Sie sich dafür entscheiden, das [übereinstimmende Zielattribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) zu ändern, müssen Sie sicherstellen, dass die Hootsuite-API das Filtern von Benutzern anhand dieses Attributs unterstützt. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

   |attribute|type|
   |---|---|
   |userName|String|
   |emails[type eq "work"].value|String|
   |aktiv|Boolean|
   |displayName|String|
   |preferredLanguage|String|
   |timezone|String|
   |urn:ietf:params:scim:schemas:extension:Hootsuite:2.0:User:organizationIds|String|
   |urn:ietf:params:scim:schemas:extension:Hootsuite:2.0:User:teamIds|String|

10. Um den Azure AD-Bereitstellungsdienst für Hootsuite zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

11. Legen Sie die Benutzer und/oder Gruppen fest, die in Hootsuite bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

12. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Durch diesen Vorgang wird der erstmalige Synchronisierungszyklus für alle Benutzer und Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert wurden. Der erste Zyklus dauert länger als nachfolgende Zyklen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

## <a name="step-6-monitor-your-deployment"></a>Schritt 6: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

* Mithilfe der [Bereitstellungsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
* Anhand der [Fortschrittsleiste](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
* Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
