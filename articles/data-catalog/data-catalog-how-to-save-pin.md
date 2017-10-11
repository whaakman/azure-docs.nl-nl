---
title: Zoekopdrachten opslaan en een pincode gegevensassets in Azure Data Catalog | Microsoft Docs
description: Hoe kan ik artikel mogelijkheden in Azure Data Catalog is gemarkeerd voor het opslaan van gegevensbronnen en gegevensassets voor later gebruik.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 8c319d0dcbe8b95af11b8be2368a9348b260446c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Zoekopdrachten en pincode gegevensassets opslaan in Azure Data Catalog
## <a name="introduction"></a>Inleiding
Azure Data Catalog bevat mogelijkheden voor detectie van gegevensbronnen. U kunt snel zoeken en filteren van de catalogus gegevensbronnen vinden en begrijpen van het beoogde gebruik, waardoor het gemakkelijker de juiste gegevens vinden voor de taak op dat moment.

Maar wat gebeurt er als u wilt regelmatig werken met dezelfde gegevens? En wat gebeurt er als u en andere gebruikers regelmatig bij te dragen uw kennis met de dezelfde gegevensbronnen in de catalogus? In deze situaties kan herhaaldelijk uitgeven de zoekopdrachten die dezelfde zijn inefficiënt. Dit is waar opgeslagen zoekopdracht en vastgemaakt gegevensassets kunnen helpen.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
Een opgeslagen zoekopdracht in Data Catalog is een definitie van een herbruikbare, per gebruiker zoeken. U kunt een zoekopdracht, zoals zoektermen, labels en andere filters definiëren en vervolgens opslaan. U kunt de definitie van de opgeslagen zoekopdracht later om te retourneren van alle gegevensassets die overeenkomen met de zoekcriteria opnieuw uitvoeren.

### <a name="create-a-saved-search"></a>Maken van een opgeslagen zoekopdracht
Ga als volgt te werk voor het maken van een opgeslagen zoekopdracht:
1. In de Azure Data Catalog-portal in de **huidige zoekopdracht** venster, klikt u op **opslaan**. 

    ![Koppeling van huidige zoekopdracht instellingen opslaan](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Voer de zoekcriteria die u wilt gebruiken en klik vervolgens op **opslaan**.

    ![Huidige zoekinstellingen opgeslagen zoeknaam](./media/data-catalog-how-to-save-pin/02-name.png)

3. Wanneer u wordt gevraagd, typt u een naam voor de opgeslagen zoekopdracht. Kies een naam die zinnig is en dat de gegevensassets die worden geretourneerd door de zoekopdracht beschrijft.

### <a name="manage-saved-searches"></a>Opgeslagen zoekopdrachten beheren
Nadat u hebt opgeslagen zoekopdrachten voor een of meer, een **opgeslagen zoekacties** optie wordt weergegeven onder de **huidige zoekopdracht** vak. Wanneer de lijst wordt uitgevouwen, worden alle opgeslagen zoekopdrachten worden weergegeven.

 ![Lijst met opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/03-list.png)

Het volgende doen:

* Selecteer een opgeslagen zoekopdracht in de lijst voor het uitvoeren van een zoekopdracht.

* Een lijst van beheeropties voor een opgeslagen zoekopdracht wilt weergeven, klikt u op de pijl-omlaag naast de naam van de zoekopdracht.

    ![Opties voor het beheer opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/04-managing.png)

* Als u wilt een nieuwe naam voor de opgeslagen zoekopdracht invoeren, selecteert u **naam**. De definitie van de zoekopdracht is niet gewijzigd.

* Als de opgeslagen zoekopdracht uit de lijst, selecteert u **verwijderen**, en vervolgens de verwijdering te bevestigen.

* Selecteer de opgeslagen zoekopdracht als uw zoekopdracht standaard markeert, **opslaan als standaard**. Als u een 'empty' zoekopdracht vanaf de startpagina van Azure Data Catalog uitvoert, wordt uw standaard-zoekopdracht wordt uitgevoerd. Bovendien de zoekopdracht die gemarkeerd als de standaardzoekopdracht wordt weergegeven boven aan de **opgeslagen zoekacties** lijst.

### <a name="organizational-saved-searches"></a>Organisatie opgeslagen zoekopdrachten
Alle gebruikers in uw organisatie kunt zoekopdrachten voor eigen gebruik opslaan. Data Catalog beheerders kunnen ook gezocht naar alle gebruikers binnen de organisatie opslaan. Wanneer beheerders een zoekopdracht opslaat, ze worden weergegeven met een **Share binnen het bedrijf** optie. Als u deze optie selecteert, deelt de opgeslagen zoekopdracht voor alle gebruikers in de organisatie.

 ![Organisatie opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Vastgemaakte gegevensassets
U kunt met opgeslagen zoekopdrachten opslaan en opnieuw zoeken definities gebruiken. De gegevensassets die worden geretourneerd door de zoekopdrachten die kunnen worden gewijzigd na verloop van tijd als de inhoud van de wijziging van de catalogus. Wanneer u gegevensassets vastmaken, kunt u specifieke gegevensassets te vereenvoudigen voor toegang tot een zoeken gebruiken zonder expliciet identificeren.

Een gegevensasset vastmaken is eenvoudig. De gegevensasset toevoegen aan uw vastgemaakte lijst, klikt u op de **pincode** pictogram. Het pictogram wordt weergegeven in de rechterbenedenhoek van de asset-tegel in de weergave tile en in de linkerkolom in de lijstweergave in de portal voor Azure Data Catalog.

![Het Punaisepictogram gegevens asset](./media/data-catalog-how-to-save-pin/05-pinning.png)

Een gegevensasset losmaken is even eenvoudig. Klik op de **losmaken** pictogram in de instelling voor de geselecteerde asset-of uitschakelen.

![De gegevensasset losmaken pictogram](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>De sectie Mijn activa
De portal startpagina van Data Catalog bevat een **Mijn activa** sectie geeft de activa van belang bij de huidige gebruiker. Deze sectie bevat zowel vastgemaakt activa en opgeslagen zoekopdrachten.

![De sectie Mijn activa op de startpagina](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Samenvatting
Azure Data Catalog bevat mogelijkheden waarmee het eenvoudiger is voor het detecteren van de gegevensbronnen die u nodig hebt, zodat u en andere leden van de organisatie kunnen minder tijd nodig voor zoekt gegevens en meer tijd ermee te werken. Opgeslagen zoekopdrachten en vastgemaakt gegevens activa op deze belangrijkste mogelijkheden, bouwen zodat gebruikers de gegevensbronnen die ze met herhaaldelijk werken gemakkelijk kunnen herkennen.
