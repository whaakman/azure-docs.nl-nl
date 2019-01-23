---
title: Hybride identiteit ontwerp - gegevensbeveiligingsvereisten van Azure | Microsoft Docs
description: Identificeer de beveiligingsvereisten voor gegevens voor uw bedrijf en welke opties zijn beschikbaar voor het best voldoen aan deze vereisten bij het plannen van uw oplossing voor hybride identiteit.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 34c284f3fdd78d8b2f56ec7c36e139f9ac9d78bf
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473500"
---
# <a name="plan-for-enhancing-data-security-through-a-strong-identity-solution"></a>Plan voor het verbeteren van de beveiliging van gegevens via een goede oplossing voor identiteitsbeheer
De eerste stap bij het beveiligen van gegevens is om te bepalen wie toegang heeft tot die gegevens. U moet ook een identiteitsoplossing die kan worden geïntegreerd met uw systeem zodat de mogelijkheden van verificatie en autorisatie hebt. Verificatie en autorisatie worden vaak verward met elkaar en hun rollen verkeerd begrepen. In werkelijkheid zijn deze verschillend, zoals wordt weergegeven in de afbeelding hieronder:

![](./media/plan-hybrid-identity-design-considerations/mobile-devicemgt-lifecycle.png)

**Fasen van levenscyclus van mobiele apparaten**

Bij het plannen van uw oplossing voor hybride identiteit, moet u begrijpen de beveiligingsvereisten voor gegevens voor uw bedrijf en welke opties zijn beschikbaar voor de beste voldoen aan deze vereisten.

> [!NOTE]
> Wanneer u klaar bent met het plannen voor de beveiliging van gegevens, Bekijk [Bepaal de vereisten voor meervoudige verificatie](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md) om ervoor te zorgen dat uw selecties met betrekking tot vereisten voor multi-factor authentication niet waren beïnvloed door de beslissingen die u in deze sectie wordt gemaakt.
> 
> 

## <a name="determine-data-protection-requirements"></a>Bepalen van de beveiligingsvereisten voor gegevens
In het tijdperk van mobiliteit, de meeste bedrijven hebben een gemeenschappelijk doel: hun gebruikers in staat om productief op hun mobiele apparaten, ze op kantoor, of extern vanaf elke locatie om de productiviteit kunt verhogen. Bedrijven die dergelijke gelden ook worden betrokken zijn bij het aantal bedreigingen die moet worden verholpen om gegevens van het bedrijf te beveiligen en onderhouden van de privacy van gebruikers. Elk bedrijf mogelijk verschillende vereisten in dit opzicht; verschillende compatibiliteitsregels die variëren op basis van welke bedrijfstak van het bedrijf fungeert zal leiden tot verschillende ontwerpbeslissingen. 

Er zijn echter enkele beveiligingsaspecten die moeten worden verkend en gevalideerd, ongeacht de branche.

## <a name="data-protection-paths"></a>Gegevenspaden voor beveiliging
![](./media/plan-hybrid-identity-design-considerations/data-protection-paths.png)

**Gegevenspaden voor beveiliging**

In het bovenstaande diagram worden het onderdeel-id de eerste optie die worden geverifieerd voordat gegevens worden geopend. Deze gegevens kan echter zich in verschillende statussen tijdens de periode dat deze is geopend. Elk getal in het volgende diagram geeft een pad op waarin gegevens gevonden op een bepaald moment in-time worden kunnen. Deze getallen worden hieronder beschreven:

1. Gegevensbeveiliging op het apparaatniveau van het.
2. Gegevensbescherming onderweg zijn.
3. Gegevensbescherming in rest on-premises.
4. Gegevensbeveiliging terwijl ze rusten in de cloud.

Is het nodig dat de oplossing voor hybride identiteit geschikt voor gebruik te maken van zowel is on-premises en in de cloud identity management-resources te identificeren van de gebruiker voordat het verleent toegang tot de gegevens. Zorg ervoor dat de volgende vragen worden beantwoord op basis van de vereisten van uw organisatie bij het plannen van uw oplossing voor hybride identiteit:

## <a name="data-protection-at-rest"></a>Bescherming van gegevens in rust
Ongeacht waar de gegevens zich in rust (apparaat, cloud of on-premises), is het belangrijk dat u een beoordeling voor meer informatie over de behoeften van de organisatie in dit opzicht uitvoeren. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Heeft uw bedrijf behoefte aan gegevens in rust beveiligen?
  * Zo ja, is de oplossing voor hybride identiteiten integreren met uw huidige on-premises infrastructuur?
  * Zo ja, is de oplossing voor hybride identiteiten integreren met uw workloads die zich in de cloud?
* Kan het identiteitsbeheer cloud om de referenties van de gebruiker en andere gegevens die zijn opgeslagen in de cloud te beveiligen?

## <a name="data-protection-in-transit"></a>Bescherming van gegevens in transit
Gegevens die onderweg zijn tussen het apparaat en het datacenter of het apparaat en de cloud moeten worden beveiligd. Echter, wordt in-transit betekent niet noodzakelijkerwijs een communicatie-proces met een component buiten uw cloudservice; wordt ook, intern, verplaatst bijvoorbeeld tussen twee virtuele netwerken. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Heeft uw bedrijf behoefte aan het beveiligen van gegevens die onderweg zijn?
  * Zo ja, is de oplossing voor hybride identiteiten integreren met beveiligde besturingselementen, zoals SSL/TLS?
* Houdt het identiteitsbeheer cloud het verkeer naar en tussen de Active directory-archief (binnen en tussen datacenters) dat wordt ondertekend

## <a name="compliance"></a>Naleving
Voorschriften, wettelijke en nalevingsvereisten is afhankelijk van de branche die deel uitmaakt van uw bedrijf. Identiteitsbeheer problemen met betrekking tot problemen met apparaatcompatibiliteit moeten rekening houden met bedrijven in hoge gereguleerde branches. Regelgeving zoals Sarbanes-Oxley (SOX), de Health Insurance Portability and Accountability Act (HIPAA), de Gramm-Leach-Bliley Act (GLBA) en de Payment Card Industry Data Security Standard (PCI DSS) zijn strikte met betrekking tot de identiteits- en toegangsbeheer. De oplossing voor hybride identiteiten die uw bedrijf gaat hanteren beschikken over de belangrijkste mogelijkheden die zal voldoen aan de vereisten van een of meer van deze voorschriften. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

* Is de oplossing voor hybride identiteit voldoen aan de wettelijke vereisten voor uw bedrijf?
* Hiermee worden die de oplossing voor hybride identiteit is gemaakt 
* in de mogelijkheden waarmee uw bedrijf moet voldoen aan het beleid reglementaire voorschriften? 

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Definieer Gegevensbeveiligingsstrategie](plan-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die kunt u selecteren welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
 [Bepaal de vereisten voor inhoudsbeheer](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

