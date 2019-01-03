---
title: Azure en Dynamics 365 advies service-aanbieding - definiëren aanbiedingsinstellingen | Microsoft Docs
description: Handleiding voor het definiëren van instellingen in een Azure- of Dynamics 365 advies serviceaanbieding vallen in de Cloud Partner-Portal bieden.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 9cb52aea56e9c1183af9ca76201ab65c62a58f22
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972168"
---
# <a name="offer-settings-tab"></a>Tabblad instellingen bieden

Op de **nieuwe aanbieding** scherm, de eerste stap is het maken van de identiteit van de aanbieding. De identiteit van de aanbieding bestaat uit drie onderdelen: **Aanbiedings-ID**, **uitgevers-ID**, en **naam**. Elk van deze onderdelen wordt in de volgende secties beschreven.

![Het maken van nieuwe advies service aanbieding - tabblad voor instellingen van aanbieding](media/consultingoffer-settings-tab.png)

*Aanbiedings-ID*

Deze id is een unieke naam die u maakt wanneer u de aanbieding voor het eerst verzendt. Deze moet bestaan alleen uit kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. De **aanbiedings-ID** is zichtbaar in de URL en gevolgen engine zoekresultaten. Bijvoorbeeld, *yourcompanyname_exampleservice*

Zoals wordt weergegeven in het voorbeeld wordt de **aanbiedings-ID** wordt toegevoegd aan de uitgevers-ID te maken van een unieke id. Dit wordt weergegeven als een permanente koppeling die kan worden gereserveerd en wordt geïndexeerd door de zoekmachines.

*Nadat een aanbieding gepubliceerd is, kan niet de id worden bijgewerkt*

*Uitgevers-ID*

Deze id is gerelateerd aan uw account. Wanneer u bent aangemeld met uw organisatieaccount, uw **Publisher** ID wordt weergegeven in de vervolgkeuzelijst.

*Naam*

Deze tekenreeks vormt wat wordt weergegeven als de aanbiedingsnaam van de op AppSource of Azure Marketplace. De *naam* veld is beperkt tot 50 tekens.  De revisor moet mogelijk uw titel om toe te staan voor het toevoegen van de aanbiedingsnaam van de met de duur bewerken en type aanbieding.

Het volgende voorbeeld door Edgewater Fullscope laat zien hoe de aanbiedingsnaam van de is samengesteld. De aanbiedingsnaam van de wordt weergegeven als:

![Het maken van een nieuwe service-aanbieding advies](media/cppsampleconsultingoffer.png)

De naam van de aanbieding bestaat uit vier onderdelen:

-   **Duur:** - gedefinieerd in de **Storefront Details** tabblad van de editor. Duur kan worden uitgedrukt in uren, dagen of weken.
-   **Type van de service:** - gedefinieerd in de **Storefront Details** tabblad van de editor. Typen van de services zijn `Assessment`, `Briefing`, `Implementation`, `Proof of concept`, en `Workshop`.
-   **Voorzetsel:** - ingevoegd door de revisor
-   **Naam:** - gedefinieerd in de **instellingen bieden** pagina.

>[!Note]
>Het naamveld is beperkt tot 50 tekens. De naam die u verzendt, moet mogelijk worden bewerkt door de revisor toestaan voor de duur en het type moet worden toegevoegd aan de naam van de aanbieding.

De volgende lijst bevat enkele goed benoemde aanbieding namen:

-   Essentials voor professionele diensten: 1 uur Briefing
-   Cloudplatform voor migratie: 1 uur Briefing
-   PowerApps en Microsoft Flow: 1 dag-Workshop
-   Azure Machine Learning-Services: 3-wk PoC
-   Brick en klikt u op van een Retailanalyse oplossing: 1 uur Briefing
-   Gebruik uw eigen gegevens: 1 wk-Workshop
-   Cloud-analyse: 3 dagen-Workshop
-   Training voor Power BI: 3 dagen-Workshop
-   Verkoop Management-oplossing: 1 week-implementatie
-   CRM-Snelstartgids: 1 dag-Workshop
-   Dynamics 365 for Sales: 2-daagse evaluatie

Nadat u hebt de **instellingen bieden** tabblad kunt u uw inzending opslaan. De aanbiedingsnaam van de wordt nu weergegeven boven de editor en u vindt deze in alle biedt.

**Volgende stappen**

Nu kunt u [Storefront details en bepalen of u wilt publiceren in Azure Marketplace of AppSource](./cpp-consulting-service-storefront-details.md).
