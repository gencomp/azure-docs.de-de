---
title: Secure Score in Azure Security Center
description: Beschreibung des Secure Scores von Azure Security Center und seiner Sicherheitskontrollen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2020
ms.author: memildin
ms.openlocfilehash: 1956090f1b927d9fd4e205eaa57bf1952cb1b070
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083989"
---
# <a name="enhanced-secure-score-in-azure-security-center"></a>Erweiterte Sicherheitsbewertung in Azure Security Center

## <a name="introduction-to-secure-score"></a>Einführung in Secure Score

Azure Security Center hat zwei Hauptziele: Unterstützung beim Ermitteln Ihrer aktuellen Sicherheitssituation sowie beim effizienten und effektiven Verbessern Ihrer Sicherheit. Der zentrale Aspekt von Security Center, mit dem Sie diese Ziele erreichen können, ist Secure Score.

Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko.

Die Seite „Secure Score“ von Security Center umfasst Folgendes:

- **Score:** Die Sicherheitsbewertung wird als Prozentwert angezeigt, die zugrunde liegenden Werte sind aber ebenfalls klar:

    [![Sicherheitsbewertung als Prozentwert mit deutlich erkennbaren zugrunde liegenden Zahlen](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)

- **Sicherheitskontrollen:** Jede Sicherheitskontrolle ist eine logische Gruppe verwandter Sicherheitsempfehlungen, die anfällige Angriffsflächen widerspiegelt. Eine Sicherheitskontrolle umfasst eine Reihe von Sicherheitsempfehlungen mit Anweisungen, mit denen Sie diese Empfehlungen implementieren können. Ihre Bewertung verbessert sich nur, wenn Sie *alle* Empfehlungen für eine einzelne Ressource innerhalb einer Sicherheitskontrolle umsetzen.

    Um sofort zu sehen, wie gut jede einzelne Angriffsfläche in Ihrer Organisation geschützt ist, sehen Sie sich die Bewertungen für die einzelnen Sicherheitskontrollen an.

    Weitere Informationen finden Sie unter [Berechnen Ihrer Sicherheitsbewertung](secure-score-security-controls.md#how-your-secure-score-is-calculated). 


>[!TIP]
> Frühere Versionen von Security Center verteilten Punkte auf Empfehlungsebene: Wenn Sie eine Empfehlung für eine einzelne Ressource umgesetzt hatten, wurde Ihre Sicherheitsbewertung verbessert. Nun wird die Bewertung nur verbessert, wenn Sie *alle* Empfehlungen für eine einzelne Ressource innerhalb einer Sicherheitskontrolle umsetzen. Das bedeutet, dass sich Bewertung nur verbessert, wenn die Sicherheit einer Ressource verbessert wird.


## <a name="accessing-your-secure-score"></a>Zugriff auf Ihre Sicherheitsbewertung

Ihre Sicherheitsbewertung insgesamt sowie Ihre Bewertung pro Abonnement können Sie im Azure-Portal finden oder programmatisch mit der Azure Security Center-REST-API abrufen.

### <a name="getting-your-secure-score-from-the-portal"></a>Abrufen ihrer Sicherheitsbewertung aus dem Portal

Security Center zeigt Ihre Bewertung im Portal hervorgehoben an. Sie ist auf der Übersichtsseite als Erstes zu sehen. Wenn Sie sich zur dedizierten Seite von Secure Score durchklicken, wird die Bewertung nach Abonnement aufgeschlüsselt angezeigt. Klicken Sie auf ein einzelnes Abonnement, um die detaillierte Liste mit priorisierten Empfehlungen und die möglichen Auswirkungen anzuzeigen, die eine Umsetzung dieser Empfehlungen auf die Bewertung des Abonnements hat.

![Sicherheitsbewertung insgesamt, wie im Portal angezeigt](media/secure-score-security-controls/single-secure-score-via-ui.png)

### <a name="getting-your-secure-score-from-the-rest-api"></a>Abrufen ihrer Sicherheitsbewertung über die REST-API

Sie können auf Ihre Bewertung über die [Sicherheitsbewertungs-API](https://docs.microsoft.com/rest/api/securitycenter/securescores/) (derzeit in der Vorschau) zugreifen. Die API-Methoden bieten die Flexibilität, die Daten abzufragen und im Laufe der Zeit einen eigenen Berichtsmechanismus für Ihre Sicherheitsbewertungen zu erstellen. Beispielsweise können Sie die API **Sicherheitsbewertungen** verwenden, um die Bewertung für ein bestimmtes Abonnement zu erhalten. Darüber hinaus können Sie die API **Sicherheitsbewertungs-Steuerelemente** verwenden, um die Sicherheitskontrollen und die aktuelle Bewertung Ihrer Abonnements aufzulisten.

![Abrufen einer einzelnen Sicherheitsbewertung über die API](media/secure-score-security-controls/single-secure-score-via-api.png)

Beispiele für Tools, die auf der Sicherheitsbewertungs-API basieren, finden Sie im [Bereich zu den Sicherheitsbewertungen in unserer GitHub-Community](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score). 

## <a name="how-your-secure-score-is-calculated"></a>Berechnen Ihrer Sicherheitsbewertung 

Der jeweilige Beitrag der einzelnen Sicherheitskontrollen zur gesamten Sicherheitsbewertung ist auf der Seite mit Empfehlungen eindeutig angegeben.

[![Mit dem erweiterten Secure Score (Vorschauversion) werden Sicherheitskontrollen eingeführt](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Um alle zu erzielenden Punkte für eine Sicherheitskontrolle zu erhalten, müssen alle Ihre Ressourcen allen Sicherheitsempfehlungen innerhalb der Sicherheitskontrolle entsprechen. Beispielsweise enthält Security Center mehrere Empfehlungen zum Schützen Ihrer Verwaltungsports. In der Vergangenheit konnten Sie einige dieser zusammengehörigen und voneinander abhängigen Empfehlungen umsetzen, während andere nicht berücksichtigt wurden, und Ihre Sicherheitsbewertung verbesserte sich trotzdem. Bei objektiver Betrachtung kann man sagen, dass sich die Sicherheit erst dann verbessert hat, wenn alle Empfehlungen umgesetzt wurden. Zum jetzigen Zeitpunkt müssen alle Empfehlungen umgesetzt werden, um eine Änderung bei der Sicherheitsbewertung zu erzielen.

Beispielsweise wird die Sicherheitskontrolle „Systemupdates anwenden“ mit maximal sechs Punkten bewertet. Dieser Wert ist in der QuickInfo zur potenziellen Steigerung der Sicherheitskontrolle angegeben:

[![Sicherheitskontrolle „Systemupdates anwenden“](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Der maximal mögliche Wert für das Steuerelement „Systemupdates anwenden“ ist immer 6. In diesem Beispiel gibt es 50 Ressourcen. Der maximale Wert wird daher durch 50 geteilt, woraus sich ergibt, dass jede Ressource 0,12 Punkte beiträgt. 

* **Potenzielle Steigerung** (0,12 x 8 fehlerhafte Ressourcen = 0,96): Die Anzahl der noch verfügbaren Punkte des Steuerelements. Wenn Sie alle Empfehlungen für dieses Steuerelement umsetzen, steigt die Bewertung um 2 % (in diesem Fall um 0,96 Punkte, die auf 1 Punkt aufgerundet werden). 
* **Aktuelle Bewertung** (0,12 x 42 fehlerfreie Ressourcen = 5,04): Die aktuelle Bewertung für dieses Steuerelement. Jedes Kontrollelement trägt zur Gesamtbewertung bei. In diesem Beispiel trägt das Steuerelement 5,04 Punkte zur aktuellen Gesamtbewertung bei.
* **Maximale Bewertung**: Die maximale Anzahl von Punkten, die Sie durch Erfüllen aller Empfehlungen innerhalb eines Kontrollelements erhalten können. Die maximale Bewertung für ein Kontrollelement gibt die relative Bedeutung dieses Kontrollelements an. Verwenden Sie die maximale Bewertung, um die Probleme zu selektieren, die zuerst behandelt werden müssen. 


### <a name="calculations---understanding-your-score"></a>Berechnungen – Verstehen der Bewertung

|Metrik|Formel und Beispiel|
|-|-|
|**Aktuelle Bewertung der Sicherheitskontrolle**|<br>![Gleichung zum Berechnen der aktuellen Bewertung einer Sicherheitskontrolle](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>Jede einzelne Sicherheitskontrolle trägt zur Sicherheitsbewertung bei. Jede Ressource, die von einer Empfehlung innerhalb des Kontrollelements betroffen ist, trägt zur aktuellen Bewertung des Kontrollelements bei. Die aktuelle Bewertung für jedes Kontrollelement ist ein Maß für den Status der Ressourcen *innerhalb* des Kontrollelements.<br>![QuickInfos mit den Werten, die beim Berechnen der aktuellen Bewertung der Sicherheitskontrolle verwendet werden](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>In diesem Beispiel wird die maximale Bewertung von 6 durch 78 dividiert, da dies die Summe aus fehlerfreien und fehlerhaften Ressourcen ist.<br>6 / 78 = 0,0769<br>Die Multiplikation dieses Ergebnisses mit der Anzahl der fehlerfreien Ressourcen (4) ergibt die aktuelle Bewertung:<br>0,0769 * 4 = **0,31**<br><br>|
|**Sicherheitsbewertung**<br>Ein Abonnement|<br>![Gleichung zum Berechnen der aktuellen Sicherheitsbewertung](media/secure-score-security-controls/secure-score-equation.png)<br><br>![Sicherheitsbewertung eines einzelnen Abonnements, wenn alle Kontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>In diesem Beispiel gibt es ein einzelnes Abonnement, für das alle Sicherheitskontrollen verfügbar sind (potenzielle Höchstbewertung sind 60 Punkte). Die Bewertung gibt 28 Punkte der möglichen 60 Punkte an. Die verbleibenden 32 Punkte sind in den Zahlen für die potenzielle Bewertungssteigerung der Sicherheitskontrollen enthalten.<br>![Liste der Kontrollen und potenziellen Bewertungssteigerung](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Sicherheitsbewertung**<br>Mehrere Abonnements|<br>Die aktuellen Bewertungen für alle Ressourcen in allen Abonnements werden addiert. Der anschließende Berechnungsweg entspricht dem für ein einzelnes Abonnement.<br><br>Beim Anzeigen mehrerer Abonnements werden von Secure Score alle Ressourcen in allen aktivierten Richtlinien ausgewertet und nach deren kombinierter Auswirkung auf die Höchstbewertung der einzelnen Sicherheitskontrollen gruppiert.<br>![Sicherheitsbewertung für mehrere Abonnements, wenn alle Sicherheitskontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Die kombinierte Bewertung ist **kein** Durchschnittswert, sondern vielmehr die Auswertung des Status aller Ressourcen in allen Abonnements.<br>Wenn Sie die Seite mit den Empfehlungen aufrufen und die potenziell verfügbaren Punkte addieren, werden Sie feststellen, dass es sich hierbei um die Differenz zwischen der aktuellen Bewertung (24) und der verfügbaren Höchstbewertung (60) handelt.|
||||

## <a name="improving-your-secure-score"></a>Verbessern Ihrer Sicherheitsbewertung

Zum Verbessern Ihrer Sicherheitsbewertung setzen Sie die Sicherheitsempfehlungen in Ihrer Empfehlungsliste um. Sie können jede Empfehlung manuell für die einzelnen Ressourcen umsetzen oder die Option **Schnelle Problembehebung** verwenden (falls verfügbar), um eine Empfehlung für eine Gruppe von Ressourcen schnell anzuwenden. Weitere Informationen finden Sie unter [Umsetzen von Empfehlungen](security-center-remediate-recommendations.md).

>[!IMPORTANT]
> Nur integrierte Empfehlungen haben eine Auswirkung auf die Sicherheitsbewertung.


## <a name="security-controls-and-their-recommendations"></a>Sicherheitskontrollen und deren Empfehlungen

In der folgenden Tabelle sind die Sicherheitskontrollen in Azure Security Center aufgelistet. Für jede Sicherheitskontrolle ist die maximale Anzahl von Punkten angegeben, die Ihrer Sicherheitsbewertung hinzugefügt wird, wenn Sie *alle* in der Sicherheitskontrolle aufgeführten Empfehlungen für *alle* Ihre Ressourcen umsetzen. 

<div class="foo">

<style type="text/css"> .tg  {border-collapse:collapse;border-spacing:0;} .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px; overflow:hidden;padding:10px 5px;word-break:normal;} .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:18px; font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;} .tg .tg-cly1{text-align:left;vertical-align:middle} .tg .tg-lboi{border-color:inherit;text-align:left;vertical-align:middle} </style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-cly1"><b>Sicherheitssteuerelement, Sicherheitsbewertung und Beschreibung</b><br></th>
    <th class="tg-cly1"><b>Empfehlungen</b></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">MFA aktivieren (max. Bewertung: 10)</p></strong>Wenn Sie zum Authentifizieren von Benutzern nur ein Kennwort nutzen, kann dies einen Angriffsvektor darstellen. Falls das Kennwort nicht sicher ist oder offengelegt wurde, können Sie nicht sicher sein, ob es wirklich der Benutzer ist, der sich mit dem Benutzernamen und dem Kennwort anmeldet.<br>Wenn <a href="https://www.microsoft.com/security/business/identity/mfa">Multi-Factor Authentication</a> (MFA) aktiviert ist, sind Ihre Konten sicherer, und Benutzer können sich weiterhin bei nahezu allen Anwendungen mit der einmaligen Anmeldung (Single Sign-On, SSO) authentifizieren.</td>
    <td class="tg-lboi"; width=55%>- Für Konten mit Besitzerberechtigungen für Ihr Abonnement sollte MFA aktiviert sein.<br>- Für Konten mit Schreibberechtigungen für Ihr Abonnement sollte MFA aktiviert sein.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Secure management ports (Verwaltungsports sichern) (max. Bewertung: 8)</p></strong>Bei Brute-Force-Angriffen wird versucht, über Verwaltungsports Zugriff auf einen virtuellen Computer zu erlangen. Da die Ports nicht immer offen sein müssen, besteht eine Strategie zur Risikominderung darin, die Gefährdung der Ports mithilfe von Just-in-Time-Netzwerkzugriffssteuerungen, Netzwerksicherheitsgruppen (Network Security Groups, NSGs) und mithilfe der VM-Portverwaltung zu verringern.<br>Da viele IT-Organisationen die aus ihrem Netzwerk ausgehende SSH-Kommunikation nicht blockieren, können Angreifer verschlüsselte Tunnel erstellen, über die RDP-Ports in infizierten Systemen mit dem Angreiferbefehl kommunizieren können, um Server zu steuern. Über das Subsystem der Windows-Remoteverwaltung können Angreifer sich seitlich durch Ihre Umgebung bewegen und mithilfe gestohlenen Anmeldeinformationen auf weitere Ressourcen in einem Netzwerk zugreifen.</td>
    <td class="tg-lboi"; width=55%>- Verwaltungsports virtueller Computer müssen mit der Just-In-Time-Netzwerkzugriffssteuerung geschützt werden<br>- Virtuelle Computer sollten einer NSG zugeordnet werden.<br>- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Systemupdates anwenden (max. Bewertung: 6)</p></strong>Systemupdates bieten Organisationen die Möglichkeit, betriebliche Effizienz aufrechtzuerhalten, Sicherheitsrisiken zu verringern und eine stabilere Umgebung für Endbenutzer bereitzustellen. Werden Updates versäumt, bleiben Sicherheitsrisiken ungepatcht, und die Umgebungen werden anfällig für Angriffe. Diese Sicherheitslücken können ausgenutzt werden und zu Datenverlusten, Datenexfiltration, Ransomware und Ressourcenmissbrauch führen. Für das Bereitstellen von Systemupdates können Sie die <a href="https://docs.microsoft.com/azure/automation/automation-update-management">Updateverwaltungslösung verwenden, um Patches und Updates für Ihre virtuellen Computer zu verwalten</a>. Die Updateverwaltung ist der Vorgang, der die Bereitstellung und Wartung von Softwarereleases steuert.</td>
    <td class="tg-lboi"; width=55%>- Integritätsprobleme im Überwachungs-Agent sollten auf Ihren Computern behoben werden.<br>- Der Überwachungs-Agent sollte in VM-Skalierungsgruppen installiert werden.<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.<br>- Die Betriebssystemversion sollte für Ihre Clouddienstrollen aktualisiert werden.<br>- Systemupdates für VM-Skalierungsgruppen sollten installiert werden.<br>- Systemupdates sollten auf Ihren Computern installiert werden.<br>- Ihre Computer sollten neu gestartet werden, damit Systemupdates wirksam werden.<br>- Kubernetes-Dienste sollten auf eine Kubernetes-Version ohne Sicherheitsrisiko upgegradet werden.<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden.<br>- Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein (Vorschau)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Remediate vulnerabilities (Sicherheitsrisiken beseitigen) (max. Bewertung: 6)</p></strong>Ein Sicherheitsrisiko ist eine Schwachstelle, die ein Angreifer nutzen kann, um die Vertraulichkeit, Verfügbarkeit oder Integrität einer Ressource zu kompromittieren. Durch das <a href="https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/next-gen-threat-and-vuln-mgt">Verwalten von Sicherheitsrisiken</a> lässt sich die Anfälligkeit von Organisationen verringern, die Endpunktoberfläche härten, die Resilienz von Organisationen erhöhen und die Angriffsfläche von Ressourcen verkleinern. Die Funktion „Bedrohungs- und Sicherheitsrisikomanagement“ ermöglicht Einblicke in Fehlkonfigurationen von Software und Sicherheitsfunktionen und bietet Empfehlungen zur Risikominderung.</td>
    <td class="tg-lboi"; width=55%>- Advanced Data Security sollte auf Ihren SQL-Datenbank-Instanzen aktiviert sein.<br>- Sicherheitsrisiken in Azure Container Registry-Images sollten behoben werden.<br>- Sicherheitsrisiken in Ihren SQL-Datenbanken sollten entschärft werden.<br>- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung entschärft werden.<br>- Für SQL Managed Instance muss eine Sicherheitsrisikobewertung aktiviert sein.<br>- Für Ihre SQL Server-Instanzen sollte die Sicherheitsrisikobewertung aktiviert sein.<br>- Die Lösung zur Sicherheitsrisikobewertung sollte auf Ihren virtuellen Computern installiert werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Enable encryption at rest (Verschlüsselung ruhender Daten aktivieren) (max. Bewertung: 4)</p></strong>Die <a href="https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest">Verschlüsselung ruhender Daten</a> bietet Schutz für gespeicherte Daten. Zu Angriffen auf ruhende Daten zählen auch Versuche, physischen Zugriff auf die Hardware zu erhalten, auf der die Daten gespeichert sind. In Azure werden große Mengen von ruhenden Daten symmetrisch ver- und entschlüsselt. Ein symmetrischer Verschlüsselungsschlüssel wird zur Verschlüsselung von Daten verwendet, wenn diese in Speicher geschrieben werden. Diese Daten werden auch mit diesem Verschlüsselungsschlüssel entschlüsselt, wenn diese auf den Gebrauch im Arbeitsspeicher vorbereitet werden. Schlüssel müssen an einem sicheren Standort mit identitätsbasierter Zugriffssteuerung und Überwachungsrichtlinien gespeichert werden. Ein solcher sicherer Speicherort ist Azure Key Vault. Wenn ein Angreifer sich Zugriff auf verschlüsselte Daten verschafft, aber nicht über die Verschlüsselungsschlüssel verfügt, kann der Angreifer die Daten nur lesen, wenn er die Verschlüsselung knackt.</td>
    <td class="tg-lboi"; width=55%>- Auf virtuellen Computern sollte die Datenträgerverschlüsselung angewendet werden.<br>- Für SQL-Datenbank sollte TDE (Transparent Data Encryption) aktiviert sein<br>- Automation-Kontovariablen sollten verschlüsselt werden.<br>- Für Service Fabric-Cluster sollte die Eigenschaft „ClusterProtectionLevel“ auf „EncryptAndSign“ festgelegt sein.<br>- SQL Server-TDE-Schutz sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px"> Encrypt data in transit (Daten bei der Übertragung verschlüsseln) (max. Bewertung: 4)</p></strong>Bei der Übertragung“ bedeutet, dass Daten gerade zwischen Komponenten, Speicherorten oder Programmen übertragen werden. Organisationen, die ihre Daten während der Übertragung nicht schützen, sind anfälliger für Man-in-the-Middle-Angriffe, Abhöraktionen und Session Hijacking. Beim Austausch von Daten sollten SSL-/TLS-Protokolle verwendet werden. Außerdem ist ein VPN ratsam. Wenn Sie verschlüsselte Daten zwischen einem virtuellen Azure-Computer und einem lokalen Standort über das Internet senden, können Sie ein virtuelles Netzwerkgateway wie <a href="https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways">Azure VPN Gateway</a> verwenden.</td>
    <td class="tg-lboi"; width=55%>- Die API-App sollte nur über HTTPS zugänglich sein.<br>- Die Funktions-App sollte nur über HTTPS zugänglich sein.<br>- Für Ihren Redis Cache sollten nur sichere Verbindungen aktiviert sein.<br>- Für Speicherkonten sollte die sichere Übertragung aktiviert sein.<br>- Webanwendungen sollten nur über HTTPS zugänglich sein.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px"> Manage access and permissions (Zugriff und Berechtigungen verwalten) (max. Bewertung: 4)</p></strong>Ein zentraler Bestandteil von Sicherheitsprogrammen ist es, sicherzustellen, dass Ihre Benutzer über die für ihre Aufgaben erforderlichen Zugriffsberechtigungen verfügen, und gleichzeitig das <a href="https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models">Modell des geringstprivilegierten Zugriffs</a> umzusetzen.<br>Sie steuern den Zugriff auf Ihre Ressourcen, indem Sie mit <a href="https://docs.microsoft.com/azure/role-based-access-control/overview">rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)</a> Rollenzuweisungen erstellen. Eine Rollenzuweisung besteht aus drei Elementen:<br>- <strong>Sicherheitsprinzipal:</strong> das Objekt, auf das der Benutzer Zugriff anfordert<br>- <strong>Rollendefinition:</strong> die Berechtigungen<br>- <strong>Umfang:</strong> die Ressourcen, für die die Berechtigungen gelten</td>
    <td class="tg-lboi"; width=55%>- Veraltete Konten sollten aus Ihrem Abonnement entfernt werden (Vorschau).<br>- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden (Vorschau).<br>- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden (Vorschau).<br>- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden (Vorschau).<br>- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.<br>- Für Kubernetes-Dienste sollte die rollenbasierte Zugriffssteuerung verwendet werden (Vorschau).<br>- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Sicherheitskonfigurationen korrigieren (max. Bewertung: 4)</p></strong>Falsch konfigurierte IT-Ressourcen haben ein höheres Angriffsrisiko. Grundlegende Härtungsmaßnahmen werden häufig vergessen, wenn Ressourcen bereitgestellt und Fristen erfüllt werden müssen. Sicherheitsfehlkonfigurationen können auf jeder Infrastrukturebene vorliegen: in den Betriebssystemen und Netzwerkgeräten bis hin zu den Cloudressourcen.<br>Azure Security Center vergleicht die Konfiguration Ihrer Ressourcen kontinuierlich mit den Anforderungen von Branchenstandards, Vorschriften und Benchmarks. Wenn Sie die relevanten Compliancepakete (Standards und Baselines) konfiguriert haben, die für Ihre Organisation relevant sind, werden zu allen Lücken Sicherheitsempfehlungen gegeben, die die CCEID und eine Erläuterung der möglichen Sicherheitsauswirkungen enthalten.<br>Häufig verwendete Pakete sind <a href="https://docs.microsoft.com/azure/security/benchmarks/introduction">Azure Security Benchmark</a> und <a href="https://www.cisecurity.org/benchmark/azure/">CIS Microsoft Azure Foundations Benchmark, Version 1.1.0</a>.</td>
    <td class="tg-lboi"; width=55%>- Podsicherheitsrichtlinien sollten in Kubernetes-Diensten definiert sein.<br>- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.<br>- Sicherheitsrisiken in der Sicherheitskonfiguration auf Ihren Computern sollten entschärft werden.<br>- Sicherheitsrisiken in der Sicherheitskonfiguration Ihrer VM-Skalierungsgruppen sollten entschärft werden.<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden.<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.<br>- Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Der Überwachungs-Agent sollte in VM-Skalierungsgruppen installiert werden.<br>- Integritätsprobleme im Überwachungs-Agent sollten auf Ihren Computern behoben werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Restrict unauthorized network access (Nicht autorisierten Netzwerkzugriff einschränken) (max. Bewertung: 4)</p></strong>Endpunkte in einer Organisation stellen eine direkte Verbindung von Ihrem virtuellen Netzwerk zu unterstützten Azure-Diensten bereit. Virtuelle Computer in einem Subnetz können mit allen Ressourcen kommunizieren. Wenn Sie die bidirektionale Kommunikation aller Ressourcen in einem Subnetz einschränken möchten, müssen Sie eine NSG erstellen und sie dem Subnetz zuordnen. Organisationen können unautorisierten Datenverkehr einschränken und sich davor schützen, indem sie Eingans- und Ausgangsregeln erstellen.</td>
    <td class="tg-lboi"; width=55%>- Die IP-Weiterleitung sollte auf Ihrem virtuellen Computer deaktiviert werden.<br>- Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden (Vorschau).<br>- (VERALTET) Der Zugriff auf App Services sollte eingeschränkt werden (Vorschau).<br>- (VERALTET) Die Regeln für Webanwendungen in IaaS-NSGs sollten verstärkt werden.<br>- Virtuelle Computer sollten einer NSG zugeordnet werden.<br>- CORS sollte nicht allen Ressourcen Zugriff auf Ihre API-App gestatten.<br>- CORS sollte nicht jeder Ressource Zugriff auf Ihre Funktions-App erteilen.<br>- CORS sollte nicht jeder Ressource Zugriff auf Ihre Webanwendungen erteilen.<br>- Das Remotedebuggen sollte für die API-App deaktiviert sein.<br>- Das Remotedebuggen sollte für die Funktions-App deaktiviert sein.<br>- Das Remotedebuggen sollte für die Webanwendung deaktiviert sein.<br>- Der Zugriff sollte für freizügige NSGs mit virtuellen Computern mit Internetzugriff eingeschränkt werden.<br>- Für virtuelle Computer mit Internetzugriff sollten die NSG-Regeln verstärkt werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Apply adaptive application control (Adaptive Anwendungssteuerung anwenden) (max. Bewertung: 3)</p></strong>Die adaptive Anwendungssteuerung ist eine intelligente, automatisierte End-to-End-Lösung, mit der Sie steuern können, welche Anwendungen auf Ihren Computern mit oder ohne Azure ausgeführt werden können. Sie trägt außerdem dazu bei, Ihre Computer gegen Malware zu schützen.<br>Das Security Center verwendet maschinelles Lernen, um eine Liste mit bekannten, sicheren Anwendungen für eine Gruppe von Computern zu erstellen.<br>Dieser innovative Ansatz für das Auflisten genehmigter Anwendungen bietet Sicherheitsvorteile ohne Verwaltungsaufwand.<br>Die adaptive Anwendungssteuerung ist besonders relevant für zweckgebundene Server, die bestimmte Anwendungen ausführen müssen.</td>
    <td class="tg-lboi"; width=55%>- Auf virtuellen Computern sollte die adaptive Anwendungssteuerung aktiviert sein.<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden.<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.<br>- Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Integritätsprobleme im Überwachungs-Agent sollten auf Ihren Computern behoben werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Apply data classification (Datenklassifizierung anwenden) (max. Bewertung: 2)</p></strong>Wenn Sie die Daten Ihrer Organisation nach Vertraulichkeit und Geschäftsauswirkungen klassifizieren, können Sie ermitteln, welche Daten die Strategie und die Grundlage für Governance bestimmen, und diesen Daten einen Wert zuweisen.<br><a href="https://docs.microsoft.com/azure/information-protection/what-is-information-protection">Azure Information Protection</a> ist ein nützliches Tool für die Datenklassifizierung. Dieser Dienst verwendet Verschlüsselungs-, Identitäts- und Autorisierungsrichtlinien, um Daten zu schützen und den Datenzugriff einzuschränken. Einige Klassifizierungen, die Microsoft verwendet, sind „Non-business“ (Nicht geschäftlich), „Public“ (Öffentlich), „Allgemein“, „Vertraulich“ und „Streng vertraulich“.</td>
    <td class="tg-lboi"; width=55%>- Vertrauliche Daten in Ihren SQL-Datenbanken sollten klassifiziert werden (Vorschau).</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Protect applications against DDoS attacks (Anwendungen vor DDoS-Angriffen schützen) (max. Bewertung: 2)</p></strong>Bei verteilten Denial-of-Service-Angriffen (DDoS) werden Ressourcen überlastet, sodass Anwendungen nicht mehr verfügbar sind. Mit <a href="https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview">Azure DDoS Protection Standard</a> können Sie Ihre Organisation gegen die drei Haupttypen von DDoS-Angriffen verteidigen:<br>Bei - <strong>volumetrischen Angriffen</strong> wird das Netzwerk mit legitimem Datenverkehr überflutet. DDoS Protection Standard schützt vor diesen Angriffen, indem der Datenverkehr automatisch absorbiert oder bereinigt wird.<br>- <strong>Protokollangriffe</strong> machen den Zugriff auf ein Ziel unmöglich, indem sie Schwachstellen in den Schichten 3 und 4 des Protokollstapels ausnutzen. DDoS Protection Standard schützt vor diesen Angriffen, indem der Dienst böswilligen Datenverkehr blockiert.<br>- <strong>Angriffe auf Ressourcenebene (Anwendungsebene)</strong> zielen auf Webanwendungspakete ab. Gegen diesen Angriffstyp schützen Sie sich mit einer Web Application Firewall und DDoS Protection Standard.</td>
    <td class="tg-lboi"; width=55%>- DDoS Protection Standard sollte aktiviert sein.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Enable endpoint protection (Endpoint Protection aktivieren) (max. Bewertung: 2)</p></strong>Verhaltenssensoren erfassen und verarbeiten Daten von den Betriebssystemen Ihrer Endpunkte und senden diese zur Analyse an die private Cloud. So wird sichergestellt, dass Ihre Endpunkte vor Malware geschützt sind. Für die Sicherheitsanalysen werden Big-Data-, Machine-Learning- sowie weitere Quellen herangezogen, um Maßnahmen gegen Bedrohungen zu empfehlen. In <a href="https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection">Microsoft Defender ATP</a> werden beispielsweise mithilfe von Threat Intelligence Angriffsmethoden ermittelt und Sicherheitswarnungen generiert.<br>Das Security Center unterstützt die folgende Lösungen zum Schutz von Endpunkten: Windows Defender, System Center Endpoint Protection, Trend Micro, Symantec v12.1.1.1100, McAfee v10 für Windows, McAfee v10 für Linux und Sophos v9 für Linux. Wenn das Security Center eine dieser Lösungen erkennt, wird die Empfehlung, Endpoint Protection zu installieren, nicht mehr angezeigt.</td>
    <td class="tg-lboi"; width=55%>- Integritätsfehler in Endpoint Protection sollten für VM-Skalierungsgruppen behoben werden.<br>- Integritätsprobleme in Endpoint Protection sollten auf Ihren Computern behoben werden.<br>- In VM-Skalierungsgruppen sollte Endpoint Protection installiert sein.<br>- Endpoint Protection sollte auf virtuellen Computern installiert werden.<br>- Integritätsprobleme im Überwachungs-Agent sollten auf Ihren Computern behoben werden.<br>- Der Überwachungs-Agent sollte in VM-Skalierungsgruppen installiert werden.<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden.<br>- Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein (Vorschau)<br>- Endpoint Protection sollte auf Ihren Computern installiert werden.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Enable auditing and logging (Überwachung und Protokollierung aktivieren) (max. Bewertung: 1)</p></strong>Protokollierungsdaten bieten Einblicke in vergangene Probleme, verhindern mögliche zukünftige Probleme, können die Anwendungsleistung verbessern und bieten die Möglichkeit, Aktionen zu automatisieren, die andernfalls manuell erfolgen würden.<br>- <strong>Steuerungs- und Verwaltungsprotokolle</strong> enthalten Informationen zu Vorgängen in <a href="https://docs.microsoft.com/azure/azure-resource-manager/management/overview">Azure Resource Manager</a>.<br>- <strong>Datenebenenprotokolle</strong> enthalten Informationen über Ereignisse, die bei der Nutzung der Azure-Ressource aufgetreten sind.<br>- <strong>Verarbeitete Ereignisse</strong> enthalten Informationen zu analysierten Ereignissen und Warnungen, die verarbeitet wurden.</td>
    <td class="tg-lboi"; width=55%>- Die Überwachung in SQL Server sollte aktiviert sein.<br>- In App Services sollten Diagnoseprotokolle aktiviert sein.<br>- In Azure Data Lake Store sollten Diagnoseprotokolle aktiviert sein.<br>- In Azure Stream Analytics sollten Diagnoseprotokolle aktiviert sein.<br>- In Batch-Konten sollten Diagnoseprotokolle aktiviert sein.<br>- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein.<br>- In Event Hubs sollten Diagnoseprotokolle aktiviert sein.<br>- In IoT Hub sollten Diagnoseprotokolle aktiviert sein.<br>- In Key Vault sollten Diagnoseprotokolle aktiviert sein.<br>- In Logic Apps sollten Diagnoseprotokolle aktiviert sein.<br>- Im Suchdienst sollten Diagnoseprotokolle aktiviert sein.<br>- In Service Bus sollten Diagnoseprotokolle aktiviert sein.<br>- In Virtual Machine Scale Sets sollten Diagnoseprotokolle aktiviert sein.<br>- Für Batch-Konten sollten Warnungsregeln für Metriken konfiguriert sein.<br>- Für SQL-Überwachungseinstellungen sollten Aktionsgruppen zum Erfassen kritischer Aktivitäten konfiguriert sein.<br>- Für SQL Server-Instanzen sollte eine Überwachungsaufbewahrungsdauer von mehr als 90 Tagen konfiguriert sein.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Implement security best practices (Bewährte Sicherheitsmethoden implementieren) (max. Bewertung: 0)</p></strong>Moderne Sicherheitsmethoden gehen von unzulässigen Zugriffen auf die Netzwerkumgebung aus. Aus diesem Grund konzentrieren sich viele bewährte Methoden in diesem Steuerelement auf die Verwaltung von Identitäten.<br>Der Verlust von Schlüsseln und Anmeldeinformationen ist ein verbreitetes Problem. <a href="https://docs.microsoft.com/azure/key-vault/key-vault-overview">Azure Key Vault</a> schützt Schlüssel und Geheimnisse durch das Verschlüsseln von Schlüsseln, PFX-Dateien und Kennwörtern.<br>Virtuelle private Netzwerke (Virtual Private Networks, VPNs) stellen eine sichere Methode dar, auf Ihre virtuellen Computer zuzugreifen. Sollten keine VPNs verfügbar sein, verwenden Sie unbedingt komplexe Passphrasen und die zweistufige Authentifizierung, beispielsweise <a href="https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks">Azure Multi-Factor Authentication</a>. Durch die zweistufige Authentifizierung werden die Schwächen umgangen, die bei der Verwendung von Benutzernamen und Kennwörtern bestehen.<br>Die Verwendung einer sicheren Authentifizierungs- und Autorisierungsplattform ist eine weitere bewährte Methode. Durch die Verwendung von föderierten Identitäten können Organisationen die Verwaltung autorisierter Identitäten delegieren. Dies ist auch wichtig, wenn Mitarbeiter eine Organisation verlassen und ihr Zugriff aufgehoben werden muss.</td>
    <td class="tg-lboi"; width=55%>- Für Ihr Abonnement sollten maximal 3 Besitzer festgelegt werden.<br>- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden.<br>- Für Konten mit Leseberechtigungen für Ihr Abonnement sollte MFA aktiviert sein.<br>- Der Zugriff auf Speicherkonten mit Firewall- und VNET-Konfigurationen sollte eingeschränkt werden.<br>- Alle Autorisierungsregeln mit Ausnahme von RootManageSharedAccessKey sollten aus dem Event Hub-Namespace entfernt werden.<br>- Für SQL Server-Instanzen sollte ein Azure Active Directory-Administrator bereitgestellt werden.<br>- Für Ihre verwalteten Instanzen sollte Advanced Data Security aktiviert sein.<br>- Für die Event Hub-Instanz sollten Autorisierungsregeln definiert werden.<br>- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.<br>- Virtuelle Computer sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.<br>- Subnetze sollten einer NSG zugeordnet werden.<br>- [Vorschau] Microsoft Exploit Guard sollte aktiviert sein. <br>- [Vorschau] Der Gastkonfigurations-Agent sollte installiert sein.<br>- Virtuelle Computer ohne Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.</td>
  </tr>
</tbody>
</table>


</div>




## <a name="secure-score-faq"></a>Häufig gestellte Fragen zu Secure Score

### <a name="why-has-my-secure-score-gone-down"></a>Warum ist meine Sicherheitsbewertung niedriger ausgefallen?
Security Center wurde auf eine erweiterte Sicherheitsbewertung umgestellt, die Änderungen in der Art der Berechnung der Bewertung umfasst. Sie müssen nun alle Empfehlungen für eine Ressource umsetzen, um Punkte zu erhalten. Die Bewertungen wurden außerdem in eine Skala von 0 bis 10 geändert.

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Ändert sich meine Sicherheitsbewertung, wenn ich nur drei von vier Empfehlungen in einer Sicherheitskontrolle umsetze?
Nein. Die Bewertung ändert sich erst, wenn Sie alle Empfehlungen für eine einzelne Ressource umgesetzt haben. Um die maximale Bewertung für ein Kontrollelement zu erhalten, müssen Sie alle Empfehlungen für alle Ressourcen umsetzen.

### <a name="is-the-previous-experience-of-the-secure-score-still-available"></a>Ist die vorherige Version von Secure Score noch verfügbar? 
Nein. Eine Zeit lang wurden die beiden Bewertungen parallel ausgeführt, um den Übergang zu erleichtern. Das vorherige Modell ist jetzt veraltet. 

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Wenn eine Empfehlung für mich nicht anwendbar ist und ich sie in der Richtlinie deaktiviere, ist dann die Sicherheitskontrolle erfüllt, und wird meine Sicherheitsbewertung entsprechend aktualisiert?
Ja. Es wird empfohlen, Empfehlungen zu deaktivieren, wenn sie in Ihrer Umgebung nicht anwendbar sind. Anweisungen zum Deaktivieren einer bestimmten Empfehlung finden Sie unter [Deaktivieren von Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/tutorial-security-policy#disable-security-policies).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Wenn durch eine Sicherheitskontrolle keine Punkte für meine Sicherheitsbewertung erzielt werden, soll ich diese ignorieren?
In einigen Fällen wird als maximale Bewertung für ein Kontrollelement ein Wert größer 0 (null) angezeigt, jedoch ist die Auswirkung gleich null. Wenn der inkrementelle Wert für das Korrigieren von Ressourcen unerheblich ist, wird auf null gerundet. Ignorieren Sie diese Empfehlungen aber nicht, da Sie dennoch Sicherheitsverbesserungen bieten. Die einzige Ausnahme ist das Kontrollelement „Zusätzliche bewährte Methode“. Durch das Umsetzen dieser Empfehlungen erhöht sich die Bewertung nicht, doch Sie verbessern die Gesamtsicherheit.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden Secure Score und die damit eingeführten Sicherheitskontrollen beschrieben. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md)
