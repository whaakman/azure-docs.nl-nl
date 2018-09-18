---
title: Hoe om te herstellen of een onlangs verwijderde gebruiker permanent te verwijderen in Azure Active Directory | Microsoft Docs
description: Informatie over het weergeven van gebruikers die kunnen worden hersteld, een verwijderde gebruiker herstellen of permanent verwijderen van een gebruiker met Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 88d3c672cd072cd4b252f7ce4ede3a4c7b13a7db
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736169"
---
# <a name="how-to-restore-or-permanently-remove-a-recently-deleted-user-with-azure-active-directory"></a>Hoe: herstellen of permanent verwijderen van een onlangs verwijderde gebruiker met Azure Active Directory
Nadat u een gebruiker verwijdert, wordt het account de status onderbroken blijft gedurende 30 dagen. Tijdens deze periode 30-daagse kan account van de gebruiker worden hersteld, samen met de eigenschappen ervan. Als dit venster 30 dagen is verstreken, de gebruiker wordt automatisch en definitief verwijderd.

U kunt weergeven van uw gebruikers te herstellen, een verwijderde gebruiker herstellen of permanent verwijderen van een gebruiker met behulp van Azure Active Directory (Azure AD) in Azure portal.

>[!Important]
>U noch klantondersteuning van Microsoft kunt herstellen van een gebruiker is definitief verwijderd.

## <a name="required-permissions"></a>Vereiste machtigingen
U moet een van de volgende rollen te herstellen en gebruikers permanent verwijderen.

- Bedrijfsbeheerder

- Laag1-ondersteuning voor partner

- Laag2-ondersteuning voor partner

- Beheerder van gebruikersaccounts

## <a name="view-your-restorable-users"></a>Uw die kunnen worden hersteld gebruikers weergeven
Hier ziet u alle gebruikers die minder dan 30 dagen geleden zijn verwijderd. Deze gebruikers kunnen worden hersteld.

### <a name="to-view-your-restorable-users"></a>Om terug te zetten gebruikers weer te geven
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **verwijderde gebruikers**.

    Bekijk de lijst met gebruikers die beschikbaar zijn voor het herstellen.

    ![Gebruikers - pagina van de verwijderde gebruikers, aan gebruikers die nog steeds kunnen worden hersteld.](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Een onlangs verwijderde gebruiker herstellen
Terwijl een gebruikersaccount is onderbroken, blijft de gerelateerde directorygegevens behouden. Wanneer u een gebruiker herstelt, wordt deze directory-informatie ook hersteld.

### <a name="to-restore-a-user"></a>Een gebruiker moet herstellen
1. Op de **gebruikers - verwijderde gebruikers** pagina, zoek en selecteer een van de beschikbare gebruikers. Bijvoorbeeld, _Mary Parker_.

2. Selecteer **terugzetten gebruiker**.

    ![Gebruikers - pagina van de verwijderde gebruikers, met de optie gebruiker herstellen gemarkeerd](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Een gebruiker permanent verwijderen
U kunt een gebruiker permanent verwijderen uit uw directory zonder te wachten op de 30 dagen voor de automatische verwijdering. Een gebruiker is definitief verwijderd kan niet worden hersteld door u, als een andere beheerder, of door de klantondersteuning van Microsoft.

>[!Note]
>Als u een gebruiker permanent per ongeluk hebt verwijdert, hebt u een nieuwe gebruiker maken en de vorige gegevens handmatig invoeren. Zie voor meer informatie over het maken van een nieuwe gebruiker [toevoegen of verwijderen gebruikers](add-users-azure-active-directory.md).

### <a name="to-permanently-delete-a-user"></a>Een gebruiker permanent verwijderen

1. Op de **gebruikers - verwijderde gebruikers** pagina, zoek en selecteer een van de beschikbare gebruikers. Bijvoorbeeld, _Rae Huff_.

2. Selecteer **definitief verwijderen**.

    ![Gebruikers - pagina van de verwijderde gebruikers, met de optie gebruiker herstellen gemarkeerd](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt hersteld of verwijderd van uw gebruikers, kunt u de volgende basis-processen uitvoeren:

- [Toevoegen of verwijderen van gebruikers](add-users-azure-active-directory.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Gastgebruikers uit een andere directory toevoegen](../b2b/what-is-b2b.md) 

Voor meer informatie over andere taken voor het beheer van beschikbare [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).
