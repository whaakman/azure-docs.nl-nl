---
title: Excel-invoegtoepassing voor Machine Learning-webservices | Microsoft Docs
description: Het gebruik van Azure Machine Learning-webservices rechtstreeks in Excel zonder een code te schrijven.
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: 4dbb1779bf36be74ee83aeed8e4940902915603a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-invoegtoepassing voor Azure Machine Learning-webservices
Excel kunt u gemakkelijk om aan te roepen webservices rechtstreeks zonder code te schrijven.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Stappen voor het gebruik van een bestaande webservice in de werkmap

1. Open de [voorbeeld-Excel-bestand](http://aka.ms/amlexcel-sample-2), die de Excel-invoegtoepassing en de gegevens over passagiers op de Titanic bevat.
2. De webservice kiezen door erop te klikken-' Titanic nagelaten manier (Excel-invoegtoepassing voorbeeld) [Score] ' in dit voorbeeld.
   
    ![-Webservice selecteren][01]
3. Hiermee gaat u naar de **Predict** sectie.  Deze werkmap bevat al de voorbeeldgegevens, maar voor een lege werkmap kunt u Selecteer een cel in Excel en klik op **voorbeeldgegevens**.
4. Selecteer de gegevens met kopteksten en klik op het pictogram van invoergegevens bereik.  Zorg ervoor dat het selectievakje 'Mijn gegevens heeft headers' is ingeschakeld.
5. Onder **uitvoer**, voer het celnummer waar u de uitvoer te zijn, bijvoorbeeld 'H1' hier.
6. Klik op **voorspellen**.
   
    ![Sectie voorspellen][02]

Implementeer een webservice of een bestaande webservice gebruiken. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: de Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md).

De API-sleutel voor uw webservice ophalen. Wanneer u uitvoeren met deze actie is afhankelijk van of u een klassieke Machine Learning-webservice van een nieuwe Machine-Learning-webservice gepubliceerd.

**Een klassieke webservice gebruiken** 

1. In Machine Learning Studio, klikt u op de **WEBSERVICES** sectie in het linkerdeelvenster en selecteer vervolgens de webservice.
   
    ![Studio, selecteer een webservice][04]
2. Kopieer de API-sleutel voor de webservice.
   
    ![Studio-API-sleutel][05]
3. Op de **DASHBOARD** voor de webservice en klik op de **aanvragen/reacties** koppeling.
4. Zoek naar de **aanvraag-URI** sectie.  Kopieer en sla de URL.

> [!NOTE]
> Het is nu mogelijk om aan te melden bij de [Azure Machine Learning-webservices](https://services.azureml.net) portal om op te halen van de API-sleutel voor een klassieke Machine Learning-webservice.
> 
> 

**Een nieuwe webservice gebruiken**

1. In de [Azure Machine Learning-webservices](https://services.azureml.net) en klik op **webservices**, selecteer vervolgens uw webservice. 
2. Klik op **verbruiken**.
3. Zoek naar de **Basic verbruik info** sectie. Kopieer en sla de **primaire sleutel** en de **aanvragen en antwoorden** URL.

## <a name="steps-to-add-a-new-web-service"></a>Stappen voor een nieuwe webservice toevoegen

1. Implementeer een webservice of een bestaande webservice gebruiken. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: de Azure Machine Learning-webservice implementeren](walkthrough-5-publish-web-service.md).
2. Klik op **verbruiken**.
3. Zoek naar de **Basic verbruik info** sectie. Kopieer en sla de **primaire sleutel** en de **aanvragen en antwoorden** URL.
4. In Excel, gaat u naar de **webservices** sectie (als u zich in de **Predict** sectie, klikt u op de pijl naar links naar de lijst met web-services).
   
    ![Ga naar de Web service selecteren][03]
5. Klik op **-webservice toevoegen**.
6. Plak de URL in de Excel-invoegtoepassing tekstvak met het label **URL**.
7. Plak de API/primaire sleutel in het tekstvak met het label **API-sleutel**.
8. Klik op **Add**.
   
    ![URL en API-sleutel voor een klassieke webservice.][06]
9. Volg voor het gebruik van de webservice, de voorgaande instructies, 'Stappen voor het gebruik van een bestaande web Service'.

## <a name="sharing-your-workbook"></a>Delen van uw werkmap
Als u de werkmap opslaan, klikt u vervolgens de API/primaire sleutel voor de webservices die u hebt toegevoegd ook opgeslagen. Dit betekent dat u moet de werkmap alleen delen met personen die u vertrouwt.

Vragen te stellen in de volgende sectie Opmerking of op onze [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/excel-add-in-for-web-services/image1.png
[02]: ./media/excel-add-in-for-web-services/image2.png
[03]: ./media/excel-add-in-for-web-services/image3.png
[04]: ./media/excel-add-in-for-web-services/image4.png
[05]: ./media/excel-add-in-for-web-services/image5.png
[06]: ./media/excel-add-in-for-web-services/image6.png
