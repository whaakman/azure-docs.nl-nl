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
ms.date: 02/16/2019
ms.author: lizross
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 206e501860691cccc0578a0df4eec2b161b99b4c
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341369"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Wat zijn de standaardmachtigingen van de gebruiker in Azure Active Directory?
In Azure Active Directory (Azure AD) wordt aan alle gebruikers een reeks standaardmachtigingen verleend. De toegang van gebruikers bestaat uit het type van de gebruiker, hun [roltoewijzingen](active-directory-users-assign-role-azure-portal.md), en het eigendom van de afzonderlijke objecten. Dit artikel beschrijft deze standaardmachtigingen en bevat een vergelijking van de standaardinstellingen voor lid- en gastgebruikers. De standaardmachtigingen van de gebruiker kunnen alleen in de gebruikersinstellingen worden gewijzigd in Azure AD.

## <a name="member-and-guest-users"></a>Lid- en gastgebruikers
De set standaardmachtigingen ontvangen is afhankelijk van of de gebruiker een systeemeigen lid van de tenant (lid van de gebruiker is) of als de gebruiker wordt geïnstalleerd vanuit een andere directory als gast B2B-samenwerking (gastgebruiker). Zie [wat is Azure AD B2B-samenwerking?](../b2b/what-is-b2b.md) voor meer informatie over het toevoegen van gastgebruikers.
* Lidgebruikers kunnen toepassingen registreren, hun eigen profielfoto en mobiele telefoonnummer beheren, hun eigen wachtwoord wijzigen en B2B-gasten uitnodigen. Gebruikers kunnen bovendien (op een paar uitzonderingen na) alle directorygegevens lezen. 
* Gastgebruikers hebben beperkte machtigingen. Gastgebruikers kunnen bijvoorbeeld afgezien van hun eigen profielgegevens niet bladeren door informatie van de tenant. Een gastgebruiker kan echter informatie over een andere gebruiker ophalen door de Principal-naam van gebruiker of de object-id te verstrekken. Een gastgebruiker kan lezen eigenschappen van de groepen waartoe ze, zoals groepslidmaatschap, ongeacht behoren de **machtigingen van gastgebruikers zijn beperkt** instelling. Een Gast kan geen informatie over andere tenant-objecten weergeven.

Standaardmachtigingen voor gasten zijn standaard beperkt. Gasten kunnen worden toegevoegd aan beheerdersrollen, waarmee aan hen volledige lees- en schrijftoegang voor de rol wordt verleend. Er is een aanvullende beperking beschikbaar, namelijk de mogelijkheid voor gasten om andere gasten uit te nodigen. Als de instelling **Gasten kunnen uitnodigingen versturen** op **Nee** wordt ingesteld, kunnen gasten geen andere gasten uitnodigen. Zie [Uitnodigingen delegeren voor B2B-samenwerking](../b2b/delegate-invitations.md) voor meer informatie. Als u gastgebruikers standaard dezelfde machtigingen wilt verlenen als lidgebruikers, stelt u **De machtigingen van gastgebruikers zijn beperkt** in op **Nee**. Met deze instelling worden alle gebruikersmachtigingen van leden standaard verleend aan gastgebruikers, en is het ook toegestaan gastgebruikers toe te voegen aan beheerdersrollen.

## <a name="compare-member-and-guest-default-permissions"></a>Standaardmachtigingen voor leden en gasten vergelijken

