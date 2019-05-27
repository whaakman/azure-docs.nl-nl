---
title: Veelgestelde vragen voor Azure Kubernetes Service (AKS)
description: Vind antwoorden op enkele veelgestelde vragen over Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 6bfcd11dd6bfd31583fb2d0cd3f4229d3dd70065
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65887362"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Veelgestelde vragen over Azure Kubernetes Service (AKS)

In dit artikel adressen regelmatig vragen over Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-currently-provide-aks"></a>Welke Azure-regio's bieden momenteel AKS?

Zie voor een volledige lijst van beschikbare regio's, [AKS regio's en beschikbaarheid][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>Biedt ondersteuning voor AKS knooppunt automatisch schalen?

Ja, automatisch schalen is beschikbaar via de [Kubernetes automatisch schalen] [ auto-scaler] vanaf Kubernetes 1.10. Zie voor meer informatie over het handmatig configureren en gebruiken van het cluster automatisch schalen [Cluster automatisch schalen in AKS][aks-cluster-autoscale].

U kunt ook de ingebouwde cluster automatisch schalen (momenteel in preview in AKS) gebruiken voor het beheren van de schaal van de knooppunten. Zie voor meer informatie, [automatisch schalen van een cluster om te voldoen aan de eisen van de toepassing in AKS][aks-cluster-autoscaler].

## <a name="does-aks-support-kubernetes-rbac"></a>Biedt ondersteuning voor AKS Kubernetes RBAC?

Ja, Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC) is standaard ingeschakeld wanneer er clusters worden gemaakt met de Azure CLI. U kunt RBAC inschakelen voor clusters die zijn gemaakt met behulp van de Azure portal of de sjablonen.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan ik AKS implementeren in mijn bestaand virtueel netwerk?

Ja, u een AKS-cluster in een bestaand virtueel netwerk kunt implementeren met behulp van de [geavanceerde netwerken functie][aks-advanced-networking].

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Kan ik de Kubernetes API-server toegankelijk maken alleen binnen het virtuele netwerk?

Momenteel niet. De Kubernetes API-server wordt weergegeven als een openbare volledig domeinnaam (FQDN gekwalificeerde). U kunt toegang tot uw cluster beheren met behulp van [Kubernetes RBAC en Azure Active Directory (Azure AD)][aks-rbac-aad].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gelden er beveiligingsupdates voor AKS agentknooppunten?

Azure wordt automatisch beveiligingspatches geldt voor de Linux-knooppunten in uw cluster volgens een schema 's nachts. U bent echter verantwoordelijk om ervoor te zorgen dat deze knooppunten worden opnieuw opgestart als Linux vereist. U hebt verschillende mogelijkheden voor het opnieuw opstarten van knooppunten:

