---
title: Tabblad van de virtuele machine Marketplace in de Cloud Partner-Portal voor Azure | Microsoft Docs
description: Hierin wordt beschreven in de Marketplace-tabblad gebruikt bij het maken van een virtuele machine in Azure Marketplace-aanbieding.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: aa43a6f1f7c757366e321e7da0fb1e47d4928cee
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639683"
---
# <a name="virtual-machine-marketplace-tab"></a>Virtuele machine Marketplace tabblad

De **Marketplace** tabblad van de **nieuwe aanbieding** pagina kunt u uw potentiële klanten voorzien van marketing, verkoop en juridische informatie en -overeenkomsten en beheer van leads die zijn gegenereerd op basis van de Marketplace. Dit lang formulier is onderverdeeld in vier secties: **overzicht**, **Marketing artefacten**, **leiden Management**, en **juridische**. 

## <a name="overview-section"></a>Overzichtssectie
In deze sectie voert u de algemene informatie over uw Azure Marketplace bieden.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is.

![De sectie overzicht van het tabblad Marketplace in het formulier nieuwe aanbieding voor virtuele machines](./media/publishvm_008.png)

De volgende tabel beschrijft het doel en de inhoud van deze velden.

|  **Veld**                |     **Beschrijving**                                                          |
|  ---------                |     ---------------                                                          |
| **Titel**                 | De titel van de aanbieding, vaak de lang, formele naam. Deze titel wordt prominent weergegeven in de marketplace.  Maximale lengte van maximaal 50 tekens. |
| **Samenvatting**               | Korte doel of de functie van de oplossing.  Maximale lengte van 100 tekens. |
| **Lange samenvatting**          | Doel of de functie van de oplossing.  Mag maximaal 256 tekens bevatten. |
| **Beschrijving**           | Beschrijving van de oplossing.  Maximale lengte van 3000 tekens ondersteunt eenvoudige HTML-opmaak. |
| **Marketing-id**  | Een unieke URL om te koppelen aan deze aanbieding omvat gewoonlijk van uw organisatie en de naam van de oplossing, maximaal 50 tekens.  Bijvoorbeeld: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  |
| **Preview-versie abonnement-id 's** | Toevoegen van 1 tot 100 abonnements-id's van viewers. Deze technische vermeld abonnementen hebben toegang tot de aanbieding zodra deze gepubliceerd, voordat u deze meteen live. |
| **Nuttige koppelingen**          | Meervoudige selectie van zakelijke en technische categorieën die worden geboden door mag beste is gekoppeld.  Maximaal vijf toegestaan.  |
|  |  |


## <a name="marketing-artifacts-section"></a>Gedeelte van de artefacten Marketing

Deze tweede sectie is onderverdeeld in drie subsecties: **logo's**, **schermafbeelding**, en **video's**. Logo's zijn de enige vereiste marketing-artefacten, maar alle sterk voor de beste beroep van de klant aanbevolen wordt.

![Marketing artefacten gedeelte van de Marketplace-tabblad in het formulier nieuwe aanbieding voor virtuele machines](./media/publishvm_009.png)

|  **Veld**                |     **Beschrijving**                                                          |
|  ---------                |     ---------------                                                          |
| *Logo 's*  |  |
| **Kleine**                 | 40, 40 pixel .ico bitmap                                                      |
| **Gemiddeld**                | 90 x 90 pixel .ico bitmap                                                      |
| **Grote**                 | 115 x 115 pixel .ico bitmap                                                   |
| **Breed**                  | 255 x 115 pixel .ico bitmap                                                    |
| **Hero**                  | 815 x 290 bitmap.  Optioneel, maar eenmaal geüploade de hero-pictogram kan niet worden verwijderd. |
| *Schermafbeeldingen*  | Optioneel, maar maximaal vijf schermafbeeldingen per SKU. |
| **Naam**                  | Naam of titel <!-- TODO - max char length? none specified in UI -->                               |
| **Installatiekopie**                 | Afbeelding van schermopname, 533 x 324 pixel                                         |
| *Video's*  |  |
| **Naam**                  | Naam of titel  <!-- TODO - max char length? -->                              |
| **Link**                  | Video-URL, die worden gehost op YouTube of Vimeo                                        |
| **Miniatuur**             | 533 x 324 bitmap                                                               |
|  |  |


### <a name="logo-guidelines"></a>Logorichtlijnen

<!-- TD: It seems like this section could be better located in some common area, maybe a AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Alle de logo's uploaden naar de Cloud Partner-Portal moeten de richtlijnen te volgen:

