---
title: Veelgestelde vragen over Azure Kubernetes Service
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Azure Kubernetes Service.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/25/2018
ms.author: nepeters
ms.openlocfilehash: 5155d0c85e5b3698b0a13d2d5256a235858f0e82
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938418"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Veelgestelde vragen over Azure Kubernetes Service (AKS)

Dit artikel adressen regelmatige vragen over Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welke Azure-regio's bieden vandaag de dag van de Azure Kubernetes Service (AKS)?

- Australië - oost
- Canada - centraal
- Canada - oost
- VS - centraal
- VS - oost
- Oost-US2
- Noord-Europa
- VK - zuid
- West-Europa
- VS - west
- VS - west 2

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gelden er beveiligingsupdates voor AKS agent knooppunten?

Beveiligingspatches Azure automatisch toegepast op de knooppunten in uw cluster volgens een schema 's nachts. Echter, bent u verantwoordelijk voor gezorgd dat de knooppunten worden opgestart zoals vereist is. U hebt verschillende mogelijkheden voor het uitvoeren van knooppunt opnieuw wordt opgestart:

- Handmatig via de Azure-portal of Azure CLI.
- Door het upgraden van uw cluster AKS. Cluster-upgrades automatisch [cordon en leegmaken van knooppunten](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), vervolgens een back-up brengen met de meest recente Ubuntu-installatiekopie. Bijwerken van de installatiekopie van het besturingssysteem op uw knooppunten zonder Kubernetes versies wijzigen door op te geven van de huidige versie van het cluster in `az aks upgrade`.
- Met behulp van [Kured](https://github.com/weaveworks/kured), een open source opnieuw opstarten-daemon Kubernetes. Kured wordt uitgevoerd als een [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en controleert u elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. Het vervolgens opnieuw wordt opgestart ingedeeld in het cluster, de dezelfde cordon en leegmaken proces, zoals eerder beschreven.

## <a name="does-aks-support-node-autoscaling"></a>Ondersteunt AKS knooppunt automatisch schalen?

Ja, automatisch schalen is beschikbaar via de [Kubernetes autoscaler] [ auto-scaler] vanaf Kubernetes 1.10.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Ondersteunt AKS Kubernetes rollen gebaseerd toegangsbeheer (RBAC)?

Ja, RBAC kan worden ingeschakeld bij het implementeren van een cluster AKS van de Azure CLI of Azure Resource Manager-sjabloon. Deze functionaliteit wordt binnenkort worden geleverd bij de Azure portal.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Welke Kubernetes toelating domeincontrollers ondersteunt AKS? Kan dit worden geconfigureerd?

AKS ondersteunt de volgende [toelating domeincontrollers][admission-controllers]:

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

Het is niet op dit moment mogelijk aan de lijst met domeincontrollers in AKS toelating wijzigen.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan ik AKS implementeren in mijn bestaande virtuele netwerk?

Ja, kunt u een cluster AKS naar een bestaand virtueel netwerk met behulp implementeren de [geavanceerde netwerken functie](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Sleutelkluis geïntegreerd met AKS?

AKS niet systeemeigen geïntegreerd met Azure Key Vault op dit moment. Er zijn echter community oplossingen zoals [de acs-keyvault-agent van Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan ik Windows Server-containers op AKS uitvoeren?

Windows Server-containers, hebt u nodig om uit te voeren op basis van Windows Server-knooppunten. Windows Server gebaseerde knooppunten zijn niet beschikbaar in AKS op dit moment. Als u uitvoeren van Windows Server-containers op Kubernetes in Azure wilt, raadpleegt u de [documentatie voor acs-engine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom worden twee resourcegroepen met AKS gemaakt?

Elke implementatie AKS omvat twee resourcegroepen. De eerste door u is gemaakt en alleen resource voor de service Kubernetes bevat. De tweede overgebracht tijdens de implementatie van de resourceprovider AKS automatisch maakt met een naam zoals *MC_myResourceGroup_myAKSCluster_eastus*. De tweede resourcegroep bevat alle van de infrastructuurresources die zijn gekoppeld aan het cluster, zoals virtuele machines, netwerken en opslag. Het is gemaakt voor het opruimen van de resource te vereenvoudigen.

Als u bij het maken van resources die worden gebruikt met uw cluster AKS zoals storage-accounts of gereserveerde openbare IP-adres, moet u in de automatisch gegenereerde resourcegroep plaatst.

## <a name="does-aks-offer-a-service-level-agreement"></a>Biedt AKS een service level agreement?

In een serviceovereenkomst (SLA) stemt de provider in met het betalen van de klant voor de kosten van de service moet het gepubliceerde serviceniveau niet worden opgehaald. Aangezien AKS zelf gratis is, is er gratis beschikbaar voor het betalen en dus geen formele SLA. Echter, zoeken we beschikbaarheid van minimaal 99.5% voor de Kubernetes API-server onderhouden.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
