---
title: Beheerdersrollen toewijzen in Azure Active Directory | Microsoft Docs
description: Een beheerdersrol kunt gebruikers toevoegen, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren. Een gebruiker aan wie een beheerdersrol is toegewezen, heeft dezelfde machtigingen van alle cloudservices waarop uw organisatie is geabonneerd.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 2768ba4726ccaf5e2249e356e425aeafaaaf91f6
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44349242"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Beheerdersrollen toewijzen in Azure Active Directory

Met Azure Active Directory (Azure AD), kunt u afzonderlijke beheerders verschillende functies vervullen aanwijzen. Beheerders worden aangewezen in de Azure AD-portal om uit te voeren taken, zoals het toevoegen of wijzigen, gebruikers, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren en beheren van domeinnamen.

## <a name="details-about-the-global-administrator-role"></a>Meer informatie over de rol globale beheerder

De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen.

## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen

Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Beschikbare rollen

De volgende beheerdersrollen zijn beschikbaar:

* **[Toepassingsbeheerder](#application-administrator)**: gebruikers in deze rol kunnen maken en beheren van alle aspecten van zakelijke toepassingen, registratie en instellingen van de toepassingsproxy. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Toepassingsontwikkelaar](#application-developer)**: gebruikers in deze rol kunnen toepassingsregistraties maken wanneer de 'Gebruikers kunnen toepassingen registreren' is ingesteld op Nee. Deze rol kan ook leden toe te staan hun eigen namens wanneer de 'Gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens in hun naam' is ingesteld op Nee. Leden van deze rol worden toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Factureringsbeheerder](#billing-administrator)**: doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.

* **[Beheerder van de cloudtoepassing](#cloud-application-administrator)**: gebruikers in deze rol hebben dezelfde machtigingen als de rol beheerder van de toepassing, met uitzondering van de mogelijkheid voor het beheren van de toepassingsproxy. Deze rol hebben de mogelijkheid om te maken en beheren van alle aspecten van bedrijfstoepassingen en registratie. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Cloud-Apparaatbeheerder](#cloud-device-administrator)**: gebruikers in deze rol kunnen inschakelen, uitschakelen, en apparaten verwijderen in Azure AD en Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal lezen. De rol verleent machtigingen voor het beheren van andere eigenschappen op het apparaat.

* **[Beheerder voor naleving](#compliance-administrator)**: gebruikers met deze rol hebben beheermachtigingen in de Office 365-beveiligings- en Compliancecentrum- en Exchange-beheercentrum. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Beheerder van voorwaardelijke toegang](#conditional-access-administrator)**: gebruikers met deze rol hebben de mogelijkheid voor het beheren van instellingen voor voorwaardelijke toegang van Azure Active Directory.
  > [!NOTE]
  > Voor het implementeren van Exchange ActiveSync-beleid voor voorwaardelijke toegang in Azure, moet de gebruiker ook een globale beheerder zijn.
  
* **[Apparaatbeheerders](#device-administrators)**: deze functie is beschikbaar voor toewijzing alleen als een lokale beheerder in [apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaatobjecten in Azure Active Directory. 

* **[Adreslijstlezers](#directory-readers)**: dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Directory-Accounts voor synchronisatie](#directory-synchronization-accounts)**: niet gebruiken. Deze rol is wordt automatisch toegewezen aan de Azure AD Connect-service en niet bedoeld of ondersteund voor ander gebruik.

* **[Schrijvers van mappen](#directory-writers)**: dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Dynamics 365-servicebeheerder / CRM-servicebeheerder](#dynamics-365-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Dynamics 365 Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets te beheren en servicestatus controleren. Meer informatie op [de rol admin gebruiken voor het beheren van uw tenant](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Exchange Service-beheerder](#exchange-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Exchange Online, wanneer de service aanwezig is. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Globale beheerder / bedrijfsbeheerder](#company-administrator)**: gebruikers met deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die gebruikmaken van Azure Active Directory-identiteiten, zoals Exchange Online SharePoint Online en Skype voor bedrijven Online. De persoon die zich aanmeldt voor de Azure Active Directory-tenant wordt globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er is meer dan één globale beheerder in uw bedrijf. Globale beheerders kunnen het wachtwoord voor elke gebruiker en alle andere beheerders opnieuw instellen.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Company Administrator". Het 'Globale beheerder' is in de [Azure-portal](https://portal.azure.com).
  >
  >

* **[Gastuitnodiging](#guest-inviter)**: gebruikers in deze rol kunnen uitnodigingen voor Azure Active Directory B2B Gast beheren wanneer de **leden kunnen uitnodigen** gebruikersinstelling is ingesteld op Nee. Meer informatie over B2B-samenwerking bij [over Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Deze omvatten geen andere machtigingen.

* **[Information Protection-beheerder](#information-protection-administrator)**: gebruikers met deze rol hebben alle machtigingen in de Azure Information Protection-service. Deze rol kan labels voor de Azure Information Protection-beleid configureren, beveiligingssjablonen beheren en beveiliging activeren. Deze rol verleent alle machtigingen in Identity Protection Center, Privileged Identity Management, Monitor Office 365-servicestatus of Office 365 Centrum voor beveiliging en naleving.

* **[Intune-servicebeheerder](#intune-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Intune Online, wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren. Meer informatie op [rollen gebaseerd toegangsbeheer (RBAC) met Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)

* **[Licentiebeheerder](#license-administrator)**: gebruikers in deze rol kunnen toevoegen, verwijderen, en toewijzen van licenties op gebruikers, groepen (met Groepslicenties) bijwerken en beheren van de gebruikslocatie op gebruikers. De rol heeft niet de mogelijkheid om te kopen of beheren van abonnementen, maken of beheren van groepen, of maken of beheren van gebruikers buiten de gebruikslocatie verlenen.

* **[Message Center lezer](#message-center-reader)**: gebruikers in deze rol kunnen controleren, meldingen en de gezondheid van advies-updates in [Office 365-berichtencentrum](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie op de geconfigureerde services zoals Exchange, Intune en Microsoft Teams. Berichtencentrum-lezer wekelijkse e-mailbericht verwerkingen van berichten, updates, ontvangen en message center berichten in Office 365 kunnen delen. In Azure AD, wordt gebruikers die zijn toegewezen aan deze rol alleen alleen-lezen toegang hebben op Azure AD-services, zoals gebruikers en groepen. 

* **[Laag1-ondersteuning voor partner](#partner-tier1-support)**: niet gebruiken. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Laag2-ondersteuning voor partner](#partner-tier2-support)**: niet gebruiken. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Wachtwoordbeheerder / Helpdeskbeheerder](#helpdesk-administrator)**: gebruikers met deze rol kunnen wachtwoorden wijzigen, vernieuwingstokens ongeldig te maken, serviceaanvragen beheren en servicestatus controleren. Helpdesk-beheerders kunnen wachtwoorden wijzigen en vernieuwingstokens alleen voor gebruikers en andere helpdeskbeheerders ongeldig te maken. Ongeldig vernieuwingstoken zorgt ervoor dat de gebruiker zich opnieuw aanmelden.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Helpdesk-beheerder'. 'Wachtwoordbeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >
  
* **[Power BI-servicebeheerder](#power-bi-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Power BI, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [inzicht in de Power BI-beheerdersrol](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Rol van beheerder in beschermde modus](#privileged-role-administrator)**: gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

* **[Lezer-rapporten](#reports-reader)**: gebruikers met deze rol gebruiksrapporten gegevens en het dashboard rapporten in Office 365-beheercentrum en de acceptatie-context pack in Power BI kunnen bekijken. Bovendien de rol biedt toegang tot de aanmeldings-rapporten en -activiteit in Azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. De gebruiker die is toegewezen aan de rol Rapportenlezer toegang alleen relevante gebruik en acceptatie metrische gegevens. Ze geen geen admin-machtigingen voor het configureren van instellingen of toegang tot die het product specifieke beheercentrums zoals Exchange. 

* **[Beveiligingsbeheerder](#security-administrator)**: gebruikers met deze rol hebben alle alleen-lezen machtigingen van de rol beveiligingslezer, plus de mogelijkheid voor het beheren van configuratie voor beveiliging-gerelateerde services: Azure Active Directory Identity Protection Azure Information Protection, en Office 365 Centrum voor beveiliging en naleving. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | In | Kan doen |
  | --- | --- |
  | Identity Protection Center |<ul><li>Alle machtigingen van de rol van Beveiligingslezer.<li>Bovendien de mogelijkheid om alle IPC-bewerkingen, met uitzondering van opnieuw instellen van wachtwoorden uitvoeren. |
  | Privileged Identity Management |<ul><li>Alle machtigingen van de rol van Beveiligingslezer.<li>**Kan geen** rollidmaatschappen voor Azure AD- of -instellingen beheren. |
  | <p>Monitor voor Office 365-servicestatus</p><p>Office 365-centrum voor beveiliging en naleving |<ul><li>Alle machtigingen van de rol van Beveiligingslezer.<li>Kan alle instellingen configureren in de functie Advanced Threat Protection (beveiliging voor schadelijke software en virussen, schadelijke URL config, URL-tracering, enzovoort). |
  
* **[Beveiligingslezer](#security-reader)**: gebruikers met deze rol hebben globale alleen-lezen toegang, met inbegrip van alle gegevens in Azure Active Directory, Identity Protection, Privileged Identity Management, evenals de mogelijkheid om te lezen van Azure Active Directory -aanmeldingsrapporten en controlelogboeken. De rol biedt tevens alleen-lezen-machtiging in Office 365-beveiligings- en Compliancecentrum. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | In | Kan doen |
  | --- | --- |
  | Identity Protection Center |Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde beveiliging tegen bedreigingen<li>Anti-phishing<li>Mailflow regels |
  | Privileged Identity Management |<p>Alleen-lezen toegang tot alle gegevens zichtbaar is in Azure AD PIM: beleid en rapporten voor Azure AD-roltoewijzingen security beoordeelt en toegang tot gegevens en rapporten voor scenario's behalve Azure AD-roltoewijzing in de toekomst te lezen.<p>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengen. In de PIM-portal of via PowerShell kunt iemand zich in deze rol aanvullende rollen (bijvoorbeeld: globale beheerder of beheerder met bevoorrechte rol), activeren als de gebruiker een kandidaat een voor hen. |
  | <p>Monitor voor Office 365-servicestatus</p><p>Office 365-centrum voor beveiliging en naleving</p> |<ul><li>Lezen en waarschuwingen beheren<li>Beveiligingsbeleid lezen<li>Bedreigingsinformatie, Cloud App Discovery en in quarantaine plaatsen in Search en onderzoeken<li>Alle rapporten lezen |

* **[De rol beheerder serviceondersteuning](#service-support-administrator)**: gebruikers met deze rol kunnen ondersteuningsaanvragen openen met Microsoft Azure en Office 365-services, weergaven en het servicedashboard en berichtencentrum weergeven in Azure portal en Office 365-beheerportal. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[SharePoint-servicebeheerder](#sharepoint-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Skype voor bedrijven / Lync-servicebeheerder](#lync-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Skype voor bedrijven, wanneer de service aanwezig is, evenals Skype-specifieke gebruikerskenmerken in Azure Active beheren De map. Deze rol hebben bovendien de mogelijkheid ondersteuningstickets beheren en servicestatus controleren en de toegang tot de Teams en Skype voor bedrijven-beheercentrum. Het account moet ook een licentie hebben voor Teams of Teams PowerShell-cmdlets kan niet worden uitgevoerd. Meer informatie op [over de Skype voor bedrijven-beheerdersrol](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) en Teams informatie over licenties op [Skype voor bedrijven en Microsoft Teams-Add-on-licentieverlening](https://docs.microsoft.com/en-us/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Lync-servicebeheerder'. Het 'Skype voor bedrijven-servicebeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >

* **[Beheerder van gebruikersaccounts](#user-account-administrator)**: gebruikers met deze rol kunnen maken en beheren van alle aspecten van gebruikers en groepen. Daarnaast bevat deze rol de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Er zijn enkele beperkingen zijn van toepassing. Deze rol kunnen bijvoorbeeld niet verwijderen van een globale beheerder. Gebruiker accountbeheerders kunnen wachtwoorden wijzigen en vernieuwen van tokens voor gebruikers, helpdeskbeheerders en andere beheerders gebruikersaccount alleen ongeldig te maken. Ongeldig vernieuwingstoken zorgt ervoor dat de gebruiker zich opnieuw aanmelden.

| Kan doen | Niet mogelijk is |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Wachtwoorden wijzigen voor gebruikers, helpdeskbeheerders en andere alleen beheerders van gebruikersaccount</p><p>Gebruikersweergaven maken en beheren</p><p>Maken, bewerken en verwijderen van gebruikers en groepen beheren van gebruikerslicenties, met beperkingen. Hij of zij kan niet verwijderen van een globale beheerder of andere beheerders maken.</p> |<p>Facturering en aankopen bewerkingen voor Office-producten</p><p>Domeinen beheren</p><p>Bedrijfsinformatie beheren</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor Authentication-verificatie</p><p>Auditlogboeken weergeven</p> |

## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit Azure AD in de toekomst.

* Ad-hoclicentiebeheerder
* Toevoegen
* Apparaatbeheerders
* Gebruikers van apparaten
* Maker van geverifieerde e-mailgebruiker
* Postvakbeheerder
* Werkplekapparaat toevoegen

## <a name="detailed-azure-active-directory-permissions"></a>Gedetailleerde machtigingen voor Azure Active Directory
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory die aan elke rol. Sommige rollen, zoals de hoofdbeheerder mogelijk extra machtigingen in Microsoft-services outide van Azure Active Directory.


### <a name="adhoc-license-administrator"></a>Ad-hoclicentiebeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Domains/default/Read | Lees de basiseigenschappen van domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lezen groups.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/default/Read | Lees de basiseigenschappen van groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lezen groups.memberOf in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Members/Read | Lezen groups.members in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Owners/Read | Lezen groups.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Settings/Read | Lezen groups.settings in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lees de basiseigenschappen van oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Update oAuth2PermissionGrants in Azure Active Directory. |
| Microsoft.AAD.Directory/Organization/default/Read | Lees de basiseigenschappen van de organisatie in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lezen organization.trustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Default/Read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lezen users.invitedBy in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lezen users.invitedUsers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |

### <a name="application-administrator"></a>Toepassingsbeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/Audience/update | De eigenschap applications.audience in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Authentication/update | De eigenschap applications.authentication in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/default/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Create | Toepassingen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/credentials/update | De eigenschap applications.credentials in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/DELETE | Verwijder toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Lees appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.reports/allEntities/read | Lees Azure AD-rapporten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="application-developer"></a>Toepassingsontwikkelaar
Kan toepassingsregistraties maken, onafhankelijk van de gebruikers kan zich registreren toepassingen instellen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Toepassingen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | AppRoleAssignments maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | OAuth2PermissionGrants maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | ServicePrincipals maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kan algemene taken met betrekking tot facturering uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Organization/default/update | Eenvoudige eigenschappen op de organisatie in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | De eigenschap organization.trustedCAsForPasswordlessAuth in Azure Active Directory bijgewerkt. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-application-administrator"></a>Beheerder van de cloudtoepassing
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren, behalve App Proxy.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/Audience/update | De eigenschap applications.audience in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Authentication/update | De eigenschap applications.authentication in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/default/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Create | Toepassingen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/credentials/update | De eigenschap applications.credentials in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/DELETE | Verwijder toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/default/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.reports/allEntities/read | Lees Azure AD-rapporten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-device-administrator"></a>Cloud-Apparaatbeheerder
Volledige toegang om apparaten te beheren in Azure AD.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Devices/DELETE | Apparaten verwijderen in Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/update | Werk de eigenschap accountEnabled op apparaten in Azure Active Directory |
| microsoft.aad.reports/allEntities/read | Lees Azure AD-rapporten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="company-administrator"></a>Bedrijfsbeheerder
Kan alle aspecten beheren van Azure AD en Microsoft-services die Azure AD-identiteiten gebruiken.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Maken en verwijderen van administrativeUnits, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Maken en verwijderen van toepassingen, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Maken en verwijderen van appRoleAssignments, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Maken en contactpersonen, verwijderen en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Maken en verwijderen van contracten, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/devices/allProperties/allTasks | Maken en verwijderen van apparaten, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Maken en verwijderen van directoryRoles, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Maken en verwijderen van directoryRoleTemplates, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/domains/allProperties/allTasks | Maken en verwijderen van domeinen, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/groups/allProperties/allTasks | Maken en verwijderen van groepen, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Maken en verwijderen van groupSettings, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Maken en verwijderen van groupSettingTemplates, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Maken en verwijderen van loginTenantBranding, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Maken en verwijderen van oAuth2PermissionGrants, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/organization/allProperties/allTasks | Maken en verwijderen van de organisatie, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/policies/allProperties/allTasks | Maken en verwijderen van beleid, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Maken en verwijderen van roleAssignments, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Maken en verwijderen van roleDefinitions, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Maken en verwijderen van scopedRoleMemberships, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/serviceAction/activateService | De actie van de service Activateservice kunt uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | De actie van de service Disabledirectoryfeature kunt uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | De actie van de service Enabledirectoryfeature kunt uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | De actie van de service Getavailableextentionproperties kunt uitvoeren in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Maken en verwijderen van servicePrincipals, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Maken en verwijderen van subscribedSkus, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Maken en verwijderen van gebruikers, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.aad.reports/allEntities/allTasks | Lees en configureer Azure AD-rapporten. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.informationProtection/allEntities/allTasks | Alle aspecten van Azure Information Protection beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Alle aspecten van Office 365-Nalevingsframework Manager beheren |
| Microsoft.Office365.Exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Alle aspecten van Office 365-klant Lockbox beheren |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| Microsoft.Office365.protectionCenter/allEntities/allTasks | Alle aspecten van Office 365 Protection Center beheren. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |

### <a name="compliance-administrator"></a>Beheerder voor naleving
Kan nalevingsconfiguratie en -rapporten lezen en beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Alle aspecten van Office 365-Nalevingsframework Manager beheren |
| Microsoft.Office365.Exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder
Kan de mogelijkheden van voorwaardelijke toegang beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/default/read | Lezen policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/default/update | De eigenschap policies.conditionalAccess in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/conditionalAccess/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lezen policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | De eigenschap policies.conditionalAccess in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lezen policies.conditionalAccess in Azure Active Directory. |

### <a name="crm-service-administrator"></a>CRM-servicebeheerder
Kan alle aspecten van het Dynamics 365-product beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="customer-lockbox-access-approver"></a>Toegangsfiatteur voor Klanten-lockbox
Kan Microsoft-ondersteuningsaanvragen voor toegang tot bedrijfsgegevens van klanten goedkeuren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Alle aspecten van Office 365-klant Lockbox beheren |

### <a name="device-administrators"></a>Apparaatadministrators
Leden van deze rol worden toegevoegd aan de groep lokale beheerders op Azure AD join-apparaten.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groupSettings/default/read | Lees de basiseigenschappen van groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lees de basiseigenschappen van groupSettingTemplates in Azure Active Directory. |

### <a name="device-managers"></a>Apparaatbeheerders
Kan Microsoft-ondersteuningsaanvragen voor toegang tot bedrijfsgegevens van klanten goedkeuren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Devices/default/Read | Lees de basiseigenschappen van apparaten in Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/default/update | Eenvoudige eigenschappen op apparaten in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/devices/memberOf/read | Lezen devices.memberOf in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lezen devices.registeredOwners in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | De eigenschap devices.registeredOwners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/devices/registeredUsers/read | Lezen devices.registeredUsers in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | De eigenschap devices.registeredUsers in Azure Active Directory bijgewerkt. |

### <a name="directory-readers"></a>Adreslijstlezers
Basic directory-informatie kan worden gelezen. Voor het verlenen van toegang tot toepassingen

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/default/read | Lees de basiseigenschappen van administrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Lezen administrativeUnits.members in Azure Active Directory. |
Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/default/Read | Lees de basiseigenschappen van toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Owners/Read | Lezen applications.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/default/Read | Lees de basiseigenschappen van contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Lezen contacts.memberOf in Azure Active Directory. |
| Microsoft.AAD.Directory/Contracts/default/Read | Lees de basiseigenschappen van opdrachten in Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/default/Read | Lees de basiseigenschappen van apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lezen devices.memberOf in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lezen devices.registeredOwners in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lezen devices.registeredUsers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/default/read | Lees de basiseigenschappen van directoryRoles in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lezen directoryRoles.eligibleMembers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Lezen directoryRoles.members in Azure Active Directory. |
| Microsoft.AAD.Directory/Domains/default/Read | Lees de basiseigenschappen van domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lezen groups.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/default/Read | Lees de basiseigenschappen van groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lezen groups.memberOf in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Members/Read | Lezen groups.members in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Owners/Read | Lezen groups.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Settings/Read | Lezen groups.settings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/default/read | Lees de basiseigenschappen van groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lees de basiseigenschappen van groupSettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lees de basiseigenschappen van oAuth2PermissionGrants in Azure Active Directory. |
| Microsoft.AAD.Directory/Organization/default/Read | Lees de basiseigenschappen van de organisatie in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lezen organization.trustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lezen servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lezen servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Lees de basiseigenschappen van servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lezen servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lezen servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lezen servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lezen servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lezen servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/default/read | Lees de basiseigenschappen van subscribedSkus in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Default/Read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lezen users.invitedBy in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lezen users.invitedUsers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Synchronisatie van Active Directory-Accounts
Alleen gebruikt door Azure AD Connect-service.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | De eigenschap organization.dirSync in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Policies/Create | Maak beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/DELETE | Beleidsregels in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Policies/default/Read | Lees de basiseigenschappen van beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/default/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/Owners/Read | Lezen policies.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/Owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lezen policies.policiesAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lezen servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lezen servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/default/read | Lees de basiseigenschappen van servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lezen servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lezen servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lezen servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lezen servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lezen servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |

### <a name="directory-writers"></a>Schrijvers van mappen
Kan lezen en schrijven van basic directory-informatie. Voor het verlenen van toegang tot toepassingen

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Groups/Create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/default/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groupSettings/default/update | Eenvoudige eigenschappen op groupSettings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groupSettings/create | GroupSettings maken in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Verwijder groupSettings in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| Microsoft.AAD.Directory/Users/Default/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |

### <a name="exchange-service-administrator"></a>Exchange Service-beheerder
Kan alle aspecten van het product Exchange beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="guest"></a>Gast
De standaardrol van gastgebruikers. Een beperkte set van directory-informatie kan worden gelezen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/default/Read | Lees de basiseigenschappen van toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Owners/Read | Lezen applications.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Domains/default/Read | Lees de basiseigenschappen van domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lezen groups.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/default/Read | Lees de basiseigenschappen van groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lezen groups.memberOf in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Members/Read | Lezen groups.members in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Owners/Read | Lezen groups.owners in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Settings/Read | Lezen groups.settings in Azure Active Directory. |
| microsoft.aad.directory/organization/basicProfile/read | Lees basisinformatie in het organisatieprofiel in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lezen servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lezen servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Lees de basiseigenschappen van servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lezen servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/members/read | Lezen servicePrincipals.members in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lezen servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lezen servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lezen servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lezen servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/read | Lezen users.basicProfile in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Default/Read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/eligibleMemberOf/read | Lezen users.eligibleMemberOf in Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lezen users.invitedBy in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lezen users.invitedUsers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/pendingMemberOf/read | Lezen users.pendingMemberOf in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |
| microsoft.aad.directory/users/scopedAdministratorOf/read | Lezen users.scopedAdministratorOf in Azure Active Directory. |

### <a name="guest-inviter"></a>Gastuitnodiging
Uitnodiging gastgebruikers ook kunnen onafhankelijk van de leden kunnen kunnen gasten instellen uitnodigen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Default/Read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lezen users.invitedBy in Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Nodig gastgebruikers uit in Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lezen users.invitedUsers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdeskbeheerder
Kan wachtwoorden voor niet-beheerders en Helpdesk-medewerkers opnieuw instellen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="information-protection-administrator"></a>Information Protection-beheerder
Kan alle aspecten van het product Azure Information Protection beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Alle aspecten van Azure Information Protection beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/default/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Contacts/Create | Contactpersonen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Contactpersonen in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Devices/default/update | Eenvoudige eigenschappen op apparaten in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Devices/Create | Apparaten maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Devices/DELETE | Apparaten verwijderen in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | De eigenschap devices.registeredOwners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/devices/registeredUsers/update | De eigenschap devices.registeredUsers in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/default/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| Microsoft.AAD.Directory/Groups/Create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| Microsoft.AAD.Directory/Groups/DELETE | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lezen groups.hiddenMembers in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Restore | Groepen in Azure Active Directory herstellen. |
| Microsoft.AAD.Directory/Groups/Settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Users/Default/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="license-administrator"></a>Licentiebeheerder
Kan productlicenties voor gebruikers en groepen beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/usageLocation/update | De eigenschap users.usageLocation in Azure Active Directory bijgewerkt. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="lync-service-administrator"></a>Lync-servicebeheerder
Kan alle aspecten van het product Skype voor Bedrijven beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in het Office 365-berichtencentrum lezen. 

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessmessagecenter/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in Berichtencentrum en werk deze bij. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/default/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Contacts/Create | Contactpersonen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Contactpersonen in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Groups/Create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| Microsoft.AAD.Directory/Users/Default/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/DELETE | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Users/Password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| Microsoft.AAD.Directory/Users/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Contacts/default/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Contacts/Create | Contactpersonen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/domains/allTasks | Maken en verwijderen van domeinen en lezen en bijwerken van de standaardeigenschappen in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Create | Groepen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/DELETE | Groepen in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Restore | Groepen in Azure Active Directory herstellen. |
| Microsoft.AAD.Directory/Organization/default/update | Eenvoudige eigenschappen op de organisatie in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | De eigenschap organization.trustedCAsForPasswordlessAuth in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| Microsoft.AAD.Directory/Users/Default/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/DELETE | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Users/Password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| Microsoft.AAD.Directory/Users/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="power-bi-service-administrator"></a>Servicebeheerder van Power BI
Kan alle aspecten van het Power BI-product beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kunnen roltoewijzingen beheren in Azure AD

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Update directoryRoles in Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.aad.privilegedIdentityManagement. |

### <a name="reports-reader"></a>Rapportenlezer
Kan rapporten met betrekking tot aanmeldingen en controles lezen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | Lees Azure AD-rapporten. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.usageReports/allEntities/Read | Lees Office 365-gebruiksrapporten. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligingsgegevens en -rapporten lezen

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| Microsoft.AAD.Directory/Applications/Policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Policies/default/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/Create | Maak beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/DELETE | Beleidsregels in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Policies/Owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.identityProtection/allEntities/read | Alle resources in microsoft.aad.identityProtection lezen. |
| microsoft.aad.identityProtection/allEntities/update | Alle resources in microsoft.aad.identityProtection bijwerken. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Lees alle aspecten van Office 365 Protection Center. |
| Microsoft.Office365.protectionCenter/allEntities/update | Alle resources in microsoft.office365.protectionCenter bijwerken. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="security-reader"></a>Beveiligingslezer
Kan beveiligingsgegevens en -rapporten lezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Alle resources in microsoft.aad.identityProtection lezen. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Lees alle aspecten van Office 365 Protection Center. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="service-support-administrator"></a>Beheerder serviceondersteuning
Kan gegevens over de servicestatus lezen en ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="sharepoint-service-administrator"></a>SharePoint-servicebeheerder
Kan alle aspecten van de SharePoint-service beheren.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft machtigingen addditonal buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="user-account-administrator"></a>Beheerder van gebruikersaccounts
Kan alle aspecten van gebruikers en groepen beheren

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/default/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Contacts/Create | Contactpersonen maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Contacts/DELETE | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/default/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| Microsoft.AAD.Directory/Groups/Create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| Microsoft.AAD.Directory/Groups/DELETE | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lezen groups.hiddenMembers in Azure Active Directory. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Restore | Groepen in Azure Active Directory herstellen. |
| Microsoft.AAD.Directory/Groups/Settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| Microsoft.AAD.Directory/Users/Default/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Create | Gebruikers maken in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/DELETE | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| Microsoft.AAD.Directory/Users/Manager/Update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Users/Password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| Microsoft.AAD.Directory/Users/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.accessService/allEntities/allTasks | Alle aspecten van de toegang tot Azure-service beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="user"></a>Gebruiker
De standaardrol van gebruikers. Kan alle lezen en schrijven van een beperkte set van directory-informatie.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Toepassingen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| Microsoft.AAD.Directory/Groups/default/Read | Lees de basiseigenschappen van groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/oAuth2PermissionGrants/create | OAuth2PermissionGrants maken in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | Verwijder oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Update oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | ServicePrincipals maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/users/activateServicePlan | Activateserviceplan gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Nodig gastgebruikers uit in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/default/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/DELETE | Verwijder toepassingen in Azure Active Directory. |
| Microsoft.AAD.Directory/Applications/Owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Applications/Restore | Toepassingen in Azure Active Directory herstellen. |
| Microsoft.AAD.Directory/Devices/disable | Apparaten in Azure Active Directory uitschakelen. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/default/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| Microsoft.AAD.Directory/Groups/DELETE | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | De eigenschap groups.dynamicMembershipRule in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Groups/Restore | Groepen in Azure Active Directory herstellen. |
| Microsoft.AAD.Directory/Groups/Settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Policies/default/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| Microsoft.AAD.Directory/Policies/DELETE | Beleidsregels in Azure Active Directory verwijderen. |
| Microsoft.AAD.Directory/Policies/Owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/default/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/changePassword | Wachtwoorden wijzigen voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/update | De eigenschap users.basicProfile in Azure Active Directory bijgewerkt. |
| Microsoft.AAD.Directory/Users/Mobile/update | De eigenschap users.mobile in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/searchableDeviceKey/update | De eigenschap users.searchableDeviceKey in Azure Active Directory bijgewerkt. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het toewijzen van een gebruiker als een beheerder van een Azure-abonnement, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
