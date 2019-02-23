---
title: 'Toepassing-beheerdersrollen: Azure Active Directory delegeren | Microsoft Docs'
description: Toegangsbeheer voor toepassingen rollen om toegang te verlenen van machtigingen rechten in Azure Active Directory delegeren
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9409d196e307b75a14a272dee6c8c93789ca8556
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669785"
---
# <a name="delegate-app-administrator-roles-in-azure-active-directory"></a>App-beheerdersrollen in Azure Active Directory delegeren

 Azure AD kunt u beheer van toegang tot toepassingen op een set ingebouwde beheerdersrollen delegeren. Naast het vermindert de overhead van de globale beheerder, kan gespecialiseerde bevoegdheden voor het beheren van de toepassing toegang tot taken delegeren uw beveiligingspostuur verbeteren en verminderen de kans op onbevoegde toegang. Overdracht oplossen en algemene richtlijnen beschreven die worden beschreven [beheer in Azure Active Directory delegeren](roles-concept-delegation.md).

## <a name="delegate-app-administration"></a>App-beheer overdragen

De volgende rollen verleent machtigingen voor het beheren van toepassingsregistraties, instellingen voor eenmalige aanmelding, gebruiker en groepstoewijzingen en akkoord gaan met gedelegeerde machtigingen en Toepassingsmachtigingen (met uitzondering van Microsoft Graph en Azure AD Graph). Het enige verschil is dat de beheerdersrol van de toepassing ook machtigingen verleend aan de Application Proxy-instellingen beheren. Geen van beide rollen hebben de mogelijkheid voor het beheren van instellingen voor voorwaardelijke toegang.
> [!IMPORTANT]
> Gebruikers aan deze rol toegewezen kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om te imiteren identiteit van de toepassing. Deze imitatie van de identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker onder hun roltoewijzingen in Azure AD doen kan. Een gebruiker die is toegewezen aan deze rol kan mogelijk maken of bijwerken van gebruikers of andere objecten tijdens het imiteren van de toepassing.

De mogelijkheid voor het beheren van toegang tot toepassingen in Azure portal verlenen:

1. Aanmelden bij uw [Azure AD-tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat in aanmerking komen voor de rol globale beheerder in de tenant.
2. Als u voldoende machtigingen hebt, opent u de [rollen en beheerders pagina](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators).
3. Open een van de volgende rollen zijn lid toewijzingen weer te geven:
  * **Toepassingsbeheerder**
  * **Beheerder van de cloudtoepassing**
4. Op de **leden** pagina voor de rol, selecteer **lid toevoegen**.
5. Selecteer een of meer leden toe te voegen aan de rol. <!--Members can be users or groups.-->

U kunt de beschrijving voor deze rollen in weergeven [beschikbare rollen](directory-assign-admin-roles.md#available-roles).

## <a name="delegate-app-registration"></a>App-registratie delegeren

Standaard alle gebruikers kunnen toepassingsregistraties maken, maar u kunt selectief verlenen machtigingen voor het maken van toepassingsregistraties of machtiging toe te staan om een app te autoriseren.

1. Aanmelden bij uw [Azure AD-tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat in aanmerking komen voor de rol globale beheerder in de tenant.
2. Wanneer u voldoende machtigingen hebt verkregen, stelt u een of beide van de volgende opties:
  * Op de [pagina van de gebruiker-instellingen voor uw tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings), stel **gebruikers kunnen toepassingen registreren** op Nee.
  * Op de [gebruikersinstellingen voor bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/), stel **gebruikers toestemming kunnen geven tot toepassingen die toegang tot bedrijfsgegevens namens hen** op Nee.
3. Gebruikers die deze machtiging aan leden van de toepassingsrol ontwikkelaars zo nodig worden vervolgens toewijzen.

Wanneer een gebruiker een toepassing registreert, worden ze automatisch toegevoegd als de eigenaar van de eerste voor de toepassing.

## <a name="delegate-app-ownership"></a>App-eigenaar delegeren

Eigenaren en eigenaren van de app-registratie kunnen elk beheren alleen de toepassingen van de app of de app-registraties waarvan ze eigenaar. Bijvoorbeeld, wanneer u een eigenaar voor de Salesforce-toepassing toevoegen, kunt die eigenaar beheren de toegang tot en configuratie voor Salesforce, maar niet alle andere toepassingen. Een app kan veel eigenaars hebben en een gebruiker kan de eigenaar van veel apps zijn.

De eigenaar van een toepassing kunt doen:

* Toepassingseigenschappen, zoals de naam en de machtigingen wijzigen de app-aanvragen
* Referenties beheren
* Eenmalige aanmelding configureren
* Gebruikerstoegang toewijzen
* Toevoegen of verwijderen van andere eigenaren
* Het app-manifest bewerken
* Publiceer de app naar de app-galerie

> [!IMPORTANT]
> Gebruikers aan deze rol toegewezen kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om te imiteren identiteit van de toepassing. Deze imitatie van de identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker onder hun roltoewijzingen in Azure AD doen kan. Een gebruiker die is toegewezen aan deze rol kan mogelijk maken of bijwerken van gebruikers of andere objecten tijdens het imiteren van de toepassing.

De eigenaar van de registratie van een app kunt weergeven en bewerken van de app-registratie.

<!-- ### To assign an enterprise app ownership role to a user

1. Sign in to your [Azure AD tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with an account that is the Global Administrator for the tenant.
2. On the [Roles and administrators page](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators), open one of the following roles to see its member assignments:
  * **Enterprise Application Owner**
  * **Application Registration Owner**
3. On the **Members** page for the role, select **Add member**.
4. Select one or more members to add to the role. -->

### <a name="to-assign-an-owner-to-an-application"></a>Een eigenaar toewijzen aan een toepassing

1. Aanmelden bij uw [Azure AD-tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat in aanmerking komen voor de beheerder van de toepassing of de beheerder van de Cloudtoepassing voor de tenant.
2. Op de [pagina met App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) voor de tenant, selecteert u een app te openen de **overzicht** pagina voor de app.
3. Selecteer **eigenaren** om te zien van de lijst met de eigenaren voor de app.
4. Selecteer **toevoegen** om te selecteren van een of meer eigenaren toe te voegen aan de app.

### <a name="to-assign-an-owner-to-an-application-registration"></a>Een eigenaar toewijzen aan een toepassing registreren

1. Aanmelden bij uw [Azure AD-tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat in aanmerking komen voor de beheerder van de toepassing of de beheerdersrol van Cloud-toepassing in de tenant.
2. Wanneer u gemachtigd bent, op de [bedrijfstoepassingen pagina](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) voor de tenant, selecteert u een app-registratie om dit te openen.
3. Selecteer **instellingen**.
4. Selecteer **eigenaren** op de **instellingen** pagina om te bekijken van de lijst met de eigenaren voor de app.
5. Selecteer **toevoegen eigenaar** om te selecteren van een of meer eigenaren toe te voegen aan de app.

## <a name="next-steps"></a>Volgende stappen

* [Referentie voor Azure AD-beheerder-rol](directory-assign-admin-roles.md)
