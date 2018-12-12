---
title: Web-parameters van de service - Azure Machine Learning Studio | Microsoft Docs
description: Het gebruik van Azure Machine Learning Web Service Parameters aan het gedrag van uw model wijzigen wanneer de web-service wordt geopend.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.openlocfilehash: 526c89a83a102c80bf1ddb5a5d950b0367a9cd69
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53076369"
---
# <a name="use-azure-machine-learning-studio-web-service-parameters"></a>Azure Machine Learning Studio webserviceparameters gebruiken
Een Azure Machine Learning-webservice wordt gemaakt door het publiceren van een experiment met modules met configureerbare parameters. In sommige gevallen kunt u het gedrag van de module niet wijzigen terwijl de webservice wordt uitgevoerd. *Web-Parameters van de Service* zodat u deze taak. 

Een veelvoorkomend voorbeeld is het instellen van de [importgegevens] [ reader] module zodat de gebruiker van de gepubliceerde webservice een andere gegevensbron opgeven kan wanneer de web-service wordt geopend. Of het configureren van de [gegevens exporteren] [ writer] module zodat een andere bestemming kan worden opgegeven. Enkele andere voorbeelden zijn wijzigen van het aantal bits voor de [hash-functies] [ feature-hashing] module of het aantal gewenste functies voor de [Functieselectie op basis van een Filter] [ filter-based-feature-selection] module. 

U kunt Parameters van de Web Service en deze koppelen aan een of meer moduleparameters in uw experiment, en kunt u opgeven of ze verplicht of optioneel zijn. De gebruiker van de webservice kan vervolgens waarden opgeven voor deze parameters wanneer ze de webservice aanroept. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a>Het instellen en Webserviceparameters gebruiken
U definieert een Parameter van Web Service door te klikken op het pictogram naast de parameter voor een module en selecteer 'Instellen als web Serviceparameter'. Hiermee maakt u een nieuwe Web Service-Parameter en verbindt u deze met deze module-parameter. Als de webservice wordt geopend, vervolgens de gebruiker kan een waarde opgeven voor de Web Service-Parameter en wordt toegepast op de module-parameter.

Wanneer u een Web Service-Parameter definiëren, is het beschikbaar voor een andere module-parameter in het experiment. Als u een Web-Service-Parameter die is gekoppeld aan een parameter voor één module definiëren, kunt u die dezelfde Web Service-Parameter voor een andere module, zolang de parameter wordt verwacht de hetzelfde type waarde dat. Bijvoorbeeld, als de Web Service-Parameter een numerieke waarde is, kan klikt u vervolgens deze alleen worden gebruikt voor de parameters van de module die een numerieke waarde verwacht. Wanneer de gebruiker wordt een waarde voor de Web Service-Parameter ingesteld, wordt deze worden toegepast op alle bijbehorende module-parameters.

U kunt vervolgens beslissen of een standaardwaarde opgeven voor de Web Service-Parameter. Als u dit doet, klikt u vervolgens is de parameter optioneel voor de gebruiker van de webservice. Als u een standaardwaarde niet opgeeft, is de gebruiker vereist om een waarde wanneer de web-service wordt geopend.

De API-documentatie voor de webservice bevat informatie voor de gebruiker van de web-service voor het opgeven van de Web Service-Parameter is het via een programma bij het openen van de webservice.

> [!NOTE]
> De API-documentatie voor een klassieke webservice wordt geboden via de **API help-pagina** koppeling in de webservice **DASHBOARD** in Machine Learning Studio. De API-documentatie voor een nieuwe webservice wordt geleverd via de [Azure Machine Learning-webservices](https://services.azureml.net/Quickstart) portal op de **verbruiken** en **Swagger API** pagina's voor uw web- de service.
> 
> 

## <a name="example"></a>Voorbeeld
Een voorbeeld: Stel dat we hebben een experiment met een [gegevens exporteren] [ writer] module waarmee gegevens worden verzonden naar Azure blob-opslag. Definiëren we een Web-Service-Parameter met de naam 'Blob-pad' waarmee de gebruiker van de web service het pad naar de blob-opslag wijzigen wanneer de service wordt geopend.

1. In Machine Learning Studio, klikt u op de [gegevens exporteren] [ writer] module om deze te selecteren. De eigenschappen worden weergegeven in het deelvenster met eigenschappen aan de rechterkant van het experimentcanvas.
2. Geef het opslagtype:
   
   * Onder **Geef bestemming**, selecteert u 'Azure Blob-opslag.
   * Onder **Geef verificatietype**, selecteert u 'Account'.
   * Voer de accountgegevens voor de Azure blob-opslag. 

3. Klik op het pictogram aan de rechterkant van de **pad naar de blob die begint met de parameter container**. Het ziet er als volgt uit:
   
   ![Web Service Parameter-pictogram][icon]
   
   Selecteer 'Instellen als web Serviceparameter'.
   
   Een vermelding wordt toegevoegd onder **Webserviceparameters** onderaan in het deelvenster met eigenschappen met de naam 'Pad naar de blob die begint met de container'. Dit is de Web Service-Parameter die is nu gekoppeld aan dit [gegevens exporteren] [ writer] module-parameter.
4. Wijzig de naam van de Web Service-Parameter, klikt u op de naam, typt u 'blobpad', en druk op de **Enter** sleutel. 
5. Als u wilt een standaardwaarde opgeven voor de Web Service-Parameter, klik op het pictogram aan de rechterkant van de naam, selecteert u "Geef standaardwaarde", voer een waarde (bijvoorbeeld ' container1/output1.csv'), en druk op de **Enter** sleutel.
   
   ![Web Service-Parameter][parameter]
6. Klik op **Run**. 
7. Klik op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [Nieuw]** om de webservice te implementeren.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waarvoor u de webservice implementeert. Zie voor meer informatie, [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md). 

De gebruiker van de webservice kan nu opgeven voor een nieuwe bestemming voor de [gegevens exporteren] [ writer] module bij het openen van de webservice.

## <a name="more-information"></a>Meer informatie
Zie voor een uitgebreider voorbeeld de [Webserviceparameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) vermelding in de [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Zie voor meer informatie over het verkrijgen van toegang tot een Machine Learning-webservice [hoe u een Azure Machine Learning-webservice gebruiken](consume-web-services.md).

<!-- Images -->
[icon]: ./media/web-service-parameters/icon.png
[parameter]: ./media/web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

