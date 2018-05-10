---
title: Azure Active Directory B2B-samenwerking API en de aanpassing | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/11/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f1bd93ac2ef6aa75e07eeec3e3cb2222b6febc1c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B-samenwerking API en de aanpassing

We hebben veel klanten laat ons weten dat ze willen aanpassen van het proces uitnodiging op een manier die het meest geschikt is voor hun organisatie gehad. Met onze API kunt u dat doen. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Mogelijkheden van de uitnodiging API
De API biedt de volgende mogelijkheden:

1. Een externe gebruiker met uitnodigen *eventuele* e-mailadres.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Aanpassen waar u uw gebruikers land wanneer ze hun uitnodiging hebt geaccepteerd.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Kies de uitnodiging voor een standaard e-mail via ons

    ```
    "sendInvitationMessage": true
    ```

  met een bericht naar de ontvanger die u kunt aanpassen

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. En kies met cc: mensen die u wilt behouden in de lus uw uitnodigen deze medewerker.

5. Of volledig aanpassen aan uw uitnodiging en werkstroom voorbereiden door te kiezen geen om meldingen te verzenden via Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  In dit geval terughalen u een inwisseling-URL van de API die u in een e-mailsjabloon, IM of andere distributiemethode van uw keuze insluiten kunt.

6. Ten slotte, als u een beheerder bent, kunt u om uit te nodigen van de gebruiker die lid is.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Autorisatie-model
De API kan worden uitgevoerd in de volgende autorisatie-modi:

### <a name="app--user-mode"></a>App + gebruikersmodus
In deze modus is degene die de machtigingen om te worden uitnodigingen voor B2B met behulp van de behoeften van de API.

### <a name="app-only-mode"></a>App alleen de modus
De app moet in de app alleen context, het bereik User.Invite.All voor de uitnodiging te kunnen uitvoeren.

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

U kunt ook de uitnodiging voor een API-verwijzing in uitchecken [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
- [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
- [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)

