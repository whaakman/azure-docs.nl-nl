---
title: Verificatie van de service-naar-service voor Azure Sleutelkluis met .NET
description: "De bibliotheek Microsoft.Azure.Services.AppAuthentication gebruiken om te verifiëren voor Azure Sleutelkluis met .NET."
keywords: lokale referenties van Azure sleutelkluis verificatie
author: lleonard-msft
manager: mbaldwin
services: key-vault
ms.author: alleonar
ms.date: 11/15/2017
ms.topic: article
ms.prod: 
ms.service: microsoft-keyvault
ms.technology: 
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: bff4b15ca2f1c985c4b4e27d159adaa5fd039553
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Verificatie van de service-naar-service voor Azure Sleutelkluis met .NET

Om te verifiëren voor Azure Sleutelkluis, moet u een referentie voor de Azure Active Directory (AD), een gedeeld geheim of een certificaat. Het beheren van deze referenties kan lastig zijn en is verleidelijk referenties bundelen in een app door ze in de bron-of configuratiebestanden.

De `Microsoft.Azure.Services.AppAuthentication` voor .NET-bibliotheek dit probleem vereenvoudigt. Deze referenties van de ontwikkelaar voor verificatie tijdens het ontwikkelen van lokale gebruikt. Wanneer de oplossing later naar Azure is geïmplementeerd, wordt de bibliotheek automatisch overgeschakeld naar de referenties van toepassing.  

Met de referenties van de ontwikkelaar van tijdens het ontwikkelen van lokale is veiliger omdat u niet wilt maken van Azure AD-referenties of referenties tussen ontwikkelaars delen.

De `Microsoft.Azure.Services.AppAuthentication` bibliotheek beheert verificatie automatisch, op zijn beurt waarmee u zich richten op uw oplossing in plaats van uw referenties.

De `Microsoft.Azure.Services.AppAuthentication` bibliotheek ondersteunt de lokale ontwikkeling met Microsoft Visual Studio, Azure CLI of Azure AD-geïntegreerde verificatie. Wanneer dit wordt geïmplementeerd met Azure App Services of een Azure-virtuele Machine (VM), gebruikt de bibliotheek automatisch [beheerde Service-identiteit](/azure/active-directory/msi-overview) (MSI). Er zijn geen wijzigingen code of configuratie vereist. De bibliotheek ondersteunt ook direct gebruik van Azure AD [clientreferenties](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) wanneer MSI is niet beschikbaar of wanneer de beveiligingscontext van de ontwikkelaar kan niet worden bepaald tijdens het ontwikkelen van lokale.

<a name="asal"></a>
## <a name="using-the-library"></a>Met behulp van de bibliotheek

Voor .NET-toepassingen, de eenvoudigste manier om te werken met een beheerde Service identiteit (MSI) is via de `Microsoft.Azure.Services.AppAuthentication` pakket. Ga als volgt aan de slag:

1. Voeg een verwijzing naar de [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) NuGet-pakket voor uw toepassing.

