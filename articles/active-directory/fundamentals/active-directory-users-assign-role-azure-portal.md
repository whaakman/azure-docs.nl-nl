---
title: Beheerdersrollen toewijzen aan gebruikers in Azure Active Directory | Microsoft Docs
description: Lees hier hoe u beheerdersgegevens van gebruikers wijzigt in Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 06/25/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: 7ed564d5954841f96109568b33183908d25bb8be
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939517"
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Beheerdersrollen toewijzen aan gebruikers in Azure Active Directory
In dit artikel wordt uitgelegd hoe u een beheerdersrol toewijst aan een gebruiker in Azure Active Directory (Azure AD). Zie [Add new users to Azure Active Directory](../add-users-azure-active-directory.md) (Engelstalig) voor meer informatie over het toevoegen van nieuwe gebruikers in uw organisatie. Toegevoegde gebruikers hebben standaard geen gebruikersrechten, maar u kunt op elk gewenst moment rollen aan ze toewijzen.

## <a name="assign-a-role-to-a-user"></a>Een rol toewijzen aan een gebruiker
1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een hoofdbeheerder of rolbeheerder voor de directory.

2. Selecteer **Azure Active Directory**, selecteer **Gebruikers** en selecteer een specifieke gebruiker in de lijst.

    ![Gebruikersbeheer openen](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3. Selecteer voor de geselecteerde gebruiker **Maprol**, selecteer **Rol toevoegen** en selecteer vervolgens de juiste beheerdersrollen in de lijst **Maprollen**, zoals  **Voorwaardelijke toegang beheerder**. Zie [Assigning administrator roles in Azure AD](../active-directory-assign-admin-roles-azure-portal.md) (Engelstalig) voor meer informatie over beheerdersrollen. 

    ![Een gebruiker toewijzen aan een rol](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

1. Selecteer **Selecteren** om op te slaan.

## <a name="next-steps"></a>Volgende stappen
* [Snelstartgids: Gebruikers toevoegen of verwijderen in Azure Active Directory](add-users-azure-active-directory.md)
* [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
* [Gastgebruikers uit een andere directory toevoegen](../b2b/what-is-b2b.md) 
* [Een rol toewijzen aan een gebruiker in Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Een verwijderde gebruiker herstellen](active-directory-users-restore.md)
