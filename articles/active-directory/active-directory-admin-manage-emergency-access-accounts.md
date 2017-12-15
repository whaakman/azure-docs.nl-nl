---
title: Het beheren van accounts voor Noodtoegang administratieve in Azure AD | Microsoft Docs
description: Beschrijft het gebruik van accounts voor Noodtoegang waarmee organisaties bevoorrechte toegang in een bestaande Azure Active Directory-omgeving te beperken.
services: active-directory
keywords: Voeg geen of trefwoorden zonder overleg met uw EXPERT in Zoekmachineoptimalisatie bewerken.
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 75ea8e445c890e7af5ab14f4fc4c53cfb1af4568
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="managing-emergency-access-administrative-accounts-in-azure-ad"></a>Beheerdersaccounts Noodtoegang beheren in Azure AD 

Voor de meeste dagelijkse activiteiten, zijn er geen globale beheerdersrol rechten nodig.  Gebruikers moeten niet permanent worden toegewezen aan de rol, omdat de gebruiker per ongeluk voor een taak met hogere machtigingen uitvoeren kan dan vereist. Wanneer een gebruiker nodig heeft om te fungeren als een globale beheerder, moeten ze de toewijzing van rol met behulp van Azure AD PIM op hun eigen account of een alternatieve Administrator-account te activeren.

Naast de gebruikers duurt van Administrator-rechten voor zichzelf, moeten klanten voorbereiden om ervoor te zorgen dat ze geen ophalen in een situatie waarin deze kunnen worden per ongeluk vergrendeld buiten het beheer van hun Azure AD-tenant vanwege hun onvermogen om te ondertekenen in of het account als een beheerder een bestaande afzonderlijke gebruiker activeren.  Ze kunnen het risico van de impact van onbedoeld gebrek aan beheerderstoegang door middel van opslaan in twee of meer van de tenant *accounts voor Noodtoegang*.

Accounts voor Noodtoegang waarmee organisaties bevoorrechte toegang in een bestaande Azure Active Directory-omgeving te beperken. Deze accounts over uitgebreide beheerdersmogelijkheden en zijn niet toegewezen aan specifieke personen. Accounts voor Noodtoegang zijn beperkt tot noodgevallen voor 'break glas'-scenario's waarin de normale beheerdersaccounts kunnen niet worden gebruikt.  Organisaties moet het doel van het beheren en informatie over het gebruik van de noodgevallen account beperken tot alleen die tijd die nodig is.

Scenario's waarin een organisatie moet maken van een Noodtoegang-account gebruiken:

 - De gebruikersaccounts zijn federatieve en federation is niet beschikbaar als gevolg van een netwerkverbinding wordt verbroken of een storing optreedt van de id-provider.  Bijvoorbeeld als de host van de provider identiteit in de omgeving van de klant is niet meer beschikbaar, kan klikt u vervolgens een gebruiker niet worden kunnen zich aanmelden bij Azure AD wordt omgeleid naar de id-provider. 
 - De beheerders worden geregistreerd via MFA en al hun afzonderlijke apparaten zijn niet beschikbaar.  Het is mogelijk dat gebruikers kan niet worden voltooid multi-factor authentication aan een rol activeren als bijvoorbeeld een netwerkstoring cel kan voorkomen dat gebruikers kunnen beantwoorden telefoongesprekken of SMS-berichten ontvangen en die het enige mechanisme is zijn dat zij geregistreerd voor hun apparaat. 
 - De laatste persoon die globale beheerderstoegang heeft de organisatie heeft verlaten.  Azure AD voorkomt u dat de globale beheerdersaccount wordt verwijderd, maar voorkomt niet dat het account wordt verwijderd of uitgeschakeld on-premises, wat leidt tot een situatie waarin de organisatie is niet kan herstellen dat account.

## <a name="initial-configuration"></a>De aanvankelijke configuratie

Twee of meer accounts voor Noodtoegang maken.  Dit moeten alleen in de cloud accounts met behulp van de *. onmicrosoft.com-domein, of zijn niet van federatieve of gesynchroniseerd met een on-premises omgeving.  

Zij mag niet zijn gekoppeld aan een afzonderlijke gebruiker in de organisatie.  Organisaties moet om te controleren of dat de referenties voor deze accounts zijn veilig bewaard en bekende alleen naar personen die gemachtigd zijn ze te gebruiken. 

Opmerking: Meestal het accountwachtwoord voor een account toegang in noodgevallen is onderverdeeld in twee of drie onderdelen, en vervolgens op afzonderlijke stukjes papier geschreven en opgeslagen in veilige, brandveilige brandkasten in beveiligde afzonderlijke locaties. Zorg ervoor dat uw accounts voor Noodtoegang niet zijn verbonden met een werknemer opgegeven mobiele telefoons hardware tokens die worden verzonden met afzonderlijke werknemers of andere referenties werknemer-specifieke geval die afzonderlijke werknemer niet bereikbaar op de tijd is de referentie is vereist. 

### <a name="initial-configuration-with-permanent-assignments"></a>Initiële configuratie met permanente toewijzingen

