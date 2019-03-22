---
title: Beheerdersaccounts voor Noodtoegang - Azure Active Directory beheren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u accounts voor toegang in noodgevallen gebruiken om te voorkomen dat uw tenant Azure Active Directory (Azure AD) per ongeluk wordt vergrendeld.
services: active-directory
author: markwahl-msft
ms.author: curtand
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f430a832ffb35b95d0bf4eff2d82be5ecc3d865c
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224937"
---
# <a name="manage-emergency-access-accounts-in-azure-ad"></a>Accounts voor Noodtoegang beheren in Azure AD

Het is belangrijk dat u voorkomen per ongeluk buiten uw tenant Azure Active Directory (Azure AD dat) wordt vergrendeld omdat u niet aanmelden of activeren van een bestaande afzonderlijke gebruiker-account als beheerder. U kunt de gevolgen van het onbedoeld gebrek aan administratieve toegang beperken met het maken van twee of meer *accounts voor toegang in noodgevallen* in uw tenant.

Accounts voor toegang in noodgevallen over uitgebreide beheerdersmogelijkheden en ze niet zijn toegewezen aan specifieke personen. Accounts voor toegang in noodgevallen zijn beperkt tot EMS of "verbreken om" scenario's waarbij normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moeten een doel van het gebruik van de noodgevallen account beperken tot alleen de tijden wanneer dit absoluut noodzakelijk is behouden.

In dit artikel bevat richtlijnen voor het beheren van accounts voor toegang in noodgevallen in Azure AD.

## <a name="when-would-you-use-an-emergency-access-account"></a>Wanneer moet u een account voor toegang in noodgevallen gebruiken?

Een organisatie moet mogelijk gebruik van een account voor toegang in noodgevallen in de volgende situaties:

- De gebruikersaccounts federatief zijn en federation is momenteel niet beschikbaar vanwege een cel-netwerkverbinding wordt verbroken of een storing in de id-provider. Bijvoorbeeld, als de id-provider host in uw omgeving is uitgevallen, gebruikers mogelijk niet aanmelden bij Azure AD wordt omgeleid naar de id-provider.
- De groep administrators zijn geregistreerd via de Azure multi-factor Authentication en hun afzonderlijke apparaten zijn niet beschikbaar of de service is niet beschikbaar. Het is mogelijk dat gebruikers kan niet worden voltooid multi-factor Authentication voor het activeren van een rol. Bijvoorbeeld, een netwerkstoring cel voorkomt dat ze uit het beantwoorden van telefoongesprekken of SMS-berichten, de enige twee verificatiemechanismen die ze geregistreerd voor hun apparaat ontvangen.
- De persoon die met de meest recente toegang van de globale beheerder van heeft de organisatie verlaten. Azure AD voorkomt dat de laatste Global Administrator-account wordt verwijderd, maar voorkomt niet dat het account wordt verwijderd of uitgeschakeld on-premises. De situatie kunt maken van de organisatie kan niet worden hersteld van het account.
- Onvoorziene omstandigheden, zoals een natuurramp noodgevallen, gedurende welke een mobiele telefoon of andere netwerken zijn mogelijk niet beschikbaar. 

## <a name="create-two-cloud-based-emergency-access-accounts"></a>Maak twee accounts van cloud-gebaseerde toegang in noodgevallen

Maak twee of meer accounts voor toegang in noodgevallen. Deze accounts moeten alleen in de cloud-accounts die gebruikmaken van de \*..onmicrosoft.com-domein en die zijn niet federatief of gesynchroniseerd met een on-premises omgeving.

Bij het configureren van deze accounts, moeten de volgende vereisten worden voldaan:

