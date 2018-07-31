---
title: Veelgestelde vragen over Azure Kubernetes Service
description: Vindt u antwoorden op enkele veelgestelde vragen over Azure Kubernetes Service.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/27/2018
ms.author: iainfou
ms.openlocfilehash: b64c770bca84fba8cbed98e420abf649897f7a17
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345851"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Veelgestelde vragen over Azure Kubernetes Service (AKS)

In dit artikel adressen regelmatig vragen over Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welke Azure-regio's bieden vandaag nog Azure Kubernetes Service (AKS)?

Zie de Azure Kubernetes Service [regio's en beschikbaarheid] [ aks-regions] documentatie voor een volledige lijst.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gelden er beveiligingsupdates voor AKS agentknooppunten?

Beveiligingspatches Azure automatisch toegepast op de knooppunten in uw cluster volgens een schema 's nachts. Echter, u bent verantwoordelijk om ervoor te zorgen dat de knooppunten opnieuw zijn opgestart zoals vereist. U hebt verschillende mogelijkheden voor het uitvoeren van knooppunt opnieuw wordt opgestart:

- Handmatig via de Azure portal of de Azure CLI.
- Voer een upgrade uw AKS-cluster. Cluster-upgrades automatisch [cordon en leegmaken van knooppunten](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), vervolgens een back-up brengen met de meest recente Ubuntu-installatiekopie. De installatiekopie van het besturingssysteem op uw knooppunten bijwerken zonder dat Kubernetes-versies worden gewijzigd door de huidige clusterversie in op te geven `az aks upgrade`.
- Met behulp van [Kured](https://github.com/weaveworks/kured), een open-source opnieuw opstarten-daemon voor Kubernetes. Kured wordt uitgevoerd als een [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en bewaakt elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. Vervolgens beheert het besturingssysteem opnieuw wordt opgestart in het cluster, volgens de dezelfde cordon en drain proces die eerder zijn beschreven.

## <a name="does-aks-support-node-autoscaling"></a>Biedt ondersteuning voor AKS knooppunt automatisch schalen?

Ja, automatisch schalen is beschikbaar via de [Kubernetes automatisch schalen] [ auto-scaler] vanaf Kubernetes 1.10.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Biedt ondersteuning voor AKS Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC)?

Ja, RBAC kan worden ingeschakeld wanneer [een AKS-cluster op basis van de Azure CLI of Azure Resource Manager-sjabloon implementeren](https://docs.microsoft.com/en-us/azure/aks/aad-integration). Deze functionaliteit wordt binnenkort komen bij Azure portal.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Welke Kubernetes toelating controllers biedt ondersteuning voor AKS? Kunnen worden toelating controllers toegevoegd of verwijderd?

AKS ondersteunt de volgende [toelating controllers][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* ServiceAccount
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook
* ValidatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

Het is momenteel niet mogelijk om te wijzigen van de lijst met domeincontrollers voor toegangsbeheer in AKS.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan ik AKS implementeren in mijn bestaand virtueel netwerk?

Ja, u kunt implementeren met een AKS-cluster in een bestaand virtueel netwerk met behulp van de [geavanceerde netwerken functie](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Kan ik de Kubernetes API-server als u alleen toegankelijk binnen het virtuele netwerk wilt beperken?

Momenteel niet. De Kubernetes API-server wordt weergegeven als een openbare volledig domeinnaam (FQDN gekwalificeerde). U moet toegang tot uw cluster met beheren [Kubernetes op rollen gebaseerd beheer (RBAC) en Azure Active Directory (AAD) toegang krijgen tot](https://docs.microsoft.com/en-us/azure/aks/aad-integration).

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Key Vault geïntegreerd met AKS?

AKS is niet geïntegreerd met Azure Key Vault op dit moment. Echter, de [KeyVault inhoud aangepast aan blokkeringsstatus Volume project](https://github.com/Azure/kubernetes-keyvault-flexvol) kunnen directe integratie van Kubernetes-schillen voor KeyVault-geheimen.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan ik Windows Server-containers in AKS uitvoeren?

Als u wilt uitvoeren in Windows Server-containers, die u wilt uitvoeren op basis van Windows Server-knooppunten. Windows Server-gebaseerde knooppunten zijn op dit moment niet beschikbaar in AKS. U kunt echter Virtual Kubelet gebruiken voor het plannen van Windows-containers in Azure Container Instances en ze als onderdeel van uw AKS-cluster te beheren. Zie voor meer informatie, [Virtual Kubelet gebruiken met AKS][virtual-kubelet].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom zijn er twee resourcegroepen gemaakt met AKS?

Elke AKS-implementatie omvat twee resourcegroepen. De eerste door u is gemaakt en bevat alleen de bron van de Kubernetes-service. De tweede waarde tijdens de implementatie van de resourceprovider AKS automatisch gemaakt met een naam, zoals *MC_myResourceGroup_myAKSCluster_eastus*. De tweede resourcegroep bevat al de infrastructuurresources die zijn gekoppeld aan het cluster, zoals virtuele machines, netwerken en opslag. Ter vereenvoudiging van opruimen van de resource wordt gemaakt.

Als u het maken van resources die worden gebruikt met uw AKS-cluster, zoals storage-accounts of gereserveerde openbare IP-adressen, moet u deze in de automatisch gegenereerde resourcegroep plaatsen.

## <a name="does-aks-offer-a-service-level-agreement"></a>Biedt AKS een service level agreement?

In een serviceovereenkomst (SLA) stemt de provider in met het betalen van de klant voor de kosten van de service moet het niveau van de gepubliceerde service niet worden voldaan. Omdat AKS zelf gratis is, en is er geen kosten beschikbaar om te betalen en dus geen formeel SLA. Richt zich echter AKS voor beschikbaarheid van ten minste bij minder dan 99,5% voor de Kubernetes API-server.

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md
[virtual-kubelet]: virtual-kubelet.md

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
