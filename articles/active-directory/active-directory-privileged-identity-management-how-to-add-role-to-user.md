---
title: Toevoegen of verwijderen van een gebruikersrol | Microsoft Docs
description: Informatie over het toevoegen van functies aan bevoegde identiteiten met de Azure Active Directory Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2018
ms.author: billmath
ms.openlocfilehash: 53deb04a33a5f878c5e3f765099c54d30e6ac005
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Hoe kan ik een gebruiker toevoegen of verwijderen?
Met Azure Active Directory (AD), een algemeen beheerder (of bedrijfsbeheerder) kunt bijwerken die gebruikers **permanent** toegewezen aan rollen in Azure AD. Dit wordt gedaan met PowerShell-cmdlets, zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`. Of ze de Azure-portal kunnen gebruiken, zoals beschreven in [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md).

De Azure AD Privileged Identity Management-toepassing kan bevoorrechte rol beheerders ook permanente roltoewijzingen maken. Bovendien bevoorrechte rol administrators gebruikers kunnen aanbrengen **in aanmerking komende** voor beheerdersrollen. Een in aanmerking komende beheerder kan de rol activeren wanneer nodig en vervolgens hun machtigingen verlopen zodra ze klaar bent.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Rollen met PIM in de Azure portal beheren
In uw organisatie, kunt u gebruikers toewijzen aan andere beheerdersrollen in Azure AD, Office 365 en andere Microsoft-services en toepassingen.  Meer informatie over de beschikbare rollen kunnen worden gevonden op [rollen in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Als u wilt toevoegen of verwijderen van een gebruiker een rol met Privileged Identity Management, online zetten van de PIM-dashboard. Klik vervolgens op de **gebruikers in beheerdersrollen** knop of Selecteer een specifieke rol (zoals hoofdbeheerder) uit de tabel functies.

> [!NOTE]
> Als u nog PIM in de Azure portal dat nog niet hebt ingeschakeld, gaat u naar [aan de slag met Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

Als u een andere gebruikerstoegang geven tot PIM zelf wilt, de functies die PIM moet de gebruiker hebben worden beschreven verder in [hoe toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Een gebruiker toevoegen aan een rol
1. In de [Azure-portal](https://portal.azure.com/), selecteer de **Azure AD Privileged Identity Management** tegel op het dashboard.
2. Selecteer **bevoorrechte rollen beheren**.
3. In de **Overzicht rollen** tabel, selecteer de rol die u wilt beheren.
4. Selecteer in de rolblade **toevoegen**.
5. Klik op **Selecteer gebruikers** en zoek naar de gebruiker op de **Selecteer gebruikers** blade.  
6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **gedaan**.
7. Klik op **OK** opslaan van uw selectie. De gebruiker die u hebt geselecteerd, wordt weergegeven in de lijst als in aanmerking komen voor de rol.

> [!NOTE]
> Er zijn alleen nieuwe gebruikers met een rol in aanmerking komen voor de functie standaard. Als u de rol permanent te maken wilt, klikt u op de gebruiker in de lijst. Gegevens van de gebruiker wordt weergegeven in een nieuwe blade. Selecteer **zorg toeg** in het menu van de gebruiker informatie.  
> Als een gebruiker niet voor Azure multi-factor Authentication (MFA registreren kan) of met behulp van een Microsoft-account (meestal @outlook.com), moet u deze permanent in alle hun rollen te maken. In aanmerking komende admins wordt gevraagd om u te registreren voor MFA tijdens de activering.

Nu dat de gebruiker in aanmerking voor een rol komt, laten weten dat ze dit kunnen activeren volgens de instructies in [hoe een rol activeren of deactiveren](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Een gebruiker vanuit een functie verwijderen
U kunt gebruikers van in aanmerking komende roltoewijzingen verwijderen, maar zorg ervoor dat er altijd ten minste één gebruiker aan wie een permanente globale beheerder.

Volg deze stappen voor een specifieke gebruiker verwijderen uit een rol:

1. Navigeer naar de rol in de lijst van de functie door het selecteren van een rol in het dashboard van Azure AD PIM of door te klikken op de **gebruikers in beheerdersrollen** knop.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**. Een bericht wordt u gevraagd te bevestigen.
4. Klik op **Ja** de functie te verwijderen van de gebruiker.

Als u niet zeker weet welke gebruikers moeten nog steeds hun roltoewijzingen, dan u kunt [een revisie toegang voor de rol starten](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

