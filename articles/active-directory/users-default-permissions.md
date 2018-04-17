---
title: Standaardmachtigingen voor gebruiker in Azure Active Directory vergelijken | Microsoft Docs
description: Lid, guest, de eigenaar van de app en groepsmachtigingen eigenaar vergelijken
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/29/2018
ms.author: curtand
ms.reviewer: vincesm
ms.openlocfilehash: 7465486109af773403c61f6b4505d932949009df
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="default-user-permissions-in-azure-active-directory"></a>Standaard-gebruikersmachtigingen in Azure Active Directory

In Azure Active Directory (Azure AD), worden alle gebruikers een reeks standaardmachtigingen verleend. Een gebruiker toegang bestaat uit het type van de gebruiker, hun [rollidmaatschappen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-assign-role-azure-portal), en het eigendom van afzonderlijke objecten. Dit artikel beschrijft de standaardmachtigingen en bevat een vergelijking van de standaardinstellingen van de gebruiker lid is en de Gast.

## <a name="member-and-guest-users"></a>Lid en het Gast-gebruikers
De reeks standaardmachtigingen ontvangen afhankelijk als de gebruiker een systeemeigen lid is van de tenant (lid gebruiker) of als de gebruiker een B2B-samenwerking gast (gastgebruiker). Zie voor meer informatie over B2B-samenwerking [wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md) voor meer informatie over gastgebruikers). 
* Gebruikers lid kunnen registreren van toepassingen, hun eigen profiel foto's en mobiele telefoonnummers aantal beheren hun eigen wachtwoord wijzigen en B2B gasten uitnodigen. Gebruikers kunnen bovendien alle directorygegevens (met een paar uitzonderingen) lezen. 
* Azure AD B2B-gastgebruikers beperkte machtigingen. Gastgebruikers kunnen bijvoorbeeld informatie van de tenant, afgezien van hun eigen profielgegevens niet doorzoeken. Een gastgebruiker kan informatie over een andere gebruiker echter ophalen door middel van de Principal-naam van gebruiker of het object-id. Een Gast kan niet alle informatie weergeven over andere tenant-objecten, zoals groepen en toepassingen.

Standaardmachtigingen voor gasten zijn standaard beperkend. Gasten kunnen worden toegevoegd aan de beheerdersrollen die machtigingen verlenen voor deze volledig lezen en schrijven die zijn opgenomen in de rol. Er is een aanvullende beperking beschikbaar is, de mogelijkheden van gasten om uit te nodigen andere gasten. Instelling **gasten kunnen uitnodigen** naar **Nee** gasten wordt voorkomen dat andere gasten uitnodigen. Zie [delegeren uitnodigingen voor B2B-samenwerking](active-directory-b2b-delegate-invitations.md) voor meer informatie over hoe. Voor het verlenen van gastgebruikers dezelfde machtigingen als gebruikers standaard ingesteld **gebruikersmachtigingen Gast zijn beperkt** naar **Nee**. Deze instelling verleent alle lid gebruikersmachtigingen voor gastgebruikers standaard ook garantie voor gasten worden toegevoegd aan beheerderrollen toestaan.

## <a name="compare-member-and-guest-default-permissions"></a>Lid en het gastbesturingssysteem standaardmachtigingen vergelijken

**Onderwerp** | **Machtigingen van de gebruiker lid** | **Gebruikersmachtigingen Gast**
------------ | --------- | ----------
Gebruikers en contactpersonen | Alle openbare eigenschappen van gebruikers en contactpersonen worden gelezen<br>Gasten uitnodigen<br>Eigen wachtwoord wijzigen<br>Beheren van eigen mobiele telefoonnummer<br>Eigen photo beheren<br>Ongeldig eigen vernieuwen van tokens | Eigen eigenschappen lezen<br>Lezen van de weergavenaam, e-aanmeldingsnaam, foto's, UPN-naam en type gebruikerseigenschappen van andere gebruikers en contactpersonen<br>Eigen wachtwoord wijzigen
Groepen   | Beveiligingsgroepen maken<br>Office 365-groepen maken<br>Alle eigenschappen van de groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groep lezen<br>Eigenschappen, eigendom en het lidmaatschap van groepen in eigendom van beheren<br>Gasten in eigendom van groepen toevoegen<br>Instellingen voor dynamisch lidmaatschap beheren<br>In eigendom van groepen verwijderen<br>Herstellen in eigendom van Office 365-groepen | Alle eigenschappen van de groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groepen lezen<br>In eigendom van groepen beheren<br>Gasten in eigendom van groepen toevoegen (als toegestaan)<br>In eigendom van groepen verwijderen<br>Herstellen in eigendom van Office 365-groepen           
Toepassingen | Registreren (maken) nieuwe toepassing<br>Lezen van de eigenschappen van geregistreerd en bedrijfstoepassingen<br>Toepassingseigenschappen, toewijzingen en referenties voor de eigen toepassingen beheren<br>Maak of verwijder de toepassing wachtwoord voor gebruiker<br>Verwijder in eigendom van toepassingen<br>Herstellen in eigendom van toepassingen | Lezen van de eigenschappen van geregistreerd en bedrijfstoepassingen<br>Toepassingseigenschappen, toewijzingen en referenties voor de eigen toepassingen beheren<br>Verwijder in eigendom van toepassingen<br>Herstellen in eigendom van toepassingen
Apparaten | Alle eigenschappen van de apparaten worden gelezen<br>Alle eigenschappen van de eigen apparaten beheren<br> | Geen machtigingen<br>Verwijderen van apparaten die Bedrijfseigendom zijn<br>
Directory | Alle bedrijfsgegevens lezen<br>Lezen van alle domeinen<br>Alle partner contracten lezen | Weergavenaam lees- en domeinen geverifieerd
Rollen en bereiken | Alle beheerdersrollen en -lidmaatschappen lezen<br>Alle eigenschappen en het lidmaatschap van administratieve eenheden lezen | Geen machtigingen              
Abonnementen | Lezen van alle abonnementen<br>Service-Plan lid inschakelen | Geen machtigingen
Beleidsregels | Alle eigenschappen van het beleid lezen<br>Alle eigenschappen in eigendom van beleid beheren | Geen machtigingen