2. Voeg de volgende code toe:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // ...

    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(
    azureServiceTokenProvider.KeyVaultTokenCallback));

    // or

    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync(
       "https://management.azure.com/").ConfigureAwait(false);
    ```

De `AzureServiceTokenProvider` klasse in de cache van het token in het geheugen opgeslagen en opgehaald uit Azure AD voor de vervaldatum. Als gevolg daarvan kan niet meer hebt tot Controleer de vervaldatum voordat u de `GetAccessTokenAsync` methode. Roep de methode alleen als u wilt gebruiken van het token. 

De `GetAccessTokenAsync` methode vereist een bron-id. Zie voor meer informatie, [die Azure-services ondersteuning bieden voor Service-identiteiten beheerd](https://docs.microsoft.com/azure/active-directory/msi-overview#which-azure-services-support-managed-service-identity).


<a name="samples"></a>
## <a name="samples"></a>Voorbeelden

De volgende voorbeelden tonen de `Microsoft.Azure.Services.AppAuthentication` bibliotheek in actie:

1. [Een beheerde Service identiteit (MSI) gebruiken voor het ophalen van een geheim van Azure Sleutelkluis tijdens runtime](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Een Azure Resource Manager-sjabloon van een Azure-VM met een MSI programmatisch implementeren](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Voorbeeld voor .NET Core en MSI gebruiken om Azure-services aanroepen vanuit een Azure Linux VM](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).


<a name="local"></a>
## <a name="local-development-authentication"></a>Verificatie van lokale ontwikkeling

Er zijn twee primaire verificatie-scenario's voor lokale ontwikkeling:

- [Verificatie bij de Azure-services](#authenticating-to-azure-services)
- [Aangepaste services verifiëren](#authenticating-to-custom-services)

Hier kunt u meer informatie over de vereisten voor elk scenario en de ondersteunde hulpprogramma's.


### <a name="authenticating-to-azure-services"></a>Verificatie bij de Azure-Services

Lokale machines bieden geen ondersteuning voor Managed Service identiteit (MSI).  Als gevolg hiervan de `Microsoft.Azure.Services.AppAuthentication` bibliotheek uw developer-referenties gebruikt voor het uitvoeren in uw lokale ontwikkelingsomgeving. Wanneer de oplossing is geïmplementeerd in Azure, wordt in de bibliotheek MSI overschakelen naar een stroom grant OAuth 2.0 voor referentie gebruikt.  Dit betekent dat u kunt de dezelfde code testen lokaal en extern zonder zorgen.

Voor lokale ontwikkeling `AzureServiceTokenProvider` haalt tokens met **Visual Studio**, **Azure-opdrachtregelinterface** (CLI) of **Azure AD-geïntegreerde verificatie**. Elke optie sequentieel is geprobeerd en de bibliotheek maakt gebruik van de eerste optie die is geslaagd. Als er geen optie werkt, een `AzureServiceTokenProviderException` uitzondering met gedetailleerde informatie.

### <a name="authenticating-with-visual-studio"></a>Verificatie met Visual Studio

Controleer of voor het gebruik van Visual Studio:

1. U hebt geïnstalleerd [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) of hoger.

2. De [App verificatie-extensie voor Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354) is geïnstalleerd.
 
3. U hebt aangemeld bij Visual Studio en een account kunt gebruiken voor lokale ontwikkeling hebt geselecteerd. Gebruik **extra**&nbsp;>&nbsp;**opties**&nbsp;>&nbsp;**verificatie van Azure Service**kiezen van een account met lokale ontwikkeling. 

Als u problemen bij het gebruik van Visual Studio, zoals fouten met betrekking tot het bestand tokenprovider zorgvuldig door deze stappen. 

Het is mogelijk ook nodig om andere referenties van uw developer-token.  Om dit te doen, gaat u naar **extra**&nbsp;>&nbsp;**opties**>**Azure&nbsp;Service&nbsp;verificatie**  en zoekt u een **opnieuw verifiëren** koppeling onder het geselecteerde account.  Selecteer dit om te verifiëren. 

### <a name="authenticating-with-azure-cli"></a>Verificatie met Azure CLI

Azure CLI gebruiken voor lokale ontwikkeling:

1. Installeer [Azure CLI v2.0.12](/cli/azure/install-azure-cli) of hoger. Upgrade van eerdere versies. 

2. Gebruik **az aanmelding** aan te melden bij Azure.

Gebruik `az account get-access-token` toegang controleren.  Als u een foutbericht ontvangt, controleert u of de stap 1 is voltooid. 

Als Azure CLI is niet geïnstalleerd op de standaardmap, verschijnt er een fout rapportage die `AzureServiceTokenProvider` het pad niet vinden voor Azure CLI.  Gebruik de **AzureCLIPath**omgevingsvariabele voor het definiëren van de Azure CLI-installatiemap. `AzureServiceTokenProvider`voegt de opgegeven map in de **AzureCLIPath** omgevingsvariabele de **pad** omgevingsvariabele indien nodig.

Als u bent aangemeld bij Azure CLI met meerdere accounts of uw account toegang tot meerdere abonnementen heeft, moet u de specifieke abonnement moet worden gebruikt opgeven.  Gebruik het volgende om dit te doen:

```
az account set --subscription <subscription-id>
```

Met deze opdracht genereert uitvoer alleen bij fouten.  Het huidige account gebruiken om instellingen te controleren, gebruikt u:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Verificatie met Azure AD integreren verificatie

Azure AD om verificatie te gebruiken, moet u controleren of:

- Uw lokale active directory [gesynchroniseerd naar Azure AD](/azure/active-directory/connect/active-directory-aadconnect).

- Uw code wordt uitgevoerd op een machine domein.


### <a name="authenticating-to-custom-services"></a>Aangepaste services verifiëren

Wanneer een serviceaanroepen Azure-services, wordt de vorige stappen werken omdat toegang aan gebruikers en toepassingen voor Azure-services toestaan.  

Bij het maken van een service die een aangepaste service aanroept, moet u Azure AD clientreferenties gebruikt voor verificatie van lokale ontwikkeling.  Er zijn twee opties: 

1.  Een service-principal gebruiken om aan te melden bij Azure:

    1.  [Maken van een service-principal](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Gebruik Azure CLI aan te melden:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Omdat de service-principal geen toegang tot een abonnement hebt mogelijk, gebruikt u de `--allow-no-subscriptions` argument.

2.  Variabelen voor takenreeksomgevingen gebruiken om op te geven details van de service-principal.  Zie voor meer informatie [uitvoeren van de toepassing met behulp van een service-principal](#running-the-application-using-a-service-principal).

Zodra u zich hebt aangemeld bij Azure, `AzureServiceTokenProvider` maakt gebruik van de service-principal voor het ophalen van een token voor lokale ontwikkeling.

Dit geldt alleen voor lokale ontwikkeling. Wanneer uw oplossing is geïmplementeerd in Azure, schakelt u de bibliotheek met MSI-verificatie.

<a name="msi"></a>
## <a name="running-the-application-using-a-managed-service-identity"></a>Uitvoeren van de toepassing met behulp van een beheerde Service-identiteit 

Wanneer u de code op een Azure App Service- of een Azure-VM met MSI ingeschakeld uitvoeren, wordt in de bibliotheek automatisch beheerd Service-identiteit gebruikt. Er zijn geen codewijzigingen vereist. 


<a name="sp"></a>
## <a name="running-the-application-using-a-service-principal"></a>Uitvoeren van de toepassing met behulp van een Service-Principal 

Het is mogelijk nodig is voor het maken van een Azure AD-clientreferenties om te verifiëren. Algemene voorbeelden zijn:

1. Uw code wordt uitgevoerd op een lokale ontwikkelingsomgeving, maar niet onder de identiteit van de ontwikkelaar.  Service Fabric gebruikt bijvoorbeeld de [netwerkserviceaccount](/azure/service-fabric/service-fabric-application-secret-management) voor lokale ontwikkeling.
 
2. Uw code wordt uitgevoerd op een lokale ontwikkelingsomgeving en u aanmelden bij een aangepaste service, zodat u uw identiteit developer niet gebruiken. 
 
3. Uw code wordt uitgevoerd op een Azure berekeningsresource die beheerde Service-identiteit, zoals Azure Batch nog niet ondersteunt.

Een certificaat gebruiken voor aanmelding bij Azure AD:

1. Maak een [principal servicecertificaat](/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

2. Implementeren van het certificaat op ofwel de _LocalMachine_ of _CurrentUser_ opslaan. 

3. Instellen van een omgevingsvariabele **AzureServicesAuthConnectionString** naar:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={LocalMachine or CurrentUser}.
    ```
 
    Vervang _{AppId}_, _{TenantId}_, en _{Thumbprint}_ met waarden die zijn gegenereerd in stap 1.

    **CertificateStoreLocation** moet _CurrentUser_ of _LocalMachine_, op basis van uw implementatieplan.

