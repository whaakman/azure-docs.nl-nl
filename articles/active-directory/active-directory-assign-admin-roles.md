---
title: Beheerdersrollen toewijzen in Azure Active Directory | Microsoft Docs
description: Een beheerdersrol kan worden gebruikt voor het maken of bewerken van gebruikers, beheerdersrollen toewijzen, opnieuw instellen van gebruikerswachtwoorden, Gebruikerslicenties beheren of domeinen beheren. Een gebruiker aan wie een beheerdersrol is toegewezen, heeft dezelfde machtigingen in voor alle cloudservices waarop uw organisatie is geabonneerd.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.reviewer: Vince.Smith
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 042e2f4117a35e80694a1643dd95fa54d508f1f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Beheerrollen toewijzen in Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal]()
> * [Klassieke Azure Portal](active-directory-assign-admin-roles.md)
>
>

Azure Active Directory (Azure AD) gebruiken om aan te wijzen afzonderlijke beheerders voor verschillende functies. Deze beheerders hebben toegang tot de geselecteerde onderdelen in de Azure-portal of de klassieke Azure-portal en, afhankelijk van hun rol, is mogelijk te maken of bewerken welke gebruikers, beheerdersrollen toewijzen aan anderen gebruikerswachtwoorden, Gebruikerslicenties beheren en domeinen, onder andere beheren. Een gebruiker aan wie een beheerdersrol is toegewezen, wordt dezelfde machtigingen hebben voor alle van de cloud-services waarop uw organisatie is geabonneerd, ongeacht of u de rol in de Office 365-beheerportal of in de klassieke Azure portal of met behulp van de Azure AD-module voor Microsoft PowerShell toewijst.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen. Voor informatie over het toewijzen van beheerdersrollen in het Azure AD-beheercentrum, Zie [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).


De volgende beheerdersrollen zijn beschikbaar:

* **Financieel medewerker**: doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.

* **Naleving beheerder**: gebruikers met deze functie hebben beheermachtigingen binnen in de Office 365-beveiliging & Compliancecentrum en Exchange-beheercentrum. Meer informatie op '[over Office 365-beheerdersrollen](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d). "

* **Voorwaardelijke toegang beheerder**: gebruikers met deze functie hebben de mogelijkheid voor het beheren van instellingen voor voorwaardelijke toegang van Azure Active Directory.

* **Servicebeheerder CRM**: gebruikers met deze functie hebben algemene machtigingen binnen Microsoft CRM Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de servicestatus. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Apparaatbeheerders**: gebruikers met deze rol worden beheerders van de lokale computer op alle Windows 10-apparaten die zijn gekoppeld aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaten-objecten in Azure Active Directory.