**Onderwerp** | **Gebruikersmachtigingen voor leden** | **Gebruikersmachtigingen voor gasten**
------------ | --------- | ----------
Gebruikers en contactpersonen | Alle openbare eigenschappen lezen van gebruikers en contactpersonen<br>Gasten uitnodigen<br>Eigen wachtwoord wijzigen<br>Eigen mobiele nummer beheren<br>Eigen foto beheren<br>Eigen vernieuwingstekens ongeldig verklaren | Eigen eigenschappen lezen<br>Lezen van de weergegeven naam, e, meld u aan de naam, foto's, UPN-naam en type-eigenschappen van gebruikers van andere gebruikers en contactpersonen<br>Eigen wachtwoord wijzigen
Groepen | Beveiligingsgroepen maken<br>Office 365-groepen maken<br>Alle eigenschappen van groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groep lezen<br>Eigenschappen, het eigendom en het lidmaatschap van de gebruiker is eigenaar van groepen beheren<br>Gasten toevoegen aan groepen in eigendom<br>Instellingen voor dynamisch lidmaatschap beheren<br>Groepen in eigendom verwijderen<br>Office 365-groepen in eigendom herstellen | Alle eigenschappen van groepen lezen<br>Niet-verborgen groepslidmaatschappen lezen<br>Verborgen Office 365-groepslidmaatschappen voor gekoppelde groepen lezen<br>Groepen in eigendom beheren<br>Gasten toevoegen aan groepen in eigendom (indien toegestaan)<br>Groepen in eigendom verwijderen<br>Office 365-groepen in eigendom herstellen<br>Eigenschappen van de groepen waartoe ze behoren, met inbegrip van het lidmaatschap worden gelezen.
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
Gebruikers kunnen toepassingen registreren | Deze optie instelt op Nee, wordt voorkomen dat gebruikers toepassingsregistraties maken. De mogelijkheid kan vervolgens terug naar specifieke personen worden verleend door ze toe te voegen aan de rol van ontwikkelaar van toepassingen.
Toestaan dat gebruikers verbinding maken met werk-of schoolaccount met LinkedIn | Deze optie instelt op Nee, wordt voorkomen dat gebruikers hun werk- of school-account koppelen aan hun LinkedIn-account. Zie voor meer informatie, [LinkedIn-account verbindingen het delen van gegevens en toestemming](https://docs.microsoft.com/azure/active-directory/users-groups-roles/linkedin-user-consent).
De mogelijkheid beveiligingsgroepen te maken | Als u deze optie op Nee instelt, kunnen gebruikers geen beveiligingsgroepen maken. Globale beheerders en beheerders van gebruikers kunnen nog steeds beveiligingsgroepen maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-cmdlets.md) voor meer informatie.
De mogelijkheid Office 365-groepen te maken | Als u deze optie op Nee instelt, kunnen gebruikers geen Office 365-groepen maken. Als u deze optie op Sommige instelt, kan een beperkte selectie van gebruikers Office 365-groepen maken. Globale beheerders en Gebruikerbeheerders is nog steeds mogelijk om Office 365-groepen te maken. Zie [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](../users-groups-roles/groups-settings-cmdlets.md) voor meer informatie.
De toegang tot de Azure AD-beheerportal beperken | Deze optie instelt op Ja, wordt voorkomen dat gebruikers toegang tot Azure Active Directory via Azure portal bevat.
Mogelijkheid om andere gebruikers te lezen | Deze instelling is alleen beschikbaar in PowerShell. Deze markering instellen op $false, voorkomt u dat alle niet-beheerders gebruikersgegevens uit de map lezen. Met deze markering niet lezen van gebruikersgegevens in andere Microsoft-services zoals Exchange Online. Deze instelling is bedoeld voor speciale omstandigheden en deze markering instellen op $false wordt niet aanbevolen.

## <a name="object-ownership"></a>Eigendom van objecten

### <a name="application-registration-owner-permissions"></a>Eigenaarsmachtigingen toepassingsregistratie
Wanneer een gebruiker een toepassing registreert, wordt deze automatisch toegevoegd als een eigenaar voor de toepassing. Als eigenaar kan de gebruiker de metagegevens van de toepassing beheren, zoals de naam en de machtigingen waarom de app verzoekt. De gebruiker kan ook de tenantspecifieke configuratie van de toepassing beheren, zoals de SSO-toewijzingen voor configuratie en gebruikersbestanden. Een eigenaar kan ook andere eigenaren toevoegen of verwijderen. In tegenstelling tot globale beheerders kunnen eigenaren alleen toepassingen beheren waarvan ze eigenaar zijn.

### <a name="enterprise-application-owner-permissions"></a>Eigenaarsmachtigingen voor Enterprise-toepassing
Wanneer een gebruiker een nieuwe zakelijke toepassing toevoegt, worden ze automatisch toegevoegd als een eigenaar. Als een eigenaar, kunnen ze de tenant-specifieke configuratie van de toepassing, zoals de SSO-configuratie, inrichting en toewijzingen van gebruikers beheren. Een eigenaar kan ook andere eigenaren toevoegen of verwijderen. In tegenstelling tot globale beheerders kunnen eigenaars beheren alleen de toepassingen die waarvan ze eigenaar.

