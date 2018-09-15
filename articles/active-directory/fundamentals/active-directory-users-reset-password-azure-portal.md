---
title: Het opnieuw instellen van wachtwoord van een gebruiker in Azure Active Directory | Microsoft Docs
description: Wachtwoord opnieuw instellen van een gebruiker met Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: e6f937063832e6b2a5f301d6d61aec4b3426ea28
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603087"
---
# <a name="how-to-reset-a-users-password-using-azure-active-directory"></a>Hoe: wachtwoord opnieuw instellen van een gebruiker met Azure Active Directory
Als het wachtwoord is vergeten, als de gebruiker toegang tot een apparaat wordt geblokkeerd, of als de gebruiker een wachtwoord nooit hebt ontvangen, kunt u het wachtwoord van een gebruiker opnieuw instellen.

>[!Note]
>Als uw Azure AD-tenant de basismap van een gebruiker is, kunt u zich niet hun wachtwoord opnieuw instellen. Dit betekent dat als de gebruiker bij uw organisatie met een account van een andere organisatie, een Microsoft-account of een Google-account aanmeldt zich, kunt u zich niet op hun wachtwoord opnieuw instellen.<br><br>Als de gebruiker een bron van dienst als Windows Server Active Directory heeft, kunt u zult alleen het wachtwoord opnieuw instellen als u wachtwoord terugschrijven hebt ingeschakeld.<br><br>Als de gebruiker een bron van instantie als externe Azure AD heeft, kunt u zich niet aan het wachtwoord opnieuw instellen. Alleen de gebruiker of een beheerder in de externe Azure AD, kan het wachtwoord opnieuw instellen.

## <a name="to-reset-a-password"></a>Een wachtwoord opnieuw instellen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als een globale beheerder, gebruikerbeheerder of wachtwoordbeheerder. Zie voor meer informatie over de beschikbare rollen [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, zoekt en selecteert u de gebruiker die het opnieuw instellen moet en selecteer vervolgens **wachtwoord opnieuw instellen**.

    De **Alain Charon - profiel** pagina wordt weergegeven met de **wachtwoord opnieuw instellen** optie.

    ![De profielpagina van gebruiker met de optie voor wachtwoord opnieuw instellen is gemarkeerd](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. In de **wachtwoord opnieuw instellen** weergeeft, schakelt **wachtwoord opnieuw instellen**.

    Een tijdelijk wachtwoord wordt automatisch gegenereerd voor de gebruiker.

4. Kopiëren van het wachtwoord en geeft u het aan de gebruiker. De gebruiker moet het wachtwoord wijzigen tijdens de volgende aanmelding.

    >[!Note]
    >Het tijdelijke wachtwoord verloopt nooit. De volgende keer dat de gebruiker zich aanmeldt, wordt het wachtwoord nog steeds werken, ongeacht hoeveel tijd is verstreken sinds het tijdelijke wachtwoord is gegenereerd.

## <a name="next-steps"></a>Volgende stappen
Nadat u het wachtwoord van de gebruiker opnieuw instelt hebt, kunt u de volgende basis-processen uitvoeren:

- [Toevoegen of verwijderen van gebruikers](add-users-azure-active-directory.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Toevoegen of wijzigen van de profielgegevens](active-directory-users-profile-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

Of u complexere gebruiker-scenario's, zoals het toewijzen van gemachtigden, met behulp van beleid en het delen van gebruikersaccounts kan uitvoeren. Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).
