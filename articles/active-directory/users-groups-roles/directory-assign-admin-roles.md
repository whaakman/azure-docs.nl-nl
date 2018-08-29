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
ms.date: 08/27/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 9b56f540af2b8d35258a4db79502c9edf83cdb45
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128463"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Beheerrollen toewijzen in Azure Active Directory

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

* **[Wachtwoordbeheerder / Helpdeskbeheerder](#helpdesk-administrator)**: gebruikers met deze rol kunnen wachtwoorden wijzigen, serviceaanvragen beheren en servicestatus controleren. Helpdesk-medewerkers kunnen alleen wachtwoorden wijzigen voor gebruikers en andere helpdeskbeheerders. 

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Helpdesk-beheerder'. 'Wachtwoordbeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >
  
* **[Power BI-servicebeheerder](#power-bi-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Power BI, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [inzicht in de Power BI-beheerdersrol](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Rol van beheerder in beschermde modus](#privileged-role-administrator)**: gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

* **[Lezer-rapporten](#reports-reader)**: gebruikers met deze rol gebruiksrapporten gegevens en het dashboard rapporten in Office 365-beheercentrum en de acceptatie-context pack in Power BI kunnen bekijken. Bovendien de rol biedt toegang tot de aanmeldings-rapporten en -activiteit in Azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. De gebruiker die is toegewezen aan de rol Rapportenlezer toegang alleen relevante gebruik en acceptatie metrische gegevens. Ze geen geen admin-machtigingen voor het configureren van instellingen of toegang tot die het product specifieke beheercentrums zoals Exchange. 

* **[Beveiligingsbeheerder](#security-administrator)**: gebruikers met deze rol hebben alle alleen-lezen machtigingen van de rol beveiligingslezer, plus de mogelijkheid voor het beheren van configuratie voor beveiliging-gerelateerde services: Azure Active Directory Identity Protection Azure Information Protection, Privileged Identity Management, en Office 365-beveiliging en compliance. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
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

* **[Skype voor bedrijven / Lync-servicebeheerder](#lync-service-administrator)**: gebruikers met deze rol hebben algemene machtigingen in Microsoft Skype voor bedrijven, wanneer de service aanwezig is, evenals Skype-specifieke gebruikerskenmerken in Azure Active beheren De map. Bovendien geeft deze rol de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over de Skype voor bedrijven-beheerdersrol](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5).

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Lync-servicebeheerder'. Het 'Skype voor bedrijven-servicebeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >

* **[Beheerder van gebruikersaccounts](#user-account-administrator)**: gebruikers met deze rol kunnen maken en beheren van alle aspecten van gebruikers en groepen. Daarnaast bevat deze rol de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Er zijn enkele beperkingen zijn van toepassing. Deze rol kunnen bijvoorbeeld niet verwijderen van een globale beheerder. Gebruiker accountbeheerders kunnen wachtwoorden wijzigen voor gebruikers, helpdeskbeheerders en andere alleen beheerders van gebruikersaccount.

| Kan doen | Niet mogelijk is |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Wachtwoorden wijzigen voor gebruikers, helpdeskbeheerders en andere alleen beheerders van gebruikersaccount</p><p>Gebruikersweergaven maken en beheren</p><p>Maken, bewerken en verwijderen van gebruikers en groepen beheren van gebruikerslicenties, met beperkingen. Hij of zij kan niet verwijderen van een globale beheerder of andere beheerders maken.</p> |<p>Facturering en aankopen bewerkingen voor Office-producten</p><p>Domeinen beheren</p><p>Bedrijfsinformatie beheren</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor Authentication-verificatie</p><p>Auditlogboeken weergeven</p> |

## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit Azure AD in de toekomst.

* Ad-hoclicentiebeheerder
* Toevoegen
* Apparaatbeheerders
* Gebruikers van apparaten
* Maker van via e-mail geverifieerde gebruikers
* Postvakbeheerder
* Werkplekapparaat toevoegen


### <a name="to-add-a-colleague-as-a-global-administrator"></a>Een collega toevoegen als een globale beheerder

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder of de bevoorrechte rol beheerder voor de tenant-directory.

   ![Azure AD-beheercentrum te openen](./media/directory-assign-admin-roles/active-directory-admin-center.png)

2. Selecteer **gebruikers**.

3. De gebruiker die u wilt aanwijzen als een globale beheerder en open de blade voor die gebruiker niet vinden.

4. Selecteer op de blade van de gebruiker, **maprol**.
 
5. Selecteer op de blade van de rol directory de **hoofdbeheerder** rol, en op te slaan.

## <a name="detailed-azure-active-directory-permissions"></a>Gedetailleerde machtigingen voor Azure Active Directory
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory die aan elke rol. Sommige rollen, zoals de hoofdbeheerder mogelijk extra machtigingen in Microsoft-services outide van Azure Active Directory.


### <a name="application-administrator"></a>Toepassingsbeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Werk de standaardeigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Werk de eigenschap Applications.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Werk de eigenschap Applications.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Maak AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Verwijder AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Werk de standaardeigenschappen bij voor AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Werk de standaardeigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Werk de eigenschap Policies.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Kan namens alle gebruikers instemmen met alle resources, met uitzondering van Azure Active Directory (Azure AD Graph en Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Maak ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Verwijder ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Werk de standaardeigenschappen voor ServicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Werk de eigenschap ServicePrincipals.AppRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Werk de eigenschap ServicePrincipals.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Werk de eigenschap ServicePrincipals.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Werk de eigenschap ServicePrincipals.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lees Azure AD-rapporten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="application-developer"></a>Toepassingsontwikkelaar
Toepassingsregistraties kunt maken, onafhankelijk van de **gebruikers kunnen toepassingen registreren** instelling.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Application/CreateAsOwner | Maak toepassingen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/AppRoleAssignment/CreateAsOwner | Maak AppRoleAssignments in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/OAuth2PermissionGrant/CreateAsOwner | Maak OAuth2PermissionGrants in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/ServicePrincipal/CreateAsOwner | Maak ServicePrincipals in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kan algemene taken met betrekking tot facturering uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Werk de standaardeigenschappen voor Organisaties bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Werk de eigenschap Organizations.TrustedCAsForPasswordlessAuth bij in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.billing/AllEntities/AllActions | Beheer alle aspecten van Office 365-facturering. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-application-administrator"></a>Beheerder van de cloudtoepassing
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren, behalve App Proxy.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Werk de standaardeigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Werk de eigenschap Applications.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Werk de eigenschap Applications.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Maak AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Verwijder AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Werk de standaardeigenschappen bij voor AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Werk de standaardeigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Werk de eigenschap Policies.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Kan namens alle gebruikers instemmen met alle resources, met uitzondering van Azure Active Directory (Azure AD Graph en Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Maak ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Verwijder ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Werk de standaardeigenschappen voor ServicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Werk de eigenschap ServicePrincipals.AppRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Werk de eigenschap ServicePrincipals.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Werk de eigenschap ServicePrincipals.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Werk de eigenschap ServicePrincipals.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lees Azure AD-rapporten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="company-administrator"></a>Bedrijfsbeheerder
Kan alle aspecten beheren van Azure AD en Microsoft-services die Azure AD-identiteiten gebruiken. In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Company Administrator". Het 'Globale beheerder' is in de [Azure-portal](https://portal.azure.com).

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/AllActions/AllProperties | Maak en verwijder AdministrativeUnits en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Application/AllActions/AllProperties | Maak en verwijder toepassingen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/AppRoleAssignment/AllActions/AllProperties | Maak en verwijder AppRoleAssignments en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/CollaborationSpace/AllActions/AllProperties | Maak en verwijder CollaborationSpaces en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Contact/AllActions/AllProperties | Maak en verwijder contactpersonen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Device/AllActions/AllProperties | Maak en verwijder apparaten en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/DirectoryRole/AllActions/AllProperties | Maak en verwijder DirectoryRoles en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/DirectoryRoleTemplate/AllActions/AllProperties | Maak en verwijder DirectoryRoleTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/DirectorySetting/AllActions/AllProperties | Maak en verwijder DirectorySettings en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/DirectorySettingTemplate/AllActions/AllProperties | Maak en verwijder DirectorySettingTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Domain/AllActions/AllProperties | Maak en verwijder domeinen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Group/AllActions/AllProperties | Maak en verwijder groepen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/LoginTenantBranding/AllActions/AllProperties | Maak en verwijder LoginTenantBrandings en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/OAuth2PermissionGrant/AllActions/AllProperties | Maak en verwijder OAuth2PermissionGrants en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Policy/AllActions/AllProperties | Maak en verwijder beleid en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllWithDirectory | Kan namens alle gebruikers instemmen met alle resources, inclusief Azure Active Directory (Azure AD Graph en Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/AllActions/AllProperties | Maak en verwijder ServicePrincipals en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Organization/AllActions/AllProperties | Maak en verwijder Organisaties en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/User/AllActions/AllProperties | Maak en verwijder gebruikers en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.aadconnect/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.aadconnect en werk deze bij. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.billing/AllEntities/AllActions | Beheer alle aspecten van Office 365-facturering. |
| microsoft.aad.compliance/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Compliancecentrum en werk deze bij. |
| microsoft.aad.directorysync/AllEntities/AllActions | Voer alle acties uit in Azure AD Connect. |
| microsoft.aad.lockbox/AllEntities/AllActions | Beheer alle aspecten van de service Lockbox. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Beheer alle aspecten van de Privileged Role Management-service. |
| microsoft.aad.reports/AllEntities/AllActions | Lees en configureer Azure AD-rapporten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.crm/AllEntities/AllActions | Beheer alle aspecten van Dynamics 365. |
| microsoft.exchange/AllEntities/AllActions | Beheer alle aspecten van Exchange Online. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Beheer alle aspecten van Information Protection. |
| microsoft.intune/AllEntities/AllActions | Beheer alle aspecten van Intune. |
| microsoft.powerbi/AllEntities/AllActions | Beheer alle aspecten van Power BI. |
| microsoft.protectioncenter/AllEntities/AllActions | Beheer Office 365 Protection Center. |
| microsoft.sharepoint/AllEntities/AllActions | Beheer SharePoint Online. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Beheer Skype voor Bedrijven Online. |

### <a name="compliance-administrator"></a>Compliancebeheerder
Kan nalevingsconfiguratie en -rapporten lezen en beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.compliance/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Compliancecentrum en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.exchange/Compliance/AllActions | Beheer naleving in Exchange Online. |
| microsoft.sharepoint/Compliance/AllActions | Beheer naleving in SharePoint Online. |
| microsoft.skypeforbusiness/Compliance/AllActions | Beheer naleving in Skype voor Bedrijven Online. |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder
Kan de mogelijkheden van voorwaardelijke toegang beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/ConditionalAccessPolicy/Create | Maak ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Delete | Verwijder ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read | Lees de standaardeigenschappen voor ConditionalAccessPolicys in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/Owners | Lees de eigenschap ConditionalAccessPolicys.Owners in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/PolicyAppliedTo | Lees de eigenschap ConditionalAccessPolicys.PolicyAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update | Werk de standaardeigenschappen voor ConditionalAccessPolicys bij in Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update/Owners | Werk de eigenschap ConditionalAccessPolicys.Owners bij in Azure Active Directory. |

### <a name="device-administrators"></a>Apparaatadministrators

Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaatobjecten in Azure Active Directory.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

### <a name="directory-readers"></a>Adreslijstlezers
Basic directory-informatie kan worden gelezen. Voor het verlenen van toegang tot toepassingen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/Read | Lees de standaardeigenschappen voor AdministrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/AdministrativeUnit/Read/Members | Lees de eigenschap AdministrativeUnits.Members in Azure Active Directory. |
| microsoft.aad.directory/Application/Read | Lees de standaardeigenschappen voor toepassingen in Azure Active Directory. |
| microsoft.aad.directory/Application/Read/Owners | Lees de eigenschap Applications.Owners in Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read | Lees de standaardeigenschappen voor CollaborationSpaces in Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read/Owners | Lees de eigenschap CollaborationSpaces.Owners in Azure Active Directory. |
| microsoft.aad.directory/Contact/Read | Lees de standaardeigenschappen voor contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Read/MemberOf | Lees de eigenschap Contacts.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/Device/Read | Lees de standaardeigenschappen voor apparaten in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/MemberOf | Lees de eigenschap Devices.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredOwners | Lees de eigenschap Devices.RegisteredOwners in Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredUsers | Lees de eigenschap Devices.RegisteredUsers in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read | Lees de standaardeigenschappen voor DirectoryRoles in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/EligibleMembers | Lees de eigenschap DirectoryRoles.EligibleMembers in Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/Members | Lees de eigenschap DirectoryRoles.Members in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Read | Lees de standaardeigenschappen voor DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/Read | Lees de standaardeigenschappen voor DirectorySettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/Domain/Read | Lees de standaardeigenschappen voor domeinen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/AppRoleAssignments | Lees de eigenschap Groups.AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/MemberOf | Lees de eigenschap Groups.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Members | Lees de eigenschap Groups.Members in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Owners | Lees de eigenschap Groups.Owners in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Settings | Lees de eigenschap Groups.Settings in Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/Read | Lees de standaardeigenschappen voor OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lees de standaardeigenschappen voor ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lees de eigenschap ServicePrincipals.AppRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lees de eigenschap ServicePrincipals.AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lees de eigenschap ServicePrincipals.DefaultPolicy in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lees de eigenschap ServicePrincipals.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lees de eigenschap ServicePrincipals.OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lees de eigenschap ServicePrincipals.Owners in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lees de eigenschap ServicePrincipals.OwnedObjects in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read | Lees de standaardeigenschappen voor Organisaties in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lees de standaardeigenschappen voor Gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lees de eigenschap Users.AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lees de eigenschap Users.DirectReports in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lees de eigenschap Users.InvitedBy in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lees de eigenschap Users.InvitedUsers in Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lees de eigenschap Users.Manager in Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lees de eigenschap Users.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lees de eigenschap Users.OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lees de eigenschap Users.OwnedDevices in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lees de eigenschap Users.OwnedObjects in Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lees de eigenschap Users.RegisteredDevices in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Synchronisatie van Active Directory-Accounts
Alleen gebruikt door Azure AD Connect-service.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Policy/Create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read | Lees de standaardeigenschappen voor beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/Owners | Lees de eigenschap Policies.Owners in Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/PolicyAppliedTo | Lees de eigenschap Policies.PolicyAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Werk de standaardeigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Werk de eigenschap Policies.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Create | Maak ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lees de standaardeigenschappen voor ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lees de eigenschap ServicePrincipals.AppRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lees de eigenschap ServicePrincipals.AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lees de eigenschap ServicePrincipals.DefaultPolicy in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lees de eigenschap ServicePrincipals.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lees de eigenschap ServicePrincipals.OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lees de eigenschap ServicePrincipals.Owners in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lees de eigenschap ServicePrincipals.OwnedObjects in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Werk de standaardeigenschappen voor ServicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Werk de eigenschap ServicePrincipals.AppRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Werk de eigenschap ServicePrincipals.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Werk de eigenschap ServicePrincipals.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Werk de eigenschap ServicePrincipals.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/DirSync | Werk de eigenschap Update Organizations.DirSync bij in Azure Active Directory. |
| microsoft.aad.directorysync/AllEntities/AllActions | Voer alle acties uit in Azure AD Connect. |

### <a name="directory-writer"></a>Schrijver van de Directory
Kan lezen en schrijven van basic directory-informatie. Voor het verlenen van toegang tot toepassingen

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/DirectorySetting/Create | Maak DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Delete | Verwijder DirectorySettings in Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Update | Werk de standaardeigenschappen voor DirectorySettings bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Werk de standaardeigenschappen voor Groepen bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Werk de eigenschap Groups.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Werk de eigenschap Groups.Members bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Werk de eigenschap Groups.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Werk de eigenschap Groups.Settings bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Werk de standaardeigenschappen voor Gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Werk de eigenschap Users.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Werk de eigenschap Users.Manager bij in Azure Active Directory. |

### <a name="dynamics-365-service-administrator"></a>Dynamics 365-servicebeheerder
Kan alle aspecten van het Dynamics 365-product beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.crm/AllEntities/AllActions | Beheer alle aspecten van Dynamics 365. |

### <a name="exchange-service-administrator"></a>Exchange Service-beheerder
Kan alle aspecten van het product Exchange beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.exchange/AllEntities/AllActions | Beheer alle aspecten van Exchange Online. |

### <a name="guest-inviter"></a>Gastuitnodiging
Kunt uitnodigen van gastgebruikers ook kunnen onafhankelijk van de **leden kunnen gasten uitnodigen** instelling.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/User/InviteGuest | Nodig gastgebruikers uit in Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lees de standaardeigenschappen voor Gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lees de eigenschap Users.AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lees de eigenschap Users.DirectReports in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lees de eigenschap Users.InvitedBy in Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lees de eigenschap Users.InvitedUsers in Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lees de eigenschap Users.Manager in Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lees de eigenschap Users.MemberOf in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lees de eigenschap Users.OAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lees de eigenschap Users.OwnedDevices in Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lees de eigenschap Users.OwnedObjects in Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lees de eigenschap Users.RegisteredDevices in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdeskbeheerder
Kan wachtwoorden voor niet-beheerders en Helpdesk-medewerkers opnieuw instellen.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordHelpdeskScope | Bijwerken van wachtwoorden voor bepaalde beheerders en andere helpdeskbeheerders in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |

### <a name="information-protection-administrator"></a>Information Protection-beheerder
Kan alle aspecten van het product Azure Information Protection beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Beheer alle aspecten van Information Protection. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Contact/Create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Werk de standaardeigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/Device/Create | Maak apparaten in Azure Active Directory. |
| microsoft.aad.directory/Device/Delete | Verwijder apparaten in Azure Active Directory. |
| microsoft.aad.directory/Device/Update | Werk de standaardeigenschappen voor apparaten bij in Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredOwners | Werk de eigenschap Devices.RegisteredOwners bij in Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredUsers | Werk de eigenschap Devices.RegisteredUsers bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/Group/Delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lees de eigenschap Groups.HiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Werk de standaardeigenschappen voor Groepen bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Werk de eigenschap Groups.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Werk de eigenschap Groups.Members bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Werk de eigenschap Groups.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Werk de eigenschap Groups.Settings bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Werk de standaardeigenschappen voor Gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Werk de eigenschap Users.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Werk de eigenschap Users.Manager bij in Azure Active Directory. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.intune/AllEntities/AllActions | Beheer alle aspecten van Intune. |


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
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Beheer Skype voor Bedrijven Online. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in het Office 365-berichtencentrum lezen. 

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.accessmessagecenter/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Berichtencentrum en werk deze bij. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.directory/Contact/Create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Werk de standaardeigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Werk de eigenschap Groups.Members bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Werk de eigenschap Groups.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Werk de standaardeigenschappen voor Gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Werk de eigenschap Users.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Werk de eigenschap Users.Manager bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserScope | Bijwerken van wachtwoorden voor niet-beheerders in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.directory/Contact/Create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Werk de standaardeigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions | Maak en verwijder domeinen en lees alle standaardeigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/Group/Create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Werk de eigenschap Groups.Members bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Werk de standaardeigenschappen voor Organisaties bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Werk de eigenschap Organizations.TrustedCAsForPasswordlessAuth bij in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Werk de standaardeigenschappen voor Gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Werk de eigenschap Users.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Werk de eigenschap Users.Manager bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Password | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="power-bi-service-administrator"></a>Servicebeheerder van Power BI
Kan alle aspecten van het Power BI-product beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.powerbi/AllEntities/AllActions | Beheer alle aspecten van Power BI. |

### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kunnen roltoewijzingen beheren in Azure AD

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/DirectoryRole/Update | Werk de standaardeigenschappen voor DirectoryRoles bij in Azure Active Directory. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Beheer alle aspecten van de Privileged Role Management-service. |

### <a name="reports-reader"></a>Rapportenlezer
Kan rapporten met betrekking tot aanmeldingen en controles lezen.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.reports/AllEntities/Read | Lees Azure AD-rapporten. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| Microsoft.Office365.usagereports/AllEntities/Read | Lees Office 365-gebruiksrapporten. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligingsgegevens en -rapporten lezen

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Werk de eigenschap Applications.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Werk de standaardeigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Werk de eigenschap Policies.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Werk de eigenschap ServicePrincipals.DefaultPolicy bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.privilegedrolemanagement/AllEntities/Read | Lees alle aspecten van Office Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.protectioncenter/AllEntities/Update | Beheer Office 365 Protection Center. |

### <a name="security-reader"></a>Beveiligingslezer
Kan beveiligingsgegevens en -rapporten lezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol de aanvullende machtigingen overneemt van de rol Adreslijstlezers toe.
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.privilegedidentitymanagement/AllEntities/Read | Lees alle aspecten van Office Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lees alle aspecten van Office 365 Protection Center. |

### <a name="service-support-administrator"></a>Beheerder serviceondersteuning
Kan gegevens over de servicestatus lezen en ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |

### <a name="sharepoint-service-administrator"></a>SharePoint Service-beheerder
Kan alle aspecten van de SharePoint-service beheren.

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de beschrijving van de rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.sharepoint/AllEntities/AllActions | Beheer SharePoint Online. |

### <a name="user-account-administrator"></a>Beheerder van gebruikersaccounts
Kan alle aspecten van gebruikers en groepen beheren

  > [!NOTE]
  > Deze rol aanvullende machtigingen overneemt de [gebruikersrol](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/AppRoleAssignment/Create | Maak AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Verwijder AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Werk de standaardeigenschappen bij voor AppRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/Contact/Create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Werk de standaardeigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/Group/Delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lees de standaardeigenschappen voor Groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lees de eigenschap Groups.HiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Werk de standaardeigenschappen voor Groepen bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Werk de eigenschap Groups.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Werk de eigenschap Groups.Members bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Werk de eigenschap Groups.Owners bij in Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Werk de eigenschap Groups.Settings bij in Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lees de eigenschap Organizations.TrustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Create | Maak gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/User/Update | Werk de standaardeigenschappen voor Gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Werk de eigenschap Users.AppRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Werk de eigenschap Users.Manager bij in Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserAcctAdminScope | Bijwerken van wachtwoorden voor bepaalde beheerders, helpdeskbeheerders en andere beheerders gebruikersaccount in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.accessservice/AllEntities/AllActions | Maak en verwijder alle resources en lees de standaardeigenschappen in Azure Access Control en werk deze bij. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lees en configureer de Office 365-servicestatus. |

## <a name="next-steps"></a>Volgende stappen

* Als u meer wilt weten over het wijzigen van de beheerders van een Azure-abonnement, ziet u [Azure-beheerdersrollen toevoegen of wijzigen](../../billing/billing-add-change-azure-subscription-administrator.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
