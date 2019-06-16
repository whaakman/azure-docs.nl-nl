---
title: Azure Red Hat OpenShift oplossen | Microsoft Docs
description: Oplossen van veelvoorkomende problemen met Azure Red Hat OpenShift
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: 7f2bdf643f12671bec3d0c087d8775844099fe9a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306248"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Probleemoplossing voor Azure Red Hat OpenShift

Dit artikel worden enkele veelvoorkomende problemen aangetroffen tijdens het maken of beheren van Microsoft Azure Red Hat OpenShift-clusters.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Opnieuw proberen van het maken van een cluster is mislukt

Als het maken van een Azure Red Hat OpenShift cluster met behulp van de `az` CLI-opdracht mislukt, opnieuw wordt geprobeerd de maken blijft mislukken.
Gebruik `az openshift delete` als u wilt verwijderen van het cluster is mislukt, moet u vervolgens een volledig nieuw cluster maken.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Verborgen Azure Red Hat OpenShift-clusterresourcegroep

Op dit moment de `Microsoft.ContainerService/openShiftManagedClusters` resource dat automatisch wordt gemaakt door de Azure CLI (`az openshift create` opdracht) is niet zichtbaar in de Azure-portal. In de **resourcegroep** weergave selectievakje **verborgen typen weergeven** om de resourcegroep weer te geven.

![Schermafbeelding van het selectievakje verborgen type in de portal](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Resultaten van een cluster maken in de fout die geen geregistreerde resourceprovider gevonden

Als de resultaten van een cluster maken in een fout die `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview`, en vervolgens u onderdeel van de Preview-versie waren en nu moet [aankoop virtuele machine van Azure gereserveerde instanties](https://aka.ms/openshift/buy) gebruik van het product algemeen beschikbaar. Een reservering vermindert uw uitgaven vooraf betalen voor volledig beheerde Azure-services. Raadpleeg [ *wat zijn Azure-reserveringen* ](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) voor meer informatie over reserveringen en hoe ze u geld besparen.

## <a name="next-steps"></a>Volgende stappen

- Probeer de [Red Hat OpenShift Help en ondersteuning](https://help.openshift.com/) voor meer op OpenShift oplossen van problemen.

- Vind antwoorden op [Veelgestelde vragen over Azure Red Hat OpenShift](openshift-faq.md).
