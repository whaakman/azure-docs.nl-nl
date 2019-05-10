---
title: Veelgestelde vragen voor Azure Kubernetes Service (AKS)
description: Vindt u antwoorden op enkele veelgestelde vragen over Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2019
ms.author: iainfou
ms.openlocfilehash: 17bc1d2b7a08314f19f1bf8f87d0c774afc37500
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508180"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Veelgestelde vragen over Azure Kubernetes Service (AKS)

In dit artikel adressen regelmatig vragen over Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welke Azure-regio's bieden vandaag nog Azure Kubernetes Service (AKS)?

Zie voor een volledige lijst van beschikbare regio's, [AKS regio's en beschikbaarheid][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>Biedt ondersteuning voor AKS knooppunt automatisch schalen?

Ja, automatisch schalen is beschikbaar via de [Kubernetes automatisch schalen] [ auto-scaler] vanaf Kubernetes 1.10. Zie voor meer informatie over het configureren en gebruiken van het cluster automatisch schalen [Cluster automatisch schalen in AKS][aks-cluster-autoscale].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Biedt ondersteuning voor AKS Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC)?

Ja, Kubernetes RBAC is standaard ingeschakeld wanneer er clusters worden gemaakt met de Azure CLI. RBAC kan worden ingeschakeld voor clusters die zijn gemaakt met behulp van de Azure portal of de sjablonen.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan ik AKS implementeren in mijn bestaand virtueel netwerk?

Ja, u kunt implementeren met een AKS-cluster in een bestaand virtueel netwerk met behulp van de [geavanceerde netwerken functie][aks-advanced-networking].

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Kan ik de Kubernetes API-server als u alleen toegankelijk binnen het virtuele netwerk wilt beperken?

Momenteel niet. De Kubernetes API-server wordt weergegeven als een openbare volledig domeinnaam (FQDN gekwalificeerde). U kunt toegang tot uw cluster met beheren [Kubernetes op rollen gebaseerd beheer (RBAC) en Azure Active Directory (AAD) toegang][aks-rbac-aad]

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gelden er beveiligingsupdates voor AKS agentknooppunten?

Ja, geldt Azure automatisch beveiligingspatches voor de knooppunten in uw cluster volgens een schema 's nachts. Echter, u bent verantwoordelijk om ervoor te zorgen dat de knooppunten opnieuw zijn opgestart zoals vereist. U hebt verschillende mogelijkheden voor het uitvoeren van knooppunt opnieuw wordt opgestart:

