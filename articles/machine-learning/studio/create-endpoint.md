---
title: Het maken van Web service-eindpunten
titleSuffix: Azure Machine Learning Studio
description: Het maken van Web service-eindpunten in Azure Machine Learning. Elk eindpunt in de webservice is onafhankelijk van elkaar geadresseerd, beperkt en beheerd.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/12/2019
ms.openlocfilehash: a6945ac7bfb750916e229ae04376f895f2b3d506
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267265"
---
# <a name="creating-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Het maken van eindpunten voor geïmplementeerde Azure Machine Learning Studio-webservices

> [!NOTE]
> Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice.

Na de implementatie van een webservice wordt een standaardeindpunt voor de service gemaakt. Het standaardeindpunt kan worden aangeroepen met de API-sleutel. U kunt meer eindpunten toevoegen met hun eigen sleutels van de Web Services-portal.
Elk eindpunt in de webservice is onafhankelijk van elkaar geadresseerd, beperkt en beheerd. Elk eindpunt is een unieke URL met een autorisatiesleutel die u aan uw klanten kunt distribueren.

## <a name="adding-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice

U kunt een eindpunt toevoegen aan een webservice met behulp van de portal van Azure Machine Learning-webservices. Zodra het eindpunt is gemaakt, kunt u deze via synchrone API's, batch-API's, gebruiken en excel-werkbladen.

> [!NOTE]
> Als u extra eindpunten aan de Web-service hebt toegevoegd, kunt u het standaardeindpunt niet verwijderen.

1. In Machine Learning Studio, klikt u op de linkernavigatiekolom op Web Services.
2. Klik aan de onderkant van het dashboard van de webservice op **-eindpunten beheren**. De Azure Machine Learning-webserviceportal geopend op de pagina eindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Namen van eindpunten 24 tekens of minder lang moeten zijn en moeten bestaan uit kleine letters of cijfers. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a name="next-steps"></a>Volgende stappen

[Het gebruik van een Azure Machine Learning-webservice](consume-web-services.md).
