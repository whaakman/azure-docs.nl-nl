---
title: Veelgestelde vragen over Azure Red Hat OpenShift | Microsoft Docs
description: Hier vindt u antwoorden op veelgestelde vragen over Microsoft Azure Red Hat OpenShift
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 881734caf855ccfc4f001693fe261b8448b49bc4
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466201"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift Veelgestelde vragen

In dit artikel komen de antwoorden op veelgestelde vragen over Microsoft Azure Red Hat OpenShift.

## <a name="which-azure-regions-are-supported"></a>Welke Azure-regio's worden ondersteund?

Zie [ondersteunde resources](supported-resources.md#azure-regions) voor een lijst van wereldwijde regio's waar Azure Red Hat OpenShift wordt ondersteund.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Kan ik een cluster in een bestaand virtueel netwerk implementeren?

Nee. Maar u kunt een Azure Red Hat OpenShift-cluster koppelen aan een bestaand VNET via peering. Zie [van een cluster virtuele netwerk verbinden met een bestaand virtueel netwerk ](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) voor meer informatie.

## <a name="what-cluster-operations-are-available"></a>Welke bewerkingen voor een cluster zijn beschikbaar?

U kunt alleen schaal omhoog of omlaag het aantal rekenknooppunten. Er zijn geen andere wijzigingen mogen de `Microsoft.ContainerService/openShiftManagedClusters` resource nadat is gemaakt. Het maximum aantal compute-knooppunten is beperkt tot 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Welke grootten van virtuele machines kan ik gebruiken?

Zie [grootten van virtuele machines van Azure Red Hat OpenShift](supported-resources.md#virtual-machine-sizes) voor een lijst met grootten van virtuele machines kunt u met een Azure Red Hat OpenShift-cluster.

## <a name="is-data-on-my-cluster-encrypted"></a>Zijn de gegevens op mijn cluster versleuteld?

Er is versleuteling-at-rest standaard. Het Azure Storage-platform codeert uw gegevens automatisch voordat het permanent worden gemaakt en ontsleutelt de gegevens voor het ophalen van. Zie [Azure Storage-Serviceversleuteling voor data-at-rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) voor meer informatie.

## <a name="can-i-use-prometheusgrafana-to-monitor-containers-and-manage-capacity"></a>Kan ik Prometheus/Grafana gebruiken voor het bewaken van containers en capaciteit beheren?

Nee, niet op de huidige tijd.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Is de Docker-register beschikbaar extern zodat ik kan hulpprogramma's zoals Jenkins gebruiken?

De Docker-register is beschikbaar via `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` echter een houdbaarheidsgarantie sterke opslag is niet opgegeven. U kunt ook [Azure Container Registry](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>Wordt ondersteund netwerken cross-naamruimte?

Klant- en beheerders van afzonderlijke project cross-namespace-netwerken (met inbegrip van het weigeren) kunt aanpassen op een per-project uit te voeren met `NetworkPolicy` objecten.

## <a name="can-an-admin-manage-users-and-quotas"></a>Kan de beheerder gebruikers en quota beheren?

Ja. Een Azure Red Hat OpenShift-beheerder kan beheren, gebruikers en quota's naast de toegang tot alle gebruikers die zijn gemaakt van projecten.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Kan ik een cluster beperken tot alleen bepaalde gebruikers Azure AD?

Ja. U kunt beperken welke Azure AD-gebruikers kunnen zich aanmelden met een cluster door het configureren van de Azure AD-toepassing. Zie voor meer informatie, [het: Uw app tot een groep gebruikers beperken](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Een cluster kunt compute-knooppunten hebben meerdere Azure-regio's?

Nee. Alle knooppunten in een cluster Azure Red Hat OpenShift moeten afkomstig zijn van dezelfde Azure-regio.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Hoofd- en -infrastructuur knooppunten ze kunnen worden opgeslagen omdat ze met Azure Kubernetes Service (AKS zijn)?

Nee. Alle resources, met inbegrip van de cluster-master, worden uitgevoerd in uw abonnement van de klant. Deze typen resources worden in een alleen-lezen resourcegroep geplaatst.
