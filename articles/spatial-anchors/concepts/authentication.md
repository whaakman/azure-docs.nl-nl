---
title: Verificatie en autorisatie voor Azure ruimtelijke ankers | Microsoft Docs
description: Meer informatie over de verschillende manieren die kan worden geverifieerd door een app of service voor Azure ruimtelijke ankers en de niveaus van controle die u poort toegang tot Azure ruimtelijke ankers moet.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 05/28/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: c7ffa432c9311ba9d4ecf4ba82c375e2dad988d0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478549"
---
# <a name="authentication-and-authorization-to-azure-spatial-anchors"></a>Verificatie en autorisatie voor Azure ruimtelijke ankers

In deze sectie wordt behandelen we de verschillende manieren die kan worden geverifieerd voor Azure ruimtelijke ankers van uw app of webservice en de manieren waarop u toegangsbeheer op basis van rollen in Azure Directory (Azure AD) kunt gebruiken voor toegang tot uw ruimtelijke ankers-accounts beheren.  

## <a name="overview"></a>Overzicht

![Een overzicht van verificatie bij Azure ruimtelijke ankers](./media/spatial-anchors-authentication-overview.png)

Voor toegang tot een bepaald ruimtelijke ankers Azure-account, moeten clients eerst een toegangstoken ophalen uit Azure gemengde realiteit Security Token Service (STS). Tokens die zijn verkregen van de STS live voor 24 uur en bevatten informatie voor de services ruimtelijke ankers te maken van autorisatie beslissingen te nemen voor het account en ervoor te zorgen dat alleen geautoriseerde principals toegang heeft tot dat account. 

Toegangstokens kunnen exchange worden verkregen vanuit een van beide sleutels of vanuit Azure AD-uitgegeven tokens. 

Accountsleutels kunnen u snel aan de slag over het gebruik van de service Azure ruimtelijke ankers; echter, voordat u uw toepassing naar productie implementeert, is het raadzaam dat u uw app voor het gebruik van Azure AD gebaseerde verificatie bijwerkt. 

Azure AD-verificatietokens kunnen op twee manieren worden verkregen:

- Als u een zakelijke toepassing bouwen en uw bedrijf van Azure AD als de identiteitssysteem gebruikmaakt, u kunt op basis van gebruiker Azure AD-verificatie in uw app en toegang verlenen tot uw ruimtelijke ankers accounts met behulp van uw bestaande Azure AD-beveiligingsgroepen, of rechtstreeks aan gebruikers in uw organisatie. 
- Anders is het raadzaam dat u Azure AD-tokens verkrijgen van een webservice ondersteunen van uw app. Met behulp van een ondersteunende webservice is de aanbevolen methode verificatie voor productie-Apps, zoals dit voorkomt het insluiten van de referenties voor toegang tot Azure ruimtelijke ankers in uw clienttoepassing. 

## <a name="account-keys"></a>Accountsleutels

Sleutels gebruiken voor toegang tot uw account Azure ruimtelijke ankers is de eenvoudigste manier om aan de slag. U vindt uw accountsleutels in Azure portal. Navigeer naar uw account en selecteer het tabblad 'Sleutels'.

![Een overzicht van verificatie bij Azure ruimtelijke ankers](../../../includes/media/spatial-anchors-get-started-create-resource/view-account-key.png)


Twee sleutels worden beschikbaar gemaakt, die beide zijn tegelijkertijd geldig zijn voor toegang tot aan de ruimtelijke ankers-account. Het is raadzaam dat u regelmatig bijwerken van de sleutel die u gebruiken voor toegang tot het account hebben twee afzonderlijke geldige sleutels inschakelen, zoals updates zonder uitvaltijd; u hoeft alleen te werken ook de primaire sleutel en de secundaire sleutel. 

