---
title: Een Azure CDN-eindpunt leegmaken | Microsoft Docs
description: Informatie over het wissen van alle inhoud in cache van een Azure CDN-eindpunt.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b035c232bb58d653960190d4974cc3789d55a51d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a>Een Azure CDN-eindpunt leegmaken
## <a name="overview"></a>Overzicht
Azure CDN edge-knooppunten cache activa totdat de asset time to live (TTL) is verlopen.  Nadat de asset TTL is verlopen wanneer een client de asset-van het edge-knooppunt aanvragen, het edge-knooppunt wordt een nieuwe, bijgewerkte kopie van de asset voor aanvraag van de client opgehaald en store Vernieuw het cachegeheugen.

De beste manier om ervoor te zorgen dat uw gebruikers altijd de meest recente kopie van uw assets ophalen is versie van uw assets voor elke update en deze publiceren als nieuwe URL's.  CDN haalt onmiddellijk nieuwe elementen voor de volgende aanvragen van clients.  Soms wilt u mogelijk de inhoud in cache wissen van alle knooppunten van de rand en zorgen dat ze alle nieuwe bijgewerkte activa ophalen.  Dit kan zijn vanwege updates op uw webtoepassing, of op snel update assets die onjuiste gegevens bevatten.

> [!TIP]
> Houd er rekening mee dat alleen de inhoud in de cache op de servers van de rand CDN opschonen wist.  Eventuele downstream caches, zoals proxyservers en caches van lokale, kunnen nog steeds een kopie van het bestand worden gehouden.  Het is belangrijk om te onthouden dit als u een bestand time to live.  U kunt een downstream-client om aan te vragen van de meest recente versie van het bestand door een unieke naam geven telkens wanneer u deze bijwerken of door gebruik te maken van afdwingen [query opslaan in cache](cdn-query-string.md).  
> 
> 

Deze zelfstudie leert u het opschonen van de activa van alle knooppunten van de rand van een eindpunt.

## <a name="walkthrough"></a>Walkthrough
1. In de [Azure Portal](https://portal.azure.com), blader naar de CDN-profiel met het eindpunt dat u wilt wissen.
2. Klik op de knop opschonen van de blade CDN-profiel.
   
    ![Blade CDN-profiel](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    De blade opschonen wordt geopend.
   
    ![Blade CDN-opschonen](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Selecteer op de blade opschonen het serviceadres die u wilt verwijderen uit de vervolgkeuzelijst URL.
   
    ![Formulier opschonen](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > U kunt ook naar de blade opschonen opvragen door te klikken op de **opschonen** knop op de blade CDN-eindpunt.  In dat geval de **URL** veld worden vooraf ingevuld met het serviceadres van dat specifieke eindpunt.
   > 
   > 
4. Selecteer welke elementen die u wilt verwijderen uit de edge-knooppunten.  Als u wenst te wissen van alle activa, klikt u op de **Alles opschonen** selectievakje.  Anders, typ het pad van elke asset die u wissen wilt de **pad** textbox. Hieronder indelingen worden ondersteund in het pad.
    1. **Opschonen van één URL**: individuele asset opschonen door te geven van de volledige URL met of zonder de bestandsextensie, bijvoorbeeld`/pictures/strasbourg.png`;`/pictures/strasbourg`
    2. **Jokertekens opschonen**: sterretje (\*) kan worden gebruikt als een jokerteken. Opschonen van mappen, submappen en bestanden onder een eindpunt met `/*` in het pad of opschonen alle submappen en bestanden in een specifieke map die door het opgeven van de map gevolgd door `/*`, bijvoorbeeld`/pictures/*`.  Houd er rekening mee dat leegmaken met jokertekens wordt momenteel niet ondersteund door Azure CDN van Akamai. 
    3. **Opschonen van domein hoofdmap**: opschonen van de hoofdmap van het eindpunt met '/' in het pad.
   
   > [!TIP]
   > Paden moeten worden opgegeven voor opschonen en moet een relatieve URL die voldoen aan de volgende [reguliere expressie](https://msdn.microsoft.com/library/az24scfc.aspx). **Alles opschonen** en **jokertekens opschonen** niet wordt ondersteund door **Azure CDN van Akamai** momenteel.
   > > Opschonen van één URL`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Queryreeks`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Jokertekens opschonen `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Meer **pad** tekstvakken wordt weergegeven nadat u tekst zodat u kunt een lijst samenstellen van meerdere elementen.  U kunt activa verwijderen uit de lijst door te klikken op de knop met het weglatingsteken (...).
   > 
5. Klik op de **opschonen** knop.
   
    ![Knop verwijderen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Opschonen aanvragen minuten duren voordat ongeveer 2-3 verwerkt met **Azure CDN van Verizon** (standaard en Premium), en ongeveer 7 minuten met **Azure CDN van Akamai**.  Azure CDN heeft een limiet van 50 gelijktijdige aanvragen op elk moment verwijderen. 
> 
> 

## <a name="see-also"></a>Zie ook
* [Vooraf assets op een Azure CDN-eindpunt laden](cdn-preload-endpoint.md)
* [Naslaginformatie over Azure CDN REST-API - opschonen of een eindpunt vooraf laden](https://msdn.microsoft.com/library/mt634451.aspx)

