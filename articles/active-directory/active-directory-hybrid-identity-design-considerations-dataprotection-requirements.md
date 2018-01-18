---
title: Ontwerp van hybride identiteit - gegevensbeveiligingsvereisten Azure | Microsoft Docs
description: Identificeer de beveiligingsvereisten van de gegevens voor uw bedrijf en welke opties zijn beschikbaar voor het beste voldoen aan deze vereisten bij het plannen van uw oplossing voor hybride identiteit.
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 1a2feca8d24fabda4c191b5a181ab6606c912657
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plan voor het verbeteren van de beveiliging van gegevens via de van de sterke-identiteitsoplossing
De eerste stap om de gegevens te beveiligen is bepalen wie toegang heeft tot die gegevens en als onderdeel van dit proces die moet u beschikken over een identiteit oplossing die kan worden geïntegreerd met uw systeem te bieden mogelijkheden voor verificatie en autorisatie. Verificatie en autorisatie worden vaak verward met elkaar en hun rollen verkeerd begrepen. In werkelijkheid zijn ze heel anders, zoals wordt weergegeven in de afbeelding hieronder:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Fasen van de levenscyclus van mobiele apparaten**

Bij het plannen van uw oplossing voor hybride identiteit u moet de beveiligingsvereisten voor gegevens voor uw bedrijf en welke opties zijn beschikbaar voor het beste voldoen aan deze vereisten kennen.

> [!NOTE]
> Wanneer u klaar bent met het plannen van de beveiliging van gegevens controleren [vereisten multi-factor authentication bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) om ervoor te zorgen dat uw selecties met betrekking tot vereisten multi-factor authentication niet zijn van invloed op de beslissingen die u in deze sectie wordt gemaakt.
> 
> 

## <a name="determine-data-protection-requirements"></a>Bepalen van de beveiligingsvereisten voor gegevens
In de leeftijd mobiliteit van de meeste bedrijven hebben een gemeenschappelijk doel: hun gebruikers in staat om productief op hun mobiele apparaten als ze op kantoor of extern vanaf elke locatie om productiviteit verhogen. Hoewel dit kan een gemeenschappelijk doel, worden bedrijven die vereist zijn om te gaan met betrekking tot de hoeveelheid bedreigingen die moeten worden beperkt om bedrijfsgegevens te beveiligen en beheren van de privacy van gebruikers ook. Elk bedrijf heeft wellicht andere vereisten in dit opzicht; van andere nalevingsregels die variëren volgens welke bedrijfstak van het bedrijf fungeert zal leiden tot verschillende ontwerpbeslissingen. 

Er zijn echter enkele beveiligingsaspecten die moeten worden verkend en gevalideerd, ongeacht de branche die worden beschreven in de volgende sectie.

## <a name="data-protection-paths"></a>Gegevenspaden voor beveiliging
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Gegevenspaden voor beveiliging**

In het bovenstaande diagram worden het onderdeel-id de eerste die worden geverifieerd voordat gegevens worden geopend. Deze gegevens kunnen zich in verschillende statussen tijdens de periode die is aangesproken. Elk getal in dit diagram vertegenwoordigt een pad waarin gegevens gevonden op een bepaald punt in tijd worden kunnen. Deze getallen worden hieronder beschreven:

1. Gegevensbescherming op het niveau van apparaten.
2. Gegevensbescherming onderweg.
3. Bescherming van cloudgegevens in rust lokale.
4. Gegevensbescherming in rust in de cloud.

Hoewel de technische dat besturingselementen inschakelen IT de gegevens zelf beveiligen op elk van deze fasen worden niet rechtstreeks aangeboden door de oplossing voor hybride identiteit, is het nodig dat de oplossing voor hybride identiteit is geschikt voor gebruik van zowel on-premises en cloud Identity management-resources voor het identificeren van de gebruiker voordat toegang verlenen tot de gegevens. Wanneer uw oplossing voor hybride identiteit planning Zorg ervoor dat de volgende vragen worden beantwoord volgens de vereisten van uw organisatie:

## <a name="data-protection-at-rest"></a>Gegevensbescherming van in rust
Ongeacht waar de gegevens in rust (apparaat, cloud of on-premises), is het belangrijk dat u een beoordeling voor in dit opzicht inzicht in de behoeften van de organisatie. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Heeft uw bedrijf behoefte om gegevens in rust te beveiligen?
  * Zo ja, kan de oplossing voor hybride identiteiten integreren met uw huidige on-premises infrastructuur?
  * Zo ja, kan de oplossing voor hybride identiteiten integreren met uw workloads die zich in de cloud?
* Kan de cloud identity management om referenties van de gebruiker en andere gegevens die zijn opgeslagen in de cloud te beveiligen?

## <a name="data-protection-in-transit"></a>Gegevensbescherming onderweg
Gegevens onderweg tussen het apparaat en het datacenter of tussen het apparaat en de cloud moeten worden beveiligd. Echter, worden in transit betekent niet noodzakelijkerwijs een proces voor communicatie met een component buiten uw cloudservice; verplaatst intern ook, bijvoorbeeld tussen twee virtuele netwerken. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Heeft uw bedrijf behoefte om gegevens onderweg te beveiligen?
  * Zo ja, kan de oplossing voor hybride identiteiten integreren met veilige besturingselementen zoals SSL/TLS?
* Houdt de cloud identity management het verkeer van en in de Active directory-archief (binnen en tussen datacenters) ondertekend

## <a name="compliance"></a>Naleving
Voorschriften, wetten en regelgeving vereisten zijn afhankelijk van de branche die deel uitmaakt van uw bedrijf. Bedrijven in hoog gereglementeerde branches moeten identiteitsbeheer problemen die betrekking hebben op problemen met naleving houden. Regelgeving zoals Sarbanes-Oxley (SOX), de Health Insurance Portability and Accountability Act (HIPAA), de Gramm-Leach-Bliley Act (GLBA) en de Payment Card Industry Data Security Standard (PCI DSS) zijn zeer strikte met betrekking tot identiteits- en toegangsbeheer. De oplossing voor hybride identiteiten die uw bedrijf invoert, moet de belangrijkste mogelijkheden die zal voldoen aan de vereisten van een of meer van deze voorschriften hebben. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Is de oplossing voor hybride identiteit voldoen aan de wettelijke vereisten voor uw bedrijf?
* De hybride identiteitsoplossing met ingebouwde mogelijkheden waarmee u uw bedrijf compatibel voorschriften worden? 

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Definieer de strategie voor gegevensbescherming](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die u selecteert welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
 [Vereisten voor inhoudsbeheer bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