- De accounts voor toegang in noodgevallen mag niet zijn gekoppeld aan een afzonderlijke gebruiker in de organisatie. Zorg ervoor dat uw accounts niet zijn verbonden met een mobiele werknemer opgegeven telefoons, hardware-tokens die worden verzonden met afzonderlijke werknemers, of andere werknemer-specifieke-referenties. Deze voorzorgsmaatregel heeft betrekking op exemplaren wanneer een individuele werknemer onbereikbaar is als de referentie op die nodig is. Het is belangrijk om ervoor te zorgen dat alle geregistreerde apparaten moeten worden bewaard in een bekende, veilige locatie met meerdere middel om te communiceren met Azure AD.
- Het verificatiemechanisme gebruikt voor een account voor toegang in noodgevallen moet anders zijn dan die door uw andere administratieve accounts, met inbegrip van andere accounts voor toegang in noodgevallen gebruikt.  Bijvoorbeeld, als uw normale beheerder aanmelden via on-premises MFA, is klikt u vervolgens Azure MFA een ander mechanisme.  Maar als de Azure MFA uw primaire deel uitmaakt van verificatie voor uw administratieve accounts, overweeg dan om een andere benadering voor deze opties, zoals het gebruik van voorwaardelijke toegang met een MFA-provider van derden.
- Het apparaat of de referentie moet niet verlopen of worden binnen het bereik van geautomatiseerde opschoning vanwege gebrek aan gebruiken.  
- U moet de toewijzing van de rol globale beheerder permanent maken voor uw accounts voor toegang in noodgevallen. 


### <a name="exclude-at-least-one-account-from-phone-based-multi-factor-authentication"></a>Ten minste één account uitsluiten van meervoudige verificatie op basis van een telefoon

Om het risico van een aanval die voortvloeien uit een wachtwoord waarmee is geknoeid, Azure AD wordt aanbevolen dat u meervoudige verificatie voor alle afzonderlijke gebruikers vereisen. Deze groep bevat beheerders en alle andere (bijvoorbeeld financiële officers) waarvan het account waarmee is geknoeid aanzienlijke gevolgen zou hebben.

Ten minste een van uw accounts voor toegang in noodgevallen moet echter niet hetzelfde MFA-mechanisme als uw andere niet-EMS-accounts hebben. Dit omvat de oplossingen van derden met meervoudige verificatie. Hebt u een beleid voor voorwaardelijke toegang om te vereisen [multi-factor authentication voor iedere beheerder](../authentication/howto-mfa-userstates.md) voor Azure AD en andere verbonden software as a service (SaaS)-apps, moet u accounts voor toegang in noodgevallen uitsluiten van dit vereiste, en een ander mechanisme in plaats daarvan configureren. Bovendien moet u controleren of dat de accounts ondersteunen geen een beleid per gebruiker meervoudige verificatie.

### <a name="exclude-at-least-one-account-from-conditional-access-policies"></a>Ten minste één account uitsluiten van beleidsregels voor voorwaardelijke toegang

In een noodsituatie wilt u niet dat een beleid mogelijk uw om toegang te blokkeren om een probleem te verhelpen. Toegang in noodgevallen ten minste één account moet worden uitgesloten van alle beleidsregels voor voorwaardelijke toegang. Als u hebt ingeschakeld een [Basisbeleid](../conditional-access/baseline-protection.md), moet u uw accounts voor toegang in noodgevallen uitsluiten.

## <a name="additional-guidance-for-hybrid-customers"></a>Aanvullende richtlijnen voor hybride klanten

Een extra optie voor organisaties die gebruikmaken van AD Domain Services en ADFS of soortgelijke id-provider om te federeren naar Azure AD, is het configureren van een account voor toegang in noodgevallen is waarvan MFA-claim door deze id-provider kan worden opgegeven.  Het account voor toegang in noodgevallen kan bijvoorbeeld worden ondersteund door een certificaat en het sleutelpaar, zoals een die zijn opgeslagen op een smartcard.  Wanneer de gebruiker is geverifieerd naar AD, kunnen AD FS een claim naar Azure AD die aangeven dat MFA-vereisten is voldaan aan de gebruiker opgeven.  Zelfs met deze methode moeten organisaties nog steeds toegang in noodgevallen cloudgebaseerde accounts hebben in het geval Federatie kan niet tot stand worden gebracht. 

## <a name="store-devices-and-credentials-in-a-safe-location"></a>Store-apparaten en referenties op een veilige locatie

Organisaties moeten om ervoor te zorgen dat de referenties voor toegang in noodgevallen geldt zijn veilig bewaard en bekende alleen naar personen die gemachtigd zijn om ze te gebruiken. Sommige klanten gebruikmaken van een smartcard en andere wachtwoorden gebruiken. Een wachtwoord voor een account voor toegang in noodgevallen is meestal verdeeld in twee of drie delen, op afzonderlijke stukjes papier geschreven en opgeslagen in de veilige, brandveilige brandkasten die zich in afzonderlijke, veilige locaties.

