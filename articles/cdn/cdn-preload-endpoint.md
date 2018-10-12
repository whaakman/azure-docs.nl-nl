---
title: Vooraf assets op een Azure CDN-eindpunt laden | Microsoft Docs
description: Informatie over het vooraf laden van inhoud in cache op een Azure CDN-eindpunt.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: magattus
ms.openlocfilehash: d2dc8ad1e4b7e429dc758a96e49aa4825ae108e5
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091314"
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vooraf assets op een Azure CDN-eindpunt laden
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standaard assets in cache zijn opgeslagen alleen wanneer ze worden aangevraagd. Omdat de edge-servers nog niet zijn opgeslagen op de inhoud en moeten de aanvraag doorsturen naar de oorspronkelijke server, is de eerste aanvraag uit elke regio kunt duurt langer dan de volgende aanvragen. Om te voorkomen dat deze latentie eerste kiest, kunt u uw assets vooraf laden. Naast het bieden van een betere ervaring voor klanten, kunt vooraf laden van uw assets in cache verminderen netwerkverkeer op de oorspronkelijke server.

> [!NOTE]
> Vooraf laden van assets is handig voor grote gebeurtenissen of inhoud die tegelijkertijd beschikbaar gesteld aan veel gebruikers, zoals een nieuwe versie van de film of een software-update wordt.
> 
> 

Deze zelfstudie leert u het vooraf laden van inhoud in cache op alle Azure CDN edge-knooppunten.

## <a name="to-pre-load-assets"></a>Vooraf laden van assets
1. In de [Azure-portal](https://portal.azure.com), blader naar het CDN-profiel met het eindpunt dat u wilt vooraf laden. Het deelvenster profiel wordt geopend.
    
2. Klik op het eindpunt in de lijst. Het deelvenster eindpunt wordt geopend.
3. Selecteer in het deelvenster CDN-eindpunt **Load**.
   
    ![Deelvenster CDN-eindpunt](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    De **Load** deelvenster wordt geopend.
   
    ![CDN load deelvenster](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Voor **Inhoudspad**, geef het volledige pad van de activa die u wilt laden (bijvoorbeeld `/pictures/kitten.png`).
   
   > [!TIP]
   > Nadat u bent begonnen met het invoeren van tekst, meer **Inhoudspad** tekstvakken weergegeven waarmee u kunt een lijst samenstellen van meerdere assets. Als u wilt verwijderen activa in de lijst, selecteert u de knop met het weglatingsteken (...) en vervolgens **verwijderen**.
   > 
   > Elke Inhoudspad moet een relatieve URL die past bij de volgende [reguliere expressies](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Pad naar een enkel bestand laden: `^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$`  
   > - Laden van één bestand met query-tekenreeks: `^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$` 
   > 
   > Omdat elke asset een eigen pad hebben moet, is er geen jokerteken-functionaliteit voor vooraf laden van assets.
   > 
   > 
   
    ![Knop laden](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Wanneer u klaar bent met het invoeren van paden naar inhoud, selecteert u **Load**.
   

> [!NOTE]
> Er is een limiet van 10 load-aanvragen per minuut per CDN-profiel en 50 gelijktijdige paden kunnen in één keer worden verwerkt. Elk pad heeft een limiet pad heeft een lengte van 1024 tekens.
> 
> 

## <a name="see-also"></a>Zie ook
* [Een Azure CDN-eindpunt leegmaken](cdn-purge-endpoint.md)
* [Azure CDN REST API-naslaginformatie: vooraf laden van inhoud op een eindpunt](https://docs.microsoft.com/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN REST API-naslaginformatie: inhoud van een eindpunt leegmaken](https://docs.microsoft.com/rest/api/cdn/endpoints/purgecontent)

