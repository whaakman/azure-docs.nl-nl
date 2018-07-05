---
title: Beheerdersaccounts voor EMS-toegang beheren in Azure AD | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van accounts voor toegang in noodgevallen voor de organisaties bevoorrechte toegang beperken in een bestaande Azure Active Directory-omgeving.
services: active-directory
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 185f6730babe077332be9f054ba338ff48295eca
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449823"
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Beheerdersaccounts voor EMS-toegang beheren in Azure AD 

Voor de meeste dagelijkse activiteiten, *hoofdbeheerder* rechten zijn niet nodig voor uw gebruikers. Gebruikers moeten niet permanent worden toegewezen aan de rol, omdat ze mogelijk per ongeluk een taak die vereist hogere machtigingen dan ze moeten uitvoeren. Wanneer gebruikers niet nodig hebt om te fungeren als een globale beheerder, moeten ze de roltoewijzing activeren met behulp van Azure Active Directory (Azure AD) Privileged Identity Management (PIM), op hun eigen account of een alternatieve Administrator-account.

Naast de gebruikers te nemen op administratieve toegangsrechten voor zichzelf, moet u om te voorkomen dat per ongeluk wordt vergrendeld buiten het beheer van uw Azure AD-tenant omdat u kunt niet aanmelden of activeren van een bestaande afzonderlijke gebruiker-account als een de beheerder. U kunt de gevolgen van het onbedoeld gebrek aan administratieve toegang beperken met het opslaan van twee of meer *accounts voor toegang in noodgevallen* in uw tenant.

Accounts voor toegang in noodgevallen kunt organisaties bevoorrechte toegang beperken in een bestaande Azure Active Directory-omgeving. Dergelijke accounts over uitgebreide beheerdersmogelijkheden en ze niet zijn toegewezen aan specifieke personen. Accounts voor toegang in noodgevallen zijn beperkt tot EMS of "verbreken om" scenario's, normale beheerdersaccounts niet kunnen worden gebruikt. Organisaties behouden de moeten een doel van het gebruik van de noodgevallen account beperken tot alleen die tijd gedurende welke het nodig is.

Een organisatie moet mogelijk gebruik van een account voor toegang in noodgevallen in de volgende situaties:

 - De gebruikersaccounts federatief zijn en federation is momenteel niet beschikbaar vanwege een cel-netwerkverbinding wordt verbroken of een storing in de id-provider. Bijvoorbeeld, als de id-provider host in uw omgeving is uitgevallen, gebruikers mogelijk niet aanmelden bij Azure AD wordt omgeleid naar de id-provider. 
 - De groep administrators zijn geregistreerd via de Azure multi-factor Authentication en hun afzonderlijke apparaten zijn niet beschikbaar. Het is mogelijk dat gebruikers kan niet worden voltooid multi-factor Authentication voor het activeren van een rol. Bijvoorbeeld, een netwerkstoring cel voorkomt dat ze uit het beantwoorden van telefoongesprekken of SMS-berichten, de enige twee verificatiemechanismen die ze geregistreerd voor hun apparaat ontvangen. 
 - De persoon die met de meest recente globale beheerderstoegang tot heeft de organisatie verlaten. Azure AD wordt voorkomen dat de laatste *hoofdbeheerder* account wordt verwijderd, maar voorkomt niet dat het account wordt verwijderd of uitgeschakeld on-premises. De situatie kunt maken van de organisatie kan niet worden hersteld van het account.

## <a name="initial-configuration"></a>Eerste configuratie

Maak twee of meer accounts voor toegang in noodgevallen. Dit moeten alleen in de cloud-accounts die gebruikmaken van de \*..onmicrosoft.com-domein en die zijn niet federatief of gesynchroniseerd met een on-premises omgeving. 

De accounts mag niet zijn gekoppeld aan een afzonderlijke gebruiker in de organisatie. Organisaties moeten om ervoor te zorgen dat de referenties voor deze accounts zijn veilig bewaard en bekende alleen naar personen die gemachtigd zijn om ze te gebruiken. 

> [!NOTE]
> Het wachtwoord van een account voor een account voor toegang in noodgevallen is meestal verdeeld in twee of drie delen, op afzonderlijke stukjes papier geschreven en die zijn opgeslagen in de veilige, brandveilige brandkasten die zich in afzonderlijke, veilige locaties. 
>
> Zorg ervoor dat uw accounts voor toegang in noodgevallen niet zijn verbonden met een mobiele werknemer opgegeven telefoons, hardware-tokens die worden verzonden met afzonderlijke werknemers, of andere werknemer-specifieke-referenties. Deze voorzorgsmaatregel heeft betrekking op exemplaren wanneer een individuele werknemer onbereikbaar is als de referentie op die nodig is. 

### <a name="initial-configuration-with-permanent-assignments"></a>Eerste configuratie met permanente toewijzingen

Een optie is om gebruikers permanent lid zijn van de *hoofdbeheerder* rol. Deze optie is geschikt voor organisaties die nog geen Azure AD Premium P2-abonnementen.

