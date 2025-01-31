---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een Azure container instance-bron.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 38addf4651373ba0f4df411325218a255c835508
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717188"
---
## <a name="create-an-azure-container-instance-resource"></a>Maak een Azure Container Instances-resource

1. Ga naar de [maken](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) pagina voor Container Instances.

2. Op de **basisbeginselen** tabblad, voer de volgende details:

    |Instelling|Value|
    |--|--|
    |Subscription|Selecteer uw abonnement.|
    |Resource group|Selecteer de beschikbare resourcegroep of maak een nieuwe zoals `cognitive-services`.|
    |Containernaam|Voer een naam zoals `cognitive-container-instance`. De naam mag niet in lagere limieten.|
    |Location|Selecteer een regio voor implementatie.|
    |Type installatiekopie|`Public`|
    |Naam van installatiekopie|Geef de locatie van de container Cognitive Services. De locatie mag hetzelfde zijn als voor de `docker pull` opdracht, verwijzen naar de [container opslagplaatsen en installatiekopieën](../../cognitive-services-container-support.md#container-repositories-and-images) voor de namen van de beschikbare installatiekopieën en hun bijbehorende opslagplaats.|
    |Type besturingssysteem|`Linux`|
    |Size|Wijzig de grootte in de voorgestelde aanbevelingen voor uw specifieke Cognitive Service-container:<br>2 CPU-kernen<br>4 GB

3. Op de **netwerken** tabblad, voer de volgende details:

    |Instelling|Value|
    |--|--|
    |Poorten|De TCP-poort ingesteld op `5000`. Wordt aangegeven dat de container op poort 5000.|

4. Op de **Geavanceerd** tabblad, voer de vereiste **omgevingsvariabelen** voor de container [instellingen facturering](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) van de ACI-resource:

    | Sleutel | Value |
    |--|--|
    |`apikey`|Gekopieerd van de **sleutels** pagina van de Text Analytics-resource. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Gekopieerd van de **overzicht** pagina van de Text Analytics-resource. Voorbeeld: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Klik op **controleren en maken**
1. Nadat de validatie is geslaagd, klikt u op **maken** voor het maken van het proces te voltooien
1. Wanneer de resource is geïmplementeerd, is het gereed