---
title: Zoekopdrachten en pincode gegevensassets opslaat in Azure Data Catalog
description: Hoe kan ik artikel markeren mogelijkheden in Azure Data Catalog voor het opslaan van gegevensbronnen en gegevensassets voor later gebruik.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: f22dd0911a8e86e6a9e370c7e08add935c31e936
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409929"
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Zoekopdrachten en pincode gegevensassets opslaat in Azure Data Catalog
## <a name="introduction"></a>Inleiding
Azure Data Catalog biedt mogelijkheden voor detectie van gegevensbronnen. U kunt snel zoeken en filteren van de catalogus naar gegevensbronnen en informatie over het beoogde gebruik, waardoor het gemakkelijker te vinden van de juiste gegevens voor de taak bij de hand.

Maar wat gebeurt er als u wilt regelmatig werken met dezelfde gegevens? En wat gebeurt er als u en andere gebruikers regelmatig bij te dragen uw kennis met de dezelfde gegevensbronnen in de catalogus? In deze situaties kan om uit te geven meerdere keren dezelfde zoekopdrachten zijn inefficiënt. Dit is waar de opgeslagen zoekopdracht en vastgemaakte gegevensassets kunnen helpen.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
Een opgeslagen zoekopdracht in Data Catalog is een definitie van een herbruikbare, per gebruiker zoeken. U kunt een zoekopdracht, met inbegrip van zoektermen, labels en andere filters definiëren en vervolgens op te slaan. U kunt de definitie van de opgeslagen zoekopdracht later om te retourneren van alle gegevensassets die overeenkomen met de zoekcriteria opnieuw uitvoeren.

### <a name="create-a-saved-search"></a>Een opgeslagen zoekactie maken
Voor het maken van een opgeslagen zoekopdracht, het volgende doen:
1. In de Azure Data Catalog-portal in de **huidige zoekopdracht** venster, klikt u op **opslaan**. 

    ![Koppeling van huidige zoekopdracht instellingen opslaan](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Voer de zoekcriteria die u wilt gebruiken, en klik vervolgens op **opslaan**.

    ![Huidige instellingen voor het zoeken die op de naam van opgeslagen zoekopdracht](./media/data-catalog-how-to-save-pin/02-name.png)

3. Wanneer u wordt gevraagd, voert u een naam op voor de opgeslagen zoekopdracht. Kies een naam die zinvol is en met de beschrijving van de gegevensassets die door de zoekopdracht worden geretourneerd.

### <a name="manage-saved-searches"></a>Opgeslagen zoekopdrachten beheren
Nadat u hebt opgeslagen zoekopdrachten voor een of meer, een **opgeslagen zoekopdrachten** optie wordt weergegeven onder de **huidige zoekopdracht** vak. Wanneer de lijst wordt uitgevouwen, worden alle opgeslagen zoekopdrachten worden weergegeven.

 ![Lijst met opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/03-list.png)

Het volgende doen:

* Voor het uitvoeren van een zoekopdracht, selecteert u een opgeslagen zoekopdracht in de lijst.

* Een lijst van opties voor een opgeslagen zoekopdracht wilt weergeven, klikt u op de pijl-omlaag naast de naam van de zoekopdracht.

    ![Opties voor het beheer opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/04-managing.png)

* Als u wilt een nieuwe naam voor de opgeslagen zoekopdracht invoeren, selecteert u **naam**. De definitie van de zoekopdracht is niet gewijzigd.

* Als u wilt de opgeslagen zoekopdracht verwijderen uit de lijst, selecteer **verwijderen**, en Bevestig de verwijdering.

* Als u wilt markeren de opgeslagen zoekopdracht als uw Standaardzoekopdracht, selecteer **opslaan als standaard**. Als u een 'lege' zoekopdracht vanaf de startpagina van Azure Data Catalog uitvoert, wordt uw standaardzoekopdracht wordt uitgevoerd. Bovendien de zoekopdracht die gemarkeerd als de standaardzoekopdracht wordt weergegeven aan de bovenkant van de **opgeslagen zoekopdrachten** lijst.

### <a name="organizational-saved-searches"></a>Organisatie-opgeslagen zoekopdrachten
Alle gebruikers in uw organisatie kunt zoekopdrachten voor eigen gebruik opslaan. Data Catalog-beheerders kunnen ook zoekopdrachten voor alle gebruikers binnen de organisatie opslaan. Wanneer beheerders een zoekopdracht wilt opslaan, ze worden weergegeven met een **delen binnen het bedrijf** optie. Als u deze optie selecteert, deelt de opgeslagen zoekopdracht voor alle gebruikers in de organisatie.

 ![Organisatie-opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Vastgemaakte gegevensassets
U kunt met opgeslagen zoekopdrachten opslaan en opnieuw gebruiken van definities van de zoekopdracht. De gegevensassets die worden geretourneerd door de zoekopdrachten kunnen worden gewijzigd na verloop van tijd als de inhoud van de wijziging van de catalogus. Wanneer u gegevensassets vastzetten, kunt u specifieke gegevensassets zodat ze beter toegankelijk zonder gebruik van een zoekopdracht kunt expliciet aangeven.

Een gegevensasset vast te maken is eenvoudig. Als u wilt de gegevensasset toevoegen aan uw vastgemaakte lijst, klikt u op de **pincode** pictogram. Het pictogram wordt weergegeven in de linkerbovenhoek van de asset-tegel in de tegelweergave en in de meest linkse kolom in de lijstweergave in de Azure Data Catalog-portal.

![Het pictogram van de gegevensasset vastmaken](./media/data-catalog-how-to-save-pin/05-pinning.png)

Een gegevensasset losmaakt is even eenvoudig. Klik op de **losmaken** pictogram om uit te schakelen van de instelling voor de geselecteerde asset.

![De gegevensasset losmaken pictogram](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>De sectie Mijn Assets
De startpagina voor Data Catalog-portal bevat een **mijn Assets** sectie waarin activa van belang zijn worden weergegeven met de huidige gebruiker. In deze sectie bevat beide vastgemaakte assets en opgeslagen zoekopdrachten.

![De sectie Mijn Assets op de startpagina](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Samenvatting
Azure Data Catalog biedt mogelijkheden die het eenvoudiger is voor het detecteren van de gegevensbronnen die u nodig hebt, zodat u en andere leden van de organisatie kunnen besteden aan minder tijd zoeken naar gegevens en meer tijd aan het werk. Opgeslagen zoekopdrachten en vastgemaakt gegevens activa van deze core-mogelijkheden gebruikmaken, zodat gebruikers de gegevensbronnen die ze met herhaaldelijk werken eenvoudig kunnen herkennen.