### <a name="group-owner-permissions"></a>Machtigingen groepseigenaar
Wanneer een gebruiker een groep maakt, wordt deze automatisch toegevoegd als een eigenaar voor die groep. Als eigenaar van een kunnen ze beheren van de eigenschappen van de groep, zoals de naam, evenals groepslidmaatschap van beheren. Een eigenaar kan ook andere eigenaren toevoegen of verwijderen. Eigenaren in tegenstelling tot globale beheerders en beheerders van de gebruiker kunnen alleen groepen die waarvan ze eigenaar beheren. Zie [Eigenaren beheren voor een groep](active-directory-accessmanagement-managing-group-owners.md) voor informatie over het toewijzen van een groepseigenaar.

### <a name="ownership-permissions"></a>Eigendom van machtigingen
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory gebruikers via eigendom objecten hebben. De gebruiker heeft alleen deze machtigingen voor objecten die waarvan ze eigenaar.

#### <a name="owned-application-registrations"></a>Eigendom toepassingsregistraties
Gebruikers kunnen de volgende acties uitvoeren op toepassingsregistraties die eigendom zijn.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.directory/applications/audience/update | De eigenschap applications.audience in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/authentication/update | De eigenschap applications.authentication in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/basic/update | Werk de basiseigenschappen van toepassingen in Azure Active Directory. |
| microsoft.directory/applications/credentials/update | De eigenschap applications.credentials in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.directory/applications/owners/update | De eigenschap applications.owners in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/permissions/update | De eigenschap applications.permissions in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/policies/update | De eigenschap applications.policies in Azure Active Directory bijgewerkt. |
| microsoft.directory/applications/restore | Toepassingen in Azure Active Directory herstellen. |

#### <a name="owned-enterprise-applications"></a>Eigendom bedrijfstoepassingen
Gebruikers kunnen de volgende acties uitvoeren op die eigendom zijn zakelijke toepassingen. Een zakelijke toepassing bestaat uit de service-principal, een of meer beleidsregels van toepassing, en soms een toepassingsobject in dezelfde tenant als de service-principal.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op auditLogs in Azure Active Directory te lezen. |
| microsoft.directory/policies/basic/update | Werk de basiseigenschappen van beleidsregels in Azure Active Directory. |
| microsoft.directory/policies/delete | Beleidsregels in Azure Active Directory verwijderen. |
| microsoft.directory/policies/owners/update | De eigenschap policies.owners in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | De eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | De eigenschap users.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/basic/update | Eenvoudige eigenschappen op servicePrincipals in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/update | De eigenschap servicePrincipals.owners in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions in Azure Active Directory bijgewerkt. |
| microsoft.directory/servicePrincipals/policies/update | De eigenschap servicePrincipals.policies in Azure Active Directory bijgewerkt. |
| microsoft.directory/signInReports/allProperties/read | Alle eigenschappen (met inbegrip van bevoegde eigenschappen) op signInReports in Azure Active Directory te lezen. |

#### <a name="owned-devices"></a>Apparaten die eigendom zijn
Gebruikers kunnen de volgende acties uitvoeren op apparaten die eigendom zijn.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lezen devices.bitLockerRecoveryKeys in Azure Active Directory. |
| microsoft.directory/devices/disable | Apparaten in Azure Active Directory uitschakelen. |

#### <a name="owned-groups"></a>Eigendom groepen
Gebruikers kunnen de volgende acties uitvoeren op groepen die eigendom zijn.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.directory/groups/appRoleAssignments/update | De eigenschap groups.appRoleAssignments in Azure Active Directory bijgewerkt. |
| microsoft.directory/groups/basic/update | Basic-eigenschappen op in Azure Active Directory-groepen bijwerken. |
| microsoft.directory/groups/delete | Groepen in Azure Active Directory verwijderen. |
| microsoft.directory/groups/dynamicMembershipRule/update | De eigenschap groups.dynamicMembershipRule in Azure Active Directory bijgewerkt. |
| microsoft.directory/groups/members/update | De eigenschap groups.members in Azure Active Directory bijgewerkt. |
| microsoft.directory/groups/owners/update | De eigenschap groups.owners in Azure Active Directory bijgewerkt. |
| microsoft.directory/groups/restore | Groepen in Azure Active Directory herstellen. |
| microsoft.directory/groups/settings/update | De eigenschap groups.settings in Azure Active Directory bijgewerkt. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over hoe u Azure AD-beheerdersrollen toewijzen [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
* [Gebruikers beheren](add-users-azure-active-directory.md)
