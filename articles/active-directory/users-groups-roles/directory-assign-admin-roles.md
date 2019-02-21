---
title: Machtigingen - Azure Active Directory en beschrijvingen van de rol Administrator | Microsoft Docs
description: Een beheerdersrol kunt gebruikers toevoegen, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren.
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/16/19
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1496653c319b4732614cd1c8148afb5c5b06215
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456738"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Rol beheerdersmachtigingen in Azure Active Directory

Met Azure Active Directory (Azure AD), kunt u beperkte beheerders functies vervullen in minder bevoegdheden rollen opgeven. Beheerders worden aangewezen in de Azure AD-portal om uit te voeren taken, zoals het toevoegen of wijzigen, gebruikers, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren en beheren van domeinnamen. De standaardmachtigingen van de gebruiker kunnen alleen in de gebruikersinstellingen worden gewijzigd in Azure AD.

De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. Alleen globale beheerders en beheerders met bevoorrechte rol kunt beheerdersrollen delegeren. U wordt aangeraden dat u deze rol aan slechts een paar mensen in uw bedrijf toewijzen om het risico voor uw bedrijf.


## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen

Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [weergeven en toewijzen beheerdersrollen in Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Beschikbare rollen

De volgende beheerdersrollen zijn beschikbaar:

* **[Toepassingsbeheerder](#application-administrator)**: Gebruikers in deze rol kunnen maken en beheren van alle aspecten van zakelijke toepassingen, registratie en instellingen van de toepassingsproxy. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de toepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Toepassingsontwikkelaar](#application-developer)**: Gebruikers in deze rol kunnen toepassingsregistraties maken wanneer de 'Gebruikers kunnen toepassingen registreren' is ingesteld op Nee. Deze rol kan ook leden toe te staan hun eigen namens wanneer de 'Gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens in hun naam' is ingesteld op Nee. Leden van deze rol worden toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Authentication-beheerder](#authentication-administrator)**: Gebruikers met deze rol kunnen instellen of referenties in niet-wachtwoord opnieuw instellen. Verificatie-beheerders kunnen afdwingen dat gebruikers kunnen opnieuw worden geregistreerd op basis van bestaande niet-wachtwoord referentie (bijvoorbeeld MFA, FIDO) en intrekken 'MFA herinneren op het apparaat', dat u wordt gevraagd voor MFA op de volgende aanmelding van andere gebruikers die niet-beheerders of leden van de alleen volgende rollen:
  * Verificatiebeheerder
  * Adreslijstlezers
  * Gastuitnodiging
  * Berichtencentrum-lezer
  * Rapportenlezer
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen referenties wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van de referenties van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de verificatie-beheerders. Via dit pad een Authentication-beheerder kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

* **[Factureringsbeheerder](#billing-administrator)**: Doet aankopen, beheert abonnementen, beheert ondersteuningstickets en controleert de status van de service.

* **[Beheerder van de cloudtoepassing](#cloud-application-administrator)**: Gebruikers in deze rol hebben dezelfde machtigingen als de rol beheerder van de toepassing, met uitzondering van de mogelijkheid voor het beheren van de toepassingsproxy. Deze rol hebben de mogelijkheid om te maken en beheren van alle aspecten van bedrijfstoepassingen en registratie. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de Cloudtoepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Cloud-Apparaatbeheerder](#cloud-device-administrator)**: Gebruikers in deze rol kunnen inschakelen, uitschakelen, en apparaten verwijderen in Azure AD en Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal lezen. De rol verleent machtigingen voor het beheren van andere eigenschappen op het apparaat.

* **[Beheerder voor naleving](#compliance-administrator)**: Gebruikers met deze rol hebben machtigingen voor het beheren van functies met betrekking tot naleving in de compliancecentrum Microsoft 365, Microsoft 365-beheercentrum, Azure, en Office 365-beveiliging en compliance. Gebruikers kunnen ook alle functies in de Exchange-beheercentrum, Teams en Skype voor bedrijven-beheercentrum beheren en maken van ondersteuningstickets voor Azure en Microsoft 365. Meer informatie vindt u op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  In | Kan doen
  ----- | ----------
  [Microsoft 365 compliancecentrum](https://protection.microsoft.com) | Beveiligen en beheren van gegevens van uw organisatie via Microsoft 365-services<br>Naleving-waarschuwingen beheren
  [Voor naleving](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Bijhouden, toewijzen en controleer of de activiteiten van de naleving van regelgeving van uw organisatie
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Gegevensbeheer beheren<br>Wettelijke informatie en gegevens onderzoek uitvoeren<br>Aanvraag voor het onderwerp van gegevens beheren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle Intune-controlegegevens weergeven
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren<br>Kunt maken en wijzigen van de beleidsregels voor bestanden en governance-acties voor toestaan<br> De ingebouwde rapporten onder beheer van de gegevens kunt weergeven

<!--* **[Compliance Data Administrator](#compliance-data-administrator)**: Users with this role have permissions to protect and track data in the Microsoft 365 compliance center, Microsoft 365 admin center, and Azure. Users can also manage all features within the Exchange admin center, Compliance Manager, and Teams & Skype for Business admin center and create support tickets for Azure and Microsoft 365.

  In | Can do
  ----- | ----------
  [Microsoft 365 compliance center](https://protection.microsoft.com) | Monitor compliance-related policies across Microsoft 365 services<br>Manage compliance alerts
  [Compliance Manager](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Track, assign, and verify your organization's regulatory compliance activities
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Manage data governance<br>Perform legal and data investigation<br>Manage Data Subject Request
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | View all Intune audit data
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Has read-only permissions and can manage alerts<br>Can create and modify file policies and allow file governance actions<br> Can view all the built-in reports under Data Management
-->
* **[Beheerder van voorwaardelijke toegang](#conditional-access-administrator)**: Gebruikers met deze rol kunnen Azure Active Directory-instellingen voor voorwaardelijke toegang beheren.
  > [!NOTE]
  > Voor het implementeren van Exchange ActiveSync-beleid voor voorwaardelijke toegang in Azure, moet de gebruiker ook een globale beheerder zijn.
  
* **[Klanten-Lockbox toegang goedkeurder](#customer-lockbox-access-approver)**: Beheert [aanvragen van klanten-Lockbox](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) in uw organisatie. Ze kunnen ontvangen van e-mailmeldingen voor aanvragen van klanten-Lockbox goedkeuren en weigeren van de Microsoft 365-beheercentrum. Ze kunnen ook de functie voor klanten-Lockbox inschakelen of uitschakelen. Alleen globale beheerders kunnen de wachtwoorden van personen die zijn toegewezen aan deze rol opnieuw instellen.
<!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Apparaatbeheerders](#device-administrators)**: Deze functie is beschikbaar voor toewijzing alleen als een lokale beheerder in [apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaatobjecten in Azure Active Directory. 

* **[Adreslijstlezers](#directory-readers)**: Dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Directory-Accounts voor synchronisatie](#directory-synchronization-accounts)**: Gebruik geen. Deze rol is wordt automatisch toegewezen aan de Azure AD Connect-service en niet bedoeld of ondersteund voor ander gebruik.

* **[Schrijvers van mappen](#directory-writers)**: Dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Dynamics 365-beheerder / CRM-beheerder](#crm-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Dynamics 365 Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [de rol admin gebruiken voor het beheren van uw tenant](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Dynamics 365-servicebeheerder'. Het 'Dynamics 365-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Exchange-beheerder](#exchange-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft CRM Online, wanneer de service aanwezig is. Heeft ook de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Exchange Service Administrator". Het 'Exchange-beheerder' is in de [Azure-portal](https://portal.azure.com). Het 'Exchange Online-beheerder' is in de [Exchange-beheercentrum](https://go.microsoft.com/fwlink/p/?LinkID=529144). 


* **[Globale beheerder / Company Administrator](#company-administrator)**: Gebruikers met deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die gebruikmaken van Azure Active Directory-identiteiten, zoals Microsoft 365 security center, Microsoft 365 compliancecentrum, Exchange Online, SharePoint Online, en Skype voor bedrijven Online. De persoon die zich aanmeldt voor de Azure Active Directory-tenant wordt globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er is meer dan één globale beheerder in uw bedrijf. Globale beheerders kunnen het wachtwoord voor elke gebruiker en alle andere beheerders opnieuw instellen.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Company Administrator". Het 'Globale beheerder' is in de [Azure-portal](https://portal.azure.com).
  >
  >

* **[Gastuitnodiging](#guest-inviter)**: Gebruikers in deze rol kunnen uitnodigingen voor Azure Active Directory B2B Gast beheren wanneer de **leden kunnen uitnodigen** gebruikersinstelling is ingesteld op Nee. Meer informatie over B2B-samenwerking bij [over Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Deze omvatten geen andere machtigingen.

* **[Information Protection-beheerder](#information-protection-administrator)**: Gebruikers met deze rol hebben alle machtigingen in de Azure Information Protection-service. Deze rol kan labels voor de Azure Information Protection-beleid configureren, beveiligingssjablonen beheren en beveiliging activeren. Deze rol verleent alle machtigingen in Identity Protection Center, Privileged Identity Management, Monitor Office 365-servicestatus of Office 365 Centrum voor beveiliging en naleving.

* **[Intune-beheerder](#intune-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Intune Online, wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren. Meer informatie op [rollen gebaseerd toegangsbeheer (RBAC) met Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Intune-servicebeheerder '. Het 'Intune-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Licentiebeheerder](#license-administrator)**: Gebruikers in deze rol kunnen toevoegen, verwijderen en update licentie toewijzingen aan gebruikers, groepen (met Groepslicenties) en de gebruikslocatie van gebruikers beheren. De rol heeft niet de mogelijkheid om te kopen of beheren van abonnementen, maken of beheren van groepen, of maken of beheren van gebruikers buiten de gebruikslocatie verlenen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Berichtencentrum-lezer](#message-center-reader)**: Gebruikers in deze rol kunnen controleren, meldingen en de gezondheid van advies-updates in [Office 365-berichtencentrum](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie op de geconfigureerde services zoals Exchange, Intune en Microsoft Teams. Berichtencentrum-lezer wekelijkse e-mailbericht verwerkingen van berichten, updates, ontvangen en message center berichten in Office 365 kunnen delen. In Azure AD, wordt gebruikers die zijn toegewezen aan deze rol alleen alleen-lezen toegang hebben op Azure AD-services, zoals gebruikers en groepen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Laag1-ondersteuning voor partner](#partner-tier1-support)**: Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Laag2-ondersteuning voor partner](#partner-tier2-support)**: Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Wachtwoordbeheerder / Helpdeskbeheerder](#helpdesk-administrator)**: Gebruikers met deze rol kunnen wachtwoorden wijzigen, vernieuwingstokens ongeldig te maken, serviceaanvragen beheren en servicestatus controleren. Ongeldig vernieuwingstoken zorgt ervoor dat de gebruiker zich opnieuw aanmelden. Helpdesk-beheerders kunnen wachtwoorden opnieuw instellen en vernieuwen van tokens van andere gebruikers die niet-beheerders of leden van de volgende rollen alleen ongeldig te maken:
  * Adreslijstlezers
  * Gastuitnodiging
  * Helpdeskbeheerder
  * Berichtencentrum-lezer
  * Rapportenlezer
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de Helpdesk-medewerkers. Via dit pad die een Helpdesk-beheerder kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Helpdesk-beheerder'. 'Wachtwoordbeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  
* **[Power BI Administrator](#power-bi-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Online Power BI, wanneer de service aanwezig is, evenals de mogelijkheid om ondersteuningstickets te beheren en de servicestatus te controleren. Meer informatie op [inzicht in de Power BI-beheerdersrol](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Power BI-servicebeheerder '. ' Power BI-beheerder ' is in de [Azure-portal](https://portal.azure.com).

* **[Rol van beheerder in beschermde modus](#privileged-role-administrator)**: Gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van het lidmaatschap van alle Azure AD-rollen, met inbegrip van de rol globale beheerder. Deze rol bevat geen andere bevoegde mogelijkheden in Azure AD, zoals het maken of bijwerken van gebruikers. Echter kunnen gebruikers zijn toegewezen aan deze rol verlenen zichzelf of andere aanvullende bevoegdheden door aanvullende rollen toewijzen.

* **[Lezer-rapporten](#reports-reader)**: Gebruikers met deze rol kunnen reporting-gebruiksgegevens weergeven en het dashboard rapporten in Office 365-beheercentrum en de acceptatie-context pack in Power BI. Bovendien de rol biedt toegang tot aanmelden-rapporten en -activiteit in Azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. De gebruiker die is toegewezen aan de rol Rapportenlezer toegang alleen relevante gebruik en acceptatie metrische gegevens. Ze geen geen admin-machtigingen voor het configureren van instellingen of toegang tot die het beheercentrums productspecifieke zoals Exchange. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Beveiligingsbeheerder](#security-administrator)**: Gebruikers met deze rol hebben machtigingen voor het beheren van beveiligingsfuncties in de Microsoft 365 security center, Azure Active Directory Identity Protection, Azure Information Protection, en Office 365-beveiliging en compliance. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.microsoft.com) | Beveiligingsbeleid controleren in Microsoft 365-services<br>Beveiligingsrisico's en waarschuwingen beheren<br>Rapporten weergeven
  Identity Protection Center | Alle machtigingen van de rol van Beveiligingslezer<br>Bovendien de mogelijkheid om uit te voeren van alle Identity Protection Center-bewerkingen, met uitzondering van wachtwoorden opnieuw instellen
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Alle machtigingen van de rol van Beveiligingslezer<br>**Kan geen** rollidmaatschappen voor Azure AD- of -instellingen beheren
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid beheren<br>Weergeven, onderzoeken en direct reageren op bedreigingen<br>Rapporten weergeven
  Azure Advanced Threat Protection | Bewaken van en reageren op verdachte activiteit
  Windows Defender ATP en EDR | Rollen toewijzen<br>Computergroepen beheren<br>Detectie van bedreigingen eindpunt en geautomatiseerd herstel configureren<br>Weergeven, onderzoeken en reageren op waarschuwingen
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Weergaven gebruiker, apparaat, inschrijving, configuratie en informatie over toepassingen<br>Kan geen wijzigingen aanbrengen aan Intune
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Beheerders toevoegen, beleidsregels en instellingen, logboeken te uploaden en beheeracties uitvoeren toevoegen
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Kan weergeven beveiligingsbeleid, security-status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen
  [Office 365-servicestatus](https://docs.microsoft.com/office365/enterprise/view-service-health) | Bekijk de status van Office 365-services

<!--* **[Security operator](#security-operator)**: Users with this role can manage alerts and have global read-only access on security-related feature, including all information in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, as well as the ability to read Azure Active Directory sign-in reports and audit logs, and in Office 365 Security & Compliance Center.

  In | Can do
  --- | ---
  [Microsoft 365 security center](https://protection.microsoft.com) | All permissions of the Security Reader role<br>View, investigate, and respond to security threats alerts
  Identity Protection Center | All permissions of the Security Reader role<br>Additionally, the ability to perform all Identity Protection Center operations except for resetting passwords
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | All permissions of the Security Reader role
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  Windows Defender ATP and EDR | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | All permissions of the Security Reader role
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | All permissions of the Security Reader role
  [Office 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health) | View the health of Office 365 services
-->
* **[Beveiligingslezer](#security-reader)**: Gebruikers met deze rol hebben globale alleen-lezen toegang op functies met betrekking tot beveiliging, met inbegrip van alle gegevens in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, evenals de mogelijkheid om te lezen Azure Active Directory-aanmeldingsrapporten en controlelogboeken, en in Office 365-beveiligings- en Compliancecentrum. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.microsoft.com) | Beveiligingsbeleid voor Microsoft 365-services weergeven<br>Weergave beveiligingsrisico's en waarschuwingen<br>Rapporten weergeven
  Identity Protection Center | Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<br><ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde beveiliging tegen bedreigingen<li>Anti-phishing<li>Mailflow regels
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Is alleen-lezen toegang tot alle gegevens in Azure AD PIM opgehaald: Beleid en rapporten voor Azure AD-roltoewijzingen wordt beveiliging beoordeelt en toegang tot gegevens en rapporten voor scenario's behalve Azure AD-roltoewijzing in de toekomst te lezen.<br>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengen. In de PIM-portal of via PowerShell, kan iemand zich in deze rol aanvullende rollen (bijvoorbeeld: globale beheerder of beheerder met bevoorrechte rol), activeren als de gebruiker een komen in aanmerking voor deze.
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid bekijken<br>Weergeven en bedreigingen te onderzoeken<br>Rapporten weergeven
  Windows Defender ATP en EDR | Weergeven en onderzoeken van waarschuwingen
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Weergaven gebruiker, apparaat, inschrijving, configuratie en informatie over toepassingen. Kan geen wijzigingen aanbrengen aan Intune.
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven
  [Office 365-servicestatus](https://docs.microsoft.com/office365/enterprise/view-service-health) | Bekijk de status van Office 365-services

* **[Beheerder serviceondersteuning](#service-support-administrator)**: Gebruikers met deze rol kunnen ondersteuningsaanvragen openen met Microsoft voor Azure- en Office 365-services, en het servicedashboard en berichtencentrum weergeven in Azure Portal en Office 365-beheerportal. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als ' beheerder serviceondersteuning. " Het is 'Servicebeheerder' de [Azure-portal](https://portal.azure.com), het Office 365-beheerportal en de Intune-portal.

* **[SharePoint-beheerder](#sharepoint-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als "SharePoint-servicebeheerder." Het 'SharePoint-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Skype voor bedrijven / Lync beheerder](#lync-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Skype voor bedrijven, wanneer de service aanwezig is, evenals beheren van de kenmerken van de Skype-specifieke gebruiker in Azure Active Directory. Deze rol hebben bovendien de mogelijkheid ondersteuningstickets beheren en servicestatus controleren en de toegang tot de Teams en Skype voor bedrijven-beheercentrum. Het account moet ook een licentie hebben voor Teams of Teams PowerShell-cmdlets kan niet worden uitgevoerd. Meer informatie op [over de Skype voor bedrijven-beheerdersrol](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) en Teams informatie over licenties op [Skype voor bedrijven en Microsoft Teams-Add-on-licentieverlening](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Lync-servicebeheerder'. Het 'Skype voor bedrijven-beheerder' is in de [Azure-portal](https://portal.azure.com/).

* **[Beheerder teams](#teams-service-administrator)**: Gebruikers in deze rol kunnen alle aspecten van de Microsoft Teams-werkbelasting via de Microsoft Teams en Skype voor bedrijven-beheercentrum en de bijbehorende PowerShell-modules beheren. Dit omvat onder andere gebieden, alle beheerprogramma's met betrekking tot de telefoon, chatberichten, vergaderingen en teams zelf. Deze rol hebben bovendien de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren.
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Teams Service Administrator ". Het 'Teams beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Communicatie-beheerder teams](#teams-communications-administrator)**: Gebruikers in deze rol kunnen aspecten van de Microsoft Teams-workload met betrekking tot de spraak- en TAPI beheren. Dit omvat de beheerhulpprogramma's voor de toewijzing van telefoon, spraak- en voldoen aan beleidsregels en volledige toegang tot de aanroep analytics toolset.

* **[Communicatie-ondersteuningstechnicus teams](#teams-communications-support-engineer)**: Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen bekijken aanroep van de volledige gegevens voor alle deelnemers die betrokken zijn. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Communicatie ondersteuning voor gespecialiseerde teams](#teams-communications-support-specialist)**: Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen alleen gebruikersgegevens weergeven in de aanroep voor de specifieke gebruiker dat ze hebt opgezocht. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Beheerder van gebruikersaccounts](#user-account-administrator)**: Gebruikers met deze rol kunnen gebruikers maken en beheren van alle aspecten van gebruikers met enkele beperkingen (Zie hieronder). Gebruikers met deze rol kunnen bovendien maken en beheren van alle groepen. Deze rol omvat ook de mogelijkheid om te maken en beheren van gebruikersweergaven, ondersteuningstickets beheren en servicestatus controleren.

  | | |
  | --- | --- |
  |Algemene machtigingen|<p>Gebruikers en groepen maken</p><p>Gebruikersweergaven maken en beheren</p><p>Office-ondersteuningstickets beheren|
  |<p>Op alle gebruikers, met inbegrip van alle beheerders</p>|<p>Licenties beheren</p><p>Eigenschappen van alle gebruikers, behalve de User Principal Name beheren</p>
  |Alleen op gebruikers die niet-beheerders of beperkte beheerdersrollen in het volgende:<ul><li>Adreslijstlezers<li>Gastuitnodiging<li>Helpdeskbeheerder<li>Berichtencentrum-lezer<li>Rapportenlezer<li>Beheerder van gebruikersaccounts|<p>Verwijderen en herstellen</p><p>Uitschakelen en inschakelen</p><p>Ongeldig vernieuwingstokens</p><p>Eigenschappen van alle gebruikers met inbegrip van de User Principal Name beheren</p><p>Wachtwoord opnieuw instellen</p><p>Apparaatsleutels (FIDO) bijwerken</p>
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan beheerders. Via dit pad voor de beheerder van een gebruiker kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

## <a name="role-permissions"></a>Machtigingen van de rol
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory die aan elke rol. Sommige functies mogelijk extra machtigingen in Microsoft-services buiten Azure Active Directory.

### <a name="application-administrator"></a>Toepassingsbeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Werk de eigenschap applications.audience bij in Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Werk de eigenschap applications.authentication bij in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Werk de eigenschap applications.credentials bij in Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Werk de eigenschap applications.owners bij in Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Werk de eigenschap applications.permissions bij in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Lees de appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="application-developer"></a>Toepassingsontwikkelaar
Kan toepassingsregistraties onafhankelijk van de 'gebruikers kunnen toepassingen registreren' maken instelling.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Maak toepassingen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Maak appRoleAssignments in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Maak oAuth2PermissionGrants in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Maak servicePrincipals in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |

### <a name="authentication-administrator"></a>Verificatiebeheerder
Toegestaan wilt weergeven, instellen en opnieuw instellen van verificatie methode-informatie voor elke gebruiker die geen beheerder.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | De eigenschappen van sterke verificatie bijwerken, bijvoorbeeld MFA-referentiegegevens. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kan algemene taken met betrekking tot facturering uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | Werk de basiseigenschappen voor een organisatie bij in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Werk de eigenschap organization.trustedCAsForPasswordlessAuth bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics-beheerder
Openen en beheren van bureaubladbeheerhulpprogramma's en services, met inbegrip van Intune.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van Desktop Analytics beheren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-application-administrator"></a>Beheerder van de cloudtoepassing
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren, behalve App Proxy.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Werk de eigenschap applications.audience bij in Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Werk de eigenschap applications.authentication bij in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Werk de eigenschap applications.credentials bij in Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Werk de eigenschap applications.owners bij in Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Werk de eigenschap applications.permissions bij in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-device-administrator"></a>Cloud-apparaatbeheerder
Volledige toegang om apparaten te beheren in Azure AD.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Verwijder apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/disable | Schakel apparaten uit in Azure Active Directory. |
| microsoft.aad.directory/devices/enable | Apparaten in Azure Active Directory inschakelen. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="company-administrator"></a>Bedrijfsbeheerder
Kan alle aspecten beheren van Azure AD en Microsoft-services die Azure AD-identiteiten gebruiken.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Alle resources maken en verwijderen en de standaardeigenschappen in microsoft.aad.cloudAppSecurity lezen en bijwerken. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Maak en verwijder administrativeUnits en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/applications/allProperties/allTasks | Maak en verwijder toepassingen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Maak en verwijder appRoleAssignments en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Maak en verwijder contactpersonen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Maak en verwijder contracten en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/devices/allProperties/allTasks | Maak en verwijder apparaten en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Maak en verwijder directoryRoles en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Maak en verwijder directoryRoleTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/domains/allProperties/allTasks | Maak en verwijder domeinen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groups/allProperties/allTasks | Maak en verwijder groepen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Maak en verwijder groupSettings en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Maak en verwijder groupSettingTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Maak en verwijder loginTenantBranding en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Maak en verwijder oAuth2PermissionGrants en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/organization/allProperties/allTasks | Maak en verwijder een organisatie en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/policies/allProperties/allTasks | Maak en verwijder beleid en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Maak en verwijder roleAssignments en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Maak en verwijder roleDefinitions en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Maak en verwijder scopedRoleMemberships en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/serviceAction/activateService | Mag de serviceactie Activateservice uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Mag de serviceactie Disabledirectoryfeature uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Mag de serviceactie Enabledirectoryfeature uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Mag de serviceactie Getavailableextentionproperties uitvoeren in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Maak en verwijder servicePrincipals en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Maak en verwijder subscribedSkus en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/users/allProperties/allTasks | Maak en verwijder gebruikers en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.identityProtection en werk deze bij. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Alle resources lezen in microsoft.azure.advancedThreatProtection. |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.complianceManager/allEntities/allTasks | Beheer alle aspecten van Office 365 Compliancebeheer |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van Desktop Analytics beheren. |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.lockbox/allEntities/allTasks | Beheer alle aspecten van Office 365 Klanten-lockbox |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read | securityMessages lezen in microsoft.office365.messageCenter. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Beheer alle aspecten van Office 365 Protection Center. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Alle resources maken en verwijderen, de standaardeigenschappen in microsoft.office365.securityComplianceCenter lezen en bijwerken. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Alle resources lezen in microsoft.windows.defenderAdvancedThreatProtection. |

### <a name="compliance-administrator"></a>Beheerder voor naleving
Kan nalevingsconfiguratie en -rapporten lezen en beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.complianceManager/allEntities/allTasks | Beheer alle aspecten van Office 365 Compliancebeheer |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder
Kan de mogelijkheden van voorwaardelijke toegang beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |

### <a name="crm-service-administrator"></a>CRM-servicebeheerder
Kan alle aspecten van het Dynamics 365-product beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="customer-lockbox-access-approver"></a>Klanten-Lockbox toegang goedkeurder
Kan Microsoft-ondersteuningsaanvragen voor toegang tot bedrijfsgegevens van klanten goedkeuren. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.lockbox/allEntities/allTasks | Beheer alle aspecten van Office 365 Klanten-lockbox |

### <a name="device-administrators"></a>Apparaatadministrators
Leden van deze rol worden toegevoegd aan de groep lokale beheerders op Azure AD join-apparaten.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen voor groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen voor groupSettingTemplates in Azure Active Directory. |

### <a name="directory-readers"></a>Adreslijstlezers
Basic directory-informatie kan worden gelezen. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Lees de basiseigenschappen van administrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Lees de eigenschap administrativeUnits.members in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/read | Lees de basiseigenschappen voor toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/read | Lees de eigenschap applications.owners in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/read | Lees de eigenschap applications.policies in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/read | Lees de basiseigenschappen voor contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Lees de eigenschap contacts.memberOf in Azure Active Directory. |
| microsoft.aad.directory/contracts/basic/read | Lees de basiseigenschappen voor contracten in Azure Active Directory. |
| microsoft.aad.directory/devices/basic/read | Lees de basiseigenschappen voor apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lees de eigenschap devices.memberOf in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lees de eigenschap devices.registeredOwners in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lees de eigenschap devices.registeredUsers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/basic/read | Lees de basiseigenschappen voor directoryRoles in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lees de eigenschap directoryRoles.eligibleMembers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Lees de eigenschap directoryRoles.members in Azure Active Directory. |
| microsoft.aad.directory/domains/basic/read | Lees de basiseigenschappen voor domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lees de eigenschap groups.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/read | Lees de basiseigenschappen voor groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lees de eigenschap groups.memberOf in Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lees de eigenschap groups.members in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lees de eigenschap groups.owners in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lees de eigenschap groups.settings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen voor groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen voor groupSettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Lees de basiseigenschappen voor oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/organization/basic/read | Lees de basiseigenschappen voor een organisatie in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lees de eigenschap organization.trustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/basic/read | De basiseigenschappen voor roltoewijzingen lezen in Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/basic/read | De basiseigenschappen voor roldefinities lezen in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lees de eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lees de eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen voor servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lees de eigenschap servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lees de eigenschap servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lees de eigenschap servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lees de eigenschap servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lees de eigenschap servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/basic/read | Lees de basiseigenschappen voor subscribedSkus in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lees de eigenschap users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lees de eigenschap users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lees de eigenschap users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lees de eigenschap users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lees de eigenschap users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lees de eigenschap users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lees de eigenschap users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lees de eigenschap users.registeredDevices in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Synchronisatie van Active Directory-Accounts
Alleen gebruikt door Azure AD Connect-service.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Werk de eigenschap organization.dirSync bij in Azure Active Directory. |
| microsoft.aad.directory/policies/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/read | Lees de eigen basiseigenschappen voor beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/read | Lees de eigenschap policies.owners in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Werk de eigenschap policies.owners bij in Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lees de eigenschap policies.policiesAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lees de eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lees de eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen voor servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lees de eigenschap servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lees de eigenschap servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lees de eigenschap servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lees de eigenschap servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lees de eigenschap servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |

### <a name="directory-writers"></a>Schrijvers van mappen
Kan lezen en schrijven van basic directory-informatie. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/update | Werk de basiseigenschappen voor groupSettings bij in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/create | Maak groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Verwijder groupSettings in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |

### <a name="exchange-service-administrator"></a>Exchange Service-beheerder
Kan alle aspecten van het product Exchange beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="guest-inviter"></a>Gastuitnodiging
Kan onafhankelijk van de instelling 'leden kunnen gasten uitnodigen' gastgebruikers uitnodigen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lees de eigenschap users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lees de eigenschap users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Nodig gastgebruikers uit in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lees de eigenschap users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lees de eigenschap users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lees de eigenschap users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lees de eigenschap users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lees de eigenschap users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lees de eigenschap users.registeredDevices in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdeskbeheerder
Kan wachtwoorden voor niet-beheerders en Helpdesk-medewerkers opnieuw instellen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="information-protection-administrator"></a>Information Protection-beheerder
Kan alle aspecten van het product Azure Information Protection beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/devices/basic/update | Werk de basiseigenschappen voor apparaten bij in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/devices/create | Maak apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Verwijder apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Werk de eigenschap devices.registeredOwners bij in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Werk de eigenschap devices.registeredUsers bij in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |

### <a name="license-administrator"></a>Licentiebeheerder
Kan productlicenties voor gebruikers en groepen beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Werk de eigenschap users.usageLocation bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="lync-service-administrator"></a>Lync-servicebeheerder
Kan alle aspecten van het product Skype voor Bedrijven beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in het Office 365-berichtencentrum lezen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Maak en verwijder domeinen en lees alle standaardeigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/organization/basic/update | Werk de basiseigenschappen voor een organisatie bij in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Werk de eigenschap organization.trustedCAsForPasswordlessAuth bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="power-bi-service-administrator"></a>Servicebeheerder van Power BI
Kan alle aspecten van het Power BI-product beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kan roltoewijzingen in Azure AD en alle aspecten van Privileged Identity Management beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Werk directoryRoles bij in Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.privilegedIdentityManagement en werk deze bij. |

### <a name="reports-reader"></a>Rapportenlezer
Kan rapporten met betrekking tot aanmeldingen en controles lezen.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligingsgegevens en -rapporten lezen en configuratie beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/policies/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Werk de eigenschap policies.owners bij in Azure Active Directory. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lees alle resources in microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Werk alle resources bij in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.protectionCenter/allEntities/update | Werk alle resources bij in microsoft.office365.protectionCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="security-reader"></a>Beveiligingslezer
Kan beveiligingsgegevens en -rapporten lezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lees alle resources in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="service-support-administrator"></a>Beheerder serviceondersteuning
Kan gegevens over de servicestatus lezen en ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="sharepoint-service-administrator"></a>SharePoint-servicebeheerder
Kan alle aspecten van de SharePoint-service beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="teams-communications-administrator"></a>Teams-communicatiebeheerder
Kan de aanroep- en vergaderfuncties van de service Microsoft Teams beheren.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="teams-communications-support-engineer"></a>Ondersteuningstechnicus voor Teams-communicatie
Kan communicatieproblemen in Teams oplossen met geavanceerde hulpprogramma's.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="teams-communications-support-specialist"></a>Ondersteuningsspecialist voor Teams-communicatie
Kan communicatieproblemen in Teams oplossen met basishulpprogramma's.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="teams-service-administrator"></a>Teams-servicebeheerder
Kan de service Microsoft Teams beheren. 

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="user-account-administrator"></a>Beheerder van gebruikersaccounts
Kan alle aspecten van gebruikers en groepen beheren, inclusief het opnieuw instellen van wachtwoorden voor bepaalde beheerders.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/create | Maak gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit Azure AD in de toekomst.

* Ad-hoclicentiebeheerder
* Toevoegen
* Apparaatbeheerders
* Gebruikers van apparaten
* Maker van geverifieerde e-mailgebruiker
* Postvakbeheerder
* Werkplekapparaat toevoegen

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het toewijzen van een gebruiker als een beheerder van een Azure-abonnement, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
