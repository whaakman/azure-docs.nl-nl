---
title: Azure Container Instance recept
titleSuffix: Azure Cognitive Services
description: Informatie over het implementeren van Cognitive Services-Containers in Azure Container Instances
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 06/12/2019
ms.author: dapine
ms.openlocfilehash: 5de11b62bb2a2302c247907627e27339400fcf25
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207489"
---
# <a name="deploy-and-run-container-on-azure-container-instance-aci"></a>Implementeren en uitvoeren van container in Azure Container exemplaar (ACI)

Met de volgende stappen, schalen van Azure Cognitive Services-toepassingen in de cloud eenvoudig met Azure [Containerinstantie](https://docs.microsoft.com/azure/container-instances/) (ACI). Met deze kunt u zich richten op het bouwen van uw toepassingen in plaats van het beheren van de infrastructuur.

## <a name="prerequisites"></a>Vereisten

Deze oplossing werkt met alle Cognitive Services-container. Resource voor de Cognitive Service moet worden gemaakt in Azure portal voordat u deze recept. Elke Cognitive Service die ondersteuning biedt voor containers heeft een document 'How to install' specifiek voor het installeren en configureren van de service voor een container. Omdat sommige services een bestand of een set van bestanden als invoer voor de container moet, is het belangrijk dat u begrijpt en de container is al eerder hebben gebruikt met behulp van deze oplossing.

* Een Cognitive Service resource gemaakt in Azure portal.
* Cognitive Service **eindpunt-URL** -Raadpleeg uw specifieke service "installeren" voor de container, om te zoeken waar de eindpunt-URL is vanuit de Azure-portal, en wat een juist voorbeeld van de URL ziet eruit als. De precieze indeling kunt wijzigen van services.
* Cognitive Service **sleutel** -de sleutels zich op de **sleutels** -pagina voor de Azure-resource. U hoeft slechts een van de twee sleutels. De sleutel is een tekenreeks van 32 alfanumerieke tekens.
* Een enkele Cognitive Services-Container op uw lokale host (uw computer). Zorg ervoor dat u kunt:
  * Haal de installatiekopie met een `docker pull` opdracht.
  * De lokale container is uitgevoerd met alle vereiste configuratie-instellingen met een `docker run` opdracht.
  * Aanroepen van de container-eindpunt, krijgen een reactie van 2xx en een JSON-antwoord.

Alle variabelen in vierkante haken, `<>`, moeten worden vervangen door uw eigen waarden. Deze vervanging bevat de punthaken.

## <a name="step-2-launch-your-container-on-azure-container-instances-aci"></a>Stap 2: Start uw container in Azure Container Instances (ACI)

**Het maken van de resource van Azure Container exemplaar (ACI).**

1. Ga naar de [maken](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) pagina voor Container Instances.

1. Op de **basisbeginselen** tabblad, voer de volgende details:

    |Pagina|Instelling|Value|
    |--|--|--|
    |Basics|Abonnement|Selecteer uw abonnement.|
    |Basics|Resourcegroep|Selecteer de beschikbare resourcegroep of maak een nieuwe zoals `cognitive-services`.|
    |Basics|Containernaam|Voer een naam zoals `cognitive-container-instance`. Deze naam moet zich in lagere limieten.|
    |Basics|Locatie|Selecteer een regio voor implementatie.|
    |Basics|Type installatiekopie|`Public`|
    |Basics|De naam van installatiekopie|Geef de locatie van de Cognitive Services-Container. Dit kan zijn dat u hebt gebruikt in dezelfde locatie de `docker pull` opdracht _bijvoorbeeld_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Basics|Type besturingssysteem|`Linux`|
    |Basics|Grootte|Grootte wijzigen in de voorgestelde aanbevelingen voor uw specifieke Cognitive Service-container.:<br>2 kernen<br>4 GB
    ||||
  
1. Op de **netwerken** tabblad, voer de volgende details:

    |Pagina|Instelling|Value|
    |--|--|--|
    |Netwerken|Poorten|Het bewerken van de bestaande poort voor TCP van `80` naar `5000`. Dit betekent dat u de container op poort 5000 wordt weergegeven.|
    ||||

1. Op de **Geavanceerd** tabblad, voer de volgende gegevens doorgeven aan de vereiste instellingen voor de resource Containerinstantie facturering container:

    |Pagina Geavanceerde sleutel|Pagina Geavanceerd waarde|
    |--|--|
    |`apikey`|Gekopieerd van de **sleutels** pagina van de resource. U moet slechts één van de twee sleutels. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Gekopieerd van de **overzicht** pagina van de resource. |
    |`eula`|`accept`|

    Als de container andere configuratie-instellingen zoals de koppeling van de invoer, uitvoer koppelingen of logboekregistratie heeft, moeten deze instellingen ook moeten worden toegevoegd.

1. Selecteer **controleren en maken**.
1. Nadat de validatie is geslaagd, schakelt u **maken** voor het maken van het proces te voltooien.
1. Selecteer het belpictogram in de bovenste navigatiebalk. Dit is het meldingenvenster. Er verschijnt een blauwe **naar de resource gaan** wanneer de resource is gemaakt. Selecteer de knop gaat u de nieuwe resource.

## <a name="use-the-container-instance"></a>Gebruik de Containerinstantie

1. Selecteer de **overzicht** en kopieert u het IP-adres. Dit is een numerieke IP-adres zoals `55.55.55.55`.
1. Open een nieuw browsertabblad en gebruikt u het IP-adres bijvoorbeeld `http://<IP-address>:5000 (http://55.55.55.55:5000`). U ziet de startpagina van de container, zodat u weet dat de container wordt uitgevoerd.

1. Selecteer **beschrijving van de API-Service** om de swagger-pagina voor de container weer te geven.

1. Selecteer een van de **POST** API's en selecteer **Try it out in**.  De parameters worden weergegeven met inbegrip van de invoer. Vul de parameters in.

1. Selecteer **Execute** de aanvraag verzenden naar uw Container-exemplaar.

    U hebt gemaakt en gebruikt de Cognitive Services-containers in Azure Container Instances.
