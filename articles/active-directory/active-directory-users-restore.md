---
title: Een verwijderde gebruiker in Azure Active Directory herstellen | Microsoft Docs
description: Het herstellen van een verwijderde gebruiker, terug te zetten gebruikers bekijken of permanent verwijderen van een gebruiker in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: c3b7550c2aea0e8bcb7998e0e8c732894b500403
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Een verwijderde gebruiker in Azure Active Directory terugzetten

In dit artikel bevat instructies voor het herstellen of een eerder verwijderde gebruiker permanent te verwijderen. Wanneer u een gebruiker in de Azure Active Directory (Azure AD) verwijdert, wordt de verwijderde gebruiker bewaard voor 30 dagen na de datum van verwijdering. Gedurende die tijd kunnen en de eigenschappen van de gebruiker worden hersteld. 

## <a name="required-permissions"></a>Vereiste machtigingen
De volgende machtigingen zijn voldoende zijn voor het herstellen van een gebruiker.

Rol  | Machtigingen 
--------- | ---------
Bedrijfsbeheerder<p>Laag1-ondersteuning voor partner<p>Laag2-ondersteuning voor partner<p>Beheerder van gebruikersaccounts | Verwijderde gebruikers kunt herstellen 
Bedrijfsbeheerder<p>Laag1-ondersteuning voor partner<p>Laag2-ondersteuning voor partner<p>Beheerder van gebruikersaccounts | Kan gebruikers definitief verwijderen

## <a name="how-to-restore-a-deleted-user"></a>Een verwijderde gebruiker herstellen

In de Azure portal kunt u zowel een verwijderde gebruiker herstellen als een verwijderde gebruiker permanent te verwijderen.

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **gebruikers en groepen** &gt; **alle gebruikers**. 
2. Onder **weergeven**, de pagina om weer te geven filteren **onlangs verwijderd gebruikers**. 
3. Selecteer een of meer onlangs verwijderde gebruikers.
4. Selecteer **terugzetten gebruiker** of **definitief verwijderen**.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over het beheer van Azure Active Directory-gebruiker.

* [Snelstartgids: Toevoegen of verwijderen van gebruikers met Azure Active Directory](add-users-azure-active-directory.md)
* [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
* [Gastgebruikers van een andere map toevoegen](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Een gebruiker toewijzen aan een rol in uw Azure AD](active-directory-users-assign-role-azure-portal.md)
