---
title: Beheerdersaccounts voor EMS-toegang beheren in Azure AD | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met accounts voor Noodtoegang waarmee organisaties bevoorrechte toegang in een bestaande Azure Active Directory-omgeving te beperken.
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
ms.openlocfilehash: 1545fb9a89794a74efbb855c4480040973c3308e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Beheerdersaccounts voor EMS-toegang beheren in Azure AD 

Voor de meeste dagelijkse activiteiten, *hoofdbeheerder* rechten niet nodig zijn voor uw gebruikers. Gebruikers moeten niet permanent worden toegewezen aan de rol, omdat ze mogelijk per ongeluk een taak waarvoor hogere machtigingen dan ze moeten uitvoeren. Wanneer gebruikers hoeven niet te fungeren als een globale beheerder is, moeten ze de roltoewijzing activeren met behulp van Azure Active Directory (Azure AD) Privileged Identity Management (PIM), op hun eigen account of een alternatieve Administrator-account.

Naast de gebruikers te nemen van Administrator-rechten voor zichzelf, moet u voorkomen dat per ongeluk buiten het beheer van uw Azure AD-tenant wordt vergrendeld omdat u kunt niet aanmelden of activeren van een bestaande afzonderlijke gebruiker account als een beheerder. U kunt de impact van onbedoelde gebrek aan beheerderstoegang verhelpen door het opslaan van twee of meer *accounts voor Noodtoegang* in uw tenant.

Accounts voor Noodtoegang kunt organisaties bevoorrechte toegang in een bestaande Azure Active Directory-omgeving te beperken. Dergelijke accounts over uitgebreide beheerdersmogelijkheden en ze niet zijn toegewezen aan specifieke personen. Accounts voor Noodtoegang zijn beperkt tot emergency of 'break glas' scenario's, situaties waarin de normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moeten een doel van het gebruik van de noodgevallen account beperken tot alleen die tijd gedurende welke hoeft te onderhouden.

Een organisatie moet mogelijk een Noodtoegang gebruikt in de volgende situaties:

 - De gebruikersaccounts zijn federatieve en federation is momenteel niet beschikbaar vanwege een cel netwerkverbinding wordt verbroken of een storing id-provider. Bijvoorbeeld, als de id-provider host in uw omgeving is niet meer beschikbaar, gebruikers mogelijk niet aanmelden bij Azure AD wordt omgeleid naar de id-provider. 
 - De beheerders worden geregistreerd via de Azure multi-factor Authentication en hun afzonderlijke apparaten zijn niet beschikbaar. Gebruikers kunnen mogelijk kan niet worden voltooid multi-factor Authentication voor het activeren van een rol. Bijvoorbeeld houdt een netwerkstoring cel worden ze tegen het beantwoorden van telefoongesprekken of ontvangen van tekstberichten, de slechts twee verificatiemechanismen die ze geregistreerd voor het apparaat. 
 - De persoon met de meest recente globale beheerderstoegang tot heeft de organisatie verlaten. Azure AD wordt voorkomen dat de laatste *hoofdbeheerder* account wordt verwijderd, maar voorkomt niet dat het account wordt verwijderd of uitgeschakeld on-premises. Beide gevallen kan de organisatie die niet kan herstellen van het account te maken.

## <a name="initial-configuration"></a>De aanvankelijke configuratie

Twee of meer accounts voor Noodtoegang maken. Dit moeten alleen in de cloud-mailaccounts die gebruikmaken van de \*. onmicrosoft.com-domein en die zijn niet federatieve of gesynchroniseerd met een on-premises omgeving. 

De accounts mag niet zijn gekoppeld aan een afzonderlijke gebruiker in de organisatie. Organisaties moeten om ervoor te zorgen dat de referenties voor deze accounts zijn veilig bewaard en bekende alleen naar personen die gemachtigd zijn ze te gebruiken. 

> [!NOTE]
> Het wachtwoord van een account voor een account Noodtoegang is meestal verdeeld in twee of drie delen, op afzonderlijke stukjes papier geschreven en opgeslagen in de veilige, brandveilige brandkasten in afzonderlijke, veilige locaties. 
>
> Zorg ervoor dat uw accounts voor Noodtoegang niet zijn verbonden met een werknemer opgegeven mobiele telefoons, hardware-tokens die worden verzonden met afzonderlijke werknemers of andere referenties speciaal voor werknemers. Deze voorzorgsmaatregel bevat informatie over exemplaren waar een afzonderlijke werknemer is onbereikbaar wanneer de referentie nodig is. 

### <a name="initial-configuration-with-permanent-assignments"></a>Initiële configuratie met permanente toewijzingen

Een mogelijkheid is zodat gebruikers permanent lid zijn van de *hoofdbeheerder* rol. Deze optie is geschikt voor organisaties die u geen Azure AD Premium P2-abonnementen hebt.

