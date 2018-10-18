---
title: Veelgestelde vragen voor Azure Kubernetes Service (AKS)
description: Vindt u antwoorden op enkele veelgestelde vragen over Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/17/2018
ms.author: iainfou
ms.openlocfilehash: 1e101e308ec350e9900c1347da730ca02b16c7bb
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377461"
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

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom zijn er twee resourcegroepen gemaakt met AKS?

Elke AKS-implementatie omvat twee resourcegroepen:

- De eerste resourcegroep wordt gemaakt door u en bevat alleen de bron van de Kubernetes-service. De resourceprovider AKS maakt automatisch de tweede waarde tijdens de implementatie, zoals *MC_myResourceGroup_myAKSCluster_eastus*.
- Deze tweede resourcegroep, zoals *MC_myResourceGroup_myAKSCluster_eastus*, bevat alle van de infrastructuurresources die zijn gekoppeld aan het cluster. Deze bronnen omvatten de Kubernetes-knooppunt virtuele machines, virtuele netwerken en opslag. Deze afzonderlijke resourcegroep wordt gemaakt voor het opruimen van de resource te vereenvoudigen.

Als u resources voor gebruik met uw AKS-cluster, zoals storage-accounts of gereserveerde openbare IP-adressen maken, plaatst u ze in de automatisch gegenereerde resourcegroep.

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Kan ik labels en andere eigenschappen van de AKS-resources in de resourcegroep MC_ * wijzigen?

Wijzigen en verwijderen van de Azure-gemaakte tags en andere eigenschappen van bronnen in de *MC_** resourcegroep kan leiden tot onverwachte resultaten, zoals schalen en upgraden van fouten. Het wordt ondersteund voor het maken en wijzigen van extra aangepaste labels, zoals een zakelijke eenheid of kosten center toewijzen. Wijzigen van de resources onder de *MC_** in de AKS-cluster neemt de SLO.

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

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-advanced-networking.md
[aks-rbac-aad]: ./aad-integration.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
