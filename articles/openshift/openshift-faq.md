---
title: Veelgestelde vragen over Azure Red Hat OpenShift | Microsoft Docs
description: Hier vindt u antwoorden op veelgestelde vragen over Microsoft Azure Red Hat OpenShift
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 2001b849e9c43d552889475ca237c52b141f3f04
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306266"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift Veelgestelde vragen

In dit artikel komen de antwoorden op veelgestelde vragen over Microsoft Azure Red Hat OpenShift.

## <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?

Voordat u Azure Red Hat OpenShift gebruiken kunt, moet u een minimum van 4 Azure Red Hat OpenShift gereserveerd toepassing knooppunten kopen.

Als u een Azure-klant bent,[Azure Red Hat OpenShift gereserveerde instanties kopen](https://aka.ms/openshift/buy) via Azure portal. Na de aankoop, wordt uw abonnement binnen 24 uur, waarna u wordt mogelijk clusters inrichten worden geactiveerd.

Als u niet een Azure-klant, [Neem contact op met verkoop](https://aka.ms/openshift/contact-sales) en vul het formulier de omzet in aan de onderkant van de pagina om het proces te starten.

Raadpleeg de [Azure Red Hat OpenShift pagina met prijzen](https://aka.ms/openshift/pricing) voor meer informatie.

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

## <a name="is-open-service-broker-for-azure-osba-supported"></a>Is Open Service Broker for Azure (OSBA) ondersteund?

Ja. U kunt OSBA gebruiken met Azure Red Hat OpenShift. Zie [Open Service Broker for Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) voor meer informatie.
