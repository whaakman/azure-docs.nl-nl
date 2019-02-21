---
title: Web service-eindpunten maken
titleSuffix: Azure Machine Learning Studio
description: Maak web service-eindpunten in Azure Machine Learning Studio. Elk eindpunt in de webservice is onafhankelijk van elkaar geadresseerd, beperkt en beheerd.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/15/2019
ms.openlocfilehash: 62505a89be5535f7b8b7b50ad2462e33d44db57a
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454001"
---
# <a name="create-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Eindpunten voor geïmplementeerde Azure Machine Learning Studio-webservices maken

> [!NOTE]
> Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice.

Na de implementatie van een webservice wordt een standaardeindpunt voor de service gemaakt. Het standaardeindpunt kan worden aangeroepen met de API-sleutel. U kunt meer eindpunten toevoegen met hun eigen sleutels van de Web Services-portal.
Elk eindpunt in de webservice is onafhankelijk van elkaar geadresseerd, beperkt en beheerd. Elk eindpunt is een unieke URL met een autorisatiesleutel die u aan uw klanten kunt distribueren.

## <a name="add-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice

U kunt een eindpunt toevoegen aan een webservice met behulp van de portal van Azure Machine Learning-webservices. Zodra het eindpunt is gemaakt, kunt u deze via synchrone API's, batch-API's, gebruiken en excel-werkbladen.

> [!NOTE]
> Als u extra eindpunten aan de web-service hebt toegevoegd, kunt u het standaardeindpunt niet verwijderen.

1. In Machine Learning Studio, klikt u op de linkernavigatiekolom op Web Services.
2. Klik aan de onderkant van het dashboard van de webservice op **-eindpunten beheren**. De Azure Machine Learning-webserviceportal geopend op de pagina eindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Namen van eindpunten 24 tekens of minder lang moeten zijn en moeten bestaan uit kleine letters of cijfers. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a id="scaling"></a> Een webservice schalen door extra eindpunten toe te voegen

Standaard is elke gepubliceerde webservice is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen, en mag wel 200 gelijktijdige aanvragen. Azure Machine Learning Studio wordt automatisch geoptimaliseerd voor de instelling voor de beste prestaties voor uw webservice en de portal waarde wordt genegeerd.

Als u wilt de API met een hogere belasting dan een waarde voor maximum aantal gelijktijdige aanroepen van 200 aanroepen ondersteunt, moet u meerdere eindpunten maken van de dezelfde web-service. U kunt uw load willekeurig distribueren op alle.

De schaal van een webservice is een veelvoorkomende taak. Er zijn enkele redenen om te schalen naar meer dan 200 gelijktijdige aanvragen ondersteunen, hogere mate van beschikbaarheid via meerdere eindpunten of het bieden van afzonderlijke eindpunten voor de webservice. U kunt de schaal vergroten door het toevoegen van extra eindpunten voor de dezelfde webservice via de [Azure Machine Learning-webservice](https://services.azureml.net/) portal.

Houd er rekening mee dat met behulp van een aantal hoge gelijktijdigheid schadelijk als u niet van de API met een overeenkomstige hoge frequentie aanroepen bent. U ziet er kunnen sporadisch time-outs en/of pieken in de latentie als u een relatief lage belasting voor een API die is geconfigureerd voor hoge belasting.

De synchrone API's worden meestal gebruikt in situaties waar een lage latentie is vereist. Hier latentie betekent de tijd die nodig is voor de API om een aanvraag te voltooien en niet wordt gehouden met eventuele vertragingen in het netwerk. Stel dat u hebt een API met een latentie 50 ms. Volledig gebruiken voor de beschikbare capaciteit met toevoerregelniveau hoog en het maximum aantal gelijktijdige aanroepen = 20, moet u deze API 20 * 1000 aanroepen / 50 = 400 keer per seconde. Een maximum aantal gelijktijdige aanroepen van 200 kunt dit verder uitgebreid, u voor het aanroepen van de API 4000 mislukte opvragingen per seconde, ervan uitgaande dat het een 50 ms latentie.

## <a name="next-steps"></a>Volgende stappen

[Het gebruik van een Azure Machine Learning-webservice](consume-web-services.md).
