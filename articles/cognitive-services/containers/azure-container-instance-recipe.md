---
title: Azure Container Instance recept
titleSuffix: Azure Cognitive Services
description: Informatie over het implementeren van Cognitive Services-Containers in Azure Container Instances
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 288894705e1108d6dd511b60cd2bc3bcee4c6d41
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704357"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>Implementeren en uitvoeren van de container in Azure Container Instances

Met de volgende stappen, schalen van Azure Cognitive Services-toepassingen in de cloud eenvoudig met Azure [Containerinstantie](https://docs.microsoft.com/azure/container-instances/). Met deze kunt u zich richten op het bouwen van uw toepassingen in plaats van het beheren van de infrastructuur.

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

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](includes/create-container-instances-resource.md)]

## <a name="use-the-container-instance"></a>Gebruik de Containerinstantie

1. Selecteer de **overzicht** en kopieert u het IP-adres. Dit is een numerieke IP-adres zoals `55.55.55.55`.
1. Open een nieuw browsertabblad en gebruikt u het IP-adres bijvoorbeeld `http://<IP-address>:5000 (http://55.55.55.55:5000`). U ziet de startpagina van de container, zodat u weet dat de container wordt uitgevoerd.

1. Selecteer **beschrijving van de API-Service** om de swagger-pagina voor de container weer te geven.

1. Selecteer een van de **POST** API's en selecteer **Try it out in**.  De parameters worden weergegeven met inbegrip van de invoer. Vul de parameters in.

1. Selecteer **Execute** de aanvraag verzenden naar uw Container-exemplaar.

    U hebt gemaakt en gebruikt de Cognitive Services-containers in Azure Container Instances.
