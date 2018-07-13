---
title: Azure AD Privileged Identity Management configureren | Microsoft Docs
description: In dit onderwerp wordt uitgelegd wat Azure AD Privileged Identity Management is en hoe u PIM kunt gebruiken om uw cloudbeveiliging te verbeteren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: protection
ms.topic: overview
ms.date: 03/07/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9e5039623b1f9b53c636ebaeb1e956d852185577
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952049"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Wat is Azure AD Privileged Identity Management?

Met Azure Active Directory (AD) Privileged Identity Management kunt u toegang binnen de organisatie beheren, controleren en bewaken. Dit is inclusief toegang tot resources in Azure AD, Azure-resources (preview) en andere Microsoft-onlineservices zoals Office 365 en Microsoft Intune.

> [!NOTE]
> Wanneer u Privileged Identity Management inschakelt voor uw tenant, is een geldige betaalde versie of proefversie van Azure AD Premium P2 of Enterprise Mobility + Security E5 vereist voor elke gebruiker die interactie heeft met of een van de voordelen van de service ontvangt. Voorbeelden zijn gebruikers/gebruikers in een groep die voldoen aan een of meer van deze voorwaarden:
>
>- Toegewezen aan de rol Beheerder met bevoorrechte rol 
>- Toegewezen als in aanmerking komend voor andere adreslijstrollen die worden beheerd via PIM 
>- Kan aanvragen in PIM goedkeuren/afwijzen 
>- Toegewezen aan de rol van een Azure-resource met JIT-toewijzingen (Just In Time) of directe toewijzingen (op basis van tijd)  
>- Toegewezen aan een toegangscontrole
>
>Zie [Azure Active Directory-edities](fundamentals/active-directory-whatis.md) voor meer informatie.

Organisaties willen het aantal mensen beperken dat toegang heeft tot beveiligde gegevens of resources, om zo de kans te verkleinen dat een kwaadwillende gebruiker die toegang verkrijgt of een geautoriseerde gebruiker per ongeluk een gevoelige resource beschadigt.  Gebruikers moeten echter wel bevoorrechte bewerkingen kunnen uitvoeren in Azure AD, Azure, Office 365 of SaaS-apps. Organisaties kunnen gebruikers daarom bevoorrechte toegang geven tot Azure-resources zoals abonnementen en Azure AD. Het is belangrijk dat er toezicht is over wat gebruikers doen met hun beheerdersbevoegdheden. Azure AD Privileged Identity Management vermindert het risico van overmatige, overbodige of verkeerd gebruikte toegangsrechten.

Azure AD Privileged Identity Management helpt uw organisatie met het volgende:

- Zien welke gebruikers bevoorrechte rollen hebben voor het beheren van Azure-resources (preview), en welke gebruikers rollen met beheerdersrechten hebben in Azure AD
- 'Just in time' beheerderstoegang op aanvraag inschakelen voor Microsoft-onlineservices zoals Office 365 en Intune, en voor Azure-resources (Preview) van abonnementen, resourcegroepen en afzonderlijke resources zoals virtuele machines 
-   Een historisch overzicht bekijken van activering door beheerders, met inbegrip van wat beheerders wijzigen in Azure-resources (preview)
- Waarschuwingen ontvangen over wijzigingen in beheerderstoewijzingen
- Goedkeuring vereisen voor het activeren van bevoorrechte beheerdersrollen van Azure AD (preview) 
- Lidmaatschap van rollen met beheertaken controleren en gebruikers verzoeken om een reden voor vernieuwd lidmaatschap

In Azure AD kan Azure AD Privileged Identity Management de gebruikers beheren die zijn toegewezen aan de ingebouwde organisatierollen van Azure AD, zoals Globale beheerder. In Azure kan Azure AD Privileged Identity Management de gebruikers en groepen beheren die zijn toegewezen via Azure RBAC-rollen, met inbegrip van Eigenaar of Inzender.

