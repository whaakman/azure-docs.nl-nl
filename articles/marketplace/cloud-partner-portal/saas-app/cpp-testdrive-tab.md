---
title: Azure SaaS-aanbieding test drive Toepassingsconfiguratie | Microsoft Docs
description: Configureer uitproberen voor de aanbieding van de SaaS-toepassing op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 0fa3638ef47fb8cb1d0237a2ab9530ab76d316dc
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57449265"
---
# <a name="saas-application-test-drive-tab"></a>SaaS-toepassing Test Drive-tabblad

Gebruik het tabblad Test Drive voor een proefversie van uw klanten.

## <a name="test-drive-benefits"></a>Voordelen van Test Drive

Het maken van een evaluatieversie voor uw klanten is een aanbevolen procedure om te controleren of dat ze met vertrouwen kunnen kopen. Test Drive is van de proefversie opties die beschikbaar is, de meest effectief bij het genereren van hoge kwaliteit potentiële klanten en hogere conversie van deze leads.

Test drive biedt klanten een praktische Zelfgestuurde evaluatieversie van de belangrijke functies en voordelen, gedemonstreerd in een implementatiescenario voor het werkelijke van uw product.

## <a name="how-a-test-drive-works"></a>De werking van een test uit

Een potentiële klant wordt gezocht en detecteert de toepassing op de Marketplace. De klant zich heeft aangemeld en gaat akkoord met de gebruiksvoorwaarden. Op dit moment ontvangt de klant uw vooraf geconfigureerde omgeving om te proberen voor een vast aantal uren, terwijl u een zeer gekwalificeerde lead te ontvangen. Zie voor meer informatie, [wat is Test Drive?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="publishing-steps"></a>Publiceren stappen

De belangrijkste publishing stappen voor het toevoegen van een test Drive zijn:

1. Definieer uw Test Drive-scenario
2. Bouw en/of wijzigen van de Resource Manager-sjabloon
3. Stapsgewijze handleiding van uw Test Drive maken
4. Uw aanbod opnieuw publiceren

## <a name="setting-up-a-test-drive"></a>Instellen van een test uit

Er zijn vier verschillende typen Test Drives beschikbaar, elk op basis van het type van het product, scenario en u bent op marketplace.
|  **Type**          |  **Beschrijving**  |  **Installatie-instructies**  |
|  ---------------   |  ---------------  |  ---------------  |
|     Azure Resource Manager               |    Een Azure Resource Manager Test Drive is een sjabloon voor de implementatie met alle Azure-resources die deel uitmaken van een oplossing wordt gemaakt door de uitgever. Producten die geschikt zijn voor dit type Test Drive zijn die alleen Azure-bronnen.               |       [Test Drive voor Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)            |
|       Gehost             |       Een gehost Test Drive wordt de complexiteit van de installatie door Microsoft hosting en onderhouden van de service die de gebruiker Test Drive-inrichting en ongedaan maken van inrichting uitvoert.             |         [Gehoste Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/hosted-test-drive)          |
|      Logische apps              |       Een logische App Test Drive is een sjabloon voor de implementatie die is bedoeld om alle complexe architecturen omvatten. Alle Dynamics-toepassingen of aangepaste producten moeten gebruiken voor dit type Test Drive.            |      [Logische App Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)             |
|       Power BI             |         Een Power BI Test Drive bestaat uit een ingesloten koppeling naar een op maat gemaakte dashboard. Alle producten die wil ter illustratie van dat een interactieve Power BI-visualisatie dit type Test Drive moet gebruiken. U moet uploaden, is de URL van uw ingesloten Power BI.          |        [Power BI-Test Drive](#Power-BI-test-drive)           |

### <a name="power-bi-test-drive"></a>Power BI uitproberen

Gebruik de volgende stappen uit om te configureren van een test uit.

1. Selecteer onder de nieuwe aanbieding **Test Drive**.
2. Selecteer op de Test Drive **Ja**.

   ![Test station inschakelen](./media/saas-enable-test-drive.png)

   Wanneer u een test uit inschakelt, ziet u de Details en de configuratie van technische formulieren, worden weergegeven in de volgende schermopname.

   ![Formulier voor configuratie van test drive](./media/saas-test-drive-yes.png)

3. Onder **Details**, informatie te verstrekken voor de volgende velden:
  
   - Beschrijving – uw test drive en wat gebruikers kunnen doen met deze beschrijven. U kunt eenvoudige HTML-codes gebruiken om deze beschrijving.
   - Gebruiker handmatig: Upload een document handmatig van de gebruiker die uw klanten gebruiken kunnen wanneer ze de proefrit geleid. Deze handleiding moet een pdf-bestand.
   - Test Drive Demo Video (optioneel): U kunt een video (YouTube of Vimeo) opgeven voor uw klanten om te bekijken voordat ze de Test uitvoeren. Geef een URL naar de video.

4. Onder **technische configuratie**, informatie te verstrekken voor de volgende velden:

   - Type Test Drive: Selecteer **Power BI** in de vervolgkeuzelijst.
   - Koppelen aan Power BI-Dashboard: een koppeling naar het dashboard.

5. Wanneer u klaar bent met het configureren van test drive, selecteert u **opslaan**.


## <a name="next-steps"></a>Volgende stappen

[Tabblad Details van webwinkel](./cpp-storefront-tab.md)