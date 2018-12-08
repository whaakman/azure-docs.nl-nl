---
title: Toevoegen of verwijderen van gebruikers - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u nieuwe gebruikers toevoegen of verwijderen van bestaande gebruikers met Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: dd4a9132d5f04870239be3127f723afb60b3ecef
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104676"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Toevoegen of verwijderen van gebruikers met Azure Active Directory
Nieuwe gebruikers toevoegen of verwijderen van bestaande gebruikers uit uw tenant Azure Active Directory (Azure AD).

## <a name="add-a-new-user"></a>Een nieuwe gebruiker toevoegen
U kunt een nieuwe gebruiker met behulp van de Azure Active Directory-portal maken.

### <a name="to-add-a-new-user"></a>Een nieuwe gebruiker toe te voegen
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als een globale beheerder of Gebruikerbeheerder voor de map.

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **nieuwe gebruiker**.

    ![Gebruikers - pagina voor alle gebruikers met de nieuwe gebruiker is gemarkeerd](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. Op de **gebruiker** pagina, de vereiste gegevens invullen.

    ![Nieuwe gebruiker, met gebruikersgegevens op de pagina gebruiker toevoegen](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Naam (vereist).** De eerste en laatste naam van de nieuwe gebruiker. Bijvoorbeeld, Mary Parker.

    - **Gebruikersnaam (vereist).** De gebruikersnaam van de nieuwe gebruiker. Bijvoorbeeld mary@contoso.com. 
    
        Het domeingedeelte van de gebruikersnaam moet een van beide de aanvankelijke standaarddomeinnaam, gebruiken <_uwdomeinnaam_>. onmicrosoft.com of een aangepaste domeinnaam, bijvoorbeeld contoso.com. Zie voor meer informatie over het maken van een aangepaste domeinnaam [een aangepaste domeinnaam toevoegen aan Azure Active Directory](add-custom-domain.md).

    - **Profiel.** U kunt desgewenst meer informatie over de gebruiker toevoegen. U kunt ook gebruikersgegevens op een later tijdstip toevoegen. Zie voor meer informatie over het toevoegen van gebruikersgegevens [toevoegen of wijzigen van de informatie uit gebruikersprofielen](active-directory-users-profile-azure-portal.md).

    - **Groepen.** U kunt eventueel de gebruiker toevoegen aan een of meer bestaande groepen. U kunt ook de gebruiker toevoegen aan groepen op een later tijdstip. Zie voor meer informatie over het toevoegen van gebruikers aan groepen [hoe u een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md).

    - **Directory-rol.** U kunt eventueel de gebruiker toevoegen aan een directory-rol. U kunt de gebruiker moet een globale beheerder, of aan een of meer van de andere beheerdersrollen in Azure AD. Zie voor meer informatie over het toewijzen van rollen [rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md).

4. Kopieer de automatisch gegenereerde wachtwoord opgegeven in de **wachtwoord** vak. U moet dit wachtwoord geven aan de gebruiker voor de initiële aanmeldingsproces.

5. Selecteer **Maken**.

    De gebruiker is gemaakt en toegevoegd aan uw Azure AD-tenant.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Voeg een nieuwe gebruiker in een hybride omgeving
Als u een omgeving met zowel Azure Active Directory (cloud) en Windows Server Active Directory (on-premises) hebt, kunt u nieuwe gebruikers toevoegen door te synchroniseren van de bestaande gegevens van de gebruiker-account. Zie voor meer informatie over hybride omgevingen en gebruikers [uw on-premises directory's integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Een gebruiker verwijderen
U kunt een bestaande gebruiker met behulp van Azure Active Directory-portal verwijderen.

### <a name="to-delete-a-user"></a>Een gebruiker verwijderen
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en vervolgens zoekt en selecteert u de gebruiker die u wilt verwijderen uit uw Azure AD-tenant. Bijvoorbeeld, _Mary Parker_.

3. Selecteer **gebruiker verwijderen**.

    ![Gebruikers - pagina voor alle gebruikers met gebruiker verwijderen gemarkeerd](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    De gebruiker wordt verwijderd en niet meer wordt weergegeven op de **gebruikers: alle gebruikers** pagina. De gebruiker kan worden weergegeven in de **verwijderde gebruikers** pagina voor de komende 30 dagen en gedurende deze tijd kunnen worden hersteld. Zie voor meer informatie over het herstellen van een gebruiker [herstellen of permanent verwijderen van een onlangs verwijderde gebruiker](active-directory-users-restore.md).

    >[!Note]
    >U moet Windows Server Active Directory gebruiken om bij te werken van de identiteit, contactgegevens of taakgegevens voor waarvan u de bron van de instantie Windows Server Active Directory is-gebruikers. Nadat u de update hebt voltooid, moet u de volgende synchronisatiecyclus uitvoeren voordat u de wijzigingen ziet wachten.

## <a name="next-steps"></a>Volgende stappen
Nadat u uw gebruikers hebt toegevoegd, kunt u de volgende basis-processen uitvoeren:

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

- [Werken met dynamische groepen en gebruikers](../users-groups-roles/groups-create-rule.md)

Of u kunt andere taken voor het beheer van gebruikers, zoals uitvoeren [gastgebruikers toevoegen van een andere directory](../b2b/what-is-b2b.md) of [herstellen van een verwijderde gebruiker](active-directory-users-restore.md). Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).