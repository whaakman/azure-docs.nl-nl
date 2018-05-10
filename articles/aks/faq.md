---
title: Veelgestelde vragen over Azure Kubernetes Service
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Azure Kubernetes Service.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/14/2018
ms.author: nepeters
ms.openlocfilehash: 55006a3f0193c96849c52f87ab01dc13ac0c7a16
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Veelgestelde vragen over Azure Kubernetes Service (AKS)

Dit artikel adressen regelmatige vragen over Azure Kubernetes Service (AKS).

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) is geopend in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.
>

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Welke Azure-regio's bieden vandaag de dag van de Azure Kubernetes Service (AKS)?

- Canada - midden
- Canada - oost
- VS - midden
- VS - oost
- Zuidoost-Azië
- West-Europa
- VS - west 2

## <a name="when-will-additional-regions-be-added"></a>Wanneer u meer regio's worden toegevoegd?

Als u meer regio's worden toegevoegd wanneer de vraag toeneemt.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Gelden er beveiligingsupdates voor AKS agent knooppunten?

Beveiligingspatches Azure automatisch toegepast op de knooppunten in uw cluster volgens een schema 's nachts. Echter, bent u verantwoordelijk voor gezorgd dat de knooppunten worden opgestart zoals vereist is. U hebt verschillende mogelijkheden voor het uitvoeren van knooppunt opnieuw wordt opgestart:

- Handmatig via de Azure-portal of Azure CLI.
- Door het upgraden van uw cluster AKS. Cluster-upgrades automatisch [cordon en leegmaken van knooppunten](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), vervolgens een back-up brengen met de meest recente Ubuntu-installatiekopie. Bijwerken van de installatiekopie van het besturingssysteem op uw knooppunten zonder Kubernetes versies wijzigen door op te geven van de huidige versie van het cluster in `az aks upgrade`.
- Met behulp van [Kured](https://github.com/weaveworks/kured), een open source opnieuw opstarten-daemon Kubernetes. Kured wordt uitgevoerd als een [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) en controleert u elk knooppunt op de aanwezigheid van een bestand dat aangeeft dat een herstart vereist is. Deze vervolgens die opnieuw wordt opgestart ingedeeld in het cluster, de dezelfde cordon en leegmaken proces, zoals eerder beschreven.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>U kunt beter klanten gebruik ACS of AKS?

Terwijl AKS in preview blijft, wordt aangeraden maken met behulp van de ACS-Kubernetes productieclusters of [acs-engine](https://github.com/azure/acs-engine). Gebruik AKS voor bewijs van concept implementaties en omgevingen ontwikkelen en testen.

## <a name="when-will-acs-be-deprecated"></a>Als ACS afgeschaft?

ACS wordt afgeschaft rond de tijd die AKS algemene beschikbaarheid wordt. Hebt u 12 maanden na die datum clusters migreren naar AKS. U kunt alle ACS-bewerkingen uitvoeren tijdens de periode van 12 maanden.

## <a name="does-aks-support-node-autoscaling"></a>Ondersteunt AKS knooppunt automatisch schalen?

Knooppunt automatisch schalen wordt niet ondersteund, maar is in het overzicht. U kunt Kijk eens naar dit open-source [automatisch schalen implementatie][auto-scaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Ondersteunt AKS Kubernetes rollen gebaseerd toegangsbeheer (RBAC)?

Nee, RBAC wordt momenteel niet ondersteund in AKS maar is binnenkort beschikbaar.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan ik AKS implementeren in mijn bestaande virtuele netwerk?

Nee, dit is nog niet beschikbaar, maar zal binnenkort beschikbaar.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Sleutelkluis geïntegreerd met AKS?

Nee, is het niet, maar deze integratie is gepland. In de tussentijd uitproberen van de volgende oplossing van [Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan ik Windows Server-containers op AKS uitvoeren?

Nee, AKS biedt momenteel geen agent op basis van Windows Server-knooppunten, zodat u Windows Server-containers niet uitvoeren. Als u uitvoeren van Windows Server-containers op Kubernetes in Azure wilt, raadpleegt u de [documentatie voor acs-engine](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom worden twee resourcegroepen met AKS gemaakt?

Elke implementatie AKS omvat twee resourcegroepen. De eerste door u is gemaakt en alleen de bron AKS bevat. De tweede overgebracht tijdens de implementatie van de resourceprovider AKS automatisch maakt met een naam zoals *MC_myResourceGRoup_myAKSCluster_eastus*. De tweede resourcegroep bevat alle van de infrastructuurresources die zijn gekoppeld aan het cluster, zoals virtuele machines, netwerken en opslag. Het is gemaakt voor het opruimen van de resource te vereenvoudigen.

Als u bij het maken van resources die worden gebruikt met uw cluster AKS zoals storage-accounts of gereserveerde openbare IP-adres, moet u in de automatisch gegenereerde resourcegroep plaatst.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent