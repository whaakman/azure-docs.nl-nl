---
title: Google toevoegen als een id-provider voor Azure Active Directory B2B | Microsoft Docs
description: Federeren met Google om in te schakelen gastgebruikers ook kunnen aanmelden bij uw Azure AD-apps met hun eigen Gmail-account
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: 396fb947a95ebc634ab0dea24d20f35126bc006e
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389442"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Google als id-provider voor B2B-gastgebruikers toevoegen

Door het instellen van Federatie met Google, kunt u uitgenodigde gebruikers aanmelden bij uw gedeelde apps en resources met hun eigen Google-accounts, zonder te hoeven maken van Microsoft-Accounts (MSA's) of Azure AD-accounts.  
> [!NOTE]
> Uw Google-gastgebruikers moeten aanmelden met een koppeling met de tenant-context, bijvoorbeeld `https://myapps.microsoft.com/?tenantid=<tenant id>`. Directe koppelingen naar toepassingen en bronnen ook werken, zolang ze de context van de tenant zijn. Gastgebruikers ook kunnen zijn op dit moment kan niet aanmelden met behulp van eindpunten waarvoor geen tenant-context. Bijvoorbeeld, met behulp van `https://myapps.microsoft.com`, `https://portal.azure.com`, of het algemene Teams-eindpunt resulteert in een fout.
 
## <a name="what-is-the-experience-for-the-google-user"></a>Wat is de ervaring voor de Google-gebruiker?
Wanneer u een uitnodiging voor een gebruiker Google Gmail verzenden, moet de gastgebruiker toegang tot uw gedeelde apps of resources met behulp van een koppeling met de tenant-context. Hun ervaring kan verschillen, afhankelijk van of ze al bent aangemeld bij Google:
  - Als de gastgebruiker is niet aangemeld bij Google, wordt ze gevraagd of u aanmelden bij Google.
  - Als de gastgebruiker is al aangemeld bij Google, wordt ze gevraagd om te kiezen welk account moet worden gebruikt. Het account waarmee u hen uit te nodigen, moeten ze kiezen.

Als de gastgebruiker ziet een foutbericht 'header te lang', proberen ze hun cookies uit te schakelen of ze kunnen een particuliere of incognito-venster openen en meldt u zich opnieuw probeert.

![Aanmelden met Google](media/google-federation/google-sign-in.png)

## <a name="step-1-configure-a-google-developer-project"></a>Stap 1: Configureren van een Google developer-project
Maak eerst een nieuw project in de Google-ontwikkelaars-Console om op te halen van een client-ID en een clientgeheim in die u later aan Azure AD toevoegen kunt. 
1. Ga to de Google APIs op https://console.developers.google.com, en meld u aan met uw Google-account. U wordt aangeraden dat u een gedeelde team Google-account.
2. Een nieuw project maken: op het Dashboard selecteren **Project maken**, en selecteer vervolgens **maken**. Voer op de pagina Nieuw Project voor een **projectnaam**, en selecteer vervolgens **maken**.
   
   ![Nieuwe Google-project](media/google-federation/google-new-project.png)

3. Zorg ervoor dat het nieuwe project is geselecteerd in het projectmenu. Vervolgens opent u het menu in de linksboven en selecteer **API's en Services** > **referenties**.

   ![Google API-referenties](media/google-federation/google-api.png)
 
4. Kies de **Oauth instemmingsscherm** tabblad en voer een **productnaam weergegeven voor gebruikers**. (Laat de overige instellingen.) Selecteer **Opslaan**.

   ![Het instemmingsscherm Google OAuth](media/google-federation/google-oauth-consent-screen.png)

5. Kies de **referenties** tabblad. In de **referenties maken** menu, kiest u **OAuth-Clientidentiteit**.

   ![Google API-referenties](media/google-federation/google-api-credentials.png)

6. Onder **toepassingstype**, kiest u **webtoepassing**, en klik vervolgens onder **geautoriseerde omleidings-URI's**, voer de volgende URI's:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(waarbij `<directory id>` is uw directory-ID)
   
    > [!NOTE]
    > Uw directory als ID wilt vinden, gaat u naar https://portal.azure.com, en klikt u onder **Azure Active Directory**, kiest u **eigenschappen** en kopieer de **map-ID**.

   ![OAuth-client-ID maken](media/google-federation/google-create-oauth-client-id.png)

7. Selecteer **Maken**. Kopieer het client-ID en clientgeheim, gebruikt u wanneer u de id-provider in de Azure AD-portal toevoegt.

   ![OAuth-client-ID en clientgeheim](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Stap 2: Google federatie configureren in Azure AD 
Nu hebt u ingesteld de Google-client-ID en clientgeheim, door te voeren in de Azure AD-portal of met behulp van PowerShell. Zorg ervoor dat uw federation-configuratie van Google testen door het uitnodigen van uzelf met een Gmail-adres en probeer het inwisselen van de uitnodiging met het uitgenodigde Google-account. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Google om Federatie te configureren in de Azure AD-portal 
1. Ga naar de [Azure Portal](https://portal.azure.com). Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. 
2. Selecteer **organisatie relaties**.
3. Selecteer **id-providers**, en klik vervolgens op de **Google** knop.
4. Voer een naam. Voer vervolgens de client-ID en clientgeheim die u eerder hebt verkregen. Selecteer **Opslaan**. 

   ![Google-id-provider toevoegen](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>Google om Federatie te configureren met behulp van PowerShell
1. Installeer de nieuwste versie van de Azure AD PowerShell voor Graph-module ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Voer de volgende opdracht uit: `Connect-AzureAD`.
3. Aanmelden bij de prompt aanmelden met het beheerde account voor globale beheerders.  
4. Voer de volgende opdracht uit: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > Gebruik de client-id en clientgeheim in van de app die u hebt gemaakt in ' stap 1: configureren van een Google developer-project. " Zie voor meer informatie de [New-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) artikel. 
 
## <a name="how-do-i-remove-google-federation"></a>Hoe verwijder ik Google federation?
U kunt uw Google-federatie-instellingen verwijderen. Als u dit doet, Google-gastgebruikers die hun uitnodiging al hebt ingewisseld zich niet aanmelden, maar u kunt zodat ze toegang tot uw resources opnieuw door deze uit de map worden verwijderd en opnieuw uitnodiging. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Verwijderen van Google federation in de Azure AD-portal: 
1. Ga naar de [Azure Portal](https://portal.azure.com). Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. 
2. Selecteer **organisatie relaties**.
3. Selecteer **id-providers**, en klik vervolgens op de **Google** knop.
4. Selecteer **Google**, en selecteer vervolgens **verwijderen**. 
   
   ![De sociale id-provider wordt verwijderd](media/google-federation/google-social-identity-providers.png)

1. Selecteer **Ja** om verwijdering te bevestigen. 

### <a name="to-delete-google-federation-by-using-powershell"></a>Google federation verwijderen met behulp van PowerShell: 
1. Installeer de nieuwste versie van de Azure AD PowerShell voor Graph-module ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Voer `Connect-AzureAD` uit.  
4. Aanmelden met het beheerde account globale beheerder in de aanmelding in de prompt.  
5. Voer de volgende opdracht in:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Zie voor meer informatie, [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 