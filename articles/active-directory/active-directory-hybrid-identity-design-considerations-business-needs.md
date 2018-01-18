---
title: Identiteitsvereisten voor hybride voor een cloud identity Azure | Microsoft Docs
description: "Identificeer de bedrijfsbehoeften van die u voor het definiëren van de vereisten voor het ontwerp van hybride identiteit leiden."
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 90c65ca22ff384bd22819eb589b3f781f8992a4e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Identiteitsvereisten bepalen voor uw oplossing voor hybride identiteit
De eerste stap bij het ontwerpen van een hybride identiteitsoplossing is het vaststellen van de vereisten voor de organisatie van de bedrijven die zal worden gebruik van deze oplossing.  Hybride identiteit wordt gestart als een ondersteunende rol (ondersteund alle cloudoplossingen aan de hand van verificatie) en wordt op om te zorgen dat er nieuwe en interessante mogelijkheden die nieuwe werkbelastingen voor gebruikers ontgrendelen.  Deze werkbelastingen of services die u wilt vaststellen voor uw gebruikers worden de vereisten voor het ontwerp van hybride identiteit bepaald.  Deze werkbelastingen en services moeten gebruikmaken van hybride identiteit zowel on-premises en in de cloud.  

U moet deze belangrijke aspecten van het bedrijf te begrijpen wat gaan nu een vereiste is en wat het bedrijf de toekomstplannen. Als u de zichtbaarheid van de lange termijnstrategie voor het ontwerp van hybride identiteit niet hebt, wordt er waarschijnlijk hebben dat uw oplossing zal niet worden geschaald behoeften groei en veranderingen van het bedrijf.   T hij diagram hieronder bevat een voorbeeld van een architectuur van hybride identiteit en de werkbelastingen die zijn voor gebruikers wordt ontgrendeld. Dit is slechts een voorbeeld van de nieuwe mogelijkheden die kunnen worden ontgrendeld en geleverd met een strategie voor een solide hybride identiteit. 

