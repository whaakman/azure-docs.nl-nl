---
title: Vooraf laden van assets op een Azure CDN-eindpunt | Microsoft Docs
description: Informatie over het vooraf laden van inhoud in cache op een Azure CDN-eindpunt.
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vooraf assets op een Azure CDN-eindpunt laden
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standaard activa eerst in de cache opgeslagen als ze worden aangevraagd. Dit betekent dat de eerste aanvraag vanuit elke regio kan het langer duren, omdat de randservers niet de inhoud in cache en moet de aanvraag doorsturen naar de oorspronkelijke server. Vooraf laden van inhoud, wordt dit eerste treffers latentie voorkomen.

Naast het bieden van een betere klantervaring vooraf laden van uw assets in de cache kan ook leiden tot minder netwerkverkeer op de bronserver.

> [!NOTE]
> Vooraf laden van de activa is handig voor grote gebeurtenissen of inhoud die tegelijkertijd beschikbaar voor een groot aantal gebruikers, zoals een nieuwe film release of een software-update.
> 
> 

Deze zelfstudie leert u de inhoud op alle knooppunten van Azure CDN edge cache vooraf laden.

## <a name="walkthrough"></a>Walkthrough
1. In de [Azure Portal](https://portal.azure.com), blader naar de CDN-profiel met het eindpunt dat u wilt vooraf laden.  De profiel-blade wordt geopend.
2. Klik op het eindpunt in de lijst.  De eindpunt-blade wordt geopend.
3. Klik op de knop laden op de blade CDN-eindpunt.
   
    ![Blade CDN-eindpunt](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    De Load-blade wordt geopend.
   
    ![Load-blade CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Geef het volledige pad van elk actief die u wilt laden (bijv, `/pictures/kitten.png`) in de **pad** textbox.
   
   > [!TIP]
   > Meer **pad** tekstvakken wordt weergegeven nadat u tekst zodat u kunt een lijst samenstellen van meerdere elementen.  U kunt activa verwijderen uit de lijst door te klikken op de knop met het weglatingsteken (...).
   > 
   > Paden moeten een relatieve URL die past bij de volgende [reguliere expressie](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Het pad naar een enkel bestand laden `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Laden van één bestand met de query-tekenreeks`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Elke asset moet een eigen pad hebben.  Er is geen functionaliteit jokerteken voor bedrijfsmiddelen die vooraf laden.
   > 
   > 
   
    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Klik op de **Load** knop.
   
    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Er is een beperking van 10 load aanvragen per minuut per CDN-profiel. 50 paden zijn toegestaan per aanvraag. Elk pad heeft een limiet lengte van 1024 tekens.
> 
> 

## <a name="see-also"></a>Zie ook
* [Een Azure CDN-eindpunt leegmaken](cdn-purge-endpoint.md)
* [Naslaginformatie over Azure CDN REST-API - opschonen of een eindpunt vooraf laden](https://msdn.microsoft.com/library/mt634451.aspx)

