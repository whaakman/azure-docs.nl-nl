---
title: Beheerdersrollen toewijzen in Azure Active Directory | Microsoft Docs
description: Een beheerdersrol kunt gebruikers toevoegen, beheerdersrollen toewijzen, wachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren. Een gebruiker aan wie een beheerdersrol is toegewezen, heeft dezelfde machtigingen in voor alle cloudservices waarop uw organisatie is geabonneerd.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 1de2482b7795bbed82874b6eea29f89f1ff52560
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938413"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Beheerrollen toewijzen in Azure Active Directory

Met Azure Active Directory (Azure AD), kunt u afzonderlijke beheerders verschillende functies vervullen aanwijzen. Beheerders kunnen hebben toegang tot verschillende functies in de Azure portal en, afhankelijk van hun rol, maken of bewerken welke gebruikers, beheerdersrollen toewijzen aan anderen, gebruikerswachtwoorden, Gebruikerslicenties beheren en domeinen, onder andere beheren. Een gebruiker aan wie een beheerdersrol is toegewezen wordt dezelfde machtigingen hebben voor alle van de cloud-services waarop uw organisatie is geabonneerd op, ongeacht of u de rol in de Office 365-beheerportal of in de Azure portal of met behulp van de Azure AD-module voor toewijzen Windows PowerShell.

## <a name="details-about-the-global-administrator-role"></a>Meer informatie over de rol globale beheerder
De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de rol globale beheerder voor de map worden toegewezen door de persoon die zich voor een Azure-abonnement aanmeldt. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen.

## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen
Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Beschikbare rollen
De volgende beheerdersrollen zijn beschikbaar:

* **Toepassingsbeheerder**: gebruikers met deze rol kunnen maken en beheren van alle aspecten van bedrijfstoepassingen, registraties van de toepassing en proxy-instellingen van toepassing. Deze rol wordt ook de mogelijkheid om toestemming te gedelegeerde machtigingen, met uitzondering van Microsoft Graph en Azure AD Graph-toepassing worden machtigingen verleent. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe registraties van de toepassing of bedrijfstoepassingen.

* **Toepassingsontwikkelaar**: gebruikers met deze rol registraties van toepassing kunnen maken wanneer de 'Gebruikers kunnen zich registreren toepassingen' is ingesteld op Nee. Deze rol kan ook leden toe te staan hun eigen namens wanneer de 'Gebruikers kunnen instemmen met apps die toegang tot bedrijfsgegevens namens hen' is ingesteld op Nee. Leden van deze rol worden toegevoegd als eigenaars bij het maken van nieuwe registraties van de toepassing of bedrijfstoepassingen.

* **Financieel medewerker**: doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.

* **De beheerder van de cloud**: gebruikers met deze rol hebben dezelfde machtigingen als de rol beheerder van de toepassing, met uitzondering van de mogelijkheid voor het beheren van toepassingsproxy. Deze functie hebben de mogelijkheid maken en beheren van alle aspecten van bedrijfstoepassingen en -registraties van toepassing. Deze rol wordt ook de mogelijkheid om toestemming te gedelegeerde machtigingen, met uitzondering van Microsoft Graph en Azure AD Graph-toepassing worden machtigingen verleent. Leden van deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe registraties van de toepassing of bedrijfstoepassingen.

* **Naleving beheerder**: gebruikers met deze functie hebben beheermachtigingen binnen in de Office 365-beveiliging & Compliancecentrum en Exchange-beheercentrum. Meer informatie op '[over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d). "

* **Voorwaardelijke toegang beheerder**: gebruikers met deze functie hebben de mogelijkheid voor het beheren van instellingen voor voorwaardelijke toegang van Azure Active Directory.
  > [!NOTE]
  > Voor Exchange ActiveSync-beleid voor voorwaardelijke toegang in Azure implementeert, moet de gebruiker ook globale beheerder zijn.
  
