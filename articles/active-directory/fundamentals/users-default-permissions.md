---
title: Standaard gebruikersmachtigingen - Azure Active Directory | Microsoft Docs
description: Meer informatie over de andere gebruikersmachtigingen beschikbaar zijn in Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: lizross
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.openlocfilehash: c737ca9f2214df4fe06780604261995540c9aaaf
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103289"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Wat zijn de standaardmachtigingen van de gebruiker in Azure Active Directory?
In Azure Active Directory (Azure AD) wordt aan alle gebruikers een reeks standaardmachtigingen verleend. De toegang van gebruikers bestaat uit het type van de gebruiker, hun [rollidmaatschappen](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal), en het eigendom van de afzonderlijke objecten. Dit artikel beschrijft deze standaardmachtigingen en bevat een vergelijking van de standaardinstellingen voor lid- en gastgebruikers.

## <a name="member-and-guest-users"></a>Lid- en gastgebruikers
De reeks standaardmachtigingen die wordt ontvangen, is afhankelijk van de vraag of de gebruiker een systeemeigen lid van de tenant (lidgebruiker) of de gebruiker een gast voor B2B-samenwerking (gastgebruiker) is. Zie voor meer informatie over B2B-samenwerking [wat is Azure AD B2B-samenwerking?](../b2b/what-is-b2b.md) voor meer informatie over gastgebruikers. 
* Lidgebruikers kunnen toepassingen registreren, hun eigen profielfoto en mobiele telefoonnummer beheren, hun eigen wachtwoord wijzigen en B2B-gasten uitnodigen. Gebruikers kunnen bovendien (op een paar uitzonderingen na) alle directorygegevens lezen. 
* Azure AD B2B-gastgebruikers hebben beperkte directorymachtigingen. Gastgebruikers kunnen bijvoorbeeld afgezien van hun eigen profielgegevens niet bladeren door informatie van de tenant. Een gastgebruiker kan echter informatie over een andere gebruiker ophalen door de Principal-naam van gebruiker of de object-id te verstrekken. Een gast kan niet alle informatie over andere tenantobjecten bekijken, zoals groepen en toepassingen.

Standaardmachtigingen voor gasten zijn standaard beperkt. Gasten kunnen worden toegevoegd aan beheerdersrollen, waarmee aan hen volledige lees- en schrijftoegang voor de rol wordt verleend. Er is een aanvullende beperking beschikbaar, namelijk de mogelijkheid voor gasten om andere gasten uit te nodigen. Als de instelling **Gasten kunnen uitnodigingen versturen** op **Nee** wordt ingesteld, kunnen gasten geen andere gasten uitnodigen. Zie [Uitnodigingen delegeren voor B2B-samenwerking](../b2b/delegate-invitations.md) voor meer informatie. Als u gastgebruikers standaard dezelfde machtigingen wilt verlenen als lidgebruikers, stelt u **De machtigingen van gastgebruikers zijn beperkt** in op **Nee**. Met deze instelling worden alle gebruikersmachtigingen van leden standaard verleend aan gastgebruikers, en is het ook toegestaan gastgebruikers toe te voegen aan beheerdersrollen.

## <a name="compare-member-and-guest-default-permissions"></a>Standaardmachtigingen voor leden en gasten vergelijken

**Onderwerp** | **Gebruikersmachtigingen voor leden** | **Gebruikersmachtigingen voor gasten**
------------ | --------- | ----------
Gebruikers en contactpersonen | Alle openbare eigenschappen lezen van gebruikers en contactpersonen<br>Gasten uitnodigen<br>Eigen wachtwoord wijzigen<br>Eigen mobiele nummer beheren<br>Eigen foto beheren<br>Eigen vernieuwingstekens ongeldig verklaren | Eigen eigenschappen lezen<br>Weergavenaam, e-mail, aanmeldingsnaam, foto's, UPN-naam en type gebruikerseigenschappen van andere gebruikers en contactpersonen lezen<br>Eigen wachtwoord wijzigen
Groepen | Beveiligingsgroepen maken<br>Office 365-groepen maken<br>Alle eigenschappen van groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groep lezen<br>Eigenschappen, eigendom en lidmaatschap van groepen in eigendom beheren<br>Gasten toevoegen aan groepen in eigendom<br>Instellingen voor dynamisch lidmaatschap beheren<br>Groepen in eigendom verwijderen<br>Office 365-groepen in eigendom herstellen | Alle eigenschappen van groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groepen lezen<br>Groepen in eigendom beheren<br>Gasten toevoegen aan groepen in eigendom (indien toegestaan)<br>Groepen in eigendom verwijderen<br>Office 365-groepen in eigendom herstellen 
Toepassingen | Nieuwe toepassing registreren (maken)<br>Eigenschappen van geregistreerde en bedrijfstoepassingen lezen<br>Eigenschappen, toewijzingen en referenties van toepassingen beheren voor toepassingen in eigendom<br>Toepassingswachtwoord voor gebruiker maken of verwijderen<br>Toepassingen in eigendom verwijderen<br>Toepassingen in eigendom herstellen | Eigenschappen van geregistreerde en bedrijfstoepassingen lezen<br>Eigenschappen, toewijzingen en referenties van toepassingen beheren voor toepassingen in eigendom<br>Toepassingen in eigendom verwijderen<br>Toepassingen in eigendom herstellen
Apparaten | Alle eigenschappen van apparaten lezen<br>Alle eigenschappen van apparaten in eigendom lezen<br> | Geen machtigingen<br>Apparaten in eigendom verwijderen<br>
Directory | Alle bedrijfsgegevens lezen<br>Alle domeinen lezen<br>Alle partnercontracten lezen | Weergavenaam en geverifieerde domeinen lezen
Rollen en bereiken | Alle beheerdersrollen en lidmaatschappen lezen<br>Alle eigenschappen en het lidmaatschap van beheereenheden lezen | Geen machtigingen 
Abonnementen | Alle abonnementen lezen<br>Serviceplanlid inschakelen | Geen machtigingen
Beleidsregels | Alle eigenschappen van beleid lezen<br>Alle eigenschappen van beleid in eigendom lezen | Geen machtigingen

