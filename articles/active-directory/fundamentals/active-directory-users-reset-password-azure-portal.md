---
title: Wachtwoord opnieuw instellen in Azure Active Directory | Microsoft Docs
description: De beheerder geïnitieerd voor wachtwoord opnieuw instellen voor een gebruiker in Azure Active Directory
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
editor: ''
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 689ab264e56bc8559db6237eee19566e9e3c429f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644127"
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Het wachtwoord voor een gebruiker in Azure Active Directory opnieuw instellen

Beheerders moeten mogelijk opnieuw instellen van het wachtwoord van een gebruiker in gevallen waar ze vergeten, zijn vergrendeld out- of andere scenario's. De volgende stappen helpen u bij het opnieuw instellen van wachtwoord van een gebruiker.

## <a name="how-to-reset-the-password-for-a-user"></a>Het wachtwoord voor een gebruiker opnieuw instellen

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account met machtigingen van de directory voor het opnieuw instellen van wachtwoorden van gebruikers.
2. Selecteer **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer de gebruiker die u wilt het wachtwoord voor opnieuw instellen.
2. Selecteer voor de geselecteerde gebruiker **wachtwoord opnieuw instellen**.

    ![Een wachtwoord voor een gebruiker vanuit het profiel van de gebruiker opnieuw instellen in Azure AD](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. Op **wachtwoord opnieuw instellen**, selecteer **wachtwoord opnieuw instellen**.
7. Een tijdelijk wachtwoord wordt weergegeven dat u vervolgens aan de gebruiker kan bieden. De gebruiker wordt gevraagd het wachtwoord klikt u vervolgens de volgende keer dat ze zich aanmelden te wijzigen. 

   > [!NOTE]
   > Dit tijdelijke wachtwoord komt niet hebben een verlooptijd zodat deze zijn geldig totdat deze zich aanmeldt en worden vervolgens worden gedwongen om dit te wijzigen. 

## <a name="next-steps"></a>Volgende stappen
* [Een gebruiker toevoegen](../add-users-azure-active-directory.md)
* [Beheerdersrollen toewijzen aan een gebruiker](active-directory-users-assign-role-azure-portal.md)
* [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
* [Een gebruiker verwijderen in Azure AD](../add-users-azure-active-directory.md)
