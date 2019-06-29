---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een azure kubernetes (AKS)-resource.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1d68c08f6dfca74c38973af1686d614f3f10cc28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455116"
---
## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>Maak een clusterresource Azure Kubernetes Service (AKS)

1. Ga naar de [maken](https://ms.portal.azure.com/#create/microsoft.aks) voor Kubernetes-Services.

1. Op de **basisbeginselen** tabblad, voer de volgende details:

    |Instelling|Value|
    |--|--|
    |Abonnement|Selecteer de juiste abonnement|
    |Resourcegroep|Selecteer een beschikbare resourcegroep|
    |De naam van de Kubernetes-cluster|Gewenste naam (kleine letters)|
    |Regio|Selecteer een locatie in de buurt|
    |Kubernetes-versie|1.12.8 (standaard)|
    |DNS-naamvoorvoegsel|Automatisch gemaakt, maar u kunt desgewenst kunt negeren|
    |Knooppuntgrootte|Standard DS2 versie 2:<br>`2 vCPUs`, `7 GB`|
    |Aantal knooppunten|Laat u de schuifregelaar op de standaardwaarde|

1. Op de **schaal** tabblad, laat u de *virtuele knooppunten* en *virtuele-machineschaalsets* standaardwaarden.
1. Op de **verificatie** tabblad, laat u *Service-principal* en *RBAC inschakelen* standaardwaarden.
1. Op de **netwerken** tabblad, voer de volgende selecties:

    |Instelling|Value|
    |--|--|
    |Routering van HTTP-toepassing|Nee|
    |Netwerkconfiguratie|Basic|

1. Op de **bewaking** tabblad, zorg ervoor dat *containerbewaking inschakelen* is ingesteld op **Ja** en laat de *Log Analytics-werkruimte* als de standaardwaarde
1. Op de **Tags** tabblad, de naam/waarde-paren voorlopig leeg laten.
1. Klik op **controleren en maken**.
1. Nadat de validatie is geslaagd, klikt u op **maken**.

> [!NOTE]
> Als validatie mislukt, mogelijk vanwege een *Service-principal* fout. Ga terug naar de *verificatie* tabblad en weer terug naar *revisie + maken* waar validatie moet uitvoeren en klikt u vervolgens doorgeven.