Sommige onderdelen die deel van de architectuur van hybride identiteit uitmaken![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Bedrijfsbehoeften vaststellen
Elk bedrijf kent verschillende behoeften, zelfs als deze bedrijven deel uitmaken van dezelfde bedrijfstak, de echte zakelijke behoeften verschillen. U kunt nog steeds gebruikmaken van best practices uit de industrie, maar is uiteindelijk de bedrijfsbehoeften van die u voor het definiëren van de vereisten voor het ontwerp van hybride identiteit leiden. 

Zorg ervoor dat de volgende vragen om de bedrijfsbehoeften vaststellen beantwoorden:

* Is uw bedrijf op zoek naar de operationele kosten IT?
* Is uw bedrijf willen beveiligen cloud (SaaS-apps, infrastructuur)?
* Is uw bedrijf op zoek naar uw IT-afdeling moderniseren?
  * Uw gebruikers zijn mobieler en meer veeleisende IT uitzonderingen in uw Perimeternetwerk om toe te staan verschillende type verkeer voor toegang tot verschillende resources maken?
  * Heeft uw bedrijf oudere apps die worden gepubliceerd voor deze gebruikers moderne nodig maar die niet eenvoudig moeten worden herschreven?
  * Heeft uw bedrijf behoefte aan al deze taken uitvoeren en brengt u het onder controle op hetzelfde moment?
* Is uw bedrijf op zoek voor het beveiligen van de identiteit van gebruikers en risico's te beperken door nieuwe hulpprogramma's die gebruikmaken van de kennis van de Microsoft Azure-beveiliging expertise lokale te brengen?
* Is uw bedrijf probeert te verwijderen van de gevreesde 'extern' accounts on-premises en verplaats deze naar de cloud waar ze niet langer een inactieve bedreiging binnen uw on-premises omgeving zijn?

## <a name="analyze-on-premises-identity-infrastructure"></a>On-premises identity infrastructuur analyseren
Nu u een idee hebt met betrekking tot de zakelijke vereisten van uw bedrijf, moet u uw on-premises identity-infrastructuur te evalueren. Deze evaluatie is belangrijk voor het definiëren van de technische vereisten voor het integreren van uw huidige identiteitsoplossing naar de cloud identity management-systeem. Zorg ervoor dat de volgende vragen beantwoorden:

* Welke oplossing voor verificatie en autorisatie biedt uw bedrijf gebruik van lokale? 
* Heeft uw bedrijf momenteel synchronisatie van lokale services?
* Maakt uw bedrijf gebruik van een derde partij-ID-Providers (IdP)?

U moet ook rekening houden met de cloudservices die uw bedrijf kan hebben. Uitvoeren van een beoordeling voor inzicht in de huidige integratie met SaaS, IaaS of PaaS-modellen in uw omgeving is heel belangrijk. Zorg ervoor dat de volgende vragen beantwoorden tijdens deze evaluatie:

* Heeft uw bedrijf geïntegreerd met een cloudserviceprovider?
* Zo ja, welke services worden gebruikt?
* Deze integratie momenteel is in productie of is het een test?

> [!NOTE]
> Als u geen een nauwkeurige toewijzing van al uw apps hebben en cloud services, kunt u het hulpprogramma voor Cloud App Discovery. Dit hulpprogramma kunt u uw IT-afdeling met zichtbaarheid van het bedrijf van uw organisatie en consumenten cloud-apps opgeven. Daardoor is het eenvoudiger dan ooit om niet-officiële IT binnen uw organisatie te ontdekken, en gegevens te zien zoals onder andere over gebruikspatronen en over welke gebruikers toegang hebben gehad tot uw cloudtoepassingen. Gestarte Zie ophalen [Cloud app discovery](active-directory-cloudappdiscovery-whatis.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Identity Integratievereisten evalueren
Vervolgens moet u de identiteit Integratievereisten evalueren. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor hoe gebruikers worden geverifieerd, hoe de aanwezigheid van de organisatie ziet er in de cloud, hoe de organisatie autorisatie wordt toegestaan en wat de gebruikerservaring is het verstandig om. Zorg ervoor dat de volgende vragen beantwoorden:

* Uw organisatie gebruikt federation-, standaard-verificatie of beide?
* Een vereiste voor Federatie is?  Vanwege de volgende:
  * Eenmalige aanmelding op basis van Kerberos
  * Uw bedrijf heeft een on-premises toepassingen (ofwel intern of 3e party gemaakt) die gebruikmaakt van SAML of vergelijkbare federation-mogelijkheden.
  * MFA via smartcards. RSA SecurID, enzovoort.
  * Toegangsregels voor clients die het adres van de onderstaande vragen:
    1. Kan ik alle externe toegang tot Office 365 op basis van het IP-adres van de client blokkeren?
    2. Kan ik alle externe toegang tot Office 365, met uitzondering van Exchange ActiveSync blokkeren?
    3. Ik kan alle externe toegang tot Office 365, met uitzondering van de browser gebaseerde apps (OWA, gesimuleerde Productieorder) blokkeren
    4. Ik kan alle externe toegang tot Office 365 voor leden van de aangewezen AD-groepen blokkeren
* Beveiligingscontrole/problemen
* Al bestaande investeringen in federatieve verificatie
* Welke naam onze organisatie gebruikt voor dit domein in de cloud?
* Beschikt over de organisatie een aangepast domein?
  1. Is dat domein openbare en eenvoudig controleerbare via DNS?
  2. Als dit niet het geval is, klikt u vervolgens hebt u een openbaar domein die kunnen worden gebruikt voor het registreren van een alternatieve UPN in AD?
* Zijn de gebruikers-id's overeenkomen voor weergave van de cloud? 
* Beschikt over de organisatie-apps die integratie met cloudservices vereist?
* De organisatie beschikt over meerdere domeinen en alle standard of federatieve verificatie gebruikt?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Toepassingen die worden uitgevoerd in uw omgeving te evalueren
Nu u een idee met betrekking tot uw on-premises hebt en cloud-infrastructuur, moet u de toepassingen die worden uitgevoerd in een dergelijke omgeving te evalueren. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het integreren van deze toepassingen naar het beheersysteem voor cloud-identiteit. Zorg ervoor dat de volgende vragen beantwoorden:

* Waar wordt onze toepassingen woont?
* Krijgen gebruikers toegang tot on-premises toepassingen?  In de cloud? Of beide?
* Zijn er plannen moet worden overgenomen van de bestaande toepassing werklast en verplaatst u ze naar de cloud?
* Zijn er plannen voor het ontwikkelen van nieuwe toepassingen die zich on-premises of cloud in de cloud die wordt gebruikt verificatie?

## <a name="evaluate-user-requirements"></a>Gebruikersvereisten evalueren
Hebt u ook evalueren van de vereisten van de gebruiker. Deze evaluatie is belangrijk om te definiëren van de stappen die nodig zijn voor voor de twee locaties en gebruikers te helpen als ze worden overgezet naar de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

* Krijgen gebruikers toegang tot toepassingen lokale?
* Krijgen gebruikers toegang tot toepassingen in de cloud?
* Hoe gebruikers doorgaans kan aanmelden bij hun on-premises omgeving?
* Hoe wordt gebruikers aanmelden in de cloud?

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Vereisten voor respons op incidenten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties en -professionals/nadelen van elke optie.  Moet door deze vragen die u selecteert welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

