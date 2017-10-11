---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - beheertaken voor hybride identiteit Bepaal | Microsoft Docs
description: "Met voorwaardelijk toegangsbeheer controleert de Azure Active Directory de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en alvorens deze toegang tot de toepassing. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang te krijgen tot de toepassing."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 921c8391fc18eca35d10c3ade158a98ae88df397
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Plan voor de levenscyclus van hybride identiteit
Identiteit is een van de basisprincipes van uw strategie voor enterprise mobility en de toepassing. Of u aan te bij uw mobiele apparaat of SaaS-app melden bent, is uw identiteit de sleutel tot het toegang krijgen tot alles. Op het hoogste niveau omvat een oplossing voor identiteitsbeheer samen te voegen en gesynchroniseerd tussen uw opslagplaatsen identiteit waaronder automatiseren en centraal opslaan van het proces voor het leveren van bronnen. De identiteitsoplossing moet zijn van de identiteit van een gecentraliseerde via on-premises en cloud en ook gebruiken een vorm van identiteitsfederatie centrale verificatie onderhouden en veilig delen en samenwerken met externe gebruikers en bedrijven. Resources variëren van besturingssystemen en toepassingen naar mensen in of gekoppeld aan een organisatie. Organisatiestructuur kan worden gewijzigd zodat de inrichting beleidsregels en procedures.

Het is ook belangrijk dat u een oplossing die is afgestemd om te zorgen dat uw gebruikers via een selfservice-ervaring te houden productief hebben. De oplossing van uw identiteit is robuuster als deze kan eenmalige aanmelding voor gebruikers in alle bronnen die ze nodig hebben op alle beheerders toegang niveaus gestandaardiseerde procedures kunnen gebruiken voor het beheren van gebruikersreferenties. Sommige niveaus van beheer worden verlaagd of geëlimineerd, afhankelijk van de breedte van het beheersysteem voor inrichting. Bovendien kunt u veilig distribueren beheermogelijkheden, handmatig of automatisch, tussen verschillende organisaties. Een domeinbeheerder kan bijvoorbeeld alleen de personen en bronnen in dat domein fungeren. Deze gebruiker inrichting en administratieve taken kan uitvoeren, maar is niet geautoriseerd configuratietaken, zoals het maken van werkstromen.

## <a name="determine-hybrid-identity-management-tasks"></a>Beheertaken voor hybride identiteit bepalen
Beheertaken in uw organisatie distribueren verbetert de nauwkeurigheid en de effectiviteit van beheer en verbetert het saldo van de werkbelasting van een organisatie. Hier volgen de gegevensfuncties die een robuuste identiteitsbeheersysteem definiëren.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)

Als u wilt definiëren beheertaken voor hybride identiteit, moet u een aantal belangrijke kenmerken van de organisatie die de vaststelling van hybride identiteit begrijpen. Het is belangrijk om te begrijpen van de huidige opslagplaatsen wordt gebruikt voor de identiteit van bronnen. Deze elementen core weet, wordt de fundamentele vereisten en op basis van dat moet u meer gedetailleerde vragen die u tot een betere ontwerpbeslissing voor uw oplossing identiteit leiden zal.  

Tijdens het definiëren van deze vereisten, zorg ervoor dat ten minste de volgende vragen worden beantwoord

* Opties voor apparaatinrichting: 
  
  * Ondersteunt de hybride identiteitsoplossing een robuuste account toegang tot beheer en inrichting?
  * Hoe worden gebruikers, groepen en wachtwoorden die worden beheerd?
  * De levenscyclus van identiteitsbeheer reageert? 
    * Hoe lang duurt wachtwoord updates account opschorten voordat?
* Licentiebeheer: 
  
  * Ondersteunt het beheer van hybride identiteit oplossing ingangen licentie?
    * Zo ja, welke mogelijkheden zijn beschikbaar?
* Wordt de oplossing op basis van een groep Licentiebeheer verwerkt? 
  
      - Zo ja, is het mogelijk een beveiligingsgroep toewijzen aan deze? 
       - Zo ja, wordt de clouddirectory automatisch toegewezen licenties aan de leden van de groep? 
        - Wat gebeurt er als een gebruiker wordt vervolgens toegevoegd aan of verwijderd uit de groep, een licentie worden automatisch toegewezen aan of verwijderd afhankelijk van wat geschikt? 
* Integratie met andere derden id-providers:
* Deze hybride oplossing worden geïntegreerd met identiteitsproviders van derden voor het implementeren van eenmalige aanmelding?
* Is het mogelijk om alle andere identiteitsproviders verenigen in een identiteitssysteem samenhangender?
* Zo ja, hoe en dat ze zijn en welke mogelijkheden zijn beschikbaar?

## <a name="synchronization-management"></a>Synchronisatie Management
Een van de doelstellingen van een id-beheer kunnen Zorg ervoor dat alle id-providers en houden gesynchroniseerd. Behouden van de gegevens worden gesynchroniseerd op basis van een bindende master id-provider. In een hybride identiteitsscenario met een model gesynchroniseerde management alle gebruiker en apparaat-id's in een on-premises server beheren en synchroniseren van de accounts en, desgewenst, wachtwoorden naar de cloud. De gebruiker voert de hetzelfde wachtwoord on-premises als hij of zij dit doet in de cloud, en bij het aanmelden, het wachtwoord wordt geverifieerd door de oplossing voor identiteit. Dit model maakt gebruik van een directory-synchronisatie.

![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Juiste ontwerp de synchronisatie van uw oplossing voor hybride identiteit ervoor te zorgen dat de volgende vragen worden beantwoord: • wat zijn de synchronisatie-oplossingen beschikbaar zijn voor de oplossing voor hybride identiteit?
• Wat de eenmalige aanmelding mogelijkheden die beschikbaar zijn?
• Wat zijn de opties voor identiteitsfederatie tussen B2B en B2C?

## <a name="next-steps"></a>Volgende stappen
[Hybride identity management acceptatie strategie bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

