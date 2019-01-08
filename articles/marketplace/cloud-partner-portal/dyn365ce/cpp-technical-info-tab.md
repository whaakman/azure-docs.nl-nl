---
title: Dynamics 365 voor Customer Engagement technische Info tabblad - Azure Marketplace | Microsoft Docs
description: Klik hier voor meer informatie over het opgeven van de technische informatie voor een Dynamics 365 voor Customer Engagement-toepassing op de AppSource-Marketplace.
services: Dynamics 365 for Customer Engagement Offer, Azure, Marketplace, Cloud Partner Portal, AppSource
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: pbutlerm
ms.openlocfilehash: 214abd64232130dd3fd5fdde510f7545732ac82e
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082772"
---
# <a name="dynamics-365-for-customer-engagement-technical-info-tab"></a>Dynamics 365 voor Customer Engagement technische Info-tabblad

De **technische Info** tabblad van de **nieuwe aanbieding** pagina kunt u gedetailleerde informatie over uw Dynamics 365 voor Customer Engagement-toepassing, met inbegrip van CRM-pakket en marketing-logo activa opgeven.  Op dit tabblad is onderverdeeld in vier secties: **Informace aplikace**, **CRM-pakket**, **CRM pakket beschikbaarheid**, en **Marketing artefacten**. Een toegevoegde sterretje (*) op de veldnaam van een geeft aan dat dit vereist is. 


## <a name="application-info-section"></a>Sectie van de gegevens van toepassing

U krijgt u meer details over uw Dynamics 365-toepassing in deze sectie.

![Info toepassingsgedeelte van het tabblad technische gegevens](./media/dynce-technical-info-tab1.png)

De volgende tabel beschrijft deze velden.

|      Veld                    |    Description                  |
|    ---------                  |  ---------------                |
|   Basis license-model          |  Licentiemodel voor bepaalt hoe klanten uw toepassing in het beheercentrum voor Dynamics 365 worden toegewezen. **Resource** -licentieverlening is gebaseerd op het exemplaar, terwijl **gebruiker** licenties zijn toegewezen per tenant.  |
|  Uitgaande S2S & CRM Secure Store-toegang |  Schakelt de configuratie van CRM Secure Store of uitgaande toegang van Server-naar-Server (S2S). *Deze functie moet speciale aandacht van de Dynamics 365-Team tijdens de fase van de certificeringsinstantie.* Microsoft neemt contact met u extra stappen uitvoeren ter ondersteuning van deze functie.  |
| Abonneren op gebeurtenissen voor CRM-levenscyclus | Integratie met de levenscyclus van de Dynamics 365-gebeurtenissen, moet u voor een toegewezen service die is geregistreerd via een speciale overeenkomst met Microsoft. *Deze functie moet speciale aandacht van de Dynamics 365-Team tijdens de fase van de certificeringsinstantie.* U definieert voor aanvullende stappen om deze functionaliteit te ondersteunen.  |
| Url van de configuratie van toepassing | URL van de webpagina waarmee de gebruiker de toepassing configureren |
| Van toepassing zijnde Dynamics 365-producten  | Selecteer de Dynamics 365-producten die deze aanbieding is van toepassing op. Deze aanbieding worden weergegeven onder de geselecteerde producten in AppSource.  |
| Marketing, alleen wijzigen         | Deze optie op Ja geeft aan dat alleen marketing/beschrijvende wijzigingen zijn aangebracht aan de bestaande aanbieding instelt.  Dergelijke wijzigingen kunnen de aanbieding voor het overslaan van de certificeringsinstantie en de inrichting fasen.  |
|  |  |


## <a name="crm-package-section"></a>Sectie CRM-pakket

U krijgt u meer details over de AppSource-pakketbestand in deze sectie.  Deze informatie wordt gebruikt door de Dynamics 365-teams voor validatie en -certificering.

![CRM-pakket-sectie van het tabblad technische gegevens](./media/dynce-technical-info-tab2.png)

De volgende tabel beschrijft deze velden.

|      Veld                    |    Description                  |
|    ---------                  |  ---------------                |
|  De bestandsnaam van het pakket     |  De bestandsnaam van het pakket (.zip).  Deze naam *niet* openbare en intern wordt gebruikt door het team van Dynamics 365-certificering.  |
|  URL                          |  URL van een Azure Storage-account dat het bestand geüploade pakket bevat. Deze URL moet een alleen-lezen SAS-sleutel zodat ons team om op te halen uw pakket voor verificatie bevatten.  |
| Meer dan één crm-pakket     | Selecteer Ja alleen als u ondersteuning voor meerdere versies van crm met verschillende pakketten bieden.  Elke versie heeft een bijbehorende pakket bestand tht die u moet afzonderlijk maken.  |
| Scenario en gebruik case activa   | Hiermee kunt het uploaden van een functionele specificatiedocument voor uw toepassing, voor gebruik door het validatieteam van Dynamics 365.  De door u gewenste indeling voor deze specificatie is de [E2E Scenario gebruikerssjabloon](http://download.microsoft.com/download/5/1/8/51812AC9-BCD8-489F-937C-5D439C494EC1/E2E%20User%20Scenario%20Template.docx).  |
|  |  |


## <a name="crm-package-availability-section"></a>Sectie van de beschikbaarheid van CRM-pakket

In deze sectie, selecteer welke geografische regio's uw toepassing is beschikbaar voor klanten.  Implementeren in de volgende soevereine regio's *vereist speciale machtigingen en validatie* tijdens het certificeringsproces: [Duitsland](https://docs.microsoft.com/azure/germany/), [US Government-Cloud](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome), en TIP.


## <a name="marketing-artifacts-section"></a>Gedeelte van de artefacten Marketing

In deze sectie moet u een toepassing logo dat wordt gebruikt voor het pakket in de AppSource-Marketplace uploaden.  De installatiekopie van het logo moet de PNG-indeling en worden van de grootte van 255 x 115 pixels.


## <a name="next-steps"></a>Volgende stappen

Het is raadzaam dat u een demonstratie van uw toepassing voltooien van aanbieden de [Test Drive-tabblad](./cpp-testdrive-tab.md)