- Handmatig via de Azure portal of de Azure CLI.
- Voer een upgrade uw AKS-cluster. Cluster-upgrades automatisch [cordon en leegmaken van knooppunten][cordon-drain], vervolgens elk knooppunt een back-up te brengen met de meest recente Ubuntu-installatiekopie en een nieuwe patchversie of een secundaire versie van Kubernetes. Zie voor meer informatie, [een AKS-cluster upgraden][aks-upgrade].
- Met behulp van [Kured](https://github.com/weaveworks/kured), een open-source opnieuw opstarten-daemon voor Kubernetes. Kured wordt uitgevoerd als een [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en bewaakt elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. Besturingssysteem opnieuw wordt opgestart worden beheerd in het cluster met behulp van dezelfde [cordon en proces leegmaken] [ cordon-drain] als de clusterupgrade van een.

Zie voor meer informatie over het gebruik van kured [beveiligings- en -kernel-updates toepassen op knooppunten in AKS][node-updates-kured].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom zijn er twee resourcegroepen gemaakt met AKS?

Elke AKS-implementatie omvat twee resourcegroepen:

- De eerste resourcegroep wordt gemaakt door u en bevat alleen de bron van de Kubernetes-service. De resourceprovider AKS maakt automatisch de tweede waarde tijdens de implementatie, zoals *MC_myResourceGroup_myAKSCluster_eastus*. Zie voor informatie over hoe u de naam van deze tweede resourcegroep kunt opgeven, de volgende sectie.
- Deze tweede resourcegroep, zoals *MC_myResourceGroup_myAKSCluster_eastus*, bevat alle van de infrastructuurresources die zijn gekoppeld aan het cluster. Deze bronnen omvatten de Kubernetes-knooppunt virtuele machines, virtuele netwerken en opslag. Deze afzonderlijke resourcegroep wordt gemaakt voor het opruimen van de resource te vereenvoudigen.

Als u resources voor gebruik met uw AKS-cluster, zoals storage-accounts of gereserveerde openbare IP-adressen maken, plaatst u ze in de automatisch gegenereerde resourcegroep.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Kan ik mijn eigen naam op voor de resourcegroep van de AKS-infrastructuur bieden?

Ja. De resourceprovider AKS automatisch maakt standaard een secundaire resourcegroep tijdens de implementatie, zoals *MC_myResourceGroup_myAKSCluster_eastus*. U kunt uw eigen naam voor dit beheerde cluster opgeven om te voldoen aan het bedrijfsbeleid, (*MC_*) resourcegroep.

Als u uw eigen Resourcegroepnaam, installeert de [aks-preview] [ aks-preview-cli] versie van de Azure CLI-extensie *0.3.2* of hoger. Wanneer u een AKS-cluster met maakt de [az aks maken] [ az-aks-create] opdracht, gebruikt u de *--knooppunt-resource-group* parameter en geef een naam voor de resourcegroep. Als u [u een Azure Resource Manager-sjabloon] [ aks-rm-template] voor het implementeren van een AKS-cluster, kunt u de resource naam via de *nodeResourceGroup* eigenschap.

* Deze resourcegroep wordt automatisch gemaakt door de provider van de Azure-resource in uw eigen abonnement.
* U kunt alleen de naam van een aangepaste resource-groep opgeven wanneer het cluster is gemaakt.

De volgende scenario's worden niet ondersteund:

* U kunt een bestaande resourcegroep voor niet opgeven *MC_* groep.
* U kunt geen opgeven dat een ander abonnement voor de *MC_* resourcegroep.
* U kunt niet wijzigen de *MC_* groepsnaam voor accountresources nadat het cluster is gemaakt.
* U kunt geen opgeven namen voor de beheerde resources binnen de *MC_* resourcegroep.
* U kan wijzigen of verwijderen van tags van beheerde resources binnen de *MC_* resourcegroep (Zie aanvullende informatie in de volgende sectie).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Kan ik labels en andere eigenschappen van de AKS-resources in de resourcegroep MC_ * wijzigen?

Wijzigen en verwijderen van de Azure-gemaakte tags en andere eigenschappen van bronnen in de *MC_** resourcegroep kan leiden tot onverwachte resultaten, zoals schalen en upgraden van fouten. Het wordt ondersteund voor het maken en wijzigen van extra aangepaste labels, zoals een zakelijke eenheid of kosten center toewijzen. Wijzigen van de resources onder de *MC_** in de AKS-cluster neemt de serviceniveaudoelstelling (SLO). Zie voor meer informatie, [AKS is een service level agreement bieden?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Welke Kubernetes toelating controllers biedt ondersteuning voor AKS? Kunnen worden toelating controllers toegevoegd of verwijderd?

AKS ondersteunt de volgende [toelating controllers][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

Het is momenteel niet mogelijk om te wijzigen van de lijst met domeincontrollers voor toegangsbeheer in AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Key Vault geïntegreerd met AKS?

AKS is momenteel systeemeigen niet geïntegreerd met Azure Key Vault. Echter, de [Azure Key Vault FlexVolume voor Kubernetes project] [ keyvault-flexvolume] kunnen directe integratie van Kubernetes-schillen voor KeyVault-geheimen.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan ik Windows Server-containers in AKS uitvoeren?

Als u wilt uitvoeren in Windows Server-containers, die u wilt uitvoeren op basis van Windows Server-knooppunten. Windows Server-gebaseerde knooppunten zijn op dit moment niet beschikbaar in AKS. U kunt echter Virtual Kubelet gebruiken voor het plannen van Windows-containers in Azure Container Instances en ze als onderdeel van uw AKS-cluster te beheren. Zie voor meer informatie, [Virtual Kubelet gebruiken met AKS][virtual-kubelet].

## <a name="does-aks-offer-a-service-level-agreement"></a>Biedt AKS een service level agreement?

In een serviceovereenkomst (SLA) stemt de provider in met het betalen van de klant voor de kosten van de service als het niveau van de gepubliceerde service niet wordt voldaan. Omdat AKS zelf gratis is, en is er geen kosten beschikbaar om te betalen en dus geen formeel SLA. Richt zich echter AKS voor beschikbaarheid van ten minste bij minder dan 99,5% voor de Kubernetes API-server.

## <a name="why-can-i-not-set-maxpods-below-30"></a>Waarom kan ik niet ingesteld `maxPods` hieronder 30?

AKS-instelling ondersteunt de `maxPods` waarde tijdens de aanmaak van cluster via de Azure CLI en Azure Resource Manager-sjablonen. Er is echter een *minimumwaarde* (gevalideerd tijdens de aanmaak) voor zowel Kubenet als Azure CNI worden hieronder weergegeven:

| Netwerk | Minimum | Maximum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

AKS is een beheerde service, bieden we-invoegtoepassingen en schillen we implementeren en beheren als onderdeel van het cluster. In het verleden gebruikers kunnen definieert een `maxPods` waarde lager is dan de waarde die is vereist voor de beheerde schillen om uit te voeren (voorbeeld: 30), nu berekent het minimaal aantal schillen via AKS: ((maxPods of (maxPods * vm_count)) > beheerde invoegtoepassingen schillen minimum.

Gebruikers mogen niet de minimale overschrijven `maxPods` validatie.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