De SDK heeft ingebouwde ondersteuning voor verificatie met accountsleutels; u hoeft alleen de eigenschap AccountKey instellen voor uw cloudSession-object. 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccountKey = @"MyAccountKey";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accountKey = @"MyAccountKey";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accountKey = "MyAccountKey"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccountKey(R"(MyAccountKey)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccountKey(LR"(MyAccountKey)");
```

***

Wanneer dat is gebeurd, wordt de SDK verwerkt de uitwisseling van de accountsleutel voor een toegangstoken en de benodigde caching van tokens voor uw app. 

> [!WARNING] 
> Gebruik van sleutels wordt aanbevolen voor snelle bepalen, maar tijdens de ontwikkeling/prototypen alleen. Het wordt sterk aanbevolen niet voor de verzending van uw toepassing naar productie met behulp van een ingesloten accountsleutel in het, en het gebruik in plaats daarvan op basis van gebruikers of -service op basis van Azure AD-verificatie vermelde volgende nadert.

## <a name="azure-ad-user-authentication"></a>Gebruikersverificatie met Azure AD

Voor toepassingen die zijn gericht op Azure Active Directory-gebruikers, de aanbevolen aanpak is een Azure AD-token wordt gebruikt voor de gebruiker die u met behulp van de ADAL-bibliotheek verkrijgen kunt, zoals beschreven in de volgende documentatie: [ https://docs.microsoft.com/azure/active-directory/develop/v1-overview ](../../active-directory/develop/v1-overview.md); u Volg de stappen die worden vermeld onder 'Quick Start', waaronder:

1. De configuratie in Azure portal
    1.  Registreer uw toepassing in Azure AD als **systeemeigen toepassing**. Als onderdeel van het registreren moet u te bepalen of uw toepassing moet multitenant of niet worden en de URL's toegestaan voor uw toepassing omleiding bieden.  
    2.  Uw toepassing of gebruikers toegang verlenen tot uw resources: 
        1.  Navigeer naar uw ruimtelijke ankers resource in Azure portal
        2.  Schakel over naar de **toegangsbeheer (IAM)** tabblad
        3.  Raak **roltoewijzing toevoegen**
            1.  [Selecteer een rol](#role-based-access-control)
            2.  In de **Selecteer** en voer de naam van de gebruiker (s), groepen en/of toepassingen waartoe u toegang wilt toewijzen. 
            3.  Raak **opslaan**.
2. In uw code:
    1.  Zorg ervoor dat u de **toepassings-ID** en **omleidings-Uri** van uw eigen Azure AD-toepassing als de **client-ID** en **RedirectUri** parameters in ADAL
    2.  Stel in de tenant-gegevens:
        1.  Als uw toepassing ondersteunt **mijn organisatie alleen**, vervang deze waarde aan uw **Tenant-ID** of **tenantnaam** (bijvoorbeeld contoso.microsoft.com)
        2.  Als uw toepassing ondersteunt **Accounts in een organisatie-map**, vervang deze waarde met **organisaties**
        3.  Als uw toepassing ondersteunt **alle Microsoft-accountgebruikers**, vervang deze waarde met **algemene**
    3.  Stel op uw tokenaanvraag de **resource** naar 'https://sts.mixedreality.azure.com '. Deze 'resource' wordt aangegeven met Azure AD dat uw toepassing een token voor de service Azure ruimtelijke ankers aanvraagt.  

Met die uw toepassing zou het mogelijk om te verkrijgen van ADAL met een Azure AD-token; u kunt instellen dat Azure AD-token als de **authenticationtoken weergeeft** op uw cloud-sessie AppConfig-object. 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AuthenticationToken = @"MyAuthenticationToken";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.authenticationToken = @"MyAuthenticationToken";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.authenticationToken = "MyAuthenticationToken"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AuthenticationToken(R"(MyAuthenticationToken)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

***

## <a name="azure-ad-service-authentication"></a>Azure AD-service-verificatie

De aanbevolen optie voor het gebruik te maken van Azure ruimtelijke ankers naar productie-apps implementeren is gebruikmaken van een back-endservice die verificatieaanvragen wordt broker. Het algemene schema moet worden zoals beschreven in dit diagram:

![Een overzicht van verificatie bij Azure ruimtelijke ankers](./media/spatial-anchors-aad-authentication.png)

Hier wordt ervan uitgegaan dat uw app gebruikmaakt van een eigen mechanisme (bijvoorbeeld: Microsoft-account, PlayFab, Facebook, Google-ID, aangepaste gebruikersnaam en wachtwoord, enz.) aanmelden bij de back endservice. Wanneer uw gebruikers worden geverifieerd met uw back endservice, die service kan ophalen een Azure AD-token exchange het voor een toegangstoken voor ruimtelijke ankers van Azure en deze terug naar uw clienttoepassing te retourneren.

Het Azure AD-toegangstoken wordt opgehaald met behulp van de ADAL-bibliotheek, zoals beschreven in de volgende documentatie: [ https://docs.microsoft.com/azure/active-directory/develop/v1-overview ](../../active-directory/develop/v1-overview.md); u moet de stappen die worden vermeld onder 'Quick Start', waaronder:

1.  De configuratie in Azure portal:
    1.  Uw toepassing registreren in Azure AD:
        1.  Navigeer in Azure portal naar **Azure Active Directory**, en selecteer **app-registraties**
        2.  Selecteer **nieuwe toepassing registreren**
        3.  Voer de naam van uw toepassing, selecteert **Web-app / API** als het toepassingstype en voer de verificatie-URL voor uw service. Druk vervolgens op **maken**.
        4.  Klik op de betreffende toepassing, **instellingen**en selecteer vervolgens de **sleutels** tabblad. Voer de naam van de sleutel, selecteer een tijdsduur en druk op **opslaan**. Zorg ervoor dat u het opslaan van de waarde van de sleutel die op dat moment wordt weergegeven als u moet opnemen in de code van uw webservice.
    2.  Uw toepassing en/of gebruikers toegang verlenen tot uw resources:
        1.  Navigeer naar uw ruimtelijke ankers resource in Azure portal
        2.  Schakel over naar de **toegangsbeheer (IAM)** tabblad
        3.  Raak **roltoewijzing toevoegen**
        1.  [Selecteer een rol](#role-based-access-control)
        2.  In de **Selecteer** veld, voer de naam van de toepassingen die u hebt gemaakt en gebruikt die u wilt toewijzen. Als u wilt dat gebruikers van uw app hebben verschillende rollen op basis van het ruimtelijke ankers-account, moet u meerdere toepassingen in Azure AD registreren en toewijzen aan elk een afzonderlijke functie. Vervolgens implementeert u uw logica autorisatie voor het gebruik van de juiste rol voor uw gebruikers.  
    3.  Raak **opslaan**.
2.  In uw code (Opmerking: u kunt het voorbeeld van de service opgenomen op GitHub):
    1.  Zorg ervoor dat u de toepassings-ID, het toepassingsgeheim en omleidings-Uri van uw eigen Azure AD-toepassing als de client-ID, RedirectUri parameters in ADAL en -geheim
    2.  De tenant-ID ingesteld op uw eigen tenant-ID AAAzure toevoegen in de parameter instantie in ADAL
    3.  Stel op uw tokenaanvraag de **resource** naar "https://sts.mixedreality.azure.com " 

Uw back-endservice kan een Azure AD-token ophalen met die. Het kan deze vervolgens uitwisselen voor een MR-token dat wordt geretourneerd naar de client. Met behulp van een Azure AD-token ophalen een token MR wordt gedaan via een REST-aanroep. Hier volgt een voorbeeld-aanroep:

```
GET https://mrc-auth-prod.trafficmanager.net/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
Host: mrc-auth-prod.trafficmanager.net
Connection: Keep-Alive

HTTP/1.1 200 OK
Date: Sun, 24 Feb 2019 08:00:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1153
Accept: application/json
MS-CV: 05JLqWeKFkWpbdY944yl7A.0
{"AccessToken":"eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2MzYyMTk5ZTI2NjQxOGU4ZjE3MThlM2IyMThjZTIxIiwidHlwIjoiSldUIn0.eyJqdGkiOiJmMGFiNWIyMy0wMmUxLTQ1MTQtOWEzNC0xNzkzMTA1NTc4NzAiLCJjYWkiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJ0aWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJhaWQiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJhYW8iOi0xLCJhcHIiOiJlYXN0dXMyIiwicmlkIjoiL3N1YnNjcmlwdGlvbnMvNzIzOTdlN2EtNzA4NC00ODJhLTg3MzktNjM5Y2RmNTMxNTI0L3Jlc291cmNlR3JvdXBzL3NhbXBsZV9yZXNvdXJjZV9ncm91cC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1peGVkUmVhbGl0eS9TcGF0aWFsQW5jaG9yc0FjY291bnRzL2RlbW9fYWNjb3VudCIsIm5iZiI6MTU0NDU0NzkwMywiZXhwIjoxNTQ0NjM0MzAzLCJpYXQiOjE1NDQ1NDc5MDMsImlzcyI6Imh0dHBzOi8vbXJjLWF1dGgtcHJvZC50cmFmZmljbWFuYWdlci5uZXQvIiwiYXVkIjoiaHR0cHM6Ly9tcmMtYW5jaG9yLXByb2QudHJhZmZpY21hbmFnZXIubmV0LyJ9.BFdyCX9UJj0i4W3OudmNUiuaGgVrlPasNM-5VqXdNAExD8acFJnHdvSf6uLiVvPiQwY1atYyPbOnLYhEbIcxNX-YAfZ-xyxCKYb3g_dbxU2w8nX3zDz_X3XqLL8Uha-rkapKbnNgxq4GjM-EBMCill2Svluf9crDmO-SmJbxqIaWzLmlUufQMWg_r8JG7RLseK6ntUDRyDgkF4ex515l2RWqQx7cw874raKgUO4qlx0cpBAB8cRtGHC-3fA7rZPM7UQQpm-BC3suXqRgROTzrKqfn_g-qTW4jAKBIXYG7iDefV2rGMRgem06YH_bDnpkgUa1UgJRRTckkBuLkO2FvA"}
```

Waar de autorisatie-header is als volgt ingedeeld: `Bearer <accoundId>:<accountKey>`

En het antwoord bevat de token MR in tekst zonder opmaak.
 
Dit token MR wordt vervolgens geretourneerd naar de client. Uw client-app kan vervolgens ingesteld als het toegangstoken in de configuratie van de cloud-sessie.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccessToken = @"MyAccessToken";
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accessToken = @"MyAccessToken";
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accessToken = "MyAccessToken"
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccessToken(R"(MyAccessToken)");
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccessToken(LR"(MyAccessToken)");
```

***

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Om te bepalen verleend het niveau van toegang aan die toepassingen, services of Azure AD-gebruikers van uw service, de volgende rollen zijn gemaakt voor u om toe te wijzen naar behoefte op basis van uw Azure ruimtelijke ankers-accounts:

- **Ruimtelijke ankers accounteigenaar**: toepassingen of gebruikers met deze rol kunnen ruimtelijke ankers te maken, query's uitvoeren voor deze en verwijdert u deze. Wanneer u zich bij uw account met behulp van accountsleutels, verifieert de **ruimtelijke ankers accounteigenaar** rol wordt toegewezen aan de geverifieerde principal. 
- **Inzender voor ruimtelijke ankers Account**: toepassingen of gebruikers met deze rol kunnen maken van ruimtelijke ankers, query, maar ze niet verwijderen. 
- **Ruimtelijke ankers Account Reader**: toepassingen of gebruikers met deze rol kunnen alleen op te vragen voor ruimtelijke ankers, maar kan geen nieuwe te maken, bestaande verwijderen of bijwerken metagegevens op ruimtelijke ankers. Dit wordt doorgaans met de gebruikt voor toepassingen waarbij sommige gebruikers cureren de omgeving, terwijl anderen alleen ankers eerder geplaatst in die omgeving kunnen intrekken.

## <a name="next-steps"></a>Volgende stappen

Uw eerste app maken met Azure ruimtelijke ankers.

> [!div class="nextstepaction"]
> [Unity](../unity-overview.yml)

> [!div class="nextstepaction"]
> [iOS](../quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](../quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](../quickstarts/get-started-hololens.md)
