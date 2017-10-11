---
title: Azure Active Directory B2B-samenwerking API en de aanpassing | Microsoft Docs
description: Azure Active Directory B2B-samenwerking ondersteunt uw externe bedrijfsrelaties door zakelijke partners selectief toegang te verlenen tot uw zakelijke toepassingen
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: c85e05b38b4a9525e13ec510a17b7ef4841198d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B-samenwerking API en de aanpassing

We hebben veel klanten laat ons weten dat ze willen aanpassen van het proces uitnodiging op een manier die het meest geschikt is voor hun organisatie gehad. Met onze API kunt u dat doen. [https://Developer.Microsoft.com/Graph/Docs/API-Reference/V1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

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
In de app alleen context moet de app de User.ReadWrite.All of Directory.ReadWrite.All bereiken voor de uitnodiging te kunnen uitvoeren.

Raadpleeg voor meer informatie: https://graph.microsoft.io/docs/authorization/permission_scopes


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

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hoe voeg beheerders van Azure Active Directory B2B-samenwerking gebruikers?](active-directory-b2b-admin-add-users.md)
* [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](active-directory-b2b-iw-add-users.md)
* [De elementen van de uitnodigingsmail voor B2B-samenwerking](active-directory-b2b-invitation-email.md)
* [B2B-samenwerking uitnodiging inwisseling](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samenwerking licentieverlening](active-directory-b2b-licensing.md)
* [Het oplossen van Azure Active Directory B2B-samenwerking](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B-samenwerking Veelgestelde vragen (FAQ)](active-directory-b2b-faq.md)
* [Multi-Factor Authentication voor gebruikers van B2B-samenwerking](active-directory-b2b-mfa-instructions.md)
* [B2B-samenwerking gebruikers zonder een uitnodiging toevoegen](active-directory-b2b-add-user-without-invite.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
