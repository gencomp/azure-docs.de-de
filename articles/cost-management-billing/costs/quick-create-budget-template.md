---
title: 'Schnellstart: Erstellen eines Budgets mit einer Azure Resource Manager-Vorlage'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Budget mit einer Azure Resource Manager-Vorlage erstellen.
author: bandersmsft
ms.author: banders
tags: azure-resource-manager
ms.service: cost-management-billing
ms.topic: quickstart
ms.date: 06/10/2020
ms.custom: subject-armqs
ms.openlocfilehash: 5bff8e6057475701a2e78835fb5a950dcb8c8fcb
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252435"
---
# <a name="quickstart-create-a-budget-with-an-arm-template"></a>Schnellstart: Erstellen eines Budgets mit einer ARM-Vorlage

Budgets in Cost Management helfen Ihnen, die organisatorische Verantwortlichkeit zu planen und zu steigern. Mit Budgets können Sie die Azure-Dienste abrechnen, die Sie in einem bestimmten Zeitraum in Anspruch nehmen oder abonnieren. Sie unterstützen Sie dabei, andere über ihre Ausgaben zu informieren, um die Kosten proaktiv zu steuern und die Entwicklung der Ausgaben im Laufe der Zeit zu überwachen. Bei Überschreitung der erstellten Budgetschwellenwerte werden Benachrichtigungen ausgelöst. Keines Ihrer Ressourcen wird beeinträchtigt, und die Nutzung wird nicht beendet. Sie können Budgets verwenden, um Ausgaben bei der Kostenanalyse zu vergleichen und zu verfolgen. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Budget unter Verwendung einer ARM-Vorlage (Azure Resource Manager) erstellen.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Von der ARM-Vorlage werden nur Azure-Abonnements für Enterprise Agreements (EAs) unterstützt. Andere Abonnementtypen werden von der Vorlage nicht unterstützt.

Zum Erstellen und Verwalten von Budgets müssen Sie über die Berechtigung „Mitwirkender“ verfügen. Sie können individuelle Budgets für EA-Abonnements und Ressourcengruppen erstellen. Sie können jedoch keine Budgets für EA-Abrechnungskonten erstellen. Zum Anzeigen von Budgets für Azure EA-Abonnements müssen Sie über Lesezugriff verfügen.

Nach der Erstellung eines Budgets benötigen Sie mindestens Lesezugriff auf Ihr Azure-Konto, um Budgets anzeigen zu können.

Bei einem neuen Abonnement können Sie nicht sofort ein Budget erstellen oder andere Cost Management-Features nutzen. Es kann bis zu 48 Stunden dauern, bis Sie alle Cost Management-Features verwenden können.

Die folgenden Azure-Berechtigungen (oder Bereich) werden pro Abonnement für Budgets nach Benutzer und Gruppe unterstützt. Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).

- Besitzer – kann Budgets für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkender und Mitwirkender für Cost Management – kann eigene Budgets erstellen, ändern oder löschen. Kann den Betrag für von anderen Personen erstellten Budgets ändern.
- Leser und Leser für Cost Management – kann Budgets anzeigen, für die er die Berechtigung hat.

Weitere Informationen zum Zuweisen der Berechtigung für Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Daten in Cost Management](assign-access-acm-data.md).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/create-budget).

:::code language="json" source="~/quickstart-templates/create-budget/azuredeploy.json" range="1-146" highlight="110-139":::

In der Vorlage ist eine einzelne Azure-Ressource definiert:

