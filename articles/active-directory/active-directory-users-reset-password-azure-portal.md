---
title: Wachtwoord opnieuw instellen in Azure Active Directory | Microsoft Docs
description: Gestarte Administrator-wachtwoord opnieuw instellen voor een gebruiker in Azure Active Directory
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
editor: 
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 6d01dff567e49b602e98f717dace4dc75abecb4c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Het wachtwoord voor een gebruiker in Azure Active Directory opnieuw instellen

Beheerders moeten mogelijk opnieuw instellen van het wachtwoord van een gebruiker in gevallen waar ze vergeten, zijn vergrendeld out- of andere scenario's. Welke stappen volgen helpt u bij het opnieuw instellen van het wachtwoord van een gebruiker.

## <a name="how-to-reset-the-password-for-a-user"></a>Het wachtwoord voor een gebruiker opnieuw instellen

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account met machtigingen van de directory voor gebruikerswachtwoorden opnieuw instellen.
2. Selecteer **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer de gebruiker die u wilt het wachtwoord voor instellen.
2. Selecteer voor de geselecteerde gebruiker **wachtwoord opnieuw instellen**.

    ![Uw wachtwoord opnieuw instellen voor een gebruiker uit het gebruikersprofiel in Azure AD](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. Op **wachtwoord opnieuw instellen**, selecteer **wachtwoord opnieuw instellen**.
7. Een tijdelijk wachtwoord wordt weergegeven dat u kunt vervolgens met de gebruiker opgeven. De gebruiker gevraagd hun wachtwoord vervolgens de volgende keer dat ze zich aanmelden te wijzigen. 

   > [!NOTE]
   > Dit tijdelijke wachtwoord komt niet hebben een verlooptijd zodat deze alleen geldig totdat ze zich aanmelden en worden vervolgens worden gedwongen om dit te wijzigen. 

## <a name="next-steps"></a>Volgende stappen
* [Een gebruiker toevoegen](active-directory-users-create-azure-portal.md)
* [Beheerdersrollen toewijzen aan een gebruiker](active-directory-users-assign-role-azure-portal.md)
* [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
* [Verwijderen van een gebruiker in Azure AD](active-directory-users-delete-user-azure-portal.md)