## <a name="to-restrict-the-default-permissions-for-member-users"></a>De standaardmachtigingen voor gebruikers beperken


Standaardmachtigingen voor een lid van gebruikers in de volgende manieren kunnen worden beperkt. Zie voor meer informatie [Apps, machtigingen en toestemming in Azure Active Directory](active-directory-apps-permissions-consent.md).

Machtiging | Uitleg van de instelling
---------- | ------------
Mogelijkheid om beveiligingsgroepen te maken | Deze optie instelt op Nee, kunnen gebruikers beveiligingsgroepen maken. Globale beheerders en gebruikers accountbeheerders kunnen nog steeds beveiligingsgroepen maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsbeleidsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md) voor meer informatie over hoe.
Office 365-groepen maken | Deze optie instelt op Nee, kunnen gebruikers van Office 365-groepen maken. Deze optie instelt op sommige, kunt een bepaalde set gebruikers Office 365-groepen maken. Globale beheerders en gebruikers accountbeheerders is nog steeds mogelijk om Office 365-groepen te maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsbeleidsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md) voor meer informatie over hoe.
Mogelijkheid om te instemmen met toepassingen | Deze optie instelt op Nee, kunnen gebruikers ermee akkoord dat toepassingen. Globale beheerders wordt nog wel instemmen met toepassingen. Zie voor meer informatie [Apps, machtigingen en toestemming in Azure Active Directory](active-directory-apps-permissions-consent.md).
Galerie apps toevoegen aan Toegangsvenster | Deze optie instelt op Nee, kunnen gebruikers apps galerie toevoegen aan hun Toegangsvenster.
Mogelijkheid om te registreren (maken) toepassingen | Deze optie instelt op Nee, voorkomt u dat niet-beheerders maken van toepassingen. Globale beheerders is nog steeds mogelijk om toepassingen te maken. Zie voor meer informatie [Apps, machtigingen en toestemming in Azure Active Directory](active-directory-apps-permissions-consent.md).
Beheerders en gebruikers in de rol van Gast uitnodiging antwoorden kunnen uitnodigen gasten | Deze optie instelt op Nee voorkomt dat alle gebruikers uitnodigen gasten. Zie de standaardmachtigingen voor gebruikers configureren. Zie voor meer informatie [Apps, machtigingen en toestemming in Azure Active Directory](active-directory-apps-permissions-consent.md).
Leden kunnen uitnodigen gasten | In te stellen op no voorkomt dat gebruikers uitnodigen gasten. Globale beheerders, gebruiker accountbeheerders en Gast Inviters is nog steeds mogelijk om uit te nodigen gasten. Zie voor meer informatie [Apps, machtigingen en toestemming in Azure Active Directory](active-directory-apps-permissions-consent.md).
De toegang tot de Azure AD-beheerportal beperken | Deze optie instelt op Nee, kunnen gebruikers toegang tot de Azure Active Directory-portal.
Mogelijkheid om te lezen van andere gebruikers | Deze instelling is alleen beschikbaar in PowerShell. In te stellen op $false voorkomt u dat alle niet-beheerders gebruiker gegevens uit de map lezen. Dit voorkomt niet dat lezen gebruiker informatie in andere Microsoft-services, zoals Exchange Online. Deze instelling is alleen bedoeld voor speciale omstandigheden en dit op $false wordt niet aanbevolen instelling.

## <a name="object-ownership"></a>Eigendom van objecten

### <a name="application-registration-owner-permissions"></a>Toepassingsmachtigingen registratie eigenaar
Wanneer een gebruiker een toepassing registreert, worden ze automatisch toegevoegd als een eigenaar voor de toepassing. Als een eigenaar, kunnen ze de metagegevens van de toepassing beheren, zoals de naam en de machtigingen die de app verzoekt. Ook kunnen ze de tenant-specifieke configuratie van de toepassing, zoals de SSO-toewijzingen voor configuratie- en gebruikersbestanden beheren. Een eigenaar kan ook toevoegen of verwijderen van andere eigenaren. In tegenstelling tot globale beheerders kunnen eigenaren van toepassingen die ze eigenaar alleen beheren. Als de eigenaar van een toepassing registratie wilt toewijzen, Zie [Azure Active Directory-app-registratie](active-directory-app-registration.md).

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Groepsmachtigingen eigenaar

Wanneer een gebruiker een groep maakt, worden ze automatisch toegevoegd als eigenaar van die groep. Als een eigenaar kunnen ze beheren van de eigenschappen van de groep, zoals de naam, evenals lidmaatschap beheren. Een eigenaar kan ook toevoegen of verwijderen van andere eigenaren. In tegenstelling tot globale beheerders en gebruikers accountbeheerders kunnen eigenaren alleen groepen waarvan die ze eigenaar beheren. Als de Groepseigenaar van een wilt toewijzen, Zie [beheren-eigenaars voor een groep](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Volgende stappen

* Als u meer wilt weten over het wijzigen van de beheerders van een Azure-abonnement, ziet u [Azure-beheerdersrollen toevoegen of wijzigen](../billing-add-change-azure-subscription-administrator.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie voor meer informatie over hoe Azure Active Directory is gekoppeld aan uw Azure-abonnement, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Gebruikers beheren](active-directory-create-users.md)
