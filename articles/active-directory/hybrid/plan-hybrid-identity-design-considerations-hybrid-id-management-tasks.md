---
title: Hybride identiteit ontwerp - beheertaken Azure | Microsoft Docs
description: Azure Active Directory controleert met voorwaardelijk toegangsbeheer, de specifieke voorwaarden die u bij het verifiëren van de gebruiker en voordat de toegang tot de toepassing kiezen. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 436bb3c235c76f04e0f3d0fec1beeec3872f1dfb
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54475812"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Plan voor de levenscyclus van hybride identiteit
Identiteit is een van de fundamenten van uw strategie voor enterprise mobility en de toepassing. Of u zich op uw mobiele apparaat of de SaaS-app, is uw identiteit de sleutel tot het verkrijgen van toegang tot alles. Op het hoogste niveau omvat een oplossing voor identiteitsbeheer werking en synchronisatie tussen uw opslagplaatsen voor identiteit, waaronder automatiseren en het proces van inrichting van resources te centraliseren. De oplossing voor identiteit moet de identiteit van een gecentraliseerde via on-premises en cloud en een vorm van identiteitsfederatie ook gebruiken om te onderhouden centrale verificatie en veilig delen en samenwerken met externe gebruikers en bedrijven. Bronnen variëren van besturingssystemen en toepassingen naar mensen in of gekoppeld aan een organisatie. Organisatiestructuur kan worden gewijzigd om te voldoen aan de inrichting beleidsregels en procedures.

Het is ook belangrijk dat u hebt een oplossing voor identiteit die is afgestemd om te zorgen dat uw gebruikers door te voorzien in selfservice om u te blijven ze productief. Uw oplossing voor identiteit is krachtiger dat als deze eenmalige aanmelding voor gebruikers in alle resources kan ze toegang nodig. Beheerders op alle niveaus kunnen gestandaardiseerde procedures voor het beheren van gebruikersreferenties gebruiken. Bepaalde niveaus van beheer kunnen worden verlaagd of geëlimineerd, afhankelijk van de breedte van het beheersysteem voor inrichting. Bovendien kunt u veilig distribueren beheermogelijkheden, handmatig of automatisch, tussen verschillende organisaties. Een domeinbeheerder kan bijvoorbeeld alleen de personen en bronnen in dat domein fungeren. Deze gebruiker met beheerdersrechten en inrichting taken kan uitvoeren, maar is niet gemachtigd voor de configuratie van taken, zoals het maken van werkstromen.

## <a name="determine-hybrid-identity-management-tasks"></a>Beheertaken voor hybride identiteit bepalen
Administratieve taken in uw organisatie distribueren verbetert de nauwkeurigheid en de effectiviteit van beheer en verbetert het saldo van de werkbelasting van een organisatie. Hieronder vindt u het gebruik van draaitabellen die een robuuste identiteitsbeheersysteem definiëren.

 ![](./media/plan-hybrid-identity-design-considerations/Identity_management_considerations.png)

Voor het definiëren van beheertaken voor hybride identiteit, moet u enkele belangrijke kenmerken van de organisatie die vast te voor hybride identiteit stellen begrijpen. Het is belangrijk om te begrijpen van de huidige opslagplaatsen wordt gebruikt voor identiteit-bronnen. Door de wetenschap dat deze elementen core, wordt de fundamentele vereisten en op basis van dat u meer gedetailleerde vragen die u tot een betere ontwerpbeslissing voor uw oplossing voor identiteit leiden zal.  

Tijdens het definiëren van deze vereisten, zorg ervoor dat ten minste de volgende vragen worden beantwoord

* Inrichtingsopties: 
  
  * Ondersteunt de oplossing voor hybride identiteit een robuuste account toegangsbeheer en inrichting systeem?
  * Hoe worden gebruikers, groepen en wachtwoorden zal worden beheerd?
  * Het beheer van identiteitslevenscycli reageert? 
    * Hoe lang duurt wachtwoord updates account onderbreking?
* Licentiebeheer: 
  
  * Wordt de licentie voor beheer van hybride identiteit oplossing verwerkt?
    * Zo ja, is welke mogelijkheden zijn beschikbaar?
* De oplossing wordt verwerkt op basis van een groep Licentiebeheer in? 
  
      - Zo ja, is het mogelijk een beveiligingsgroep toewijzen aan deze? 
       - Als u Ja kiest, wordt de clouddirectory automatisch licenties toewijzen aan alle leden van de groep? 
        - Wat gebeurt er als een gebruiker wordt vervolgens toegevoegd aan of verwijderd uit de groep, wordt een licentie worden automatisch toegewezen of verwijderd, zoals toepasselijk? 
* Integratie met andere externe id-providers:
* Kan deze hybride oplossing worden geïntegreerd met externe id-providers voor het implementeren van eenmalige aanmelding?
* Is het mogelijk om alle andere id-providers in een samenhangend identiteitssysteem?
* Zo ja, hoe en zijn ze en welke mogelijkheden zijn beschikbaar?

## <a name="synchronization-management"></a>Synchronisatie Management
Een van de doelstellingen van een id-beheer, kunnen alle id-providers en houdt deze gesynchroniseerd. Behouden van de gegevens worden gesynchroniseerd op basis van een gezaghebbende master id-provider. In een hybride identiteitsscenario, met een model gesynchroniseerde management alle gebruiker en apparaat-id's in een on-premises server beheren en synchroniseren van de accounts en, optioneel, wachtwoorden naar de cloud. De gebruiker voert de dezelfde on-premises wachtwoord als ze in de cloud doen, en bij het aanmelden, het wachtwoord wordt geverifieerd door de oplossing voor identiteit. Dit model maakt gebruik van een hulpprogramma voor directory-synchronisatie.

![](./media/plan-hybrid-identity-design-considerations/Directory_synchronization.png) Juiste ontwerp de synchronisatie van uw oplossing voor hybride identiteiten ervoor te zorgen dat de volgende vragen worden beantwoord: • wat zijn de synchronisatie-oplossingen beschikbaar zijn voor de oplossing voor hybride identiteit?
• Wat zijn de mogelijkheden die beschikbaar zijn voor eenmalige aanmelding?
• Wat zijn de opties voor identiteitsfederatie tussen B2B en B2C?

## <a name="next-steps"></a>Volgende stappen
[Acceptatiestrategie voor hybride identiteit management bepalen](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

