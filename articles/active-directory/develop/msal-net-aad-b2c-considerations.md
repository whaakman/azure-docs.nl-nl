---
title: Azure AD B2C (Microsoft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke aandachtspunten bij het gebruik van Azure AD B2C met de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8240a487bdb01cdbe9017ddc7cb95ce4fc0e1503
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052358"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>MSAL.NET aan te melden bij de gebruikers met sociale identiteiten gebruiken

U kunt MSAL.NET aan te melden bij de gebruikers met sociale identiteiten met behulp van [Azure Active Directory B2C (Azure AD B2C)](https://aka.ms/aadb2c). Azure AD B2C is gebaseerd op het begrip van het beleid. Tot het bieden van een instantie van zet een beleid op te geven in MSAL.NET.

- Wanneer u de openbare clienttoepassing, moet u het beleid opgeven-instantie.
- Wanneer u een beleid van toepassing, moet u een onderdrukking van aanroepen `AcquireTokenInteractive` met een `authority` parameter.

Deze pagina is voor MSAL 3.x. Als u geïnteresseerd in MSAL bent 2.x gebruikt, Raadpleeg [Azure AD B2C-specificaties in MSAL 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x).

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Instantie voor een Azure AD B2C-tenant en het beleid

De instantie te gebruiken is `https://login.microsoftonline.com/tfp/{tenant}/{policyName}` waar:

- `tenant` de naam van de Azure AD B2C-tenant 
- `policyName` de naam van het beleid toe te passen (bijvoorbeeld ' b2c_1_susi' voor het teken-in/aanmelden-van).

De huidige richtlijnen van Azure AD B2C is het gebruik van `b2clogin.com` als de instantie. Bijvoorbeeld `$"https://{your-tenant-name}.b2clogin.com/tfp/{your-tenant-ID}/{policyname}"`. Zie voor meer informatie dit [documentatie](/azure/active-directory-b2c/b2clogin).

## <a name="instantiating-the-application"></a>Het instantiëren van de toepassing

Bij het bouwen van de toepassing, moet u de instantie opgeven.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://fabrikamb2c.b2clogin.com/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>Een token als een beleid wilt toepassen ophalen

Ophalen van een token voor een Azure AD B2C beveiligde API in een openbare client-toepassing, moet u de onderdrukkingen gebruiken met een instantie:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireToken(scopes, parentWindow)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .ExecuteAsync();
```

door:

- `policy` een van de vorige tekenreeksen (bijvoorbeeld `PolicySignUpSignIn`).
- `GetAccountByPolicy(IEnumerable<IAccount>, string)` is een methode waarmee wordt gezocht naar een account voor een bepaald beleid. Bijvoorbeeld:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

Toepassen van een beleid (bijvoorbeeld zodat de eindgebruiker hun profiel te bewerken of hun wachtwoord opnieuw instellen) die momenteel wordt uitgevoerd door het aanroepen van `AcquireTokenInteractive`. In het geval van deze twee beleidsregels die u het geretourneerde token niet gebruikt / verificatie resulteert.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>Speciaal geval van EditProfile en ResetPassword beleid

Als u wilt een ervaring waar uw eindgebruikers zich aanmelden met een sociale identiteit bieden, en vervolgens hun profiel te bewerken die u wilt het EditProfile van Azure AD B2C-beleid wordt toegepast. De manier waarop dit wilt doen is door het aanroepen van `AcquireTokenInteractive` met de specifieke bevoegdheid voor dat beleid en een Prompt die is ingesteld op `Prompt.NoPrompt` om te voorkomen dat het dialoogvenster voor het selecteren van account moet worden weergegeven (zoals de gebruiker al aangemeld is)

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Resource-eigenaar wachtwoordreferenties (ROPC) met Azure AD B2C
Voor meer informatie over de stroom ROPC, raadpleegt u dit [documentatie](v2-oauth-ropc.md).

Deze stroom is **niet aanbevolen** omdat uw toepassing een gebruiker wordt gevraagd het wachtwoord niet beveiligd is. Zie voor meer informatie over dit probleem [in dit artikel](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Met behulp van gebruikersnaam en wachtwoord, u zijn geven tot een aantal dingen:
- Core-tenants van moderne identiteit: wachtwoord opgehaald gevangen, opnieuw afgespeeld. Omdat we dit concept van een share-geheim die kan worden onderschept hebben. Dit is niet compatibel met de configuratie.
- Gebruikers die willen MFA doen zich niet aanmelden (wat er geen tussenkomst van de is).
- Gebruikers niet mogelijk om u te eenmalige aanmelding.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>De stroom ROPC configureren in Azure AD B2C
Een nieuwe gebruikersstroom maken in uw Azure AD B2C-tenant, en selecteer **Meld u aan met behulp van ROPC**. Hiermee schakelt u het beleid ROPC voor uw tenant. Zie [configureren de wachtwoord-referenties van de resource-eigenaar flow](/azure/active-directory-b2c/configure-ropc) voor meer informatie.

`IPublicClientApplication` bevat een methode:
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Deze methode wordt gebruikt als parameters:
- De *scopes* om aan te vragen van een toegangstoken voor.
- Een *gebruikersnaam*.
- Een SecureString *wachtwoord* voor de gebruiker.

Houd er rekening mee te gebruiken van de instantie die het beleid ROPC bevat.

### <a name="limitations-of-the-ropc-flow"></a>Beperkingen van de stroom ROPC
 - De stroom ROPC **werkt alleen voor lokale accounts** (waarbij u zich registreren bij Azure AD B2C met behulp van een e-mailadres of gebruikersnaam). Deze stroom werkt niet als federeren met een van de id-providers ondersteund door Azure AD B2C (Facebook, Google, enz.).
 - Er is momenteel **geen id_token geretourneerd uit Azure AD B2C** bij het implementeren van de stroom ROPC van MSAL. Dit betekent dat een Account-object kan niet worden gemaakt, zodat u in de cache, zal er geen Account en er geen gebruiker. De stroom AcquireTokenSilent werkt niet in dit scenario. Echter ROPC een gebruikersinterface niet wordt weergegeven, dus er wordt geen invloed hebben op de gebruikerservaring.

## <a name="google-auth-and-embedded-webview"></a>Google-verificatie en ingesloten webweergave

Als u een Azure AD B2C-ontwikkelaar bent als id-provider met behulp van Google we recommand gebruik van de browser systeem, zoals Google staat niet toe dat [verificatie van ingesloten webweergaven](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Op dit moment `login.microsoftonline.com` is van een vertrouwde instantie met Google. Met behulp van deze instantie werkt met ingesloten webweergave. Echter met behulp van `b2clogin.com` is niet een vertrouwde instantie met Google, zodat gebruikers zich niet verifiëren.

We sturen een update voor de wiki, en deze [probleem](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688) als dingen wijzigen.

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>Opslaan in cache met Azure AD B2C in MSAL.Net 

### <a name="known-issue-with-azure-ad-b2c"></a>Bekend probleem met Azure AD B2C

MSAL.Net ondersteunt een [token cache](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet). Het token sleutel opslaan in cache is gebaseerd op de claims die wordt geretourneerd door de id-Provider. MSAL.Net heeft momenteel twee claims voor het bouwen van een sleutel-tokencache nodig:  
- `tid` Dit is de Azure AD-Tenant-ID, en 
- `preferred_username` 

Beide deze claims ontbreken in veel van de Azure AD B2C-scenario's. 

De gevolgen zijn voor klanten is dat bij een poging om het gebruikersnaamveld weer te geven, u krijgt "Ontbreekt uit het token antwoord" Als de waarde? Als dit het geval is, is dit omdat Azure AD B2C geen waarde in de IdToken voor de preferred_username vanwege beperkingen in de sociale accounts en externe id-providers (IdPs retourneert). Azure AD retourneert een waarde voor preferred_username omdat deze weet wie de gebruiker is, maar voor Azure AD B2C, omdat de gebruiker met een lokale account, Facebook, Google, GitHub aanmelden kunt, enz. Er is geen een consistente waarde voor Azure AD B2C wilt gebruiken voor preferred_username. Als u wilt deblokkeren MSAL uit het uitrollen van cache-compatibiliteit met ADAL, besloten we te "Ontbreekt uit het token antwoord" aan onze kant gebruiken bij het omgaan met de Azure AD B2C-accounts wanneer de IdToken niets voor preferred_username worden geretourneerd. MSAL moet een waarde retourneren voor preferred_username voor compatibiliteit met cache voor andere bibliotheken.

### <a name="workarounds"></a>Tijdelijke oplossingen

#### <a name="mitigation-for-the-missing-tenant-id"></a>Beperking voor de tenant-ID ontbreekt

De oplossing is het gebruik van de [opslaan in cache door het beleid](#acquire-a-token-to-apply-a-policy)

U kunt ook kunt u de `tid` claim, als u de [B2C aangepast beleid](https://aka.ms/ief), omdat deze biedt de mogelijkheid om terug te keren extra claims voor de toepassing. Voor meer informatie over [claimtransformatie](/azure/active-directory-b2c/claims-transformation-technical-profile)

#### <a name="mitigation-for-missing-from-the-token-response"></a>Risicobeperking voor "Ontbreekt uit het token antwoord"
Een optie is het gebruik van de claim 'naam' als de gewenste gebruikersnaam. Het proces wordt genoemd in deze [B2C doc](../../active-directory-b2c/active-directory-b2c-reference-policies.md) -> 'In de kolom geretourneerde claim, kies de claims die u wilt laten retourneren in de autorisatietokens die is verzonden naar de toepassing na een geslaagde profielbewerking. Selecteer bijvoorbeeld weergavenaam, postcode."

## <a name="next-steps"></a>Volgende stappen 

Meer informatie over het verkrijgen van tokens interactief met MSAL.NET voor Azure AD B2C-toepassingen vindt u in het volgende voorbeeld.

| Voorbeeld | Platform | Description|
|------ | -------- | -----------|
|[active-directory-b2c-xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL.NET gebruikt om te verifiëren van gebruikers via Azure AD B2C en toegang tot een Web-API met behulp van de resulterende tokens.|