* **Apparaatbeheerders**: deze functie is beschikbaar voor toewijzing alleen als een lokale beheerder zijn aanvullende in [apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaten-objecten in Azure Active Directory.

* **Directory lezers**: dit is een verouderde rol die is toegewezen aan toepassingen die geen ondersteuning voor de [Framework toestemming](active-directory-integrating-applications.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **Directory-synchronisatie Accounts**: niet gebruiken. Deze rol is wordt automatisch toegewezen aan de service Azure AD Connect en niet bedoeld of ondersteund voor ander gebruik.

* **Directory schrijvers**: dit is een verouderde rol die is toegewezen aan toepassingen die geen ondersteuning voor de [Framework toestemming](active-directory-integrating-applications.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **Dynamics 365 beheerder**: gebruikers aan deze rol globale machtigingen in Microsoft Dynamics 365, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de status van de service hebben. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Exchange-servicebeheerder**: gebruikers met deze rol globale machtigingen in Microsoft Exchange Online zijn wanneer de service aanwezig is. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Globale beheerder / Company Administrator / Tenantbeheerder**: gebruikers aan deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die worden gefedereerd met Azure Active Directory zoals Exchange Online SharePoint Online en Skype voor bedrijven Online. De persoon die zich voor de Azure Active Directory-tenant aanmeldt wordt een globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er zijn meer dan één globale beheerder in uw bedrijf. Globale beheerders kunt opnieuw instellen van het wachtwoord voor elke gebruiker en alle andere beheerders.

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell geïdentificeerd als "Company Administrator". Het 'Globale beheerder' zich in de [Azure-portal](https://portal.azure.com).
  >
  >

* **Gast uitnodiging antwoorden**: gebruikers met deze rol kunnen Azure Active Directory B2B Gast gebruiker uitnodigingen beheren wanneer de instelling 'Leden kunnen uitnodigen' gebruiker is ingesteld op Nee. Meer informatie over B2B-samenwerking op [over Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Dit omvat geen andere machtigingen.

* **Information Protection-beheerder**: hebt gebruikersrechten hebben gebruikers met deze rol alleen op de Azure Information Protection-service. Ze worden gebruikersrechten op Identity Protection Center, Privileged Identity Management, controleprogramma Office 365-Service de status, of Office 365-beveiliging en naleving Center niet verleend. Ze kunnen labels voor het Azure Information Protection-beleid configureren, beheren van sjablonen voor beveiliging en bescherming activeren.

* **Intune-servicebeheerder**: gebruikers met deze rol globale machtigingen in Microsoft Intune Online zijn wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren.

* **Postvak beheerder**: deze rol wordt alleen gebruikt als onderdeel van Exchange Online e-mailondersteuning voor RIM Blackberry-apparaten. Gebruik deze functie niet als uw organisatie geen Exchange Online e-mail op RIM Blackberry-apparaten gebruikt.

* **Message Center lezer**: gebruikers met deze rol kunnen controleren, meldingen en advisory health-updates in [Office 365-berichtencentrum](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie op de geconfigureerde services, zoals Exchange, Intune en Microsoft Teams. Message Center lezers wekelijks e verwerkingen van berichten, updates, ontvangen en message center berichten in Office 365 kunnen delen. In Azure AD worden gebruikers die zijn toegewezen aan deze rol alleen alleen-lezen toegang hebben op Azure AD-services zoals gebruikers en groepen. 

* **Ondersteuning voor laag 1 partner**: niet gebruiken. Deze rol is gedeprecieerd en wordt verwijderd uit in de toekomst Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal Microsoft-partners zijn doorverkoop en is niet bedoeld voor algemeen gebruik.

* **Laag 2-ondersteuning partner**: niet gebruiken. Deze rol is gedeprecieerd en wordt verwijderd uit in de toekomst Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal Microsoft-partners zijn doorverkoop en is niet bedoeld voor algemeen gebruik.

* **Wachtwoordbeheerder / Helpdesk beheerder**: gebruikers met deze rol kunnen wijzigen van wachtwoorden, serviceaanvragen beheren en controleren van de servicestatus. Helpdesk-beheerders kunnen wachtwoorden alleen voor gebruikers en andere beheerders Helpdesk wijzigen. 

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell aangeduid als 'Helpdesk-beheerder'. 'Wachtwoordbeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >
  
* **Power BI-servicebeheerder**: gebruikers met deze functie hebben algemene machtigingen binnen Microsoft Power BI, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de servicestatus. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Bevoegde rol beheerder**: gebruikers met deze rol roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management kunnen beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

* **Lezer rapporten**: gebruikers met deze rol gebruiksrapportage gegevens en het dashboard met rapporten in Office 365-beheercentrum en de acceptatie-context pack in Power BI kunnen weergeven. Bovendien de rol toegang biedt tot aanmelding rapporten en de activiteit in Azure AD en gegevens die door Microsoft Graph geretourneerd rapportage-API. Een gebruiker die is toegewezen aan de rol Lezer rapporten hebben toegang tot alleen relevante informatie over het gebruik en acceptatie metrische gegevens. Ze hebben geen beheerdersmachtigingen voor het configureren van instellingen of toegang tot die het product specifieke beheercentrums zoals Exchange. 

* **Beveiligingsbeheerder**: gebruikers met deze functie hebben alle alleen-lezen machtigingen van de beveiligingsrol van de lezer, plus de mogelijkheid voor het beheren van configuratie voor beveiliging gerelateerde services: Azure Active Directory: Identity Protection, Azure Gegevensbeveiliging, Privileged Identity Management en Office 365-beveiliging en naleving Center. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365-beveiliging & Compliancecentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Beveiliging lezer**: gebruikers met deze rol globale alleen-lezen toegang hebben, met inbegrip van alle gegevens in Azure Active Directory, de bescherming van identiteiten, Privileged Identity Management, evenals de mogelijkheid om te lezen van Azure Active Directory-in rapporten en controlelogboeken. De functie geeft ook het kenmerk alleen-lezen recht in Office 365-beveiliging en naleving Center. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365-beveiliging & Compliancecentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Ondersteuning voor servicebeheerder**: gebruikers aan deze rol kunnen ondersteuningsaanvragen openen met Microsoft Azure en Office 365-services, weergaven en de servicedashboard en het bericht centreren in de Azure-portal en Office 365-beheerportal. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint-servicebeheerder**: gebruikers aan deze rol globale machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de status van de service hebben. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype voor bedrijven / Lync servicebeheerder**: gebruikers met deze rol globale machtigingen in de Microsoft Skype voor bedrijven, hebben wanneer de service aanwezig is, evenals de gebruikerskenmerken Skype-specifieke in Azure Active Directory beheren. Deze rol, hebben bovendien de mogelijkheid ondersteuningstickets beheren en controleren van de servicestatus. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell geïdentificeerd als 'Lync servicebeheerder'. Het 'Skype voor bedrijven-servicebeheerder' zich in de [Azure-portal](https://portal.azure.com/).
  >
  >

* **De accountbeheerder gebruiker**: gebruikers met deze rol kunnen maken en beheren van alle aspecten van gebruikers en groepen. Bovendien deze rol omvat de mogelijkheid voor het beheren van ondersteuningstickets en monitors health-service. Er zijn enkele beperkingen van toepassing. Deze rol kunnen bijvoorbeeld niet verwijderen van een globale beheerder. Accountbeheerders gebruiker kunnen wijzigen van wachtwoorden voor gebruikers, beheerders voor Helpdesk en andere gebruikersaccount beheerders alleen.

## <a name="administrator-permissions"></a>Administrator-machtigingen

### <a name="application-administrator"></a>Toepassingsbeheerder

| Kan doen | Is niet mogelijk |
| --- | --- |
| Lezen van alle directorygegevens<br>Registraties van de toepassing maken<br>Toepassingseigenschappen registratie bijwerken<br>Verkrijgen van bedrijfstoepassingen<br>Registratie-toepassing worden machtigingen beheren<br>Registraties van toepassing verwijderen<br>Enterprise eenmalige aanmelding Toepassingsinstellingen beheren<br>Instellingen voor het inrichten bedrijfstoepassing beheren<br>Enterprise selfservice Toepassingsinstellingen beheren<br>Enterprise machtiging Toepassingsinstellingen beheren<br>Toegang tot toepassingen beheren<br>De configuratie-instellingen beheren<br>Enterprise-toepassingen verwijderen<br>Toestemming namens iedereen voor alle machtigingsaanvragen van gedelegeerde<br>Toestemming namens iedereen voor alle aanvragen van toepassing machtiging, behalve in Azure AD Graph of Microsoft Graph<br>Application proxy-instellingen beheren<br>Toegangsinstellingen voor services<br>Status van de service monitor<br>Ondersteuningstickets beheren<br>Groepslidmaatschap voor lezen, verborgen | Maken, bewerken en verwijderen van groepen<br>Gebruikerslicenties beheren<br>Adreslijstsynchronisatie gebruiken<br>Aanmelden rapporten weergeven en controlelogboeken | 

### <a name="application-developer"></a>Toepassingsontwikkelaar

| Kan doen | Is niet mogelijk |
| --- | --- |
| Lezen van alle directorygegevens<br>Registraties van de toepassing maken<br>Toestemming namens self | Aanmelden weergeven en controlelogboeken<br>Groepslidmaatschap voor lezen, verborgen |

### <a name="billing-administrator"></a>Factureringsbeheerder

| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Factuur- en bewerkingen voor Office-producten</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p> |

### <a name="cloud-application-administrator"></a>Beheerder van de cloudtoepassing

| Kan doen | Is niet mogelijk |
| --- | --- |
| Lezen van alle directorygegevens<br>Registraties van de toepassing maken<br>Toepassingseigenschappen registratie bijwerken<br>Verkrijgen van bedrijfstoepassingen<br>Registratie-toepassing worden machtigingen beheren<br>Registraties van toepassing verwijderen<br>Enterprise eenmalige aanmelding Toepassingsinstellingen beheren<br>Instellingen voor het inrichten bedrijfstoepassing beheren<br>Enterprise selfservice Toepassingsinstellingen beheren<br>Enterprise machtiging Toepassingsinstellingen beheren<br>Toegang tot toepassingen beheren<br>De configuratie-instellingen beheren<br>Enterprise-toepassingen verwijderen<br>Toestemming namens iedereen voor alle machtigingsaanvragen van gedelegeerde<br>Toestemming namens iedereen voor alle aanvragen van toepassing machtiging, behalve in Azure AD Graph of Microsoft Graph<br>Toegangsinstellingen voor services<br>Status van de service monitor<br>Ondersteuningstickets beheren<br>Groepslidmaatschap voor lezen, verborgen | Application proxy-instellingen beheren<br>Maken, bewerken en verwijderen van groepen<br>Gebruikerslicenties beheren<br>Adreslijstsynchronisatie gebruiken<br>Aanmelden rapporten weergeven en controlelogboeken |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder

| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Instellingen voor voorwaardelijke toegang beheren</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p>|

### <a name="global-administrator"></a>Globale beheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruikerswachtwoorden opnieuw instellen</p><p>Andere beheerders wachtwoorden opnieuw instellen</p> <p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor authentication-server</p><p>Auditlogboeken weergeven</p> |N/A |

### <a name="password-administrator--helpdesk-administrator"></a>Wachtwoordbeheerder / Helpdesk beheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Wachtwoorden voor gebruikers en andere Helpdesk-beheerders alleen wijzigen</p>|<p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Rapporten weergeven</p>|

### <a name="information-protection-administrator"></a>Information Protection-beheerder
In | Kan doen
-------- | ---------
Azure Information Protection | <li>Labels en instellingen configureren in het beleid voor globale en bereik<li>Configureren en beheren van sjablonen voor beveiliging<li>Activeren of deactiveren beveiliging--
 
### <a name="reports-reader"></a>Rapportenlezer 
Kan doen | Is niet mogelijk
------ | ----------
Weergave Azure AD-aanmeldingspagina rapporten en controlelogboeken<br>Gegevens van bedrijfs- en gebruikersgegevens weergeven<br>Dashboard voor toegang tot Office 365-gebruiksgegevens | Gebruiker weergaven maken en beheren<br>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren<br>Beheerdersrollen aan anderen delegeren<br>Beheren van bedrijfsgegevens

### <a name="security-reader"></a>Beveiligingslezer
| In | Kan doen |
| --- | --- |
| Identity Protection Center |Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde threat protection<li>Antiphishing-<li>Mailflow regels |
| Privileged Identity Management |<p>Alleen-lezen toegang tot alle informatie zichtbaar is in Azure AD PIM: beleidsregels en rapporten voor Azure AD-roltoewijzingen beveiliging controleert en in de toekomst leestoegang heeft tot gegevens en rapporten voor scenario's naast de roltoewijzing Azure AD.<p>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengt. In de PIM-portal of via PowerShell kan iemand met deze rol aanvullende functies (bijvoorbeeld globale beheerder of beheerder met bevoorrechte rol) activeren als de gebruiker geschikt is voor deze. |
| <p>Monitor voor Office 365-servicestatus</p><p>Office 365-beveiliging en naleving Center</p> |<ul><li>Lees- en waarschuwingen beheren<li>Lezen van beveiligingsbeleid<li>Lezen dreigingen en Cloud App Discovery quarantaine in zoeken en onderzoeken<li>Alle rapporten lezen |

### <a name="security-administrator"></a>Beveiligingsbeheerder
| In | Kan doen |
| --- | --- |
| Identity Protection Center |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>Bovendien de mogelijkheid om alle IPC-bewerkingen, met uitzondering van opnieuw instellen van wachtwoorden uitvoeren. |
| Privileged Identity Management |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>**Kan geen** rollidmaatschappen voor Azure AD of instellingen beheren. |
| <p>Monitor voor Office 365-servicestatus</p><p>Office 365-beveiliging en naleving Center |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>Kan alle instellingen configureren in de functie Advanced Threat Protection (malware & virus beveiliging, schadelijke URL config, URL tracering, enzovoort). |

### <a name="service-administrator"></a>Servicebeheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p> |

### <a name="user-account-administrator"></a>Beheerder van gebruikersaccounts
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Wachtwoorden voor gebruikers, beheerders voor Helpdesk en andere alleen beheerders van gebruikersaccount wijzigen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties met beperkingen te beheren. Hij of zij kan geen algemeen beheerder verwijderen of andere beheerders maken.</p> |<p>Factuur- en bewerkingen voor Office-producten</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor authentication-server</p><p>Auditlogboeken weergeven</p> |

### <a name="to-add-a-user-as-a-global-administrator"></a>Een gebruiker toevoegen als globale beheerder

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder voor de tenantmap.

   ![Azure AD-beheercentrum te openen](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Selecteer **gebruikers** > **alle gebruikers**.

3. Open de pagina voor de gebruiker die u wilt aanwijzen als globale beheerder.

4. Selecteer op de opdrachtbalk **functie Directory**.

5. Selecteer **functie toevoegen**.

6. Selecteer op de directorypagina-rol de **hoofdbeheerder** rol en klik vervolgens op **Selecteer** om op te slaan.

## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit in de toekomst Azure AD.

* Ad-hoclicentiebeheerder
* Maker van via e-mail geverifieerde gebruikers
* Apparaat Join
* Apparaatbeheerders
* Gebruikers van apparaten
* Werkplekapparaat toevoegen

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het wijzigen van de beheerders voor een Azure-abonnement, [toevoegen of wijzigen beheerders van Azure-abonnement](../billing-add-change-azure-subscription-administrator.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie voor meer informatie over hoe Azure Active Directory is gekoppeld aan uw Azure-abonnement, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Gebruikers beheren](active-directory-create-users.md)
* [Wachtwoorden beheren](active-directory-manage-passwords.md)
* [Groepen beheren](fundamentals/active-directory-manage-groups.md)