* **Directory lezers**: dit is een verouderde rol die is toegewezen aan toepassingen die geen ondersteuning voor de [Framework toestemming](active-directory-integrating-applications.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **Directory-synchronisatie Accounts**: niet gebruiken. Deze rol is wordt automatisch toegewezen aan de service Azure AD Connect en niet bedoeld of ondersteund voor ander gebruik.

* **Directory schrijvers**: dit is een verouderde rol die is toegewezen aan toepassingen die geen ondersteuning voor de [Framework toestemming](active-directory-integrating-applications.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **Exchange-servicebeheerder**: gebruikers met deze rol globale machtigingen in Microsoft Exchange Online zijn wanneer de service aanwezig is. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Globale beheerder / Company Administrator**: gebruikers aan deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die worden gefedereerd met Azure Active Directory zoals Exchange Online, SharePoint Online en Skype voor bedrijven Online. De persoon die zich voor de Azure Active Directory-tenant aanmeldt wordt een globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er zijn meer dan één globale beheerder in uw bedrijf. Globale beheerders kunt opnieuw instellen van het wachtwoord voor elke gebruiker en alle andere beheerders.

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell geïdentificeerd als "Company Administrator". Het 'Globale beheerder' zich in de [Azure-portal](https://portal.azure.com).
  >
  >

* **Gast uitnodiging antwoorden**: gebruikers met deze rol kunnen Azure Active Directory B2B Gast gebruiker uitnodigingen beheren wanneer de instelling 'Leden kunnen uitnodigen' gebruiker is ingesteld op Nee. Meer informatie over B2B-samenwerking op [over de Azure AD B2B-samenwerking preview](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Dit omvat geen andere machtigingen.

* **Intune-servicebeheerder**: gebruikers met deze rol globale machtigingen in Microsoft Intune Online zijn wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren.

* **Postvak beheerder**: deze rol wordt alleen gebruikt als onderdeel van Exchange Online e-mailondersteuning voor RIM Blackberry-apparaten. Gebruik deze functie niet als uw organisatie geen Exchange Online e-mail op RIM Blackberry-apparaten gebruikt.

* **Ondersteuning voor laag 1 partner**: niet gebruiken. Deze rol is gedeprecieerd en wordt verwijderd uit in de toekomst Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal Microsoft-partners zijn doorverkoop en is niet bedoeld voor algemeen gebruik.

* **Laag 2-ondersteuning partner**: niet gebruiken. Deze rol is gedeprecieerd en wordt verwijderd uit in de toekomst Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal Microsoft-partners zijn doorverkoop en is niet bedoeld voor algemeen gebruik.

* **Wachtwoordbeheerder / Helpdesk beheerder**: gebruikers met deze rol kunnen wachtwoorden opnieuw instellen, serviceaanvragen beheren en controleren van de servicestatus. Wachtwoordbeheerders kunnen wachtwoorden alleen voor gebruikers en andere wachtwoordbeheerders opnieuw instellen.

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell aangeduid als 'Helpdesk-beheerder'. 'Wachtwoordbeheerder' is in de [Azure-portal](https://portal.azure.com/).
  >
  >
  
* **Power BI-servicebeheerder**: gebruikers met deze functie hebben algemene machtigingen binnen Microsoft Power BI, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de servicestatus. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Bevoegde rol beheerder**: gebruikers met deze rol roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management kunnen beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

* **Beveiligingsbeheerder**: gebruikers met deze functie hebben alle alleen-lezen machtigingen van de beveiligingsrol van de lezer, plus de mogelijkheid voor het beheren van configuratie voor beveiliging gerelateerde services: Azure Active Directory: Identity Protection, Azure Information Protection, Privileged Identity Management en Office 365-beveiliging en naleving Center. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365-beveiliging & Compliancecentrum](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Beveiliging lezer**: gebruikers met deze rol globale alleen-lezen toegang hebben, met inbegrip van alle gegevens in Azure Active Directory, Identity Protection, Privileged Identity Management, evenals de mogelijkheid om te lezen van Azure Active Directory-in rapporten en controlelogboeken. De functie geeft ook het kenmerk alleen-lezen recht in Office 365-beveiliging en naleving Center. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365-beveiliging & Compliancecentrum](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Ondersteuning voor servicebeheerder**: gebruikers aan deze rol kunnen ondersteuningsaanvragen openen met Microsoft Azure en Office 365-services, weergaven en de servicedashboard en het bericht centreren in de Azure-portal en Office 365-beheerportal. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint-servicebeheerder**: gebruikers aan deze rol globale machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en controleren van de status van de service hebben. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype voor bedrijven / Lync servicebeheerder**: gebruikers met deze rol globale machtigingen in de Microsoft Skype voor bedrijven, hebben wanneer de service aanwezig is, evenals de gebruikerskenmerken Skype-specifieke in Azure Active Directory beheren. Deze rol, hebben bovendien de mogelijkheid ondersteuningstickets beheren en controleren van de servicestatus. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Deze rol wordt in Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell geïdentificeerd als 'Lync servicebeheerder'. Het 'Skype voor bedrijven-servicebeheerder' zich in de [Azure-portal](https://portal.azure.com/).
  >
  >

* **De accountbeheerder gebruiker**: gebruikers met deze rol kunnen maken en beheren van alle aspecten van gebruikers en groepen. Bovendien deze rol omvat de mogelijkheid voor het beheren van ondersteuningstickets en monitors health-service. Er zijn enkele beperkingen van toepassing. Bijvoorbeeld: deze rol is niet toegestaan voor het verwijderen van een globale beheerder en terwijl u toelaat wijzigen van wachtwoorden voor niet-beheerders, kunnen geen wijzigen van wachtwoorden voor globale beheerders of andere bevoorrechte beheerders.

## <a name="administrator-permissions"></a>Administrator-machtigingen

### <a name="billing-administrator"></a>Financieel medewerker

| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Factuur- en bewerkingen voor Office-producten</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p>|

### <a name="conditional-access-administrator"></a>Beheerder van voorwaardelijke toegang
| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Instellingen voor voorwaardelijke toegang beheren</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p>|

### <a name="global-administrator"></a>Globale beheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
|<p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruikerswachtwoorden opnieuw instellen</p><p>Andere beheerder de wachtwoorden opnieuw instellen</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor authentication-server</p><p>Auditlogboeken weergeven</p> |<p>N.v.t.</p>|

### <a name="password-administrator"></a>Wachtwoordbeheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Gebruikerswachtwoorden opnieuw instellen</p> <p>Andere beheerder de wachtwoorden opnieuw instellen</p>|<p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Rapporten weergeven</p>|

### <a name="service-administrator"></a>Servicebeheerder
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p> |<p>Gebruikerswachtwoorden opnieuw instellen</p><p>Factuur- en bewerkingen voor Office-producten</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties beheren</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>Auditlogboeken weergeven</p> |

### <a name="user-administrator"></a>De Gebruikersbeheerder van de
| Kan doen | Is niet mogelijk |
| --- | --- |
| <p>Gegevens van bedrijfs- en gebruikersgegevens weergeven</p><p>Office-ondersteuningstickets beheren</p><p>Gebruikerswachtwoorden, met beperkingen.</p><p>Andere beheerder de wachtwoorden opnieuw instellen</p><p>Opnieuw instellen van wachtwoorden van andere gebruikers</p><p>Gebruiker weergaven maken en beheren</p><p>Maken, bewerken, en gebruikers en groepen verwijderen en gebruikerslicenties met beperkingen te beheren. Hij of zij kan geen algemeen beheerder verwijderen of andere beheerders maken.</p> |<p>Factuur- en bewerkingen voor Office-producten</p><p>Domeinen beheren</p><p>Beheren van bedrijfsgegevens</p><p>Beheerdersrollen aan anderen delegeren</p><p>Adreslijstsynchronisatie gebruiken</p><p>In- of uitschakelen van multi-factor authentication-server</p><p>Auditlogboeken weergeven</p> |

### <a name="security-reader"></a>Beveiliging lezer
| in | Kan doen |
| --- | --- |
| Identity Protection Center |Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<ul><li>Tegen ongewenste e-mail<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde threat protection<li>Antiphishing-<li>Mailflow regels |
| Privileged Identity Management |<p>Alleen-lezen toegang tot alle informatie zichtbaar is in Azure AD PIM: beleidsregels en rapporten voor Azure AD-roltoewijzingen beveiliging controleert en in de toekomst leestoegang heeft tot gegevens en rapporten voor scenario's naast de roltoewijzing Azure AD.<p>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengt. In de PIM-portal of via PowerShell kan iemand met deze rol aanvullende functies (bijvoorbeeld globale beheerder of beheerder met bevoorrechte rol) activeren als de gebruiker geschikt is voor deze. |
| <p>Monitor voor Office 365-servicestatus</p><p>Office 365-beveiliging en naleving Center</p> |<ul><li>Lees- en waarschuwingen beheren<li>Lezen van beveiligingsbeleid<li>Lezen dreigingen en Cloud App Discovery quarantaine in zoeken en onderzoeken<li>Alle rapporten lezen |

### <a name="security-administrator"></a>Beveiligingsbeheerder
| in | Kan doen |
| --- | --- |
| Identity Protection Center |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>Bovendien de mogelijkheid om alle IPC-bewerkingen, met uitzondering van opnieuw instellen van wachtwoorden uitvoeren. |
| Privileged Identity Management |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>**Kan geen** rollidmaatschappen voor Azure AD of instellingen beheren. |
| <p>Monitor voor Office 365-servicestatus</p><p>Office 365-beveiliging en naleving Center |<ul><li>Alle machtigingen van de rol Lezer van de beveiliging.<li>Kan alle instellingen configureren in de functie Advanced Threat Protection (malware & virus beveiliging, schadelijke URL config, URL tracering, enzovoort). |

## <a name="details-about-the-global-administrator-role"></a>Meer informatie over de rol globale beheerder
De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de rol globale beheerder voor de map worden toegewezen door de persoon die zich voor een Azure-abonnement aanmeldt. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen.

### <a name="to-add-a-colleague-as-a-global-administrator"></a>Een collega als globale beheerder toevoegen

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de tenantmap.

   ![Azure AD-beheercentrum te openen](./media/active-directory-assign-admin-roles/active-directory-admin-center.png)

2. Selecteer **gebruikers en groepen &gt; alle gebruikers**

3. De gebruiker die u wilt aanwijzen als globale beheerder en open de blade voor die gebruiker niet vinden.

4. Selecteer op de blade gebruiker **functie Directory**.
 
5. Selecteer op de blade van de rol directory de **hoofdbeheerder** rol, en op te slaan.

## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen
Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn gedeprecieerd en wordt verwijderd uit in de toekomst Azure AD.

* De beheerder van de ad-hoc-licentie
* De maker van de geverifieerde gebruiker e-mail
* Apparaat Join
* Apparaatbeheer
* Gebruikers van apparaten
* Werkplekkoppeling apparaat

## <a name="next-steps"></a>Volgende stappen
* Als u meer wilt weten over het wijzigen van de beheerders van een Azure-abonnement, ziet u [Azure-beheerdersrollen toevoegen of wijzigen](../billing-add-change-azure-subscription-administrator.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](active-directory-understanding-resource-access.md)
* Zie voor meer informatie over hoe Azure Active Directory is gekoppeld aan uw Azure-abonnement, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Gebruikers beheren](active-directory-create-users.md)
* [Wachtwoorden beheren](active-directory-manage-passwords.md)
* [Groepen beheren](active-directory-manage-groups.md)