## <a name="just-in-time-administrator-access"></a>'Just in time' beheerderstoegang

Eerder was het mogelijk om een gebruiker toe te wijzen aan een beheerdersrol via Azure Portal, andere portals van Microsoft-onlineservices of de Azure AD-cmdlets in Windows PowerShell. Het gevolg was dat de gebruiker een **permanente beheerder** werd, die altijd actief was in de toegewezen rol. In Azure AD Privileged Identity Management wordt het concept van een **in aanmerking komende beheerder** geïntroduceerd. In aanmerking komende beheerders zijn gebruikers die af en toe bevoorrechte toegang nodig hebben, maar niet de hele dag en ook niet elke dag. De rol is inactief totdat de gebruiker toegang nodig heeft. Op dat moment voeren ze een activeringsproces uit en zijn ze gedurende een vooraf bepaalde hoeveelheid tijd een actieve beheerder. Steeds meer organisaties kiezen voor deze benadering om 'doorlopende beheerderstoegang' tot bevoorrechte rollen te verminderen of uit te sluiten.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privileged Identity Management inschakelen voor uw adreslijst

Ga naar [Azure Portal](https://portal.azure.com/) om aan de slag te gaan met Azure AD Privileged Identity Management.

> [!NOTE]
> U moet een globale beheerder met een organisatie-account zijn (bijvoorbeeld @yourdomain.com), niet een Microsoft-account (bijvoorbeeld @outlook.com), om Azure AD Privileged Identity Management te kunnen inschakelen voor een adreslijst.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van uw directory.
2. Als uw organisatie meerdere directory's heeft, selecteert u uw gebruikersnaam rechtsboven in de Azure Portal. Selecteer de adreslijst waarvoor u Azure AD Privileged Identity Management wilt gebruiken.
3. Selecteer **Alle services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

Als u de eerste persoon bent die Azure AD Privileged Identity Management in uw adreslijst gebruikt en u naar de adreslijstrollen van Azure AD navigeert, wordt u via de [wizard Beveiliging](active-directory-privileged-identity-management-security-wizard.md) stapsgewijs begeleid bij de eerste toewijzing. Hierna wordt u automatisch de eerste **beveiligingsbeheerder** en **beheerder met bevoorrechte rol** van de adreslijst.

Voor Azure AD-rollen geldt dat alleen een gebruiker met de rol Beheerder met bevoorrechte rol toewijzingen voor andere beheerders kan beheren in Azure AD PIM. U kunt [andere gebruikers de mogelijkheid geven om adreslijstrollen te beheren in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md). Globale beheerders, beveiligingsbeheerders en beveiligingslezers kunnen toewijzingen aan Azure AD-rollen in Azure AD PIM weergeven.
Voor Azure RBAC-rollen kan alleen een abonnementsbeheerder, een resource-eigenaar of een beheerder voor gebruikerstoegang tot resources toewijzingen voor andere beheerders in Azure AD PIM beheren.  Gebruikers met de rol Beheerder met bevoorrechte rol, Beveiligingsbeheerder of Beveiligingslezer hebben standaard geen bevoegdheid om toewijzingen aan Azure RBAC-rollen in Azure AD PIM weer te geven.

## <a name="privileged-identity-management-overview-entry-point"></a>Overzicht van Privileged Identity Management (toegangspunt)

Azure AD Privileged Identity Management biedt ondersteuning voor het beheer van Azure AD-adreslijstrollen en rollen voor Azure Resources (preview). De functie van rollen voor Azure-resources verschillen van beheerdersrollen in Azure AD. Rollen voor Azure-resources bieden gedetailleerde machtigingen voor de resource waaraan ze worden toegewezen, en aan alle onderliggende resources in de hiërarchie van de resource ('overname' genoemd). [Lees hier meer over RBAC, resource-hiërarchie en overname](../role-based-access-control/role-assignments-portal.md). PIM kan voor zowel Azure AD-adreslijstrollen als Azure-resources (preview) worden beheerd door de desbetreffende koppeling te volgen onder de sectie Beheren van het toegangspunt Snel starten van PIM in het navigatiemenu aan de linkerkant.

PIM biedt via de sectie Taken in het navigatiemenu aan de linkerkant snel toegang tot opties voor het activeren van rollen, het bekijken van openstaande activeringen/aanvragen en van goedkeuringen die nog in behandeling zijn (voor Azure AD-adreslijstrollen), evenals beoordelingen die wachten op uw reactie.

Wanneer u een optie kiest in de sectie Taken onder het toegangspunt Snel starten, ziet u resultaten voor zowel Azure AD-adreslijstrollen als Azure-resourcerollen (preview).

![Snel starten](./media/active-directory-privileged-identity-management-configure/quick-start.png)

'Mijn rollen' bevat een lijst met actieve en in aanmerking komende roltoewijzingen voor Azure AD-adreslijstrollen en Azure-resourcerollen (preview). [Lees hier meer over het activeren van in aanmerking komende roltoewijzingen](active-directory-privileged-identity-management-how-to-activate-role.md).

Het activeren van rollen voor Azure Resources (preview) is een nieuwe functie die in aanmerking komende leden van een rol de gelegenheid biedt om activering te plannen voor een toekomstige datum/tijd, gekoppeld aan een specifieke activeringsduur binnen de maximale periode die is ingesteld door beheerders.

![](./media/active-directory-privileged-identity-management-configure/activations.png)

Als een geplande activering niet meer nodig is, kunnen gebruikers hun aanvraag annuleren door Aanvragen in behandeling te kiezen in het navigatiemenu aan de linkerkant en vervolgens op Annuleren te klikken op de regel van de aanvraag.

![Aanvragen in behandeling](./media/active-directory-privileged-identity-management-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Beheerdashboard van Privileged Identity Management

In Azure AD Privileged Identity Manager kunt u een beheerdashboard weergeven met belangrijke informatie zoals:

* Waarschuwingen die aangeven waar u de beveiliging kunt verbeteren
* Het aantal gebruikers dat aan elke bevoorrechte rol is toegewezen  
* Het aantal in aanmerking komende en permanente beheerders
* Een grafiek van activeringen van bevoorrechte rollen in uw adreslijst
*   Het aantal Just-In-Time, tijdsgebonden en permanente toewijzingen voor Azure-resourcerollen (preview)
*   Gebruikers en groepen met nieuwe roltoewijzingen in de afgelopen dertig dagen (Azure-resourcerollen)


![PIM-dashboard - schermafbeelding](./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png)

## <a name="privileged-role-management"></a>Beheer van bevoorrechte rollen

Met Azure AD Privileged Identity Management kunt u beheertaken voor de beheerders uitvoeren door permanente of in aanmerking komende beheerders toe te voegen of te verwijderen voor Azure AD-adreslijstrollen. Met PIM voor Azure Resources (preview) kunnen eigenaars, beheerders van gebruikerstoegang en globale beheerders die het beheer van abonnementen in hun tenant inschakelen, gebruikers of groepen toewijzen aan Azure-resourcerollen als in aanmerking komend (Just-In-Time toegang), als tijdsgebonden toegang (geen activering vereist) met een begin- en einddatum/-tijd of permanent (indien ingeschakeld in de rolinstellingen).

![PIM-beheerders toevoegen/verwijderen - schermafbeelding](./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png)

## <a name="configure-the-role-activation-settings"></a>Instellingen voor rolactivering configureren

Met behulp van [rolinstellingen](active-directory-privileged-identity-management-how-to-change-default-settings.md) kunt u de eigenschappen voor activering van in aanmerking komende rollen configureren voor Azure AD-adreslijstrollen, waaronder:

* De duur van de activeringsperiode voor de rol
* Meldingen bij rolactivering
* De informatie die een gebruiker moet opgeven tijdens het activeren van de rol
* Nummer van het serviceticket of incident
* [Vereisten voor goedkeuringswerkstroom - Preview](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM-instellingen - activering van beheerder - schermafbeelding](./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png)

In de bovenstaande afbeelding zijn de knoppen voor **Multi-Factor Authentication** uitgeschakeld. Voor bepaalde, zeer bevoorrechte rollen, is MFA vereist voor een nog betere beveiliging.

Rolinstellingen voor rollen van Azure Resources (preview) stellen beheerders in staat om instellingen voor Just-In-Time directe toewijzingen te configureren, zoals:

- De mogelijkheid om gebruikers of groepen toe te wijzen aan rollen zonder een einddatum/-tijd (permanente toewijzing)
- De standaardduur van een toewijzing (indien niet permanent)
- De maximale activeringsduur (wanneer een in aanmerking komend lid van een rol zich activeert)
- De informatie die een gebruiker moet opgeven tijdens het activeren van de rol (Just-In-Time toewijzingen) of het toewijzingsproces (directe toewijzingen)

![](./media/active-directory-privileged-identity-management-configure/role-settings-details.png)

## <a name="role-activation"></a>Rolactivering

Als een [rol moet worden geactiveerd](active-directory-privileged-identity-management-how-to-activate-role.md), vraagt een in aanmerking komende beheerder een tijdsgebonden 'activering' aan voor de rol. De activering kan worden aangevraagd met de optie **Mijn rol activeren** in Azure AD Privileged Identity Management.

Een beheerder die een rol wil activeren, moet Azure AD Privileged Identity Management initialiseren in Azure Portal.

Rolactivering kan worden aangepast. In de instellingen van PIM kunt u de duur van de activering bepalen en welke gegevens de beheerder moet opgeven voor het activeren van de rol.

![PIM-beheerder vraagt rolactivering aan - schermafbeelding](./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png)

## <a name="review-role-activity"></a>Rolactiviteit controleren

Er zijn twee manieren om bij te houden hoe uw werknemers en beheerders bevoorrechte rollen gebruiken. De eerste optie is met behulp van [de controlegeschiedenis voor adreslijstrollen](active-directory-privileged-identity-management-how-to-use-audit-log.md). De logboeken die samen de controlegeschiedenis vormen, bevatten informatie over wijzigingen in toewijzingen van bevoorrechte rollen, de activeringsgeschiedenis van een rol en wijzigingen in de instellingen voor rollen van Azure Resources (preview). 

![Activeringsgeschiedenis voor PIM - schermopname](./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png)

De tweede optie is door het instellen van periodieke [toegangsbeoordelingen](active-directory-privileged-identity-management-how-to-start-security-review.md). Deze toegangsbeoordelingen kunnen worden uitgevoerd door een toegewezen persoon (zoals een teammanager) of de werknemers kunnen elkaar beoordelen. Dit is de beste manier om te controleren wie er nog steeds toegang nodig heeft en wie niet.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM bij verlopen van abonnement

In een tenant moet een proefversie of betaalde versie van Azure AD Premium P2 (of EMS E5) beschikbaar zijn om Azure AD PIM te kunnen gebruiken.  Bovendien moeten er licenties worden toegewezen aan de beheerders van de tenant.  Er moeten met name licenties worden toegewezen aan beheerders in Azure AD-rollen die worden beheerd via Azure AD PIM, aan beheerders in Azure RBAC-rollen die worden beheerd via Azure AD PIM en aan gebruikers die geen beheerder zijn maar die wel toegangsbeoordelingen uitvoeren.
Als uw organisatie het abonnement op Azure AD Premium P2 niet verlengt of de proefversie laat verlopen, zijn de functies van Azure AD PIM niet meer beschikbaar in uw tenant, worden toewijzingen van in aanmerking komende rollen verwijderd en kunnen gebruikers geen rollen meer activeren. Meer informatie kunt u vinden in de [vereisten voor Azure AD PIM-abonnementen](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
