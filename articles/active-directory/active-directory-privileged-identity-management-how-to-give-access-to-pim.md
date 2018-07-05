---
title: Hoe u toegang geven tot Privileged Identity Management - Azure | Microsoft Docs
description: Informatie over het toevoegen van rollen voor gebruikers met de extensie van Azure Active Directory Privileged Identity Management, zodat ze PIM kunnen beheren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8d52469e11ac508ee8b353311f6040ac9d6f2e4e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447146"
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a>Verlenen van toegang voor het beheren van Azure AD Privileged Identity Management
De hoofdbeheerder die Azure AD Privileged Identity Management (PIM) voor een organisatie automatisch kan roltoewijzingen en toegang tot PIM ophalen. Niemand anders wordt schrijftoegang standaard, met inbegrip van andere globale beheerders. Andere globale beheerders, Beveiligingsbeheerders en beveiligingslezers hebben alleen-lezen toegang tot Azure AD PIM. Als u wilt toegang geven tot PIM, de eerste gebruiker, anderen kunt toewijzen de **beheerder met bevoorrechte rol** rol.

> [!NOTE]
> Azure AD PIM beheren, moet Azure MFA. Omdat Microsoft-accounts niet voor Azure MFA registreren kunnen, kan niet een gebruiker die zich met een Microsoft-account aanmeldt toegang tot de Azure AD PIM.
> 
> 

Zorg ervoor dat er altijd ten minste twee gebruikers in een beheerdersrol bevoorrechte rol in het geval één gebruiker is vergrendeld of hun account wordt verwijderd.

## <a name="give-another-user-access-to-manage-pim"></a>Een andere gebruikerstoegang geven tot PIM beheren
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure AD Privileged Identity Management** -app op het dashboard.
2. Selecteer **bevoorrechte rollen beheren** > **beheerder met bevoorrechte rol** > **toevoegen**.
   
    ![Toevoegen van beheerders met bevoegdheid - schermafbeelding][1]
3. Stap 1 is al voltooid op de blade van de beheerde gebruikers toevoegen. Selecteer stap 2, **Selecteer gebruikers** en zoek naar de gebruiker die u wilt toevoegen.
   
    ![Selecteer de gebruikers - schermafbeelding][2]
4. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **gedaan**.
5. Klik op **OK** om op te slaan, uw selectie. De gebruiker die u hebt geselecteerd, wordt weergegeven in de lijst met beheerders met bevoegdheid.
   
   * Wanneer u een nieuwe rol aan iemand toewijzen, worden ze automatisch ingesteld als in aanmerking komende om de rol te activeren. Als u ze in de rol permanent te maken wilt, klikt u op de gebruiker in de lijst. Selecteer **permanent maken** in het menu van de gebruiker gegevens.
6. Verzenden naar de gebruiker een koppeling naar [aan de slag met Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Verwijderen van een andere gebruiker toegangsrechten voor het beheren van PIM
Voordat u iemand uit de rol van de bevoorrechte rol beheerder verwijdert, altijd ervoor te zorgen er nog steeds worden twee gebruikers die zijn toegewezen.

1. Klik in het dashboard PIM op de rol **beheerder met bevoorrechte rol**.  De lijst met gebruikers die momenteel in deze rol wordt weergegeven.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**.  Een bevestigingsbericht wordt weergegeven.
4. Klik op **Ja** om de gebruiker verwijderen uit de rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
