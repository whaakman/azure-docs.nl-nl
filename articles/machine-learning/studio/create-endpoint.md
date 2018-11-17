---
title: Het maken van de webservice-eindpunten in Machine Learning | Microsoft Docs
description: Het maken van de webservice-eindpunten in Azure Machine Learning
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.custom: (previous ms.author yahajiza)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.component: studio
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.openlocfilehash: 45115f0a987093d7e3816891fa5f4c242b449fb7
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51819233"
---
# <a name="creating-endpoints"></a>Eindpunten maken
> [!NOTE]
>  Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice.
> 
> 

Wanneer u Web-services die u vooruit aan uw klanten verkopen maakt, moet u voor getrainde modellen voor elke klant die nog steeds zijn gekoppeld aan het experiment van waaruit de Web-service is gemaakt. Bovendien moeten eventuele updates naar het experimentcanvas worden toegepast selectief op een eindpunt worden overschreven en dat de aanpassingen.

U doet dit door kunt Azure Machine Learning u meerdere eindpunten voor een geïmplementeerde webservice maken. Elk eindpunt in de webservice is onafhankelijk van elkaar geadresseerd, beperkt en beheerd. Elk eindpunt is een unieke URL en de autorisatiesleutel die u aan uw klanten kunt distribueren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice
Er zijn twee manieren om een eindpunt toevoegen aan een webservice.

* Programmatisch
* Via de portal van Azure Machine Learning-webservices

Zodra het eindpunt is gemaakt, kunt u deze via synchrone API's, batch-API's, gebruiken en excel-werkbladen. Naast het toevoegen van eindpunten via deze gebruikersinterface, kunt u ook de eindpunt-Management API's gebruiken om toe te voegen via een programma eindpunten.

> [!NOTE]
> Als u extra eindpunten aan de Web-service hebt toegevoegd, kunt u het standaardeindpunt niet verwijderen.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Een eindpunt toevoegen via een programma
U kunt een eindpunt toevoegen aan uw Web-service programmatisch met behulp van de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) voorbeeldcode.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Toevoegen van een eindpunt met behulp van de portal van Azure Machine Learning-webservices
1. In Machine Learning Studio, klikt u op de linkernavigatiekolom op Web Services.
2. Klik aan de onderkant van het dashboard van de webservice op **-eindpunten beheren**. De Azure Machine Learning-webserviceportal geopend op de pagina eindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Namen van eindpunten 24 tekens of minder lang moeten zijn en moeten bestaan uit kleine letters of cijfers. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie, [logboekregistratie inschakelen voor Machine Learning-webservices](web-services-logging.md).

## <a name="next-steps"></a>Volgende stappen
[Het gebruik van een Azure Machine Learning-webservice](consume-web-services.md).

