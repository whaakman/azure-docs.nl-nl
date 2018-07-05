---
title: Toevoegen of verwijderen van een gebruikersrol | Microsoft Docs
description: Informatie over het toevoegen van rollen aan bevoorrechte identiteiten met de Azure Active Directory Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 01/03/2018
ms.author: rolyon
ms.openlocfilehash: 438a2fcd511fe62fa1dc7ba603c3770d5bcb56a4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446840"
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Hoe kan ik een gebruiker toevoegen of verwijderen?
Met Azure Active Directory (AD), een globale beheerder (of bedrijfsbeheerder) kunt bijwerken die gebruikers **permanent** toegewezen aan rollen in Azure AD. Dit wordt gedaan met de PowerShell-cmdlets, zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`. Of de Azure-portal kan worden gebruikt zoals beschreven in [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md).

De Azure AD Privileged Identity Management-toepassing kan beheerders met bevoegdheid kunnen ook permanente roltoewijzingen. Beheerders met bevoegdheid kunnen bovendien zodat gebruikers **in aanmerking komende** voor beheerdersrollen. Een in aanmerking komende beheerder kan de rol kunt activeren wanneer ze deze nodig hebben, en vervolgens hun machtigingen verlopen wanneer ze klaar bent.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Rollen met PIM in Azure portal beheren
In uw organisatie, kunt u gebruikers toewijzen aan andere beheerdersrollen in Azure AD, Office 365 en andere Microsoft-services en toepassingen.  Meer informatie over de beschikbare rollen kunnen worden gevonden op [rollen in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Als u wilt toevoegen of verwijderen van een gebruiker in een rol met Privileged Identity Management, zodat u de PIM-dashboard. Klik vervolgens op de **in beheerdersrollen** knop, of Selecteer een specifieke rol (zoals globale beheerder) uit de tabel functies.

> [!NOTE]
> Als u nog PIM in Azure portal nog niet hebt ingeschakeld, gaat u naar [aan de slag met Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

Als u een andere gebruikerstoegang geven tot PIM zelf wilt, de rollen waarmee PIM vereist dat de gebruiker zijn beschreven in [hoe u toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Een gebruiker toevoegen aan een rol
1. In de [Azure-portal](https://portal.azure.com/), selecteer de **Azure AD Privileged Identity Management** tegel op het dashboard.
2. Selecteer **bevoorrechte rollen beheren**.
3. In de **Rolsamenvatting** tabel, selecteert u de rol die u wilt beheren.
4. Selecteer in de rolblade **toevoegen**.
5. Klik op **gebruikers selecteren** en zoek naar de gebruiker op de **gebruikers selecteren** blade.  
6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **gedaan**.
7. Klik op **OK** om op te slaan, uw selectie. De gebruiker die u hebt geselecteerd wordt in de lijst als in aanmerking komen voor de rol weergegeven.

> [!NOTE]
> Nieuwe gebruikers in een rol zijn alleen in aanmerking komen voor de rol van standaard. Als u wilt de rol om permanent te maken, klikt u op de gebruiker in de lijst. Gegevens van de gebruiker wordt weergegeven in een nieuwe blade. Selecteer **maken permanent** in het menu van de gebruiker gegevens.  
> Als een gebruiker niet voor Azure multi-factor Authentication (MFA registreren kan), of met behulp van een Microsoft-account (meestal @outlook.com), moet u deze permanent in alle hun rollen te maken. In aanmerking komende beheerders wordt gevraagd om u te registreren voor MFA tijdens de activering.

Nu dat de gebruiker in aanmerking voor een rol komt, laat deze weten dat ze deze willen activeren op basis van de instructies in [activeren of deactiveren van een rol](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Een gebruiker verwijderen van een rol
U kunt gebruikers van in aanmerking komende roltoewijzingen verwijderen, maar zorg ervoor dat er altijd ten minste één gebruiker die een globale beheerder is permanent.

Volg deze stappen voor het verwijderen van een specifieke gebruiker vanuit een functie:

1. Navigeer naar de rol in de lijst van de functie door het selecteren van een rol in het dashboard Azure AD PIM of door te klikken op de **in beheerdersrollen** knop.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**. Een bericht wordt u gevraagd om te bevestigen.
4. Klik op **Ja** te verwijderen van de rol van de gebruiker.

Als u niet zeker weet welke gebruikers moeten nog steeds hun roltoewijzingen, dan u kunt [een toegangscontrole voor de rol starten](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