Om het risico van een aanval die voortvloeien uit een wachtwoord waarmee is geknoeid, Azure AD wordt aanbevolen dat u meervoudige verificatie voor alle afzonderlijke gebruikers vereisen. Deze groep moet zijn beheerders en alle andere (bijvoorbeeld financiële officers) waarvan het account waarmee is geknoeid aanzienlijke gevolgen zou hebben. 

Echter, als uw organisatie heeft geen gedeelde apparaten, multi-factor Authentication niet mogelijk voor deze accounts voor toegang in noodgevallen. Als u een beleid voor voorwaardelijke toegang om te vereisen configureert [multi-factor Authentication-registratie voor elke beheerder](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor Azure AD en andere verbonden apps software as a service (SaaS), moet u mogelijk beleid configureren uitsluitingen voor toegang in noodgevallen accounts uitsluiten van deze vereiste.

### <a name="initial-configuration-with-approvals"></a>Eerste configuratie met goedkeuringen

Een andere optie is het configureren van uw gebruikers als in aanmerking komende en fiatteurs activeren de *hoofdbeheerder* rol. Deze optie vereist uw organisatie Azure AD Premium P2-abonnementen hebben. Het moet ook een optie voor multi-factor Authentication die geschikt is voor gedeeld gebruik van meerdere personen en de netwerkomgeving. Deze vereisten zijn, omdat de activering van de *hoofdbeheerder* rol vereist dat gebruikers multi-factor Authentication eerder hebt uitgevoerd. Zie voor meer informatie, [vereisen van multi-factor Authentication in Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

We raden niet met multi-factor Authentication die is gekoppeld aan persoonlijke apparaten voor accounts voor toegang in noodgevallen. In geval van nood werkelijke, kan de persoon die toegang nodig tot een multi-Factor Authentication geregistreerd apparaat heeft degene die het persoonlijke apparaat heeft zijn. 

U kunt ook de bedreigingen nemen. Bijvoorbeeld, ontstaan een onvoorziene omstandigheden, zoals een natuurramp noodgevallen, gedurende welke een mobiele telefoon of andere netwerken zijn mogelijk niet beschikbaar. Het is belangrijk om ervoor te zorgen dat alle geregistreerde apparaten moeten worden bewaard in een bekende, veilige locatie met meerdere middel om te communiceren met Azure AD.

## <a name="ongoing-monitoring"></a>Continue bewaking

Monitor de [aanmelding bij Azure AD en audit logboeken](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) voor alle aanmeldingen en activiteit van de EMS-toegangsaccounts controleren. Normaal gesproken deze accounts niet moeten worden aangemeld en moeten niet worden wijzigingen, zodat het gebruik van deze waarschijnlijk afwijkende en beveiligingsonderzoek zijn vereist.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Controle validatie moet plaatsvinden met regelmatige intervallen

Voer de volgende stappen uit ten minste voor het valideren van het account:
- Om de 90 dagen.
- Wanneer er een recente wijziging in de IT-personeel, zoals een wijziging van de taak, een vertrek of een nieuwe medewerkers is.
- Wanneer de Azure AD-abonnementen in de organisatie zijn gewijzigd.

Met het trainen van medewerkers toegang in noodgevallen accounts gebruiken, dient u het volgende:

* Zorg ervoor dat medewerkers beveiligingsbewaking Houd er rekening mee dat de controle activiteit wordt momenteel is.
* Valideren dat de gebruikersaccounts van de cloud kunnen zich aanmelden en hun rollen activeren en dat gebruikers die moet mogelijk u deze stappen uitvoert tijdens een noodgeval zijn getraind op het proces.
* Zorg ervoor dat ze niet hebt geregistreerd multi-factor Authentication of selfservice wachtwoordherstel (SSPR) op het apparaat of de persoonlijke gegevens van elke afzonderlijke gebruiker. 
* Als de accounts zijn geregistreerd voor multi-factor Authentication aan een apparaat, voor gebruik tijdens de rolactivering, zorgt u ervoor dat het apparaat is toegankelijk voor alle beheerders die mogelijk deze functie wilt gebruiken in een noodsituatie. Controleer ook of dat het apparaat is geregistreerd via ten minste twee methoden die een algemene foutmodus niet delen. Bijvoorbeeld, kan het apparaat communiceren met internet via een gebouw van een draadloos netwerk en een cel provider-netwerk.
* Werk de referenties van het account.

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen van een gebruiker cloud-gebaseerde](../fundamentals/add-users-azure-active-directory.md) en [de nieuwe gebruiker toewijzen aan de rol Algemeen beheerder](../fundamentals/active-directory-users-assign-role-azure-portal.md).
- [Aanmelden voor Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md), als u al dat nog niet hebt geregistreerd.
- [Azure multi-factor Authentication vereisen voor afzonderlijke gebruikers toegewezen als beheerders](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Configureer aanvullende bescherming voor globale beheerders in Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), als u Office 365.
- [Een toegangscontrole van globale beheerders uitvoeren](../active-directory-privileged-identity-management-how-to-start-security-review.md) en [overgang van bestaande algemene beheerders meer specifieke beheerdersrollen](directory-assign-admin-roles.md).