- Handmatig via de Azure portal of de Azure CLI.
- Voer een upgrade uw AKS-cluster. Het upgraden van clusters [cordon en leegmaken van knooppunten] [ cordon-drain] automatisch en een nieuw knooppunt online met de meest recente Ubuntu-installatiekopie en een nieuwe patchversie of een secundaire versie van Kubernetes. Zie voor meer informatie, [een AKS-cluster upgraden][aks-upgrade].
- Met behulp van [Kured](https://github.com/weaveworks/kured), een open-source opnieuw opstarten-daemon voor Kubernetes. Kured wordt uitgevoerd als een [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en bewaakt elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. In de cluster OS opnieuw wordt opgestart worden beheerd door dezelfde [cordon en proces leegmaken] [ cordon-drain] als de clusterupgrade van een.

Zie voor meer informatie over het gebruik van kured [beveiligings- en -kernel-updates toepassen op knooppunten in AKS][node-updates-kured].

### <a name="windows-server-nodes"></a>Windows Server-knooppunten

Windows Update niet automatisch voor Windows Server-knooppunten (momenteel in preview in AKS), uitvoeren en de meest recente updates toepassen. Een regelmatige rond de releasecyclus van Windows Update en uw eigen validatieproces, moet u een upgrade op de Windows Server-knooppunt pool(s) uitvoeren in uw AKS-cluster. Met dit upgradeproces knooppunten met de meest recente Windows Server-installatiekopie en patches maakt en vervolgens verwijdert u de oudere knooppunten. Zie voor meer informatie over dit proces [een knooppuntgroep in AKS Upgrade][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom zijn er twee resourcegroepen gemaakt met AKS?

Elke AKS-implementatie omvat twee resourcegroepen:

1. U maken de eerste resourcegroep. Deze groep bevat alleen de bron van de Kubernetes-service. De tweede resourcegroep de AKS-resourceprovider automatisch gemaakt tijdens de implementatie. Een voorbeeld van de tweede resourcegroep is *MC_myResourceGroup_myAKSCluster_eastus*. Zie de volgende sectie voor meer informatie over hoe u de naam van de tweede resourcegroep op te geven.
1. De tweede bron groeperen, zoals *MC_myResourceGroup_myAKSCluster_eastus*, bevat alle van de infrastructuurresources die zijn gekoppeld aan het cluster. Deze bronnen omvatten de Kubernetes-knooppunt virtuele machines, virtuele netwerken en opslag. Het doel van deze resourcegroep is voor het opruimen van de resource te vereenvoudigen.

Als u resources voor gebruik met uw AKS-cluster maakt, plaats zoals storage-accounts of gereserveerde openbare IP-adressen, deze in de automatisch gegenereerde resourcegroep.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Kan ik mijn eigen naam op voor de resourcegroep van de AKS-infrastructuur bieden?

Ja. Standaard wordt de resourceprovider AKS automatisch een secundaire resourcegroep (zoals *MC_myResourceGroup_myAKSCluster_eastus*) tijdens de implementatie. U kunt uw eigen naam voor dit beheerde cluster opgeven om te voldoen aan het bedrijfsbeleid, (*MC_*) resourcegroep.

Als u uw eigen Resourcegroepnaam, installeert de [aks-preview] [ aks-preview-cli] versie van de Azure CLI-extensie *0.3.2* of hoger. Wanneer u een AKS-cluster maakt met behulp van de [az aks maken] [ az-aks-create] opdracht, gebruikt u de *--knooppunt-resource-group* parameter en geef een naam voor de resourcegroep. Als u [u een Azure Resource Manager-sjabloon] [ aks-rm-template] voor het implementeren van een AKS-cluster, kunt u de naam van de resource met behulp van de *nodeResourceGroup* eigenschap.

* De secundaire resourcegroep wordt automatisch gemaakt door de provider van de Azure-resource in uw eigen abonnement.
* Alleen wanneer u het cluster maakt, kunt u de naam van een aangepaste resource-groep opgeven.

Als u met werkt de *MC_* resourcegroep, houd er rekening mee dat u kunt geen:

* Geef een bestaande resourcegroep voor de *MC_* groep.
* Geef een ander abonnement voor de *MC_* resourcegroep.
* Wijzig de *MC_* groepsnaam voor accountresources nadat het cluster is gemaakt.
* Geef namen voor de beheerde resources binnen de *MC_* resourcegroep.
* Wijzigen of verwijderen van tags van beheerde resources binnen de *MC_* resourcegroep. (Zie aanvullende informatie in de volgende sectie.)

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Kan ik labels en andere eigenschappen van de AKS-resources in de resourcegroep MC_ wijzigen?

Als u wijzigt of verwijdert u Azure gemaakte tags en andere resource-eigenschappen in de *MC_* resourcegroep, kan er onverwachte resultaten, zoals schalen en upgraden van fouten. AKS kunt u maken en wijzigen van de aangepaste labels. U kunt maken of wijzigen van aangepaste labels, bijvoorbeeld, om toe te wijzen een zakelijke eenheid of kosten center. Door het wijzigen van de resources onder de *MC_* in de AKS-cluster, verbreekt u de service level objective (SLO). Zie voor meer informatie, [AKS is een service level agreement bieden?](#does-aks-offer-a-service-level-agreement)

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

U kunt de lijst met domeincontrollers in AKS toelating op dit moment niet wijzigen.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Key Vault geïntegreerd met AKS?

AKS niet is momenteel geïntegreerd met Azure Key Vault. Echter, de [Azure Key Vault FlexVolume voor Kubernetes project] [ keyvault-flexvolume] kunnen directe integratie van Kubernetes-schillen tot de Key Vault-geheimen.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan ik Windows Server-containers in AKS uitvoeren?

Ja, Windows Server-containers zijn beschikbaar in preview. Voor Windows Server-containers in AKS uitvoert, moet u een knooppuntgroep waarop Windows Server wordt uitgevoerd als het gastbesturingssysteem maken. Windows Server-containers kunnen alleen Windows Server 2019 gebruiken. Als u wilt beginnen, Zie [een AKS-cluster maken met een Windows Server-knooppuntgroep][aks-windows-cli].

Ondersteuning voor Windows Server voor de knooppuntgroep bevat enkele beperkingen die deel van de upstream-Windows-Server in een Kubernetes-project uitmaken. Zie voor meer informatie over deze beperkingen [Windows Server-containers in AKS beperkingen][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>Biedt AKS een service level agreement?

In een service-level agreement (SLA) stemt de provider in met het betalen van de klant voor de kosten van de service als het niveau van de gepubliceerde service niet wordt voldaan. Omdat AKS gratis is, is gratis beschikbaar om te betalen, zodat AKS geen formeel SLA heeft. Richt zich echter AKS voor beschikbaarheid van ten minste bij minder dan 99,5% voor de Kubernetes API-server.

## <a name="why-cant-i-set-maxpods-below-30"></a>Waarom kan ik maxPods hieronder 30 niet instellen?

In AKS, stelt u de `maxPods` waarde wanneer u het cluster maakt met behulp van de Azure CLI en Azure Resource Manager-sjablonen. Echter Kubenet zowel Azure CNI vereist een *minimumwaarde* (gevalideerd tijdens de aanmaak):

| Netwerk | Minimum | Maximum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

Omdat AKS een beheerde service is, we implementeren en beheren-invoegtoepassingen en schillen als onderdeel van het cluster. In het verleden gebruikers kunnen definieert een `maxPods` waarde lager is dan de waarde die de beheerde schillen vereist om uit te voeren (bijvoorbeeld, 30). Het minimale aantal schillen in AKS nu worden berekend op basis van deze formule: ((maxPods of (maxPods * vm_count)) > beheerde invoegtoepassingen schillen minimum.

De minimale kunnen gebruikers niet overschrijven `maxPods` validatie.

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
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