## <a name="to-restrict-the-default-permissions-for-member-users"></a>De standaardmachtigingen voor lidgebruikers beperken

Standaardmachtigingen voor lidgebruikers kunnen op de volgende manieren worden beperkt.

Machtiging | Uitleg van de instelling
---------- | ------------
De mogelijkheid beveiligingsgroepen te maken | Als u deze optie op Nee instelt, kunnen gebruikers geen beveiligingsgroepen maken. Globale beheerders en gebruikersaccountbeheerders kunnen nog steeds beveiligingsgroepen maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-cmdlets.md) voor meer informatie.
De mogelijkheid Office 365-groepen te maken | Als u deze optie op Nee instelt, kunnen gebruikers geen Office 365-groepen maken. Als u deze optie op Sommige instelt, kan een beperkte selectie van gebruikers Office 365-groepen maken. Globale beheerders en gebruikersaccountbeheerders kunnen nog steeds Office 365-groepen maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-cmdlets.md) voor meer informatie.
De toegang tot de Azure AD-beheerportal beperken | Deze optie instelt op Nee, wordt voorkomen dat gebruikers toegang tot Azure Active Directory.
Mogelijkheid om andere gebruikers te lezen | Deze instelling is alleen beschikbaar in PowerShell. Als u deze optie instelt op $false, voorkomt u dat alle niet-beheerders gebruikersgegevens uit de map lezen. Hiermee voorkomt u niet dat andere gebruikersgegevens in andere Microsoft-services, zoals Exchange Online, worden gelezen. Deze instelling is alleen bedoeld voor speciale omstandigheden en het wordt niet aanbevolen deze op $false in te stellen.

## <a name="object-ownership"></a>Eigendom van objecten

### <a name="application-registration-owner-permissions"></a>Eigenaarsmachtigingen toepassingsregistratie
Wanneer een gebruiker een toepassing registreert, wordt deze automatisch toegevoegd als een eigenaar voor de toepassing. Als eigenaar kan de gebruiker de metagegevens van de toepassing beheren, zoals de naam en de machtigingen waarom de app verzoekt. De gebruiker kan ook de tenantspecifieke configuratie van de toepassing beheren, zoals de SSO-toewijzingen voor configuratie en gebruikersbestanden. Een eigenaar kan ook andere eigenaren toevoegen of verwijderen. In tegenstelling tot globale beheerders kunnen eigenaren alleen toepassingen beheren waarvan ze eigenaar zijn.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Machtigingen groepseigenaar

Wanneer een gebruiker een groep maakt, wordt deze automatisch toegevoegd als een eigenaar voor die groep. Als eigenaar kan de gebruiker de eigenschappen van de groep beheren, zoals de naam, evenals het lidmaatschap. Een eigenaar kan ook andere eigenaren toevoegen of verwijderen. In tegenstelling tot globale beheerders en gebruikersaccountbeheerders kunnen eigenaren alleen groepen beheren waarvan ze eigenaar zijn. Zie [Eigenaren beheren voor een groep](active-directory-accessmanagement-managing-group-owners.md) voor informatie over het toewijzen van een groepseigenaar.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over hoe u Azure AD-beheerdersrollen toewijzen [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
* [Gebruikers beheren](add-users-azure-active-directory.md)
