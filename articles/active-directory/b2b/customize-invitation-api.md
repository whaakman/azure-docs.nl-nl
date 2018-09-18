---
title: Azure Active Directory B2B-samenwerking API en aanpassingen | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 20c824da82d6e3e66bfa2d7447c8a9573cbdce69
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985810"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B-samenwerking API en aanpassingen

We hebben veel klanten laat ons weten dat ze willen aanpassen van het uitnodigingsproces op een manier die het meest geschikt voor hun organisaties gehad. U kunt precies dat doen met onze API's. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Mogelijkheden van de uitnodiging API
De API biedt de volgende mogelijkheden:

1. Een externe gebruiker met uitnodigen *eventuele* e-mailadres.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Aanpassen waar u uw gebruikers aan de grond nadat ze hun uitnodiging accepteren.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Kies de e-mail standard uitnodiging via ons

    ```
    "sendInvitationMessage": true
    ```

  met een bericht naar de ontvanger die u kunt aanpassen

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. En kies met cc: mensen die u behouden in de lus wilt over uw deze samenwerker uitnodigen.

5. Of volledig aanpassen aan uw uitnodiging en onboarding werkstroom door te kiezen niet om meldingen te verzenden via Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  In dit geval ontvangt u weer een URL voor inschrijving van de API die u in een e-mailsjabloon IM, of een andere methode voor het distribueren van uw keuze insluiten kunt.

6. Ten slotte, als u een beheerder bent, kunt u de gebruiker als lid uitnodigen.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Autorisatiemodel
De API kan worden uitgevoerd in de volgende modi voor autorisatie:

### <a name="app--user-mode"></a>App + gebruikersmodus
In deze modus is degene die met behulp van de behoeften van de API de machtigingen hebben voor B2B-uitnodigingen worden gemaakt.

### <a name="app-only-mode"></a>Alleen App-modus
In de app alleen context moet de app het User.Invite.All bereik voor de uitnodiging te voltooien.

Zie voor meer informatie: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Het is nu mogelijk om te gebruiken van PowerShell toe te voegen en externe gebruikers uitnodigen voor een organisatie eenvoudig. Maak een uitnodiging met de cmdlet:

```
New-AzureADMSInvitation
```

U kunt de volgende opties gebruiken:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

U kunt ook de uitnodiging voor API-verwijzing in uitchecken [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [De elementen van de B2B-samenwerking uitnodigingse-mail](invitation-email-elements.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Gebruikers van B2B-samenwerking zonder uitnodiging toevoegen](add-user-without-invite.md)

