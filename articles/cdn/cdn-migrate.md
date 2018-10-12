---
title: Een Azure CDN-profiel van Standard van Verizon migreren naar Premium van Verizon | Microsoft Docs
description: Meer informatie over de details van de migratie van een profiel van Verizon Standard naar Premium van Verizon.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 7768dde424aedc295b53512db50c9dfc9db9ab8c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091467"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Een Azure CDN-profiel van de Standard-Verizon migreren naar Premium-Verizon

Wanneer u een profiel Azure Content Delivery Network (CDN) voor het beheren van uw eindpunten maakt, biedt Azure CDN vier verschillende producten voor u om uit te kiezen. Zie voor meer informatie over de verschillende producten en de beschikbare functies [vergelijken Azure CDN-producteigenschappen](cdn-features.md).

Als u hebt een **Azure CDN Standard van Verizon** profileren en te gebruiken voor het beheren van uw CDN-eindpunten, hebt u de optie voor een upgrade uitvoeren naar een **Azure CDN Premium van Verizon** profiel. Wanneer u een upgrade uitvoert, wordt uw CDN-eindpunten en al uw gegevens worden bewaard. 

> [!IMPORTANT]
> Wanneer u een upgrade hebt uitgevoerd naar een **Azure CDN Premium van Verizon** profiel u later deze niet converteren naar een **Azure CDN Standard van Verizon** profiel.
> 

Upgrade een **Azure CDN Standard van Verizon** profiel, neem contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Vergelijking van profiel
**Azure CDN Premium van Verizon** profielen hebben de volgende belangrijke verschillen van **Azure CDN Standard van Verizon** profielen:
- Voor bepaalde Azure CDN functies zoals [compressie](cdn-improve-performance.md), [regels voor caching](cdn-caching-rules.md), en [geografische filtering](cdn-restrict-access-by-country.md), u kunt de Azure CDN-interface niet gebruiken, moet u de portal Verizon via de **Beheren** knop.
- API: In tegenstelling tot met Standard-Verizon, u niet gebruiken de API voor het beheren van deze functies die worden geopend via de portal voor Premium-Verizon. U kunt echter de API gebruiken voor het beheren van andere algemene functies, zoals het maken/verwijderen van een eindpunt, assets in cache wissen/laden en in-/ uitschakelen een aangepast domein.
- Prijs: Premium-Verizon heeft een andere prijscategorie structuur voor de gegevensoverdracht dan Standard-Verizon. Zie voor meer informatie, [prijzen van Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

**Azure CDN Premium van Verizon** profielen hebben de volgende extra functies:
- [Tokenverificatie](cdn-token-auth.md): Hiermee kunnen gebruikers verkrijgen en gebruiken van een token voor het ophalen van beveiligde bronnen.
- [Regels-engine](cdn-rules-engine.md): Hiermee kunt u aanpassen hoe HTTP-aanvragen worden verwerkt.
- Geavanceerde analysehulpprogramma's:
   - [Gedetailleerde analyse van HTTP](cdn-advanced-http-reports.md)
   - [Randprestatieanalyse](cdn-edge-performance.md)
   - [Realtime analyses](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de regelengine, [naslaginformatie over Azure CDN de regelengine](cdn-rules-engine-reference.md).

