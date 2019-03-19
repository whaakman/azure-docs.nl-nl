---
title: B2B-samenwerking API en aanpassingen - Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: reference
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 610d81912ac0244f25bc39c41690ab7e7ea8897c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58111307"
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

Zie voor meer informatie: https://developer.microsoft.com/graph/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
U kunt PowerShell gebruiken toe te voegen en externe gebruikers uitnodigen voor een organisatie eenvoudig. Maak een uitnodiging met de cmdlet:

```
New-AzureADMSInvitation
```

U kunt de volgende opties gebruiken:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

### <a name="invitation-status"></a>Status van de uitnodiging

Nadat u een externe gebruiker een uitnodiging verzenden, kunt u de **Get-AzureADUser** cmdlet om te zien als ze het hebt geaccepteerd. De volgende eigenschappen van Get-AzureADUser worden ingevuld wanneer een externe gebruiker wordt een uitnodiging verzonden:

* **UserState** geeft aan of de uitnodiging **PendingAcceptance** of **geaccepteerde**.
* **UserStateChangedOn** geeft de tijdstempel weer voor de meest recente wijziging op de **UserState** eigenschap.

U kunt de **Filter** optie voor het filteren van de resultaten door **UserState**. In het volgende voorbeeld laat zien hoe resultaten om weer te geven alleen gebruikers met een uitnodiging voor een in behandeling te filteren. Het voorbeeld ziet u ook de **lijst indeling** optie, waarmee u de eigenschappen om weer te geven. 
 
```
Get-AzureADUser -Filter "UserState eq 'PendingAcceptance'" | Format-List -Property DisplayName,UserPrincipalName,UserState,UserStateChangedOn
```

> [!NOTE]
> Zorg ervoor dat u hebt de nieuwste versie van de AzureAD PowerShell-module of AzureADPreview PowerShell-module. 

## <a name="see-also"></a>Zie ook

Bekijk de uitnodiging voor API-verwijzing in [ https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation ](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [De elementen van de B2B-samenwerking uitnodigingse-mail](invitation-email-elements.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Gebruikers van B2B-samenwerking zonder uitnodiging toevoegen](add-user-without-invite.md)

