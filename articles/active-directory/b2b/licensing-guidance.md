---
title: Azure Active Directory B2B-samenwerking licentierichtlijnen | Microsoft Docs
description: Azure Active Directory B2B collaboration niet vereist-betaalde Azure AD-licenties, maar u kunt ook ophalen betaalde functies voor gastgebruikers voor B2B
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 08/09/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 2050952eb924e1eee5e6d7d6312d9cd02f475d10
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39093098"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Licentierichtlijnen voor B2B-samenwerking van Azure Active Directory

Functionaliteit voor Azure AD B2B-samenwerking kunt u gastgebruikers uitnodigen in uw Azure AD-tenant waarmee ze toegang krijgen tot Azure AD-services en andere resources in uw organisatie. Als u wilt toegang bieden tot betaalde Azure AD-functies, wordt in B2B-samenwerking gastgebruikers moeten zijn gelicentieerd met Azure AD-licenties nodig. 

Specifiek:
* Gratis mogelijkheden van Azure AD zijn beschikbaar voor gastgebruikers zonder aanvullende licenties.
* Als u toegang bieden tot betaalde Azure AD-functies voor B2B-gebruikers wilt, moet u voldoende licenties voor de ondersteuning van de B2B-gastgebruikers hebben.
* Een uitnodigende tenant met een betaalde licentie van Azure AD heeft B2B-samenwerking gebruiksrechten voor een aanvullende vijf B2B gastgebruikers uitgenodigd voor de tenant.
* De klant die eigenaar is van de uitnodigende tenant moet zijn om te bepalen hoeveel B2B-samenwerking gebruikers moeten betaalde Azure AD-mogelijkheden. Afhankelijk van de betaalde Azure AD betaalde functies die u wilt gebruiken voor uw gastgebruikers ook kunnen, moet u genoeg Azure AD hebben gebruikers van B2B-samenwerking in dezelfde 5:1 verhouding-licenties.

Een gastgebruiker van B2B-samenwerking wordt toegevoegd als een gebruiker vanuit een partnerbedrijf, niet een werknemer van uw organisatie of een werknemer van een ander bedrijf in uw conglomeraat. Een B2B-Gast-gebruiker kan zich aanmelden met externe referenties of referenties die eigendom zijn van uw organisatie zoals beschreven in dit artikel. 

Met andere woorden, B2B-licentieverlening ingesteld niet door de manier waarop de gebruiker wordt geverifieerd, maar in plaats van door de relatie van de gebruiker voor uw organisatie. Als deze gebruikers geen partners zijn, worden ze anders behandeld in de licentievoorwaarden. Ze worden niet beschouwd als een gebruiker B2B-samenwerking voor licentieverlening van toepassing, zelfs als hun UserType is gemarkeerd als "Gast." Ze moeten normaal gesproken worden gelicentieerd op één licentie per gebruiker. Deze gebruikers zijn onder andere:
* Uw werknemers
* Aanmelden met gebruikmaking van externe identiteiten personeel
* Een werknemer van een ander bedrijf in uw conglomeraat


## <a name="licensing-examples"></a>Voorbeelden-licentieverlening
- Een klant wil 100 gebruikers van B2B-samenwerking met de Azure AD-tenant uitnodigen. De klant toegewezen toegangsbeheer en inrichting voor alle gebruikers, maar 50 gebruikers ook vereisen dat MFA en voorwaardelijke toegang. De klant moet aanschaffen 10 licenties voor Azure AD Basic en 10 Azure AD Premium P1-licenties deze B2B-gebruikers correct. Als de klant wil Identity Protection-functies gebruiken met B2B-gebruikers, moeten ze Azure AD Premium P2-licenties de uitgenodigde gebruikers in dezelfde 5:1 verhouding hebben.
- Een klant heeft 10 werknemers die alle momenteel met Azure AD Premium P1 worden gelicentieerd. Ze willen nu uitnodigen 60 B2B-gebruikers die alle multi-factor authentication (MFA) vereist. De klant moet ten minste 12 Azure AD Premium P1-licenties alle 60 gebruikers van B2B-samenwerking hebben onder de licentieverlening regel 5:1. Omdat ze al 10 Premium P1-licenties voor hun 10 werknemers hebben, maar ze beschikken over rechten om uit te nodigen 50 B2B-gebruikers met Premium P1-functies zoals MFA. In dit voorbeeld moeten ze vervolgens 2 extra Premium P1-licenties aan zodat de resterende 10 B2B-samenwerking gebruikers aanschaffen.

> [!NOTE]
> Er is geen manier nog het toewijzen van licenties rechtstreeks naar de B2B-gebruikers om in te schakelen van de gebruikersrechten van deze B2B-samenwerking.

De klant die eigenaar is van de uitnodigende tenant moet zijn om te bepalen hoeveel B2B-samenwerking gebruikers moeten betaalde Azure AD-mogelijkheden. Afhankelijk van welke betaalde Azure AD-functies die u wilt gebruiken voor uw gastgebruikers ook kunnen gebruiken, hebt u genoeg Azure AD betaalde licenties voor gebruikers van B2B-samenwerking in de verhouding tussen de 5:1. 

## <a name="additional-licensing-details"></a>Aanvullende licenties details
- Er is niet nodig daadwerkelijk licenties toewijzen aan gebruikersaccounts B2B. Op basis van de 5:1 verhouding, is-licentieverlening automatisch berekend en gerapporteerd.
- Indien Nee betaalde Azure AD-licentie in de tenant bestaat elke uitgenodigde gebruiker beschikt over de rechten die de Azure AD Free edition biedt.
- Als een samenwerking gebruiker al een betaalde Azure AD heeft B2B licentie van hun organisatie, meetellen die niet als een van de licenties voor B2B-samenwerking van de uitnodigende tenant.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Geavanceerde discussie: wat zijn de licenties wanneer we gebruikers van een organisatie conglomeraat als 'leden toevoegen' met behulp van uw API's?
Een B2B-gastgebruiker is die van een partnerorganisatie wordt uitgenodigd om te werken met de organisatie van de host. Normaal gesproken alle andere gevallen komt niet in aanmerking als B2B nog gebruikt B2B-functies. Laten we kijken twee aanvragen in het bijzonder:

1. Als een host met behulp van een consumer-adres van een werknemer uitnodigingen
  * In dit scenario is niet compatibel zijn met onze licentiebeleid en wordt niet aanbevolen.

2. Als een organisatie host een gebruiker in een andere conglomeraat organisatie wilt toevoegen
  1. In dit geval de gebruiker is uitgenodigd met B2B-API's, maar deze aanvraag kan niet vroeger B2B. In het ideale geval hebben we deze organisaties de andere organisaties gebruikers uitnodigen als leden (onze API kunt die). In dit geval moeten licenties worden toegewezen aan deze leden voor deze toegang krijgen tot bronnen in de organisatie.

  2. Sommige organisaties willen gebruikers de andere organisatie moeten worden toegevoegd als 'Gast' als een beleid toevoegen. Er zijn hier twee mogelijke situaties:
      * De conglomeraat organisatie maakt al gebruik van Azure AD en de uitgenodigde gebruikers in de andere organisatie zijn gelicentieerd: in dit geval wordt niet verwacht dat uitgenodigde gebruikers hoeft uit te voeren van de lay-out van eerder in dit document 1:5-formule. 

      * De conglomeraat organisatie maakt geen gebruik van Azure AD of beschikt niet over voldoende licenties: In dit geval, volgt u de lay-out van eerder in dit document 1:5-formule.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](faq.md)
