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
ms.date: 06/04/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d31bde1a33d622c2c0b7aa716cbbbfbc8ef42ecf
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514576"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Rol beheerdersmachtigingen in Azure Active Directory

Met Azure Active Directory (Azure AD), kunt u beperkte beheerders voor het beheren van identiteit taken in minder bevoegdheden rollen opgeven. Beheerders kunnen worden toegewezen voor dergelijke doeleinden als toevoegen of wijzigen, gebruikers, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren en beheren van domeinnamen. De standaardmachtigingen van de gebruiker kunnen alleen in de gebruikersinstellingen worden gewijzigd in Azure AD.

De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. Alleen globale beheerders en beheerders met bevoorrechte rol kunt beheerdersrollen delegeren. U wordt aangeraden dat u deze rol aan slechts een paar mensen in uw bedrijf toewijzen om het risico voor uw bedrijf.


## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen

Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [weergeven en toewijzen beheerdersrollen in Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Beschikbare rollen

De volgende beheerdersrollen zijn beschikbaar:

* **[Toepassingsbeheerder](#application-administrator)** : Gebruikers in deze rol kunnen maken en beheren van alle aspecten van zakelijke toepassingen, registratie en instellingen van de toepassingsproxy. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Gebruikers die zijn toegewezen aan deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de toepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Toepassingsontwikkelaar](#application-developer)** : Gebruikers in deze rol kunnen toepassingsregistraties maken wanneer de 'Gebruikers kunnen toepassingen registreren' is ingesteld op Nee. Deze rol geeft ook het recht om in te stemmen uit eigen naam als de 'Gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens in hun naam' is ingesteld op Nee. Gebruikers die zijn toegewezen aan deze rol worden toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Authentication-beheerder](#authentication-administrator)** : Gebruikers met deze rol kunnen instellen of referenties in niet-wachtwoord opnieuw instellen. Verificatie-beheerders kunnen vereisen dat gebruikers kunnen opnieuw worden geregistreerd op basis van bestaande niet-wachtwoordreferenties (bijvoorbeeld, MFA of FIDO) en intrekken **MFA herinneren op het apparaat**, waarin wordt gevraagd voor MFA op de volgende aanmelding van gebruikers die zijn niet-beheerders of alleen de volgende rollen toegewezen:
  * Authentication-beheerder
  * Adreslijstlezers
  * Gastuitnodiging
  * Berichtencentrum-lezer
  * Rapportenreader

  De beheerdersrol voor de verificatie is momenteel in openbare preview. Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

  <b>Belangrijke</b>: Gebruikers met deze rol kunnen referenties wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van de referenties van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:

  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de verificatie-beheerders. Via dit pad beheerder verificatie mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

* **[Azure Information Protection-beheerder](#azure-information-protection-administrator)** : Gebruikers met deze rol hebben alle machtigingen in de Azure Information Protection-service. Deze rol kan labels voor de Azure Information Protection-beleid configureren, beveiligingssjablonen beheren en beveiliging activeren. Deze rol verleent alle machtigingen in Identity Protection Center, Privileged Identity Management, Monitor Office 365-servicestatus of Office 365 Centrum voor beveiliging en naleving.

* **[B2C-Gebruikerbeheerder stroom](#b2c-user-flow-administrator)** : Gebruikers met deze rol kunnen maken en beheren van B2C Gebruikersstromen (ook wel 'ingebouwde' beleidsregels) in Azure Portal. Door het maken of bewerken van de gebruikersstromen, kunnen deze gebruikers wijzigen de CSS-html/javascript-inhoud van de gebruikerservaring, MFA-vereisten per gebruikersstroom wijzigen, claims in het token wijzigen en aanpassen van de sessie-instellingen voor alle beleidsregels in de tenant. Aan de andere kant, deze rol niet de mogelijkheid om gebruikersgegevens te bekijken, of wijzigingen aanbrengen in de kenmerken die zijn opgenomen in de tenantschema. Wijzigingen in Identity-Ervaringsframework (ook wel aangepast) beleid ook is buiten het bereik van deze rol.

* **[Stroom B2C-kenmerk Gebruikerbeheerder](#b2c-user-flow-attribute-administrator)** : Gebruikers met deze rol toevoegen of verwijderen van aangepaste kenmerken die beschikbaar zijn voor alle gebruikersstromen in de tenant. Als zodanig kunnen gebruikers met deze rol wijzigen of nieuwe elementen toevoegen aan het schema van de eindgebruiker en van invloed zijn op het gedrag van alle gebruikersstromen en wijzigingen indirect leiden naar welke gegevens kan worden gevraagd van eindgebruikers en uiteindelijk worden verzonden als claims voor toepassingen. Deze rol kan gebruikersstromen niet bewerken.

* **[B2C IEF sleutelset beheerder](#b2c-ief-keyset-administrator)** :    Gebruiker maken en beheren van voor beleidssleutels en geheimen voor tokenversleuteling, token handtekening en versleuteling/ontsleuteling claim. Nieuwe sleutels toevoegt aan bestaande sleutelcontainers, kan deze beperkte beheerder rollover geheimen behoefte zonder gevolgen voor bestaande toepassingen. Deze gebruiker ziet de volledige inhoud van deze geheimen en hun verloopdatum zelfs nadat het is gemaakt.
    
  <b>Belangrijk:</b> dit is een gevoelige rol. De beheerdersrol sleutelset moet zorgvuldig worden gecontroleerd en toegewezen zorgvuldig tijdens de testfase vóór productie- en productie.

* **[B2C IEF beleid beheerder](#b2c-ief-policy-administrator)** : Gebruikers in deze rol hebben de mogelijkheid om te maken, lezen, bijwerken, en verwijderen van alle aangepaste beleidsregels in Azure AD B2C en daarom hebt volledige controle over de Identiteitservaring-Framework in de relevante Azure AD B2C-tenant. Door het bewerken van beleid, kan deze gebruiker tot stand brengen direct Federatie met externe id-providers, wijzigen van het directory-schema, alle gebruikersgerichte inhoud (HTML, CSS, JavaScript) wijzigt, wijzigen van de vereisten voor het voltooien van verificatie, maken van nieuwe gebruikers, verzenden gebruikersgegevens met externe systemen, met inbegrip van volledige migraties en bewerken van alle gebruikersgegevens, met inbegrip van tijdgevoelige velden, zoals wachtwoorden en telefoonnummers. Deze rol kan echter wijzigen van de versleutelingssleutels of bewerken van de geheimen die worden gebruikt voor Federatie in de tenant.

  <b>Belangrijk:</b> De beheerder van B2 IEF beleid is een uiterst gevoelige rol die moet worden toegewezen in een zeer beperkte mate voor tenants in de productieomgeving. Activiteiten op basis van deze gebruikers moeten worden nauw gecontroleerd, met name voor tenants in de productieomgeving.

* **[Factureringsbeheerder](#billing-administrator)** : Doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.

* **[Beheerder van de cloudtoepassing](#cloud-application-administrator)** : Gebruikers in deze rol hebben dezelfde machtigingen als de rol beheerder van de toepassing, met uitzondering van de mogelijkheid voor het beheren van de toepassingsproxy. Deze rol hebben de mogelijkheid om te maken en beheren van alle aspecten van bedrijfstoepassingen en registratie. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Gebruikers die zijn toegewezen aan deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de Cloudtoepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Cloud-Apparaatbeheerder](#cloud-device-administrator)** : Gebruikers in deze rol kunnen inschakelen, uitschakelen, en apparaten verwijderen in Azure AD en Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal lezen. De rol verleent machtigingen voor het beheren van andere eigenschappen op het apparaat.

* **[Beheerder voor naleving](#compliance-administrator)** : Gebruikers met deze rol hebben machtigingen voor het beheren van functies met betrekking tot naleving in de compliancecentrum Microsoft 365, Microsoft 365-beheercentrum, Azure, en Office 365-beveiliging en compliance. Gebruikers kunnen ook alle functies in de Exchange-beheercentrum, Teams en Skype voor bedrijven-beheercentrum beheren en maken van ondersteuningstickets voor Azure en Microsoft 365. Meer informatie vindt u op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  In | Kan doen
  ----- | ----------
  [Microsoft 365 compliancecentrum](https://protection.office.com) | Beveiligen en beheren van gegevens van uw organisatie via Microsoft 365-services<br>Naleving-waarschuwingen beheren
  [Voor naleving](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Bijhouden, toewijzen en controleer of de activiteiten van de naleving van regelgeving van uw organisatie
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Gegevensbeheer beheren<br>Wettelijke informatie en gegevens onderzoek uitvoeren<br>Aanvraag voor het onderwerp van gegevens beheren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle Intune-controlegegevens weergeven
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren<br>Kunt maken en wijzigen van de beleidsregels voor bestanden en governance-acties voor toestaan<br> De ingebouwde rapporten onder beheer van de gegevens kunt weergeven

<!--* **[Compliance Data Administrator](#compliance-data-administrator)**: Users with this role have permissions to protect and track data in the Microsoft 365 compliance center, Microsoft 365 admin center, and Azure. Users can also manage all features within the Exchange admin center, Compliance Manager, and Teams & Skype for Business admin center and create support tickets for Azure and Microsoft 365.

  In | Can do
  ----- | ----------
  [Microsoft 365 compliance center](https://protection.office.com) | Monitor compliance-related policies across Microsoft 365 services<br>Manage compliance alerts
  [Compliance Manager](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Track, assign, and verify your organization's regulatory compliance activities
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Manage data governance<br>Perform legal and data investigation<br>Manage Data Subject Request
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | View all Intune audit data
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Has read-only permissions and can manage alerts<br>Can create and modify file policies and allow file governance actions<br> Can view all the built-in reports under Data Management
-->
* **[Beheerder van voorwaardelijke toegang](#conditional-access-administrator)** : Gebruikers met deze rol hebben de mogelijkheid voor het beheren van instellingen voor voorwaardelijke toegang van Azure Active Directory.
  > [!NOTE]
  > Voor het implementeren van Exchange ActiveSync-beleid voor voorwaardelijke toegang in Azure, moet de gebruiker ook een globale beheerder zijn.
  
* **[Klanten-Lockbox toegang goedkeurder](#customer-lockbox-access-approver)** : Beheert [aanvragen van klanten-Lockbox](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) in uw organisatie. Ze kunnen ontvangen van e-mailmeldingen voor aanvragen van klanten-Lockbox goedkeuren en weigeren van de Microsoft 365-beheercentrum. Ze kunnen ook de functie voor klanten-Lockbox inschakelen of uitschakelen. Alleen globale beheerders kunnen de wachtwoorden van personen die zijn toegewezen aan deze rol opnieuw instellen.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Apparaatbeheerders](#device-administrators)** : Deze functie is beschikbaar voor toewijzing alleen als een lokale beheerder in [apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn gekoppeld aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaatobjecten in Azure Active Directory. 

* **[Adreslijstlezers](#directory-readers)** : Dit is een rol die moet worden toegewezen aan alleen oudere toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Niet toewijzen aan gebruikers.

* **[Directory-Accounts voor synchronisatie](#directory-synchronization-accounts)** : Gebruik geen. Deze rol is wordt automatisch toegewezen aan de Azure AD Connect-service en niet bedoeld of ondersteund voor ander gebruik.

* **[Schrijvers van mappen](#directory-writers)** : Dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Dynamics 365-beheerder / CRM-beheerder](#crm-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Dynamics 365 Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [de rol admin gebruiken voor het beheren van uw tenant](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Dynamics 365-servicebeheerder'. Het 'Dynamics 365-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Exchange-beheerder](#exchange-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Exchange Online, wanneer de service aanwezig is. Heeft ook de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Exchange Service Administrator". Het 'Exchange-beheerder' is in de [Azure-portal](https://portal.azure.com). Het 'Exchange Online-beheerder' is in de [Exchange-beheercentrum](https://go.microsoft.com/fwlink/p/?LinkID=529144). 

* **[Externe id-Provider beheerder](#external-identity-provider-administrator)** : Deze beheerder beheert federatie tussen tenants van Azure Active Directory en externe id-providers. Met deze rol kunnen gebruikers nieuwe id-providers toevoegen en configureren van alle beschikbare instellingen (bijvoorbeeld verificatiepad, service-id,-sleutelcontainers toegewezen). Deze gebruiker kan de tenant te vertrouwen verificaties van externe id-providers kunt inschakelen. De resulterende impact op de ervaringen van eindgebruikers, is afhankelijk van het type tenant:
  * Azure Active Directory-tenants voor werknemers en partners: Het toevoegen van een federatieve (bijvoorbeeld met Gmail) is direct van invloed op alle Gast uitnodigingen nog niet ingewisseld. Zie [Google toe te voegen als een id-provider voor B2B-gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).
  * Azure Active Directory B2C-tenants: Het toevoegen van een federatieve (bijvoorbeeld met Facebook of met een andere Azure Active Directory) is niet onmiddellijk invloed op het stromen van de eindgebruiker totdat de id-provider is toegevoegd als een optie in een gebruikersstroom (ook wel het ingebouwde beleid). Zie [configureren van een Microsoft-account als id-provider](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) voor een voorbeeld. Als u wilt wijzigen gebruikersstromen, zijn de beperkte rol van 'B2C gebruiker Flow beheerder' is vereist.

* **[Globale beheerder / Company Administrator](#company-administrator)** : Gebruikers met deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die gebruikmaken van Azure Active Directory-identiteiten, zoals Microsoft 365 security center, Microsoft 365 compliancecentrum, Exchange Online, SharePoint Online, en Skype voor bedrijven Online. De persoon die zich aanmeldt voor de Azure Active Directory-tenant wordt globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er is meer dan één globale beheerder in uw bedrijf. Globale beheerders kunnen het wachtwoord voor elke gebruiker en alle andere beheerders opnieuw instellen.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Company Administrator". Het 'Globale beheerder' is in de [Azure-portal](https://portal.azure.com).
  >
  >

* **[Gastuitnodiging](#guest-inviter)** : Gebruikers in deze rol kunnen uitnodigingen voor Azure Active Directory B2B Gast beheren wanneer de **leden kunnen uitnodigen** gebruikersinstelling is ingesteld op Nee. Meer informatie over B2B-samenwerking bij [over Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Deze omvatten geen andere machtigingen.

* **[Intune-beheerder](#intune-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Intune Online, wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren. Meer informatie op [rollen gebaseerd toegangsbeheer (RBAC) met Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Intune-servicebeheerder '. Het 'Intune-beheerder' is in de [Azure-portal](https://portal.azure.com).
  
 * **[Kaizala beheerder](#kaizala-administrator)** : Gebruikers met deze rol hebben algemene machtigingen voor het beheren van instellingen in Microsoft Kaizala, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren.
Bovendien kan de gebruiker toegang tot rapporten met betrekking tot acceptatie en het gebruik van Kaizala door leden van de organisatie- en business-rapporten die zijn gegenereerd met de acties met Kaizala. 

* **[Licentiebeheerder](#license-administrator)** : Gebruikers in deze rol kunnen toevoegen, verwijderen en update licentie toewijzingen aan gebruikers, groepen (met Groepslicenties) en de gebruikslocatie van gebruikers beheren. De rol heeft niet de mogelijkheid om te kopen of beheren van abonnementen, maken of beheren van groepen, of maken of beheren van gebruikers buiten de gebruikslocatie verlenen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Berichtencentrum-lezer voor Privacy](#message-center-privacy-reader)** : Gebruikers in deze rol kunnen alle meldingen in het berichtencentrum, met inbegrip van berichten voor privacy van gegevens controleren. Berichtencentrum Privacy lezers e-mailmeldingen met inbegrip van deze met betrekking tot privacy van gegevens ophalen en afmelden kan Message Center voorkeuren. Alleen de globale beheerder en de lezer Message Center Privacy de gegevens privacy berichten kan lezen. Daarnaast bevat deze rol de mogelijkheid om groepen, domeinen en abonnementen weer te geven. Deze rol heeft geen machtiging om te bekijken, maken of beheren van serviceaanvragen.

* **[Berichtencentrum-lezer](#message-center-reader)** : Gebruikers in deze rol kunnen controleren, meldingen en de gezondheid van advies-updates in [Office 365-berichtencentrum](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie op de geconfigureerde services zoals Exchange, Intune en Microsoft Teams. Berichtencentrum-lezer wekelijkse e-mailbericht verwerkingen van berichten, updates, ontvangen en message center berichten in Office 365 kunnen delen. In Azure AD, wordt gebruikers die zijn toegewezen aan deze rol alleen alleen-lezen toegang hebben op Azure AD-services, zoals gebruikers en groepen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Laag1-ondersteuning voor partner](#partner-tier1-support)** : Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Laag2-ondersteuning voor partner](#partner-tier2-support)** : Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Helpdeskbeheerder (wachtwoord)](#helpdesk-administrator)** : Gebruikers met deze rol kunnen wachtwoorden wijzigen, vernieuwingstokens ongeldig te maken, serviceaanvragen beheren en servicestatus controleren. Ongeldig vernieuwingstoken zorgt ervoor dat de gebruiker zich opnieuw aanmelden. Helpdesk-beheerders kunnen wachtwoorden opnieuw instellen en vernieuwen van tokens van andere gebruikers die niet-beheerders en alleen de volgende rollen toegewezen ongeldig te maken:
  * Adreslijstlezers
  * Gastuitnodiging
  * Helpdesk-beheerder
  * Berichtencentrum-lezer
  * Rapportenreader
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de Helpdesk-medewerkers. Via dit pad die een Helpdesk-beheerder kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.


  > [!NOTE]
  > Beheerdersmachtigingen overdragen via subsets van gebruikers en beleidsregels toepassen op een subset van gebruikers er mogelijk is met [Beheereenheden (preview)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).


  > [!NOTE]
  > Deze rol heette vroeger Microsoft Azure "wachtwoordbeheerder" in [Azure-portal](https://portal.azure.com/). We zijn de naam ervan wijzigen in 'Helpdesk-beheerder' zodat deze overeenkomen met de naam ervan in Azure AD PowerShell, Azure AD Graph API en Microsoft Graph API. Voor een korte periode wordt we de naam gewijzigd in '(wachtwoord) Helpdeskbeheerder' in Azure-portal voordat de wijziging door te 'Helpdesk-beheerder'.


* **[Power BI Administrator](#power-bi-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Power BI, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [inzicht in de Power BI-beheerdersrol](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Power BI-servicebeheerder '. ' Power BI-beheerder ' is in de [Azure-portal](https://portal.azure.com).

* **[Authentication-beheerder in beschermde modus](#privileged-authentication-administrator)** : Gebruikers met deze rol kunnen instellen of referenties voor alle gebruikers, met inbegrip van globale beheerders niet-wachtwoord opnieuw instellen. Bevoegde verificatie-beheerders kunnen afdwingen dat gebruikers kunnen opnieuw worden geregistreerd op basis van bestaande niet-wachtwoord referentie (bijvoorbeeld MFA, FIDO) en intrekken 'MFA herinneren op het apparaat', dat u wordt gevraagd voor MFA op de volgende aanmelding van alle gebruikers. Beheerders met bevoorrechte verificatie kunt doen:
  * Afdwingen dat gebruikers opnieuw registreren op basis van bestaande niet-wachtwoord referentie (bijvoorbeeld MFA, FIDO)
  * Intrekken 'MFA herinneren op het apparaat', dat u wordt gevraagd voor MFA op de volgende aanmelding

* **[Rol van beheerder in beschermde modus](#privileged-role-administrator)** : Gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management en administratieve eenheden.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van toewijzingen voor alle Azure AD-rollen, met inbegrip van de rol globale beheerder. Deze rol bevat geen andere bevoegde mogelijkheden in Azure AD, zoals het maken of bijwerken van gebruikers. Echter kunnen gebruikers zijn toegewezen aan deze rol verlenen zichzelf of andere aanvullende bevoegdheden door aanvullende rollen toewijzen.

* **[Lezer-rapporten](#reports-reader)** : Gebruikers met deze rol kunnen reporting-gebruiksgegevens weergeven en het dashboard rapporten in Microsoft 365-beheercentrum en de acceptatie-context pack in Power BI. Bovendien de rol biedt toegang tot aanmelden-rapporten en -activiteit in Azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. De gebruiker die is toegewezen aan de rol Rapportenlezer toegang alleen relevante gebruik en acceptatie metrische gegevens. Ze geen geen admin-machtigingen voor het configureren van instellingen of toegang tot die het beheercentrums productspecifieke zoals Exchange. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Zoekresultaten](#search-administrator)** : Gebruikers in deze rol hebben volledige toegang tot alle Microsoft Search-beheerfuncties in het Microsoft 365-beheercentrum. Beheerders zoeken kunnen delegeren van de beheerders zoeken en zoeken Editor rollen toe aan gebruikers, en maken en beheren van inhoud, zoals bladwijzers, Q & As, en locaties. Deze gebruikers kunnen daarnaast het berichtencentrum weergeven, servicestatus controleren en maken van serviceaanvragen.

* **[Search Editor](#search-editor)** : Gebruikers in deze rol kunnen maken, beheren en verwijderen van inhoud voor Microsoft Search in de Microsoft 365-beheercentrum, met inbegrip van bladwijzers, Q & als en locaties.

* **[Beveiligingsbeheerder](#security-administrator)** : Gebruikers met deze rol hebben machtigingen voor het beheren van beveiligingsfuncties in de Microsoft 365 security center, Azure Active Directory Identity Protection, Azure Information Protection, en Office 365-beveiliging en compliance. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | Beveiligingsbeleid controleren in Microsoft 365-services<br>Beveiligingsrisico's en waarschuwingen beheren<br>Rapporten weergeven
  Identity Protection Center | Alle machtigingen van de rol van Beveiligingslezer<br>Bovendien de mogelijkheid om uit te voeren van alle Identity Protection Center-bewerkingen, met uitzondering van wachtwoorden opnieuw instellen
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Alle machtigingen van de rol van Beveiligingslezer<br>**Kan geen** instellingen of Azure AD-roltoewijzingen beheren
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
  [Microsoft 365 security center](https://protection.office.com) | All permissions of the Security Reader role<br>View, investigate, and respond to security threats alerts
  Identity Protection Center | All permissions of the Security Reader role<br>Additionally, the ability to perform all Identity Protection Center operations except for resetting passwords
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | All permissions of the Security Reader role
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  Windows Defender ATP and EDR | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | All permissions of the Security Reader role
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | All permissions of the Security Reader role
  [Office 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health) | View the health of Office 365 services
-->
* **[Beveiligingslezer](#security-reader)** : Gebruikers met deze rol hebben globale alleen-lezen toegang op functies met betrekking tot beveiliging, met inbegrip van alle gegevens in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, evenals de mogelijkheid om te lezen Azure Active Directory-aanmeldingsrapporten en controlelogboeken, en in Office 365-beveiligings- en Compliancecentrum. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | Beveiligingsbeleid voor Microsoft 365-services weergeven<br>Weergave beveiligingsrisico's en waarschuwingen<br>Rapporten weergeven
  Identity Protection Center | Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<br><ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde beveiliging tegen bedreigingen<li>Anti-phishing<li>Mailflow regels
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Is alleen-lezen toegang tot alle gegevens in Azure AD PIM opgehaald: Beleid en rapporten voor Azure AD-roltoewijzingen wordt beveiliging beoordeelt en toegang tot gegevens en rapporten voor scenario's behalve Azure AD-roltoewijzing in de toekomst te lezen.<br>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengen. In de PIM-portal of via PowerShell, kan iemand zich in deze rol aanvullende rollen (bijvoorbeeld: globale beheerder of beheerder met bevoorrechte rol), activeren als de gebruiker in aanmerking komt voor deze.
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid bekijken<br>Weergeven en bedreigingen te onderzoeken<br>Rapporten weergeven
  Windows Defender ATP en EDR | Weergeven en onderzoeken van waarschuwingen
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Weergaven gebruiker, apparaat, inschrijving, configuratie en informatie over toepassingen. Kan geen wijzigingen aanbrengen aan Intune.
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven
  [Office 365-servicestatus](https://docs.microsoft.com/office365/enterprise/view-service-health) | Bekijk de status van Office 365-services

* **[Beheerder serviceondersteuning](#service-support-administrator)** : Gebruikers met deze rol kunnen ondersteuningsaanvragen openen met Microsoft Azure en Office 365-services, weergaven en het servicedashboard en berichtencentrum weergeven in de [Azure-portal](https://portal.azure.com) en [Microsoft 365-beheercentrum](https://admin.microsoft.com). Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als ' beheerder serviceondersteuning. " Het is 'Servicebeheerder' de [Azure-portal](https://portal.azure.com), wordt de [Microsoft 365-beheercentrum](https://admin.microsoft.com), en de Intune-portal.

* **[SharePoint-beheerder](#sharepoint-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als "SharePoint-servicebeheerder." Het 'SharePoint-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Skype voor bedrijven / Lync beheerder](#lync-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Skype voor bedrijven, wanneer de service aanwezig is, evenals beheren van de kenmerken van de Skype-specifieke gebruiker in Azure Active Directory. Deze rol hebben bovendien de mogelijkheid ondersteuningstickets beheren en servicestatus controleren en de toegang tot de Teams en Skype voor bedrijven-beheercentrum. Het account moet ook een licentie hebben voor Teams of Teams PowerShell-cmdlets kan niet worden uitgevoerd. Meer informatie op [over de Skype voor bedrijven-beheerdersrol](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) en Teams informatie over licenties op [Skype voor bedrijven en Microsoft Teams-Add-on-licentieverlening](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Lync-servicebeheerder'. Het 'Skype voor bedrijven-beheerder' is in de [Azure-portal](https://portal.azure.com/).

* **[Beheerder teams](#teams-service-administrator)** : Gebruikers in deze rol kunnen alle aspecten van de Microsoft Teams-werkbelasting via de Microsoft Teams en Skype voor bedrijven-beheercentrum en de bijbehorende PowerShell-modules beheren. Dit omvat onder andere gebieden, alle beheerprogramma's met betrekking tot de telefoon, chatberichten, vergaderingen en teams zelf. Deze rol hebben bovendien de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren.
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Teams Service Administrator ". Het 'Teams beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Communicatie-beheerder teams](#teams-communications-administrator)** : Gebruikers in deze rol kunnen aspecten van de Microsoft Teams-workload met betrekking tot de spraak- en TAPI beheren. Dit omvat de beheerhulpprogramma's voor de toewijzing van telefoon, spraak- en voldoen aan beleidsregels en volledige toegang tot de aanroep analytics toolset.

* **[Communicatie-ondersteuningstechnicus teams](#teams-communications-support-engineer)** : Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen bekijken aanroep van de volledige gegevens voor alle deelnemers die betrokken zijn. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Communicatie ondersteuning voor gespecialiseerde teams](#teams-communications-support-specialist)** : Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen alleen gebruikersgegevens weergeven in de aanroep voor de specifieke gebruiker dat ze hebt opgezocht. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Gebruikersbeheerder](#user-administrator)** : Gebruikers met deze rol kunnen gebruikers, maken en beheren van alle aspecten van gebruikers met enkele beperkingen (Zie hieronder) en wachtwoordverloopbeleid kunnen bijwerken. Gebruikers met deze rol kunnen bovendien maken en beheren van alle groepen. Deze rol omvat ook de mogelijkheid om te maken en beheren van gebruikersweergaven, ondersteuningstickets beheren en servicestatus controleren.

  | | |
  | --- | --- |
  |Algemene machtigingen|<p>Gebruikers en groepen maken</p><p>Gebruikersweergaven maken en beheren</p><p>Office-ondersteuningstickets beheren<p>Wachtwoordverloopbeleid bijwerken|
  |<p>Op alle gebruikers, met inbegrip van alle beheerders</p>|<p>Licenties beheren</p><p>Eigenschappen van alle gebruikers, behalve de User Principal Name beheren</p>
  |Alleen op gebruikers die niet-beheerders of beperkte beheerdersrollen in het volgende:<ul><li>Adreslijstlezers<li>Gastuitnodiging<li>Helpdesk-beheerder<li>Berichtencentrum-lezer<li>Rapportenreader<li>Gebruikerbeheerder|<p>Verwijderen en herstellen</p><p>Uitschakelen en inschakelen</p><p>Ongeldig vernieuwingstokens</p><p>Eigenschappen van alle gebruikers met inbegrip van de User Principal Name beheren</p><p>Wachtwoord opnieuw instellen</p><p>Apparaatsleutels (FIDO) bijwerken</p>
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan beheerders. Via dit pad voor de beheerder van een gebruiker kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

## <a name="role-permissions"></a>Machtigingen van de rol
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory die aan elke rol. Sommige functies mogelijk extra machtigingen in Microsoft-services buiten Azure Active Directory.

### <a name="application-administrator"></a>Toepassingsbeheerder
Kan maken en beheren van alle aspecten van app-registraties en bedrijfsapps.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | De eigenschap applications.audience in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/authentication/update | De eigenschap applications.authentication in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Toepassingen maken in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | De eigenschap applications.credentials in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Lees appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/basic/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="application-developer"></a>Toepassingsontwikkelaar
Kan toepassingsregistraties onafhankelijk van de 'gebruikers kunnen toepassingen registreren' maken instelling.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Toepassingen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | AppRoleAssignments maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | OAuth2PermissionGrants maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | ServicePrincipals maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |

### <a name="authentication-administrator"></a>Authentication-beheerder
Toegestaan wilt weergeven, instellen en opnieuw instellen van verificatie methode-informatie voor elke gebruiker die geen beheerder.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | Eigenschappen van de sterke verificatie bijwerken zoals MFA referentie-informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="azure-information-protection-administrator"></a>Azure Information Protection-beheerder
Kan alle aspecten van de Azure Information Protection-service beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Alle aspecten van Azure Information Protection beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="b2c-user-flow-administrator"></a>B2C User Flow Administrator
Maken en beheren van alle aspecten van de gebruikersstromen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Lees en gebruikersstromen configureren in Azure Active Directory B2C. |

### <a name="b2c-user-flow-attribute-administrator"></a>B2C User Flow Attribute Administrator
Maken en beheren van het kenmerkschema beschikbaar voor alle gebruikersstromen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Lees en gebruikerskenmerken configureren in Azure Active Directory B2C. |

### <a name="b2c-ief-keyset-administrator"></a>B2C IEF Keyset Administrator
Geheimen voor Federatie en versleuteling in de Identity-Ervaringsframework beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Lees en configureren van sleutel sets in Azure Active Directory B2C. |

### <a name="b2c-ief-policy-administrator"></a>B2C IEF Policy Administrator
Maken en beheren van framework vertrouwensbeleid in de Identiteitservaring-Framework.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Lees en aangepaste beleidsregels configureren in Azure Active Directory B2C. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kunnen algemene facturering gerelateerde taken uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | Eenvoudige eigenschappen op de organisatie in Azure Active Directory bijgewerkt. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Alle aspecten van Office 365 facturering beheren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics-Administrator
Kunnen de Desktop Analytics en de Office Customization & beleid services beheren. Dit omvat de mogelijkheid om te bekijken van asset-inventarisatie, implementatie plannen, status van de implementatie en de status bekijken voor Desktop-analyse. Deze rol kunnen gebruikers Office-beleid beheren voor Office Customization & Policy-service.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van bureaublad Analytics beheren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="cloud-application-administrator"></a>Beheerder van de cloudtoepassing
Kan maken en beheren van alle aspecten van app-registraties en bedrijfsapps, behalve App Proxy.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | De eigenschap applications.audience in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/authentication/update | De eigenschap applications.authentication in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Toepassingen maken in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | De eigenschap applications.credentials in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/applications/policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | De eigenschap policies.applicationConfiguration in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lezen policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/basic/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="cloud-device-administrator"></a>Cloud-Apparaatbeheerder
Volledige toegang om apparaten te beheren in Azure AD.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Apparaten verwijderen in Azure Active Directory. |
| microsoft.aad.directory/devices/disable | Apparaten in Azure Active Directory uitschakelen. |
| microsoft.aad.directory/devices/enable | Inschakelen dat apparaten in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="company-administrator"></a>Bedrijfsbeheerder
Kan alle aspecten van Azure AD beheren en Microsoft-services die met Azure AD-identiteiten.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.aad.cloudAppSecurity. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Maken en verwijderen van administrativeUnits, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Maken en verwijderen van toepassingen, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Maken en verwijderen van appRoleAssignments, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
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
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Maken en verwijderen van subscribedSkus, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Maken en verwijderen van gebruikers, en lezen en bijwerken van alle eigenschappen in Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Alle acties uitvoeren in Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Alle resources in microsoft.azure.advancedThreatProtection lezen. |
| microsoft.azure.informationProtection/allEntities/allTasks | Alle aspecten van Azure Information Protection beheren. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Alle aspecten van Office 365 facturering beheren. |
| microsoft.intune/allEntities/allTasks | Alle aspecten van Intune beheren. |
| microsoft.office365.complianceManager/allEntities/allTasks | Alle aspecten van Office 365-Nalevingsframework Manager beheren |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van bureaublad Analytics beheren. |
| microsoft.office365.exchange/allEntities/allTasks | Alle aspecten van Exchange Online te beheren. |
| microsoft.office365.lockbox/allEntities/allTasks | Alle aspecten van Office 365-klant Lockbox beheren |
| microsoft.office365.messageCenter/messages/read | Berichten in microsoft.office365.messageCenter lezen. |
| microsoft.office365.messageCenter/securityMessages/read | SecurityMessages in microsoft.office365.messageCenter lezen. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Alle aspecten van Office 365 Protection Center beheren. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.securityComplianceCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Alle aspecten van Dynamics 365 beheren. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Alle aspecten van Power BI beheren. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Alle resources in microsoft.windows.defenderAdvancedThreatProtection lezen. |

### <a name="compliance-administrator"></a>Beheerder voor naleving
Kan lezen en nalevingsconfiguratie en -rapporten beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.complianceManager/allEntities/allTasks | Alle aspecten van Office 365-Nalevingsframework Manager beheren |
| microsoft.office365.exchange/allEntities/allTasks | Alle aspecten van Exchange Online te beheren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder
Mogelijkheden voor voorwaardelijke toegang beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Lezen policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | De eigenschap policies.conditionalAccess in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/conditionalAccess/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lezen policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | De eigenschap policies.conditionalAccess in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lezen policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | De eigenschap policies.conditionalAccess in Azure Active Directory bijgewerkt. |

### <a name="crm-service-administrator"></a>CRM-servicebeheerder
Kan alle aspecten van het Dynamics 365-product beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Alle aspecten van Dynamics 365 beheren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="customer-lockbox-access-approver"></a>Toegangsfiatteur voor klanten
Kan Microsoft-ondersteuningsaanvragen voor toegang tot bedrijfsgegevens van klanten goedkeuren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.lockbox/allEntities/allTasks | Alle aspecten van Office 365-klant Lockbox beheren |

### <a name="device-administrators"></a>Apparaatbeheerders
Gebruikers die zijn toegewezen aan deze rol worden toegevoegd aan de groep lokale beheerders op Azure AD join-apparaten.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen van groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen van groupSettingTemplates in Azure Active Directory. |

### <a name="directory-readers"></a>Adreslijstlezers
Basic directory-informatie kan worden gelezen. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Lees de basiseigenschappen van administrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Lezen administrativeUnits.members in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/read | Lees de basiseigenschappen van toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/read | Lezen applications.owners in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/read | Lezen applications.policies in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/read | Lees de basiseigenschappen van contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Lezen contacts.memberOf in Azure Active Directory. |
| microsoft.aad.directory/contracts/basic/read | Lees de basiseigenschappen van opdrachten in Azure Active Directory. |
| microsoft.aad.directory/devices/basic/read | Lees de basiseigenschappen van apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lezen devices.memberOf in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lezen devices.registeredOwners in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lezen devices.registeredUsers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/basic/read | Lees de basiseigenschappen van directoryRoles in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lezen directoryRoles.eligibleMembers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Lezen directoryRoles.members in Azure Active Directory. |
| microsoft.aad.directory/domains/basic/read | Lees de basiseigenschappen van domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lezen groups.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/read | Lees de basiseigenschappen van groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lezen groups.memberOf in Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lezen groups.members in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lezen groups.owners in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lezen groups.settings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen van groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen van groupSettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Lees de basiseigenschappen van oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/organization/basic/read | Lees de basiseigenschappen van de organisatie in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lezen organization.trustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/basic/read | Lees de basiseigenschappen van roleAssignments in Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/basic/read | Lees de basiseigenschappen van roleDefinitions in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lezen servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lezen servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen van servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lezen servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lezen servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lezen servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lezen servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lezen servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/basic/read | Lees de basiseigenschappen van subscribedSkus in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Synchronisatie van Active Directory-Accounts
Alleen gebruikt door Azure AD Connect-service.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | De eigenschap organization.dirSync in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/basic/read | Lees de basiseigenschappen van beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/read | Lezen policies.owners in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lezen policies.policiesAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lezen servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lezen servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | De eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen van servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/create | ServicePrincipals maken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lezen servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lezen servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lezen servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lezen servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lezen servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directorySync/allEntities/allTasks | Alle acties uitvoeren in Azure AD Connect. |

### <a name="directory-writers"></a>Schrijvers van mappen
Kan lezen en schrijven van basic directory-informatie. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/basic/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| microsoft.aad.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groupSettings/basic/update | Eenvoudige eigenschappen op groupSettings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groupSettings/create | GroupSettings maken in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Verwijder groupSettings in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |

### <a name="exchange-service-administrator"></a>Exchange Service-beheerder
Kan alle aspecten van het product Exchange beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/unified/basic/update | Werk de basiseigenschappen van Office 365-groepen. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.exchange/allEntities/allTasks | Alle aspecten van Exchange Online te beheren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="external-identity-provider-administrator"></a>Externe id-Provider beheerder
Id-providers voor gebruik in directe federatie configureren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Lees en id-providers configureren in Azure Active Directory B2C. |

### <a name="guest-inviter"></a>Gastuitnodiging
Kan onafhankelijk van de 'leden kunnen gasten uitnodigen' gastgebruikers uitnodigen instelling.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lezen users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lezen users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Uitnodigen van gastgebruikers ook kunnen in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lezen users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lezen users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lezen users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lezen users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lezen users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lezen users.registeredDevices in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdesk-beheerder
Kunnen wachtwoorden voor niet-beheerders en Helpdesk-medewerkers opnieuw instellen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/contacts/create | Contactpersonen maken in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/devices/basic/update | Eenvoudige eigenschappen op apparaten in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.aad.directory/devices/create | Apparaten maken in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Apparaten verwijderen in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | De eigenschap devices.registeredOwners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/devices/registeredUsers/update | De eigenschap devices.registeredUsers in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/basic/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| microsoft.aad.directory/groups/create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lezen groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/restore | Groepen in Azure Active Directory herstellen. |
| microsoft.aad.directory/groups/settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.intune/allEntities/allTasks | Alle aspecten van Intune beheren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |

### <a name="kaizala-administrator"></a>Kaizala-beheerder
Kunnen instellingen voor Microsoft Kaizala beheren.  

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >  
  
| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.webPortal/allEntities/basic/read | Lezen Office 365-beheercentrum. |

### <a name="license-administrator"></a>Licentiebeheerder
Kan productlicenties voor gebruikers en groepen beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/usageLocation/update | De eigenschap users.usageLocation in Azure Active Directory bijgewerkt. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="lync-service-administrator"></a>Lync-servicebeheerder
Kan alle aspecten van het product Skype voor bedrijven beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Alle aspecten van Skype voor bedrijven Online beheren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="message-center-privacy-reader"></a>Privacy-berichtencentrum-lezer
Kan lezen berichtencentrum berichten, berichten voor privacy van gegevens, groepen, domeinen en -abonnementen.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.messageCenter/messages/read | Berichten in microsoft.office365.messageCenter lezen. |
| microsoft.office365.messageCenter/securityMessages/read | SecurityMessages in microsoft.office365.messageCenter lezen. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in Office 365-berichtencentrum lezen. 

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.messageCenter/messages/read | Berichten in microsoft.office365.messageCenter lezen. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/contacts/create | Contactpersonen maken in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Terugzetten van verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/contacts/create | Contactpersonen maken in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/domains/allTasks | Maken en verwijderen van domeinen en lezen en bijwerken van de standaardeigenschappen in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/delete | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/restore | Groepen in Azure Active Directory herstellen. |
| microsoft.aad.directory/organization/basic/update | Eenvoudige eigenschappen op de organisatie in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Terugzetten van verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="power-bi-service-administrator"></a>Power BI-servicebeheerder
Kan alle aspecten van de Power BI-product beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Alle aspecten van Power BI beheren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="privileged-authentication-administrator"></a>Beheerder met bevoorrechte verificatie
Toegestaan om weer te geven, instellen en opnieuw instellen van verificatie methode-informatie voor elke gebruiker (admin of niet-beheerders).

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | Eigenschappen van de sterke verificatie bijwerken zoals MFA referentie-informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kan roltoewijzingen in Azure AD en alle aspecten van Privileged Identity Management beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/allTasks | Lees en servicePrincipals.appRoleAssignedTo eigenschap configureren in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Lees en servicePrincipals.oAuth2PermissionGrants eigenschap configureren in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Maken en beheren van administratieve eenheden (met inbegrip van leden) |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Maken en beheren van roltoewijzingen. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Maken en beheren van roldefinities. |

### <a name="reports-reader"></a>Rapportenreader
Kan lezen aanmelden en controleren van rapporten.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |

### <a name="search-administrator"></a>Zoekresultaten
Kan maken en beheren van alle aspecten van Microsoft Search-instellingen.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Berichten in microsoft.office365.messageCenter lezen. |
| microsoft.office365.search/allEntities/allProperties/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van alle eigenschappen in microsoft.office365.search. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |

### <a name="search-editor"></a>Search-Editor
Kan maken en beheren van de redactionele inhoud, zoals de bladwijzers Q en als, locaties, plattegrond.

  > [!NOTE]
  > Deze rol heeft machtigingen voor aanvullende buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Berichten in microsoft.office365.messageCenter lezen. |
| microsoft.office365.search/content/allProperties/allTasks | Maken en inhoud te verwijderen en lezen en bijwerken van alle eigenschappen in microsoft.office365.search. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligingsgegevens en -rapporten lezen en configuratie beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/create | Maak beleidsregels in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.aad.directory/policies/owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.aad.identityProtection/allEntities/read | Alle resources in microsoft.aad.identityProtection lezen. |
| microsoft.aad.identityProtection/allEntities/update | Alle resources in microsoft.aad.identityProtection bijwerken. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.protectionCenter/allEntities/update | Alle resources in microsoft.office365.protectionCenter bijwerken. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="security-reader"></a>Beveiligingslezer
Informatie over beveiliging en rapporten kunnen worden gelezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |
| microsoft.aad.identityProtection/allEntities/read | Alle resources in microsoft.aad.identityProtection lezen. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Alle resources in microsoft.aad.privilegedIdentityManagement lezen. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="service-support-administrator"></a>Beheerder serviceondersteuning
Kan gegevens over de servicestatus lezen en ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="sharepoint-service-administrator"></a>SharePoint-servicebeheerder
Kan alle aspecten van de SharePoint-service beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/unified/basic/update | Werk de basiseigenschappen van Office 365-groepen. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maken en verwijderen van alle resources en lezen en bijwerken van de standaardeigenschappen in microsoft.office365.sharepoint. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

### <a name="teams-communications-administrator"></a>Teams communicatie beheerder
Kunnen aanroepen en functies in de service Microsoft Teams-vergaderingen beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |

### <a name="teams-communications-support-engineer"></a>Ondersteuningstechnicus van teams communicatie
Problemen kunt communicatie binnen Teams met behulp van geavanceerde hulpprogramma's.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="teams-communications-support-specialist"></a>Ondersteuningsmedewerker voor teams communicatie
Problemen kunt communicatie binnen Teams met behulp van eenvoudige hulpprogramma's.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |

### <a name="teams-service-administrator"></a>Teams-servicebeheerder
Kan de service Microsoft Teams beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Lezen groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/unified/basic/update | Werk de basiseigenschappen van Office 365-groepen. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |
| microsoft.office365.usageReports/allEntities/read | Gebruiksrapporten lezen Office 365. |

### <a name="user-administrator"></a>Gebruikerbeheerder
Kan alle aspecten van gebruikers en groepen, inclusief het opnieuw instellen van wachtwoorden voor bepaalde beheerders beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | AppRoleAssignments maken in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Update appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/update | Eenvoudige eigenschappen voor contactpersonen in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/contacts/create | Contactpersonen maken in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Contactpersonen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/basic/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| microsoft.aad.directory/groups/create | Groepen maken in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Groepen maken in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Groepen in Azure Active Directory verwijderen. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lezen groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/groups/restore | Groepen in Azure Active Directory herstellen. |
| microsoft.aad.directory/groups/settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/assignLicense | Licenties voor gebruikers in Azure Active Directory beheren. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen van gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/create | Gebruikers maken in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Gebruikers in Azure Active Directory verwijderen. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Ongeldig alle vernieuwingstokens van gebruiker in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | De eigenschap users.manager in Azure Active Directory bijgewerkt. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Terugzetten van verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | De eigenschap users.userPrincipalName in Azure Active Directory bijgewerkt. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en Azure Service Health configureren. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maken en beheren van tickets van ondersteuning van Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Eenvoudige eigenschappen voor alle resources in microsoft.office365.webPortal lezen. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en Office 365-servicestatus configureren. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maken en beheren van Office 365-ondersteuningstickets. |

## <a name="role-template-ids"></a>Rolsjabloon-id

Rolsjabloon-id worden voornamelijk door gebruikers van Graph API of PowerShell gebruikt.

Graph displayName | Weergavenaam van Azure portal | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Toepassingsbeheerder | Toepassingsbeheerder | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Toepassingsontwikkelaar | Toepassingsontwikkelaar | CF1C38E5-3621-4004-A7CB-879624DCED7C
Authentication-beheerder | Authentication-beheerder | c4e39bd9-1100-46d3-8c65-fb160da0071f
Azure Information Protection-beheerder | Azure Information Protection-beheerder | 7495fdc4-34c4-4d15-a289-98788ce399fd
B2C-gebruikersstroom beheerder | B2C-gebruikersstroom beheerder | 6e591065-9bad-43ed-90f3-e9424366d2f0
B2C User Flow Attribute Administrator | B2C User Flow Attribute Administrator | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
B2C IEF Keyset Administrator | B2C IEF Keyset Administrator | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C IEF Policy Administrator | B2C IEF Policy Administrator | 3edaf663-341e-4475-9f94-5c398ef6c070
Factureringsbeheerder | Factureringsbeheerder | b0f54661-2d74-4c50-afa3-1ec803f12efe
Desktop Analytics-Administrator | Desktop Analytics-Administrator | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Beheerder van de cloudtoepassing | Beheerder van de cloudtoepassing | 158c047a-c907-4556-b7ef-446551a6b5f7
Cloud-Apparaatbeheerder | Cloud-apparaatbeheerder | 7698a772-787b-4ac8-901f-60d6b08affd2
Bedrijfsbeheerder | Globale beheerder | 62e90394-69f5-4237-9190-012177145e10
Beheerder voor naleving | Beheerder voor naleving | 17315797-102d-40b4-93e0-432062caca18
Voorwaardelijke toegang beheerder | Voorwaardelijke toegang beheerder | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM-servicebeheerder | Dynamics 365-beheerder | 44367163-eba1-44c3-98af-f5787879f96a
Toegangsfiatteur voor klanten | Klanten-Lockbox toegang goedkeurder | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Apparaatbeheerders | Apparaatbeheerders | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Toevoegen | Toevoegen | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Device Manager-services | Device Manager-services | 2b499bcd-da44-4968-8aec-78e1674fa64d
Gebruikers van apparaten | Gebruikers van apparaten | d405c6df-0af8-4e3b-95e4-4d06e542189e
Adreslijstlezers | Adreslijstlezers | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Synchronisatie van Active Directory-Accounts | Directory-accounts voor synchronisatie | d29b2b05-8046-44ba-8758-1e26182fcf32
Schrijvers van mappen | Schrijvers van mappen | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Service-beheerder | Exchange-beheerder | 29232cdf-9323-42fd-ade2-1d097af3e4de
Externe id-Provider beheerder | Externe id-Provider beheerder | be2f45a1-457d-42af-a067-6ec1fa63bc45
Gastuitnodiging | Gastuitnodiging | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Helpdesk-beheerder | Wachtwoordbeheerder | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Intune-servicebeheerder | Intune-beheerder | 3a2c62db-5318-420d-8d74-23affee5d9d5
Kaizala-beheerder | Kaizala-beheerder | 74ef975b-6605-40af-a5d2-b9539d836353
Licentiebeheerder | Licentiebeheerder | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync-servicebeheerder | Skype voor bedrijven-beheerder | 75941009-915a-4869-abe7-691bff18279e
Privacy-berichtencentrum-lezer | Message center privacy lezer | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
Berichtencentrum-lezer | Message center lezer | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Laag1-ondersteuning voor partner | Laag1-ondersteuning voor partner | 4ba39ca4-527c-499a-b93d-d9b492c50246
Laag2-ondersteuning voor partner | Laag2-ondersteuning voor partner | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Power BI-servicebeheerder | Power BI-beheerder | a9ea8996-122f-4c74-9520-8edcd192826c
Beheerder met bevoorrechte verificatie | Bevoegde authentication-beheerder | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Beheerder met bevoorrechte rol | Beheerder met bevoorrechte rol | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapportenreader | Rapportenreader | 4a5d8f65-41da-4de4-8968-e035b65339cf
Zoekresultaten | Zoekresultaten | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Search-Editor | Search-editor | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Beveiligingsbeheerder | Beveiligingsbeheerder | 194ae4cb-b126-40b2-bd5b-6091b380977d
Beveiligingslezer | Beveiligingslezer | 5d6b6bb7-de71-4623-b4af-96380a352509
Beheerder serviceondersteuning | Servicebeheerder | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint-servicebeheerder | SharePoint-beheerder | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Teams communicatie beheerder | Teams communicatie beheerder | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Ondersteuningstechnicus van teams communicatie | Ondersteuningstechnicus van teams communicatie | f70938a0-fc10-4177-9e90-2178f8765737
Ondersteuningsmedewerker voor teams communicatie | Ondersteuningsmedewerker voor teams communicatie | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Teams-servicebeheerder | Teams-servicebeheerder | 69091246-20e8-4a56-aa4d-066075b2a7a8
Gebruiker | Gebruiker | a0b1b346-4d3e-4e8b-98f8-753987be4970
Beheerder van gebruikersaccounts | Gebruikerbeheerder | fe930be7-5e62-47db-91af-98c3a49a38b1
Werkplekapparaat toevoegen | Werkplekapparaat toevoegen | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit Azure AD in de toekomst.

* Hoclicentiebeheerder
* Toevoegen
* Device Manager-services
* Gebruikers van apparaten
* Geverifieerde e-mailgebruiker
* Postvakbeheerder
* Werkplekapparaat toevoegen

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het toewijzen van een gebruiker als een beheerder van een Azure-abonnement, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
