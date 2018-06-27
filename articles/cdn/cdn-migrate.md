---
title: Een profiel Azure CDN van Verizon standaard migreren naar Premium van Verizon | Microsoft Docs
description: Meer informatie over de details van de migratie van een profiel van Verizon standaard naar Premium van Verizon.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: 074dbb094e7ae2cd1f1719016928bd7348da3949
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958051"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Een Azure CDN-profiel van Verizon Standard migreren naar Premium Verizon

Wanneer u een profiel Azure Content Delivery Network (CDN) voor het beheren van uw eindpunten maakt, biedt Azure CDN vier verschillende producten voor waaruit u kunt kiezen. Zie voor meer informatie over de verschillende producten en de beschikbare functies [vergelijken Azure CDN productfuncties](cdn-features.md).

Als u hebt een **Azure CDN Standard van Verizon** profiel en wordt gebruikt voor het beheren van uw CDN-eindpunten, hebt u de optie om bij te werken naar een **Azure CDN Premium van Verizon** profiel. Wanneer u bijwerkt, wordt uw CDN-eindpunten en al uw gegevens worden bewaard. 

> [!IMPORTANT]
> Zodra u hebt bijgewerkt naar een **Azure CDN Premium van Verizon** profiel u later deze niet converteren naar een **Azure CDN Standard van Verizon** profiel.
> 

Upgrade een **Azure CDN Standard van Verizon** profiel, contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Vergelijking van profiel
**Azure CDN Premium van Verizon** profielen hebben de volgende belangrijke verschillen van **Azure CDN Standard van Verizon** profielen:
- Voor bepaalde Azure CDN functies zoals [compressie](cdn-improve-performance.md), [regels opslaan in cache](cdn-caching-rules.md), en [geo filteren](cdn-restrict-access-by-country.md), u kunt de Azure CDN-interface niet gebruiken, moet u de portal Verizon via de **Beheren** knop.
- API: In tegenstelling tot bij standaard Verizon u kan niet de API gebruiken om de functies die worden geopend vanuit de portal Premium Verizon te beheren. U kunt echter de API gebruiken om andere algemene functies, zoals maken/verwijderen van een eindpunt in de cache activa opschonen/laden en in-/ uitschakelen een aangepast domein te beheren.
- Prijzen: Premium Verizon heeft een andere prijscategorie structuur voor gegevensoverdrachten dan standaard Verizon. Zie voor meer informatie [Content Delivery Network prijzen](https://azure.microsoft.com/pricing/details/cdn/).

**Azure CDN Premium van Verizon** profielen hebben de volgende extra functies:
- [Tokenverificatie](cdn-token-auth.md): Hiermee kunnen gebruikers verkrijgen en gebruiken van een token op te halen van beveiligde bronnen.
- [Regelengine](cdn-rules-engine.md): Hier kunt u aanpassen hoe HTTP-aanvragen worden verwerkt.
- Geavanceerde analyses hulpprogramma's:
   - [Gedetailleerde HTTP analytics](cdn-advanced-http-reports.md)
   - [Rand prestaties analytics](cdn-edge-performance.md)
   - [Realtime-analyses](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de regelengine, [Azure CDN regels engine verwijzing](cdn-rules-engine-reference.md).

