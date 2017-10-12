---
title: Pagina-inhoud bewerken in de ontwikkelaarsportal in Azure API Management | Microsoft Docs
description: Informatie over het bewerken van pagina-inhoud in de ontwikkelaarsportal in Azure API Management.
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: 708c803c36c182ed90e04731b12d4ade00ae7ffb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>De inhoud en opmaak van pagina’s bewerken in de ontwikkelaarsportal in Azure API Management
Er zijn die manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:

* [De inhoud van statische pagina's en pagina-indelingselementen bewerken][modify-content-layout] (uitgelegd in deze handleiding)
* [De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal][customize-styles]
* [De sjablonen bewerken die worden gebruikt voor pagina's die worden gegenereerd door de portal][portal-templates] (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enz.)

## <a name="page-structure"> </a>Structuur van pagina's in de ontwikkelaarsportal

De ontwikkelaarsportal is gebaseerd op een inhoudsbeheersysteem. De indeling van elke pagina is opgebouwd op basis van een reeks kleine pagina-elementen die ook wel widgets worden genoemd:

![Paginastructuur in de ontwikkelaarsportal][api-management-customization-widget-structure]

Alle widgets kunnen worden bewerkt. 
* De belangrijkste inhoud die voor elke pagina anders is, staat in de widget Inhoud. Als u een pagina bewerkt, bewerkt u eigenlijk de inhoud van deze widget.
* Alle elementen van pagina-indeling staan in de overige widgets. Als u wijzigingen aanbrengt aan deze widgets, worden die toegepast op alle pagina's. Deze widgets heten ook wel indelingswidgets.

Voor de alledaagse paginabewerkingen bewerkt u meestal alleen de widget Inhoud. Er staat in deze widget meestal andere inhoud voor elke pagina.

## <a name="modify-layout-widget"> </a>De inhoud van een indelingswidget wijzigen

Inhoud in de ontwikkelaarsportal wordt gewijzigd via de publicatieportal, die u kunt openen vanuit de Azure Portal. U opent deze door vanaf de servicewerkbalk van het API Management-exemplaar op **Publicatieportal** te klikken.

![Publicatieportal][api-management-management-console]

Als u de inhoud van deze widget wilt bewerken, klikt u op **Widgets** in het menu **Ontwikkelaarsportal** aan de linkerkant. In dit voorbeeld gaat u de inhoud van de widget Koptekst wijzigen. Selecteer de widget **Koptekst** in de lijst.

![Widget Koptekst][api-management-widgets-header]

De inhoud van de header kan worden bewerkt vanuit het veld **Hoofdtekst**. Wijzig de tekst naar wens en klik vervolgens op **Opslaan** onder aan de pagina.

U zou nu de nieuwe header op elke pagina moeten zien binnen de ontwikkelaarsportal.

> Als u de ontwikkelaarsportal wilt openen terwijl u zich in de publicatieportal bevindt, klikt u op **Ontwikkelaarsportal** in de bovenste balk.
> 
> 

## <a name="edit-page-contents"> </a>De inhoud van een pagina bewerken

Als u de lijst met alle bestaande inhoudspagina's wilt zien, klikt u op **Inhoud** in het menu **Ontwikkelaarsportal** in de publicatieportal.

![Inhoud beheren][api-management-customization-manage-content]

Klik op de pagina **Welkom** om te bewerken wat wordt weergegeven op de startpagina van de ontwikkelaarsportal. Breng de gewenste wijzigingen aan, bekijk er indien nodig een voorbeeld van en klik vervolgens op **Nu publiceren** om ze voor iedereen zichtbaar te maken.

> Op de startpagina wordt een speciale indeling gebruikt waardoor bovenaan een banner kan worden weergegeven. Deze banner kan niet worden bewerkt in de sectie **Inhoud**. Als u deze banner wilt bewerken, klikt u op **Widgets** in het menu **Ontwikkelaarsportal**, selecteert u **Startpagina** in de vervolgkeuzelijst **Huidige laag** en opent u het item **Banner** onder de sectie **Aanbevolen**. De inhoud van deze widget kan net zoals andere pagina's worden bewerkt.
> 
> 

## <a name="next-steps"> </a>Volgende stappen
* [De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal][customize-styles]
* [De sjablonen bewerken die worden gebruikt voor pagina's die worden gegenereerd door de portal][portal-templates] (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enz.)

[Structure of developer portal pages]: #page-structure
[Modifying the contents of a layout widget]: #modify-layout-widget
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