Om het risico van een aanval ten gevolge van een wachtwoord waarmee is geknoeid, Azure AD raadt aan dat u multi-factor Authentication voor alle afzonderlijke gebruikers vereisen. Deze groep moet zijn voor administrators en alle andere (bijvoorbeeld financieel medewerkers) waarvan het account waarmee is geknoeid aanzienlijke gevolgen zou hebben. 

Echter als uw organisatie geen gedeelde apparaten is multi-factor Authentication niet mogelijk deze accounts voor Noodtoegang. Als u een beleid voor voorwaardelijke toegang om te vereisen configureert [multi-factor Authentication-registratie voor elke beheerder](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor Azure AD en andere verbonden software als een service (SaaS)-apps, moet u mogelijk beleid configureren uitsluitingen accounts voor Noodtoegang uitsluiten van deze vereiste.

### <a name="initial-configuration-with-approvals"></a>Initiële configuratie met goedkeuringen

Een andere optie is uw gebruikers configureren als in aanmerking komende en fiatteurs voor het activeren van de *hoofdbeheerder* rol. Deze optie vereist voor uw organisatie Azure AD Premium-P2 abonnementen hebt. Het moet ook een multi-Factor Authentication-optie die geschikt is voor gedeelde gebruik van meerdere personen en de netwerkomgeving. Deze vereisten zijn omdat activering van de *hoofdbeheerder* rol moet gebruikers multi-factor Authentication eerder hebt uitgevoerd. Zie voor meer informatie [vereisen meervoudige verificatie in Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

We raden niet met multi-factor Authentication die is gekoppeld aan persoonlijke apparaten voor accounts voor Noodtoegang. In een noodsituatie werkelijke, de persoon die toegang tot een multi-Factor Authentication geregistreerd apparaat nodig heeft mogelijk geen degene die het persoonlijke apparaat. 

U kunt ook de mate van gevaar. Bijvoorbeeld, ontstaan een onvoorziene omstandigheid zoals een natuurramp noodgevallen, waarin een mobiele telefoon of andere netwerken mogelijk niet beschikbaar. Het is belangrijk om ervoor te zorgen dat alle geregistreerde apparaten in een bekende, beveiligde locatie met meerdere middel om te communiceren met Azure AD worden bewaard.

## <a name="ongoing-monitoring"></a>Continue bewaking

Monitor voor de [aanmelden van Azure AD en audit logboeken](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) voor alle aanmeldingen en controle-activiteit van de EMS-toegangsaccounts. Normaal gesproken deze accounts niet moeten worden aangemeld en moeten niet worden u wijzigingen aanbrengt, zodat te gebruiken is het waarschijnlijk afwijkend en beveiliging onderzoek vereisen.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Validatie van controle van de account moet plaatsvinden met regelmatige tussenpozen

Voer de volgende stappen uit ten minste voor het valideren van het account:
- Om de 90 dagen.
- Als er een recente wijziging in de IT-personeel, zoals een wijziging in de taak, een afwijking of een nieuw aangenomen werknemer.
- Wanneer de Azure AD-abonnementen in de organisatie zijn gewijzigd.

Training van medewerkers accounts voor Noodtoegang gebruiken, kunt u het volgende:

* Zorg ervoor dat beveiligingsbewaking personeel zich bewust zijn dat de account-controle-activiteit uitgevoerd wordt.
* Valideren dat de gebruikersaccounts van de cloud kunnen aanmelden en hun rollen activeren en dat gebruikers die moet mogelijk u deze stappen uitvoert in een noodsituatie van het proces worden getraind.
* Zorg ervoor dat ze nog niet geregistreerd multi-factor Authentication of selfservice voor wachtwoordherstel (SSPR) op het apparaat of de persoonlijke gegevens van een afzonderlijke gebruiker. 
* Zorg ervoor dat het apparaat toegankelijk voor alle beheerders die mogelijk nodig hebt om in een noodsituatie is als de accounts zijn geregistreerd voor multi-factor Authentication op een apparaat, voor gebruik tijdens de rolactivering van de. Controleer ook of het apparaat is geregistreerd via ten minste twee mechanismen die geen van een algemene fout-modus delen. Bijvoorbeeld, kan het apparaat communiceren met internet via een faciliteit draadloos netwerk en een cel serviceprovider-netwerk.
* Werk de accountreferenties.

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen van een gebruiker cloud-gebaseerde](add-users-azure-active-directory.md) en [de nieuwe gebruiker toewijzen aan de rol globale beheerder](active-directory-users-assign-role-azure-portal.md).
- [Aanmelden voor Azure Active Directory Premium](active-directory-get-started-premium.md), als u al nog niet bent aangemeld.
- [Azure multi-factor Authentication vereisen voor afzonderlijke gebruikers toegewezen als beheerders](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Aanvullende bescherming voor globale beheerders configureren in Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), als u van Office 365 gebruikmaakt.
- [Uitvoeren van een beoordeling van de toegang van globale beheerders](active-directory-privileged-identity-management-how-to-start-security-review.md) en [overgang van bestaande globale beheerders meer specifieke beheerdersrollen](active-directory-assign-admin-roles-azure-portal.md).