* [Microsoft.Consumption/budgets](/azure/templates/microsoft.consumption/budgets) (zum Erstellen eines Azure-Budgets).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Die Vorlage dient zum Erstellen eines Budgets.

   [![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

2. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

   [![Resource Manager-Vorlage, Budgeterstellung, Bereitstellung (Portal)](./media/quick-create-budget-template/create-budget-using-template-portal.png)](./media/quick-create-budget-template/create-budget-using-template-portal.png#lightbox)

    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, geben Sie einen eindeutigen Namen für die Ressourcengruppe ein, und klicken Sie anschließend auf **OK**, oder wählen Sie eine bereits vorhandene Ressourcengruppe aus.
    * **Standort**: Wählen Sie einen Standort aus. Beispiel: **USA, Mitte**.
    * **Budgetname**: Geben Sie einen Namen für das Budget ein. Dieser muss innerhalb einer Ressourcengruppe eindeutig sein. Es sind nur alphanumerische Zeichen sowie Unter- und Bindestriche zulässig.
    * **Menge**: Geben Sie den Gesamtumfang der Kosten oder Nutzung ein, der mit dem Budget nachverfolgt werden soll.
    * **Budgetkategorie**: Wählen Sie aus, ob das Budget zur Nachverfolgung von **Kosten** oder zur Nachverfolgung der **Nutzung** dient.
    * **Aggregationsintervall**: Geben Sie den von einem Budget abgedeckten Zeitraum ein. Zulässige Werte sind „Monatlich“, „Vierteljährlich“ und „Jährlich“. Das Budget wird am Ende des Aggregationsintervalls zurückgesetzt.
    * **Startdatum**: Geben Sie das Startdatum mit dem ersten Tag des Monats im Format „JJJJ-MM-TT“ ein. Das Startdatum darf nicht mehr als drei Monate in der Zukunft liegen. Sie können ein in der Vergangenheit liegendes Startdatum mit dem Aggregationsintervall angeben.
    * **Enddatum**: Geben Sie das Enddatum für das Budget im Format „JJJJ-MM-TT“ ein. Ohne Angabe wird standardmäßig ein Wert verwendet, der zehn Jahre nach dem Startdatum liegt.
    * **Operator**: Wählen Sie einen Vergleichsoperator aus. Mögliche Werte sind „EqualTo“, „GreaterThan“ und „GreaterThanOrEqualTo“.
    * **Schwellenwert**: Geben Sie einen Schwellenwert für die Benachrichtigung ein. Eine Benachrichtigung wird gesendet, wenn die Kosten den Schwellenwert übersteigen. Hierbei handelt es sich immer um einen Prozentwert, und er muss zwischen „0“ und „1.000“ liegen.
    * **Contact Emails** (Kontakt-E-Mail-Adressen): Geben Sie eine Liste von E-Mail-Adressen ein, an die die Budgetbenachrichtigung gesendet werden soll, wenn der Schwellenwert überschritten wird. Erwartetes Format: `["user1@domain.com","user2@domain.com"]`.
    * **Contact Roles** (Kontaktrollen): Geben Sie die Liste der Kontaktrollen ein, an die die Budgetbenachrichtigung gesendet werden soll, wenn der Schwellenwert überschritten wird. Standardwerte sind „Besitzer“, „Mitwirkender“ und „Leser“. Erwartetes Format: `["Owner","Contributor","Reader"]`.
    * **Contact Groups** (Kontaktgruppen): Geben Sie eine Liste von Aktionsgruppenressourcen-IDs ein, an die die Budgetbenachrichtigung gesendet werden soll, wenn der Schwellenwert überschritten wird. Für diese Angabe wird ein Zeichenfolgenarray akzeptiert. Erwartetes Format: `["action group resource ID1","action group resource ID2"]`. Falls Sie keine Aktionsgruppen verwenden möchten, geben Sie `[]` ein.
    * **Resources Filter** (Ressourcenfilter): Geben Sie eine Liste mit Filtern für Ressourcen ein. Erwartetes Format: `["Resource Filter Name1","Resource Filter Name2"]`. Falls Sie keinen Filter anwenden möchten, geben Sie `[]` ein. Bei Eingabe eines Ressourcenfilters müssen auch Werte für **Meters Filter** (Verbrauchseinheitenfilter) eingegeben werden.
    * **Meters Filter** (Verbrauchseinheitenfilter): Geben Sie eine Liste mit auf Verbrauchseinheiten basierenden Filtern ein. Diese Angabe ist bei der Budgetkategorie **Nutzung** obligatorisch. Erwartetes Format: `["Meter Filter Name1","Meter Filter Name2"]`. Sollten Sie keinen **Ressourcenfilter** eingegeben haben, geben Sie `[]` ein.
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.

3. Wählen Sie die Option **Kaufen**. Nach erfolgreicher Bereitstellung des Budgets erhalten Sie eine Benachrichtigung:

   ![Resource Manager-Vorlage, Budget, Bereitstellungsbenachrichtigung (Portal)](./media/quick-create-budget-template/resource-manager-template-portal-deployment-notification.png)

Zum Bereitstellen der Vorlage wird das Azure-Portal verwendet. Neben dem Azure-Portal können Sie auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle (Azure CLI) und die REST-API verwenden. Informationen zu anderen Bereitstellungsvorlagen finden Sie unter [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Sie können im Azure-Portal überprüfen, ob das Budget erstellt wurde. Navigieren Sie hierzu zu **Kostenverwaltung + Abrechnung**, wählen Sie einen Bereich aus, und wählen Sie anschließend **Budgets** aus. Alternativ können Sie die folgenden Azure CLI- oder Azure PowerShell-Skripts verwenden, um das Budget anzuzeigen:

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
az consumption budget list
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzConsumptionBudget
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Budget nicht mehr benötigen, löschen Sie es mithilfe einer der folgenden Methoden:

### <a name="azure-portal"></a>Azure-Portal

Navigieren Sie zu **Kostenverwaltung + Abrechnung**, und wählen Sie einen Abrechnungsbereich aus. Dann klicken Sie auf **Budgets**, und wählen Sie ein Budget aus. Anschließend wählen Sie **Budget löschen** aus.

### <a name="command-line"></a>Befehlszeile

Sie können das Budget mithilfe der Azure CLI oder mit Azure PowerShell entfernen.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
echo "Enter the budget name:" &&
read budgetName &&
az consumption budget delete --budget-name $budgetName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$budgetName = Read-Host -Prompt "Enter the budget name"
Remove-AzConsumptionBudget -Name $budgetName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Azure-Budget für die Bereitstellung erstellt. Weitere Informationen zur Azure-Kostenverwaltung und -Abrechnung sowie zu Azure Resource Manager finden Sie in den folgenden Artikeln:

- Übersicht: [Was ist die Azure-Kostenverwaltung und -Abrechnung?](../cost-management-billing-overview.md)
- [Tutorial: Erstellen und Verwalten von Azure-Budgets](tutorial-acm-create-budgets.md) über das Azure-Portal
- Lesen Sie weitere Informationen zu [Azure Resource Manager](../../azure-resource-manager/management/overview.md).