Een mogelijkheid is om toe te wijzen die gebruikers als permanent lid zijn van de rol globale beheerder.  Dit zou zijn geschikt voor organisaties die geen Azure AD Premium-P2 abonnementen hebben.

Azure AD raadt aan dat u multi-factor authentication (MFA) voor alle afzonderlijke gebruikers, beheerders en andere gebruikers die aanzienlijke gevolgen hebben zou als het account is geknoeid (bijvoorbeeld financieel medewerkers) vereist. Dit vermindert het risico van een aanval als gevolg van een wachtwoord waarmee is geknoeid. Echter, als uw organisatie geen gedeelde apparaten vervolgens MFA niet mogelijk voor deze accounts voor Noodtoegang.  Als u een beleid voor voorwaardelijke toegang om te vereisen configureert [registratieprocedure voor MFA voor elke beheerder](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor Azure AD en andere verbonden SaaS-apps, moet u mogelijk uitsluitingen voor accounts voor Noodtoegang uitsluiten van dit beleid configureren vereiste.

### <a name="initial-configuration-with-approvals"></a>Initiële configuratie met goedkeuringen

Een andere optie is om te configureren die gebruikers als in aanmerking komende en fiatteurs voor het activeren van de rol globale beheerder.  Hiervoor moeten de organisatie Azure AD Premium-P2 abonnementen, evenals een MFA-optie geschikt is voor gedeelde tussen meerdere personen en de netwerkomgeving.  Dit komt doordat de activering van de rol globale beheerder vereist dat gebruikers MFA eerder hebt uitgevoerd.  Meer informatie in [MFA in Azure AD Privileged Identity Management vereisen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Met MFA die zijn gekoppeld aan persoonlijke apparaten worden niet aanbevolen voor accounts voor Noodtoegang, omdat in een werkelijke noodgevallen één persoon moet hebben toegang tot een MFA geregistreerd apparaat mogelijk niet degene die in van het persoonlijke apparaat bezit is.  Denk ook na over de liggend threat als vanwege onvoorziene omstandigheden mobiele telefoon of andere netwerken niet beschikbaar zijn tijdens een natuurramp noodgevallen.  Het is daarom belangrijk om ervoor te zorgen dat alle geregistreerde apparaten in een bekende, beveiligde locatie met meerdere middel om te communiceren met Azure AD worden bewaard.

## <a name="ongoing-monitoring"></a>Continue bewaking

Monitor voor de [Azure AD aanmelden en controlelogboeken](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-sign-ins) voor alle aanmeldingen en controle-activiteit van de accounts toegang in noodgevallen.  Normaal gesproken deze accounts niet moeten worden aangemeld en moeten niet worden u wijzigingen aanbrengt, zodat te gebruiken is het waarschijnlijk afwijkend en beveiliging onderzoek vereisen.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Selectievakje Accountvalidatie moet plaatsvinden met regelmatige tussenpozen

Voer ten minste de volgende stappen uit:
 - Om de 90 dagen
 - Als er is een recente wijziging in de IT-medewerkers: een taak, vertrek of wijzigen nieuwe aanstelling
 - Wanneer de Azure AD-abonnementen in de organisatie zijn gewijzigd

Om ervoor te zorgen personeel zijn opgeleid voor het gebruik van de accounts voor Noodtoegang:

1.  Zorg ervoor dat beveiliging bewaking personeel zijn op de hoogte dat de account controle-activiteit uitgevoerd wordt.
2.  Valideren dat de gebruikersaccounts van de cloud kunnen aanmelden en hun rollen activeren en dat gebruikers die moet mogelijk u deze stappen uitvoert in een noodsituatie van het proces worden getraind.
3.  Zorg ervoor dat ze niet zijn geregistreerd voor MFA of SSPR op het apparaat of de persoonlijke gegevens van een afzonderlijke gebruiker.  
4. Als de accounts zijn geregistreerd voor MFA op een apparaat, voor gebruik tijdens de rolactivering, zorg ervoor dat het apparaat is toegankelijk voor alle beheerders die mogelijk nodig hebt om in een noodsituatie.  Controleer ook of het apparaat is geregistreerd via ten minste twee mechanismen die geen van een algemene fout-modus delen (bijv, het apparaat kan communiceren met Internet via een faciliteit draadloze netwerk, evenals ook via een netwerk van de provider cel).
5.  De referenties bijwerken.

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen van een gebruiker cloud-gebaseerde](add-users-azure-active-directory.md) en [de nieuwe gebruiker toewijzen aan de rol globale beheerder](active-directory-users-assign-role-azure-portal.md)
- [Aanmelden voor Azure Active Directory Premium](active-directory-get-started-premium.md), als u dat nog niet gedaan hebt
- [Azure MFA vereisen voor afzonderlijke gebruikers toegewezen als beheerders](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states)
- [Aanvullende bescherming voor globale beheerders configureren in Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), als u van Office 365 gebruikmaakt
- [Uitvoeren van een beoordeling van de toegang van globale beheerders](active-directory-privileged-identity-management-how-to-start-security-review.md) en [overgang van bestaande globale beheerders meer specifieke beheerdersrollen](active-directory-assign-admin-roles-azure-portal.md)

