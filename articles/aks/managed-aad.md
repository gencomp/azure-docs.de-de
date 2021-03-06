---
title: Verwenden von Azure AD in Azure Kubernetes Service
description: Erfahren Sie, wie Sie Azure AD in Azure Kubernetes Service (AKS) verwenden.
services: container-service
manager: gwallace
author: TomGeske
ms.topic: article
ms.date: 07/08/2020
ms.author: thomasge
ms.openlocfilehash: b30c5b0e81f4748d5e94c05d016be83163c1e78e
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86251126"
---
# <a name="aks-managed-azure-active-directory-integration-preview"></a>Von AKS verwaltete Azure Active Directory-Integration (Vorschauversion)

> [!NOTE]
> Für vorhandene AKS-Cluster (Azure Kubernetes Service) mit Azure AD-Integration (Azure Active Directory) ergeben sich durch die neue von AKS verwaltete Azure AD-Umgebung keine negativen Auswirkungen.

Die von AKS verwaltete Azure AD-Integration vereinfacht deren Nutzung, weil es für Benutzer nicht mehr erforderlich ist, eine Client-App und eine Server-App zu erstellen und dem Azure AD-Mandanten Leseberechtigungen für Verzeichnisse zu gewähren. In der neuen Version verwaltet der AKS-Ressourcenanbieter die Client- und Server-App.

## <a name="azure-ad-authentication-overview"></a>Übersicht über die Azure AD-Authentifizierung

Clusteradministratoren können die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes auf der Grundlage einer Benutzeridentität oder Verzeichnisgruppenmitgliedschaft konfigurieren. Die Azure AD-Authentifizierung wird für AKS-Cluster mit OpenID Connect bereitgestellt. OpenID Connect ist eine Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. Weitere Informationen zu OpenID Connect finden Sie in der [OpenID Connect-Dokumentation][open-id-connect].

Informieren Sie sich in der [Dokumentation zu den Konzepten der Azure Active Directory-Integration](concepts-identity.md#azure-active-directory-integration) über den Ablauf der AAD-Integration.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauversionen werden teilweise vom Kundensupport auf Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> - [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> - [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

* Suchen Sie nach der Mandanten-ID Ihres Azure-Kontos, indem Sie zum Azure-Portal navigieren und „Azure Active Directory“ > „Eigenschaften“ > „Verzeichnis-ID“ auswählen.

> [!Important]
> Sie müssen Kubectl mit der Mindestversion 1.18 verwenden.

Die folgenden Ressourcen müssen installiert sein:

- Die Azure CLI mit Version 2.5.1 oder höher
- Die aks-preview 0.4.38-Erweiterung
- Kubectl mit der Mindestversion [1.18](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1180)

Verwenden Sie zum Installieren/Aktualisieren der aks-preview-Erweiterung oder einer höheren Version die folgenden Azure CLI-Befehle:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Verwenden Sie zum Installieren von Kubectl die folgenden Befehle:

```azurecli
sudo az aks install-cli
kubectl version --client
```

Verwenden Sie für andere Betriebssysteme [diese Anweisungen](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

```azurecli-interactive
az feature register --name AAD-V2 --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status als **Registriert** angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list](/cli/azure/feature?view=azure-cli-latest#az-feature-list) überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"
```

Wenn der Status als registriert angezeigt wird, können Sie die Registrierung des `Microsoft.ContainerService`-Ressourcenanbieters mit dem Befehl [az provider register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Erstellen eines AKS-Clusters mit aktiviertem Azure AD

Erstellen Sie mit den folgenden CLI-Befehlen einen AKS-Cluster.

Erstellen Sie eine Azure-Ressourcengruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Sie können eine vorhandene Azure AD-Gruppe verwenden oder eine neue erstellen. Sie benötigen die Objekt-ID für Ihre Azure AD-Gruppe.

```azurecli-interactive
# List existing groups in the directory
az ad group list
```

Verwenden Sie den folgenden Befehl, um eine neue Azure AD-Gruppe für Ihre Clusteradministratoren zu erstellen:

```azurecli-interactive
# Create an Azure AD group
az ad group create --display-name MyDisplay --mail-nickname MyDisplay
```

Erstellen Sie einen AKS-Cluster, und ermöglichen Sie den Verwaltungszugriff für Ihre Azure AD-Gruppe.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```

Eine erfolgreiche Erstellung eines von AKS verwalteten Azure AD-Clusters wird im Antworttext mit dem folgenden Abschnitt angegeben:
```
"AADProfile": {
    "adminGroupObjectIds": null,
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Der Cluster wird in wenigen Minuten erstellt.

## <a name="access-an-azure-ad-enabled-cluster"></a>Zugreifen auf einen Azure AD-fähigen Cluster

Sie benötigen die integrierte Rolle [Azure Kubernetes Service-Clusterbenutzer](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role), um die unten angegebenen Schritte auszuführen.

Rufen Sie die Benutzeranmeldeinformationen für den Zugriff auf den Cluster ab:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Befolgen Sie die Anweisungen zum Anmelden.

Verwenden Sie den Befehl „kubectl get nodes“, um die Knoten im Cluster anzuzeigen:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```
Konfigurieren Sie die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](./azure-ad-rbac.md), um zusätzliche Sicherheitsgruppen für Ihre Cluster zu konfigurieren.

## <a name="troubleshooting-access-issues-with-azure-ad"></a>Behandeln von Zugriffsproblemen mit Azure AD

> [!Important]
> In den unten beschriebenen Schritten wird die reguläre Azure AD-Gruppenauthentifizierung umgangen. Wenden Sie dies nur in Notfällen an.

Falls Sie dauerhaft blockiert werden, weil Sie nicht über Zugriff auf eine gültige Azure AD-Gruppe mit Zugriff auf Ihren Cluster verfügen, können Sie trotzdem die Administratoranmeldeinformationen abrufen, um direkt auf den Cluster zuzugreifen.

Zum Ausführen dieser Schritte benötigen Sie Zugriff auf die integrierte [Administratorrolle für Azure Kubernetes Service-Cluster](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role).

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```

## <a name="non-interactive-sign-in-with-kubelogin"></a>Nicht interaktive Anmeldung per kubelogin

Es gibt einige nicht interaktive Szenarien, z. B. Continuous Integration-Pipelines, die für Kubectl derzeit nicht verfügbar sind. Sie können [`kubelogin`](https://github.com/Azure/kubelogin) verwenden, um über eine nicht interaktive Dienstprinzipalanmeldung auf den Cluster zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die [Verwendung der Azure RBAC-Integration für die Kubernetes-Autorisierung][azure-rbac-integration].
* Informieren Sie sich über die [Azure AD-Integration mit Kubernetes RBAC][azure-ad-rbac].
* Verwenden Sie [kubelogin](https://github.com/Azure/kubelogin), um auf Features der Azure-Authentifizierung zuzugreifen, die in Kubectl nicht verfügbar sind.
* Informieren Sie sich über die [Identitätskonzepte für AKS und Kubernetes][aks-concepts-identity].
* Erstellen Sie mit [Azure Resource Manager-Vorlagen (ARM)][aks-arm-template] von AKS verwaltete Cluster, die für Azure AD geeignet sind.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - Internal -->
[azure-rbac-integration]: manage-azure-rbac.md
[aks-concepts-identity]: concepts-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