*  Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.
*  De themakleuren van het Azure-portal zijn wit en zwart. Vermijd daarom het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Sommige kleur die uw logo's opvallende in Azure portal gebruiken. We adviseren eenvoudige primaire kleuren. Als u transparante achtergrond, Controleer of de logo's / tekst zijn niet witte of zwarte of blauw.
*  Gebruik een kleurovergang achtergrond op uw logo.
*  Plaats tekst — zelfs uw bedrijf of de naam, op het logo. Het uiterlijk van uw logo moet een "platte" en vermijd kleurovergangen.
*  Het logo niet uitbreiden.

#### <a name="hero-logo"></a>Hero-logo

De Hero-logo is optioneel. maar als geüpload, kan de hero-pictogram kan niet worden verwijderd.  De Hero-logo-pictogram moet de richtlijnen te volgen:

*  Zwart, wit en transparante achtergrond zijn niet toegestaan voor de hero-pictogrammen.
*  Vermijd het gebruik van een lichte kleur als de achtergrond van het hero-pictogram.  De weergavenaam van de uitgever, plan titel en de lange samenvatting aanbieding worden weergegeven in wit tekstkleur en moeten onderscheiden op basis van de achtergrond.
*  Vermijd het gebruik van de meeste tekst bij het ontwerpen van de hero-logo.  Naam van de uitgever, de titel van het abonnement, de aanbieding voor lange samenvatting en een knop maken worden via een programma ingesloten in het hero-pictogram wanneer de aanbieding bevat. 
* Een niet-gebruikte rechthoek aan de rechterkant van het pictogram van uw hero van grootte 415 x 100 pixels zijn en 370 offset pixels vanaf de linkerkant.  

Als u bijvoorbeeld is het volgende pictogram voor de hero voor de Azure Container Service.  <!-- TD: It would be nice to have the raw bitmap, e.g.before and after embedding. -->

![Voorbeeld van de hero-pictogram voor Azure Container Service](./media/publishvm_010.png)


### <a name="marketing-information-example"></a>Voorbeeld van marketing 

De volgende afbeelding ziet u hoe marketing informatie wordt weergegeven op de pagina van de belangrijkste product Microsoft Windows Server.

![Voorbeeld van de product-pagina voor Microsoft Windows Server](./media/publishvm_011.png)


## <a name="lead-management-section"></a>De sectie over potentiële klanten beheren
<!-- this all should be referenced in a common location for lead management, not in this file. nothing unique for a vm specifically. -->

De derde sectie kunt u voor het verzamelen van klanten leads die zijn gegenereerd op basis van uw Azure Marketplace-aanbiedingen. Het biedt de volgende opties voor opslag (in een vervolgkeuzelijst) voor deze gegevens over leads.

* **Geen** -standaard, gegevens over leads worden niet verzameld.
* Azure Table - geschreven naar de Azure-tabel opgegeven door een verbindingsreeks.
* Dynamics CRM Online - geschreven naar de [Microsoft Dynamics 365 Online](https://dynamics.microsoft.com/) exemplaar, opgegeven door de referenties van een URL en verificatie.
* HTTPS-eindpunt - geschreven naar het opgegeven HTTPS-eindpunt als een JSON-nettolading.
* Marketo - geschreven naar het opgegeven [Marketo](https://www.marketo.com/) exemplaar, opgegeven door de server-ID, munchkin-ID en formulier-ID.
* SalesForce - geschreven naar een [Salesforce](https://www.salesforce.com/) database, door een object-id opgegeven.

Nadat u uw aanbieding is gepubliceerd, wordt de lead-verbinding is gevalideerd en wordt een test-lead wordt automatisch verzonden naar de geconfigureerde bestemming. Potentiële klanten informatie continu moet worden beheerd en deze instellingen onmiddellijk moeten worden bijgewerkt wanneer er wijzigingen zijn aangebracht aan de beheerarchitectuur voor uw klant.

<!-- TD: For more info, see [Need a topic on lead information and processing that mimics the Appendix of the VM Pub Guide]. -->

## <a name="legal-section"></a>Juridische sectie

Deze laatste sectie kunt u de twee juridische documenten die zijn vereist voor elke aanbieding: privacybeleid en de gebruiksvoorwaarden.

|  **Veld**                |     **Beschrijving**                                                          |
|  ---------                |     ---------------                                                          |
| **URL privacybeleid**    | URL naar uw geboekte privacybeleid                                            |
| **Gebruiksvoorwaarden**          | het beleid als tekst zonder opmaak of een eenvoudige HTML.  <!-- TODO - max char length? -->       |
|  |  |

<br/>

In de volgende [ondersteunen](./cpp-support-tab.md) tabblad, biedt u technische en gebruiker bronnen voor ondersteuning van uw aanbieding.

