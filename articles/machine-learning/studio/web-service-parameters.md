---
title: Gebruik Azure Machine Learning-Webserviceparameters | Microsoft Docs
description: Het gebruik van Azure Machine Learning Web Service Parameters om het gedrag van uw model wanneer de web-service wordt geopend.
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 715ea008b84c1a503661394da14e8af167327941
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a>Parameters voor Azure Machine Learning-webservice
Een Azure Machine Learning-webservice wordt gemaakt door het publiceren van een experiment die modules met configureerbare parameters bevat. In sommige gevallen wilt u mogelijk het gedrag van de module niet wijzigen terwijl de webservice wordt uitgevoerd. *Web-Service Parameters* kunt u deze taak. 

Een veelvoorkomend voorbeeld tot stand brengen van de [importgegevens] [ reader] module zodat de gebruiker de gepubliceerde web service een andere gegevensbron opgeven kunt wanneer de web-service wordt geopend. Of het configureren van de [gegevens exporteren] [ writer] module zodat een andere bestemming kan worden opgegeven. Enkele andere voorbeelden zijn het aantal bits voor het wijzigen van de [hash-functie] [ feature-hashing] module of het aantal gewenste functies voor de [Functieselectie op basis van het Filter] [ filter-based-feature-selection] module. 

U kunt de Parameters van de Web-Service en koppel deze aan een of meer moduleparameters in uw experiment en kunt u opgeven of ze vereist of optioneel zijn. De gebruiker van de webservice kunt vervolgens waarden opgeven voor deze parameters wanneer ze de webservice aanroept. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a>Het instellen en gebruiken van Parameters van Web Service
U definieert u een Parameter van Web-Service op het pictogram naast de parameter voor een module en selecteer 'Instellen als web Serviceparameter'. Hiermee maakt u een nieuwe Parameter van de Web-Service en met die module-parameter is verbonden. Als de webservice wordt geopend, vervolgens de gebruiker kan een waarde opgeven voor de Parameter en deze wordt toegepast op de module-parameter.

Wanneer u een Parameter van Web Service definieert, is het beschikbaar voor andere module parameter in het experiment. Als u een Web Service-Parameter die is gekoppeld aan een parameter voor één module definieert, kunt u die dezelfde Web Service-Parameter voor module, zolang de parameter hetzelfde type waarde verwacht. Bijvoorbeeld, als de Parameter een numerieke waarde is, kan klikt u vervolgens alleen worden gebruikt voor de parameters van de module die een numerieke waarde verwacht. Wanneer de gebruiker een waarde worden ingesteld voor de Parameter, wordt deze wordt toegepast op alle bijbehorende module-parameters.

U kunt beslissen of een standaardwaarde opgeven voor de Web Service-Parameter. Als u dit doet, zijn de parameter is optioneel voor de gebruiker van de webservice. Als u een standaardwaarde niet opgeeft, is de gebruiker vereist een waarde invoeren als de webservice wordt geopend.

De API-documentatie voor de webservice bevat informatie voor de gebruiker van de web-service op de Parameter programmatisch opgeven bij het openen van de webservice.

> [!NOTE]
> De API-documentatie voor een klassieke webservice is beschikbaar via de **API help-pagina** koppeling in de webservice **DASHBOARD** in Machine Learning Studio. De API-documentatie voor een nieuwe webservice is beschikbaar via de [Azure Machine Learning-webservices](https://services.azureml.net/Quickstart) portal op de **verbruiken** en **Swagger API** pagina's voor uw webservice.
> 
> 

## <a name="example"></a>Voorbeeld
Een voorbeeld: Stel hebben we een experiment met een [gegevens exporteren] [ writer] module waarmee gegevens worden verzonden naar Azure blob storage. Definiëren we Web Service Parameter met de naam 'Blob-pad' waarmee de gebruiker van de web service het pad naar de blob-opslag wijzigen wanneer de service wordt geopend.

1. In Machine Learning Studio, klikt u op de [gegevens exporteren] [ writer] module om deze te selecteren. De eigenschappen worden weergegeven in het deelvenster Properties rechts van het experimentcanvas.
2. Geef het opslagtype:
   
   * Onder **Geef gegevensbestemming**, selecteert u 'Azure Blob Storage'.
   * Onder **Geef verificatietype**, selecteert u 'Account'.
   * Geef de accountgegevens voor de Azure blob-opslag. 
     <p />
3.Klik op het pictogram aan de rechterkant van de **pad naar de blob die begint met de parameter container**. Als volgt uitziet:
   
   ![Web Service Parameter-pictogram][icon]
   
   Selecteer 'Instellen als web Serviceparameter'.
   
   Een item wordt toegevoegd onder **Webserviceparameters** onderaan in het deelvenster Eigenschappen met de naam 'Pad naar de blob die begint met de container'. Dit is de Parameter die is gekoppeld aan dit [gegevens exporteren] [ writer] module-parameter.
4. Wijzig de naam van de Parameter, klikt u op de naam, typt u 'blobpad', en druk op de **Enter** sleutel. 
5. Als u wilt een standaardwaarde opgeven voor de Parameter, klik op het pictogram aan de rechterkant van de naam, selecteert u 'Provide standaardwaarde', voer een waarde (bijvoorbeeld ' container1/output1.csv') en druk op de **Enter** sleutel.
   
   ![Web Service-Parameter][parameter]
6. Klik op **Run**. 
7. Klik op **webservice implementeren** en selecteer **webservice implementeren [klassieke]** of **Web Service implementeren [New]** voor het implementeren van de webservice.

> [!NOTE] 
> Voor het implementeren van een nieuwe webservice moet u voldoende machtigingen hebben in het abonnement waaraan u de webservice implementeren. Zie voor meer informatie, [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md). 

De gebruiker van de webservice kan nu opgeven voor een nieuwe bestemming voor de [gegevens exporteren] [ writer] module bij het openen van de webservice.

## <a name="more-information"></a>Meer informatie
Zie voor een uitgebreider voorbeeld de [Webserviceparameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) vermelding in de [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Zie voor meer informatie over de toegang tot een Machine Learning-webservice [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md).

<!-- Images -->
[icon]: ./media/web-service-parameters/icon.png
[parameter]: ./media/web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

