---
title: Vooraf laden van assets op een Azure CDN-eindpunt | Microsoft Docs
description: Informatie over het vooraf laden van inhoud in cache op een Azure CDN-eindpunt.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: mazha
ms.openlocfilehash: bf3161d756759e4b278e48ad7a49615e4a73d17f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150131"
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vooraf assets op een Azure CDN-eindpunt laden
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standaard activa zijn opgeslagen in de cache alleen wanneer ze zijn aangevraagd. Omdat de randservers nog niet zijn opgeslagen op de inhoud en moeten de aanvraag doorsturen naar de oorspronkelijke server, kan de eerste aanvraag vanuit elke regio langer duren dan de volgende aanvragen. Om te voorkomen dat deze eerste hit latentie, kunt u uw assets vooraf laden. Naast het bieden van een betere klantervaring kan vooraf laden van uw assets in de cache verminderen netwerkverkeer op de bronserver.

> [!NOTE]
> Vooraf laden van de activa is handig voor grote gebeurtenissen of inhoud die tegelijkertijd beschikbaar voor veel gebruikers, zoals een nieuwe film release of een software-update.
> 
> 

Deze zelfstudie leert u de inhoud op alle knooppunten van Azure CDN edge cache vooraf laden.

## <a name="to-pre-load-assets"></a>Vooraf laden van assets
1. In de [Azure-portal](https://portal.azure.com), blader naar de CDN-profiel met het eindpunt dat u wilt vooraf laden. Hiermee opent u het deelvenster profiel.
    
2. Klik op het eindpunt in de lijst. Hiermee opent u het deelvenster eindpunt.
3. Selecteer in het deelvenster CDN-eindpunt **Load**.
   
    ![CDN-eindpunt deelvenster](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    De **Load** deelvenster wordt geopend.
   
    ![CDN load deelvenster](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Voor **Inhoudspad**, voer het volledige pad van elk actief die u wilt laden (bijvoorbeeld `/pictures/kitten.png`).
   
   > [!TIP]
   > Nadat u bent begonnen met het invoeren van tekst, meer **Inhoudspad** tekstvakken kunt u een lijst samenstellen van meerdere elementen wordt weergegeven. Als u wilt assets verwijderen uit de lijst, selecteert u de knop met het weglatingsteken (...) en selecteer vervolgens **verwijderen**.
   > 
   > Elk pad naar inhoud moet een relatieve URL die past bij de volgende [reguliere expressies](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Het pad naar een enkel bestand laden: `^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$`  
   > - Laden van één bestand met de query-tekenreeks: `^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$` 
   > 
   > Omdat elke asset een eigen pad hebben moet, is er geen jokerteken functionaliteit voor vooraf laden activa.
   > 
   > 
   
    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Wanneer u klaar bent met het invoeren van paden naar inhoud, selecteert u **Load**.
   

> [!NOTE]
> Er is een limiet van 10 load aanvragen per minuut per CDN-profiel en 50 gelijktijdige paden kunnen tegelijk worden verwerkt. Elk pad heeft een limiet lengte van 1024 tekens.
> 
> 

## <a name="see-also"></a>Zie ook
* [Een Azure CDN-eindpunt leegmaken](cdn-purge-endpoint.md)
* [Naslaginformatie over Azure CDN REST-API: vooraf laden van inhoud op een eindpunt](https://docs.microsoft.com/rest/api/cdn/endpoints/loadcontent)
* [Naslaginformatie over Azure CDN REST-API: inhoud van een eindpunt leegmaken](https://docs.microsoft.com/rest/api/cdn/endpoints/purgecontent)

