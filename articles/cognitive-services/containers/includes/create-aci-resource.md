---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een azure container instance (ACI)-bron.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: b24a78873c3220b969fc548d2553e465e80773cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455061"
---
## <a name="create-an-azure-container-instance-aci-resource"></a>Maak een Azure Container exemplaar (ACI)-resource

1. Ga naar de [maken](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) pagina voor Container Instances.

2. Op de **basisbeginselen** tabblad, voer de volgende details:

    |Instelling|Value|
    |--|--|
    |Abonnement|Selecteer uw abonnement.|
    |Resourcegroep|Selecteer de beschikbare resourcegroep of maak een nieuwe zoals `cognitive-services`.|
    |Containernaam|Voer een naam zoals `cognitive-container-instance`. Deze naam moet zich in lagere limieten.|
    |Location|Selecteer een regio voor implementatie.|
    |Type installatiekopie|`Public`|
    |De naam van installatiekopie|Geef de locatie van de Cognitive Services-Container. Dit kan zijn dat u hebt gebruikt in dezelfde locatie de `docker pull` opdracht _bijvoorbeeld_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Type besturingssysteem|`Linux`|
    |Size|Grootte wijzigen in de voorgestelde aanbevelingen voor uw specifieke Cognitive Service-container.:<br>2 kernen<br>4 GB

3. Op de **netwerken** tabblad, voer de volgende details:

    |Instelling|Value|
    |--|--|
    |Poorten|De TCP-poort ingesteld op `5000`. Wordt aangegeven dat de container op poort 5000.|

4. Op de **Geavanceerd** tabblad, voer de volgende gegevens doorgeven aan de container [vereiste facturering instellingen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) aan de ACI-resource:

    |Pagina Geavanceerde sleutel|Pagina Geavanceerd waarde|
    |--|--|
    |`apikey`|Gekopieerd van de **sleutels** pagina van de Text Analytics-resource. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Gekopieerd van de **overzicht** pagina van de Text Analytics-resource. Voorbeeld: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Klik op **controleren en maken**
1. Nadat de validatie is geslaagd, klikt u op **maken** voor het maken van het proces te voltooien
1. Wanneer de resource is geïmplementeerd, is het gereed voor gebruik