---
title: 'Directory-rollen toewijzen aan gebruikers: Azure Active Directory | Microsoft Docs'
description: Instructies over het toewijzen van beheerder-en niet-beheerder voor gebruikers met Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd26fa53c91c53893c7f326afda5158fa430be1e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188134"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>De beheerder en niet-beheerder rollen toewijzen aan gebruikers met Azure Active Directory
Als een gebruiker in uw organisatie heeft toestemming nodig voor Azure Active Directory (Azure AD)-resources beheren, moet u de gebruiker een juiste rol in Azure AD, op basis van de acties die de gebruiker moet toestemming om uit te voeren.

Zie voor meer informatie over de beschikbare rollen [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Zie voor meer informatie over het toevoegen van gebruikers [nieuwe gebruikers toevoegen aan Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Rollen toewijzen
Een veelgebruikte manier voor Azure AD-rollen toewijzen aan een gebruiker zich op de **maprol** pagina voor een gebruiker.

U kunt ook rollen met behulp van Privileged Identity Management (PIM) toewijzen. Zie voor meer informatie over hoe u PIM gaat gebruiken, [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Een rol toewijzen aan een gebruiker
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en zoek vervolgens naar de gebruiker en selecteer de roltoewijzing aan. Bijvoorbeeld, _Alain Charon_.

3. Op de **Alain Charon - profiel** weergeeft, schakelt **maprol**.

    De **Alain Charon - Directory-rol** pagina wordt weergegeven.

4. Selecteer **functie toevoegen**, selecteer de rol wilt toewijzen aan Alain (bijvoorbeeld _toepassingsbeheerder_), en kies vervolgens **Selecteer**.

    ![Rollen directorypagina, met de geselecteerde rol](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    De toepassing-beheerdersrol is toegewezen aan Alain Charon en deze wordt weergegeven op de **Alain Charon - Directory-rol** pagina.

## <a name="remove-a-role-assignment"></a>Roltoewijzing verwijderen
Als u de roltoewijzing verwijderen uit een gebruiker wilt, kunt u ook doen vanuit de **Alain Charon - Directory-rol** pagina.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Een roltoewijzing verwijderen uit een gebruiker

1. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en vervolgens zoekt en selecteert u de gebruiker aan de roltoewijzing is verwijderd. Bijvoorbeeld, _Alain Charon_.

2. Selecteer **maprol**, selecteer **toepassingsbeheerder**, en selecteer vervolgens **verwijderen rol**.

    ![Rollen directorypagina, met de geselecteerde rol en de verwijderoptie](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    De beheerdersrol van de toepassing wordt verwijderd uit Alain Charon en deze niet meer wordt weergegeven op de **Alain Charon - Directory-rol** pagina.

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen of verwijderen van gebruikers](add-users-azure-active-directory.md)

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Gastgebruikers uit een andere directory toevoegen](../b2b/what-is-b2b.md)

Of u andere gebruiker-beheertaken, zoals het toewijzen van gemachtigden, met behulp van beleid en het delen van gebruikersaccounts kan uitvoeren. Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).


