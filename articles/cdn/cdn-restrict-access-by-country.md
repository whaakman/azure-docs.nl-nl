---
title: Azure CDN-inhoud per land beperken | Microsoft Docs
description: Informatie over het beperken van toegang tot uw Azure CDN-inhoud met de functie voor het filteren van geo.
services: cdn
documentationcenter: ''
author: lichard
manager: akucer
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: bb757ab115d03ab04dac4468d23f446696a971a9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="restrict-azure-cdn-content-by-country"></a>Azure CDN-inhoud per land beperken

## <a name="overview"></a>Overzicht
Wanneer een gebruiker wordt uw inhoud standaard aanvraagt, wordt de inhoud aangeboden ongeacht waar de gebruiker gemaakt voor deze aanvraag uit. In sommige gevallen wilt u mogelijk beperken van toegang tot uw inhoud door land. In dit artikel wordt uitgelegd hoe u de *geo filteren* functie om de service configureren voor het toestaan of blokkeren van toegang per land.

> [!IMPORTANT]
> De Azure CDN-producten bieden dezelfde functionaliteit geo filteren, maar hebben een klein verschil in landcodes te ondersteunen. Zie stap 3 voor een koppeling naar de verschillen.


Zie voor meer informatie over overwegingen die van toepassing zijn op de configuratie van dit type beperking [overwegingen](cdn-restrict-access-by-country.md#considerations).  

![Landen filteren](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>Stap 1: Het directorypad definiëren
> [!IMPORTANT]
> **Azure CDN Standard van Microsoft** profielen ondersteunen geen geo-filteren op basis van het pad.
>


Selecteer uw eindpunt in de portal en het filteren van Geo-tabblad vinden op de linkernavigatiebalk voor deze functie vindt.

Wanneer u een filter land configureert, moet u het relatieve pad naar de locatie waarnaar gebruikers wordt toegestaan of toegang geweigerd. U geo-filtering kunt toepassen voor alle bestanden met een slash (/) of de geselecteerde mappen door te geven mappaden */pictures/*. U kunt ook een geografisch filteren in één bestand toepassen door te geven van het bestand en daarbij de afsluitende slash */pictures/city.png*.

Voorbeeld van de directory pad filter:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a>Stap 2: Definieer de actie: blokkeren of toestaan
**Blokkeren:** gebruikers van de opgegeven landen wordt de toegang geweigerd tot bedrijfsmiddelen aangevraagd bij dat recursieve-pad. Als er geen andere land-filteropties zijn geconfigureerd voor die locatie, klikt u vervolgens alle andere gebruikers mogen toegang.

**Toestaan:** alleen uit de opgegeven landen gebruikers toegang tot bedrijfsmiddelen aangevraagd bij dat recursieve-pad kunnen.

## <a name="step-3-define-the-countries"></a>Stap 3: De landen definiëren
Selecteer de landen die u wilt blokkeren of toestaan voor het pad. 

De regel van het blokkeren van /Photos/Straatsburg/wordt bijvoorbeeld bestanden met inbegrip van filteren:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Landcodes
De functie geo filteren gebruikt landcodes voor het definiëren van de landen waarin een aanvraag wordt toegestaan of geblokkeerd voor een beveiligde map. Hoewel de alle Azure CDN-producten dezelfde functionaliteit geo filteren bieden, is er een klein verschil in de landcodes ondersteunen. Zie voor informatie [Azure CDN landcodes](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a name="considerations"></a>Overwegingen
* Wijzigingen in uw land filteren configuratie komen niet onmiddellijk van kracht:
   * Voor **Azure CDN Standard van Microsoft** profielen, doorgeven voltooid gewoonlijk in tien minuten. 
   * Voor **Azure CDN Standard van Akamai** profielen, doorgeven voltooit meestal binnen één minuut. 
   * Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, doorgeven voltooit meestal binnen 90 minuten.  
* Deze functie biedt geen ondersteuning voor jokertekens (bijvoorbeeld ' *').
* De configuratie van de geo-filteren die zijn gekoppeld aan het relatieve pad wordt recursief toegepast naar het opgegeven pad.
* Slechts één regel kan worden toegepast op hetzelfde relatieve pad (u kunt meerdere land filters die naar hetzelfde relatieve pad verwijzen niet maken. Een map hebben echter meerdere land filters. Dit komt door de recursieve aard van de filters land. Een submap van een eerder geconfigureerde map kan met andere woorden, een ander land filter toegewezen.

