---
title: Een resource van Azure Kubernetes Service-cluster maken
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een bron voor Azure Kubernetes Service (AKS).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: ab7ce8b4a538e6a286a00285069a22878c5d88d9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877467"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Een resource van Azure Kubernetes Service-cluster maken

1. Ga naar [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks) en selecteer **maken**.

1. Op de **basisbeginselen** tabblad, voer de volgende informatie:

    |Instelling|Value|
    |--|--|
    |Subscription|Selecteer een geschikte-abonnement|
    |Resource group|Selecteer een beschikbare resourcegroep|
    |De naam van de Kubernetes-cluster|Voer een naam (kleine letters)|
    |Regio|Selecteer een locatie in de buurt|
    |Kubernetes-versie|1.12.8 (standaard)|
    |DNS-naamvoorvoegsel|Automatisch gemaakt, maar u kunt negeren|
    |Knooppuntgrootte|Standard DS2 versie 2:<br>`2 vCPUs`, `7 GB`|
    |Aantal knooppunten|Laat u de schuifregelaar op de standaardwaarde|

1. Op de **schaal** tabblad, laat u **virtuele knooppunten** en **virtuele-machineschaalsets (preview)** ingesteld op de standaardwaarden.
1. Op de **verificatie** tabblad, laat u **Service-principal** en **RBAC inschakelen** ingesteld op de standaardwaarden.
1. Op de **netwerken** tabblad, voer de volgende selecties:

    |Instelling|Waarde|
    |--|--|
    |Routering van HTTP-toepassing|Nee|
    |Netwerkconfiguratie|Basic|

1. Op de **bewaking** tabblad, zorg ervoor dat **containerbewaking inschakelen** is ingesteld op **Ja**, en laat **Log Analytics-werkruimte** als de de standaardwaarde.
1. Op de **Tags** tabblad, de naam/waarde-paren voorlopig leeg laten.
1. Selecteer **controleren en maken**.
1. Nadat de validatie is geslaagd, schakelt u **maken**.

> [!NOTE]
> Als validatie mislukt, mogelijk vanwege een fout 'Service-principal'. Ga terug naar de **verificatie** tabblad en vervolgens weer terug naar **revisie + maken**, waarbij validatie moet uitvoeren en klikt u vervolgens doorgeven.