4. Voer de toepassing uit. 

Om aan te melden met behulp van een Azure AD de gedeelde geheime referentie:

1. Maak een [service-principal met een wachtwoord](/azure/azure-resource-manager/resource-group-authenticate-service-principal) en het toegang geven tot de Sleutelkluis. 

2. Instellen van een omgevingsvariabele **AzureServicesAuthConnectionString** naar:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}. 
    ```

    Vervang _{AppId}_, _{TenantId}_, en _{ClientSecret}_ met waarden die zijn gegenereerd in stap 1.

3. Voer de toepassing uit. 

Zodra u alles correct geen verdere codewijzigingen ingesteld, zijn vereist.  `AzureServiceTokenProvider`de omgevingsvariabele en het certificaat voor verificatie met Azure AD gebruikt. 

<a name="connectionstrings"></a>
## <a name="connection-string-support"></a>Ondersteuning voor verbindingen tekenreeks

Standaard `AzureServiceTokenProvider` maakt gebruik van meerdere methoden voor het ophalen van een token. 

Als het proces wilt aanpassen, gebruikt u een verbindingsreeks die is doorgegeven aan de `AzureServiceTokenProvider` constructor of opgegeven in de *AzureServicesAuthConnectionString* omgevingsvariabele. 

De volgende opties worden ondersteund:

| Verbinding&nbsp;tekenreeks&nbsp;optie | Scenario | Opmerkingen|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van AzureCli-token ophalen. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van Visual Studio-token ophalen. |
| `RunAs=CurrentUser;` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van Azure AD Integrated Authentication-token ophalen. |
| `RunAs=App;` | Beheerde service-identiteit | AzureServiceTokenProvider maakt gebruik van de Service-identiteit beheerd-token ophalen. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint`<br>`   ={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`  | Service-principal | `AzureServiceTokenProvider`maakt gebruik van certificaten van Azure AD-token ophalen. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};`<br>`   CertificateSubjectName={Subject};CertificateStoreLocation=`<br>`   {LocalMachine or CurrentUser}` | Service-principal | `AzureServiceTokenProvider`certificaat wordt gebruikt vanuit Azure AD-token ophalen|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Service-principal |`AzureServiceTokenProvider`maakt gebruik van geheim van Azure AD-token ophalen. |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beheerde Service-identiteit](/azure/app-service/app-service-managed-service-identity).

- Meer informatie over verschillende manieren om te [verifiëren en autoriseren van apps](/azure/app-service/app-service-authentication-overview).

- Meer informatie over Azure AD [verificatiescenario's beschreven](/azure/active-directory/develop/active-directory-authentication-scenarios#web-browser-to-web-application).