Als wachtwoorden, zorg er dan voor dat de accounts zijn sterke wachtwoorden dat niet het wachtwoord is verlopen. In het ideale geval moet de wachtwoorden die ten minste 16 tekens lang en een willekeurig gegenereerd.


## <a name="monitor-sign-in-and-audit-logs"></a>Aanmelden controleren en auditlogboeken

Monitor de [aanmelding bij Azure AD en audit logboeken](../reports-monitoring/concept-sign-ins.md) voor alle aanmeldingen en de controle-activiteit uit de accounts voor toegang in noodgevallen. Normaal gesproken moeten deze accounts niet moeten worden aangemeld en niet worden wijzigingen, zodat het gebruik van deze waarschijnlijk afwijkende en beveiligingsonderzoek zijn vereist.

## <a name="validate-accounts-at-regular-intervals"></a>Accounts met regelmatige tussenpozen valideren

Als u wilt trainen medewerkers toegang in noodgevallen accounts gebruiken en valideren van de accounts voor toegang in noodgevallen, de volgende handelingen minimale met regelmatige tussenpozen:

- Zorg ervoor dat medewerkers beveiligingsbewaking Houd er rekening mee dat de controle activiteit wordt momenteel is.
- Zorg ervoor dat het noodgevallen break glas proces voor het gebruik van deze accounts gedocumenteerde en actueel is.
- Zorg ervoor dat beheerders en security officers die moet mogelijk u deze stappen uitvoert tijdens een noodgeval zijn getraind op het proces.
- Werk de referenties van het account in het bijzonder wachtwoorden voor uw accounts voor toegang in noodgevallen, en vervolgens te valideren dat de accounts voor toegang in noodgevallen kunnen aanmelden en administratieve taken uitvoeren.
- Zorg ervoor dat gebruikers niet hebt geregistreerd, multi-factor Authentication of selfservice wachtwoordherstel (SSPR) op het apparaat of de persoonlijke gegevens van elke afzonderlijke gebruiker. 
- Als de accounts zijn geregistreerd voor multi-factor Authentication aan een apparaat, voor gebruik tijdens de rol- of activering, zorgt u ervoor dat het apparaat is toegankelijk voor alle beheerders die mogelijk deze functie wilt gebruiken in een noodsituatie. Controleer ook of dat het apparaat kan communiceren via ten minste twee netwerkpaden die geen een algemene foutmodus delen. Bijvoorbeeld, kan het apparaat communiceren met internet via een gebouw van een draadloos netwerk en een cel provider-netwerk.

Deze stappen moeten worden uitgevoerd met regelmatige tussenpozen en belangrijke wijzigingen:

- Ten minste elke 90 dagen
- Wanneer er een recente wijziging in de IT-personeel, zoals een wijziging van de taak, een vertrek of een nieuwe medewerkers is
- Wanneer de Azure AD-abonnementen in de organisatie zijn gewijzigd

## <a name="next-steps"></a>Volgende stappen

- [Bevoegde toegang beveiligen voor hybride implementaties en cloudimplementaties in Azure AD](directory-admin-roles-secure.md)
- [Gebruikers met behulp van Azure AD toevoegen](../fundamentals/add-users-azure-active-directory.md) en [de nieuwe gebruiker toewijzen aan de rol globale beheerder](../fundamentals/active-directory-users-assign-role-azure-portal.md)
- [Aanmelden voor Azure AD Premium](../fundamentals/active-directory-get-started-premium.md), als u al dat nog niet hebt aangemeld
- [Hoe u verificatie in twee stappen vereist voor een gebruiker](../authentication/howto-mfa-userstates.md)
- [Configureer aanvullende bescherming voor globale beheerders in Office 365](https://docs.microsoft.com/office365/enterprise/protect-your-global-administrator-accounts), als u Office 365
- [Een toegangscontrole van globale beheerders starten](../privileged-identity-management/pim-how-to-start-security-review.md) en [overgang van bestaande algemene beheerders meer specifieke beheerdersrollen](directory-assign-admin-roles.md)
