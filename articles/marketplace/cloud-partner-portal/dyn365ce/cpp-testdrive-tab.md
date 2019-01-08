---
title: Dynamics 365 voor Customer Engagement toepassing Test Drive-tabblad - bieden Azure Marketplace | Microsoft Docs
description: Over het configureren van test drive voor een Dynamics 365 voor Customer Engagement toepassing aanbieding op de AppSource-Marketplace.
services: Azure, Marketplace, AppSource, Cloud Partner Portal, Dynamics 365 for Customer Engagement
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
ms.date: 12/25/2018
ms.author: pbutlerm
ms.openlocfilehash: 664a2c6bfc4a73b7d792b71c4b81df54b05fcd74
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082817"
---
# <a name="dynamics-365-for-customer-engagement-application-test-drive-tab"></a>Dynamics 365 voor Customer Engagement toepassing Test Drive-tabblad

Gebruik de **Test Drive** tabblad om te maken van een evaluatieversie voor uw klanten.  Daardoor kunnen klanten met een praktische, zelfgestuurd evaluatieversie van de belangrijke functies en voordelen, gedemonstreerd in een implementatiescenario voor het werkelijke van uw aanbieding.  Test Drive is van de proefversie opties die beschikbaar is, de meest effectief bij het genereren van hoge kwaliteit potentiële klanten en hogere conversie van deze leads.  Zie voor meer informatie, [wat is Test Drive?](../../cloud-partner-portal-orig/what-is-test-drive.md)

De Test Drive-ervaring voor Dynamics 365-toepassingen wordt automatisch uitgevoerd als een Microsoft gehoste oplossing.  Zie voor meer informatie, [gehoste Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/hosted-test-drive).

Het tabblad Test Drive heeft drie mogelijke secties: **U kunt uitproberen**, **Details**, en **technische configuratie**.  De laatste twee secties worden alleen weergegeven nadat u de functionaliteit van de Test Drive inschakelen.  Een sterretje (*) toegevoegd aan de naam van het veld geeft aan dat dit is verplicht. 


## <a name="test-drive-section"></a>Test Drive-sectie

Als u deze functionaliteit, schakelt **Ja** naar **inschakelen van een Test Drive**.


## <a name="details-section"></a>Sectie voor meer informatie

Biedt u eenvoudige Test Drive-informatie in de **Details** sectie.   

![De detailsectie van Test Drive](./media/test-drive-tab-details.png)

De volgende tabel beschrijft de velden die vereist zijn voor het instellen van test drive voor uw Dynamics 365-toepassing.

|      Veld                    |    Description                  |
|    ---------                  |  ---------------                |
|      Description              |   Wat kan worden uitgevoerd op uw Test Drive wordt beschreven. U kunt eenvoudige HTML-codes gebruiken om deze beschrijving. Bijvoorbeeld, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;, en koppen.  |
|  Gebruikershandleiding                  |   Een gebruiker handmatig die uw klanten gebruiken kunnen om te zien hoe Test Drive uploaden. Dit document moet een PDF-bestand.              |
|  Test Drive demovideo (optioneel) |  U kunt een video-overzicht van uw Test Drive opgeven. Een klant kan deze video kunt bekijken voordat ze een test uitvoeren. Geef een URL naar de video op YouTube of Vimeo. Als u selecteert **+ Video toevoegen**, wordt u gevraagd om de volgende informatie te geven:<ul><li>Name</li><li>URL</li><li>Miniatuur (in PNG-indeling hebben, 533 x 324 pixels)</li></ul>  |
|   |   |


## <a name="technical-configuration-section"></a>Sectie met technische configuratie

in deze sectie wordt u vindt u technische gegevens over uw test drive.

![De detailsectie van Test Drive](./media/test-drive-tab-tech-config.png)

Waarin de velden hebben de volgende doeleinden:

|      Veld                    |    Description                  |
|    ---------                  |  ---------------                |
| Type Test Drive            | Kies **Microsoft die worden gehost (Dynamics 365 voor Customer Engagement)**.  |
| Maximale aantal gelijktijdige Test Drives    | Het aantal gelijktijdige exemplaren van een actieve Test Drive op een willekeurig moment tijd wordt opgelost. Elke gebruiker wordt een Dynamics-licentie gebruiken tijdens de Test Drive actief, is daarom u moet om ervoor te zorgen dat u ten minste dit aantal Dynamics licenties beschikbaar voor gebruikers van Test Drive. Aanbevolen waarde van 3 tot 5.  |
| Test Drive duur (uren)   | Maximum aantal uren van de gebruiker Test Drive-exemplaar zijn. Na deze periode wordt overschreden, wordt het exemplaar van uw tenant inrichting ongedaan gemaakt. Aanbevolen waarde van 2 tot 24 uur, afhankelijk van de complexiteit van uw app. De gebruiker kan een andere Test Drive altijd aanvragen als ze weinig tijd en u wilt opnieuw te evalueren.  |
| URL van instantie                  | URL die de Test Drive in eerste instantie u naar gaat. Dit is meestal de URL van uw Dynamics 365-exemplaar waarop uw app en voorbeeldgegevens op geïnstalleerd.  |
| Azure AD-Tenant-ID            | De GUID van de Azure-tenant voor uw exemplaar van Dynamics 365. Voor het ophalen van deze waarde, meld u aan bij Azure portal en gaat u naar **Azure Active Directory** > **eigenschappen selecteren** > **kopiëren van de map-ID**.  |
| Azure AD-App-ID               | GUID van uw Azure AD-toepassing  |
| Azure AD-App-sleutel              | Geheim van uw Azure AD-toepassing, bijvoorbeeld: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` |
| De naam van de Azure AD-Tenant          | De naam van de Azure-tenant voor uw exemplaar van Dynamics 365. Gebruik de indeling van < tenantname. > onmicrosoft.com, bijvoorbeeld: `testdrive.onmicrosoft.com`  |
| Web-API-URL van instantie          | De URL van de web-API voor uw exemplaar van Dynamics 365. U kunt deze waarde wordt opgehaald door te melden bij uw Microsoft Dynamics 365-exemplaar en te navigeren naar **instellingen** > **aanpassing** > **Developer Resources** > **Web-API (deze URL kopiëren)-exemplaar**. Voorbeeldwaarde: `https://testdrive.crm.dynamics.com/api/data/v9.0`  |
| Rolnaam                     | Naam van de aangepaste beveiligingsrol van Dynamics 365 u voor uw Test Drive hebt gemaakt en wordt toegewezen aan de gebruikers wanneer ze het uitvoert, bijvoorbeeld `testdriveuser`. |
|  |  |

Nadat u de vereiste gegevens hebt opgegeven, selecteert u **opslaan**.


## <a name="next-steps"></a>Volgende stappen

Vervolgens krijgt u marketing- en informatie in de [tabblad met Details van de winkel](./cpp-storefront-details-tab.md).

