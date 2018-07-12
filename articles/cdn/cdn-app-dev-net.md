---
title: Aan de slag met de Azure CDN-bibliotheek voor .NET | Microsoft Docs
description: Informatie over het schrijven van .NET-toepassingen voor het beheren van Azure CDN met Visual Studio.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5379586355ece98af6295236d6cbd09cb31c742b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38454461"
---
# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN-ontwikkeling
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

U kunt de [Azure CDN-bibliotheek voor .NET](https://msdn.microsoft.com/library/mt657769.aspx) voor het automatiseren van het maken en beheren van CDN-profielen en -eindpunten.  Deze zelfstudie leidt u door het maken van een eenvoudige .NET-consoletoepassing die ziet u enkele van de beschikbare bewerkingen.  In deze zelfstudie is niet bedoeld om alle aspecten van de Azure CDN-bibliotheek voor .NET in detail beschreven.

U moet Visual Studio 2015 om deze zelfstudie te voltooien.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is gratis te downloaden.

> [!TIP]
> De [voltooid project uit deze zelfstudie](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is beschikbaar voor downloaden op MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Maken van uw project en Nuget-pakketten toevoegen
Nu dat we hebben een resourcegroep gemaakt voor onze CDN-profielen en toestemming van onze Azure AD-toepassing voor het beheren van CDN-profielen en eindpunten in die groep, kunnen we beginnen met het maken van onze toepassing.

Klik in Visual Studio 2015, op **bestand**, **nieuw**, **Project...**  opent het dialoogvenster Nieuw project.  Vouw **Visual C#** en selecteer vervolgens **Windows** in het deelvenster aan de linkerkant.  Klik op **consoletoepassing** in het middelste deelvenster.  Geef uw project de naam en klik vervolgens op **OK**.  

![Nieuw project](./media/cdn-app-dev-net/cdn-new-project.png)

Dit project is gaan sommige Azure-bibliotheken die zijn opgenomen in de Nuget-pakketten gebruiken.  Laten we deze toevoegen aan het project.

1. Klik op de **extra** in het menu **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**.
   
    ![Nuget-pakketten beheren](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. In de Package Manager-Console, dan de volgende opdracht voor het installeren van de **Active Directory Authentication Library (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Voer het volgende uit voor het installeren van de **Azure CDN-Beheerbibliotheek**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Richtlijnen, constanten hoofdmethode en Help-methoden
Laten we nu de basisstructuur van ons programma geschreven ophalen.

1. Terug op het tabblad Program.cs vervangen de `using` richtlijnen aan de bovenkant door het volgende:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. We moeten sommige constanten die onze methoden gebruikt definiëren.  In de `Program` klasse, maar voordat de `Main` methode, voeg de volgende.  Zorg ervoor dat u de plaatsaanduidingen vervangt, met inbegrip van de  **&lt;punthaken&gt;**, door uw eigen waarden indien nodig.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Ook op het klasseniveau van de, deze twee variabelen definiëren.  We gebruiken deze later om te bepalen als onze profiel en -eindpunt al bestaan.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Vervang de `Main` methode als volgt te werk:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. Sommige van onze andere methoden gaan om te vragen van de gebruiker met "Ja/Nee" vragen.  Voeg de volgende methode die een beetje om eenvoudiger te maken:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nu dat de basisstructuur van ons programma is geschreven, maken we de methoden die worden aangeroepen door de `Main` methode.

## <a name="authentication"></a>Verificatie
Voordat we de Azure CDN-Beheerbibliotheek gebruiken kunt, moeten we onze service-principal verifiëren en verkrijgen van een verificatietoken.  Deze methode maakt gebruik van ADAL om op te halen van het token.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Als u van verificatie van de individuele gebruiker gebruikmaakt, de `GetAccessToken` methode wordt er iets anders uitzien.

> [!IMPORTANT]
> Gebruik dit codevoorbeeld alleen als u ervoor kiest om de verificatie van de afzonderlijke gebruiker in plaats van een service-principal.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Vervang `<redirect URI>` met de omleidings-URI die u hebt ingevoerd toen u de toepassing in Azure AD hebt geregistreerd.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst met CDN-profielen en -eindpunten
Nu we gaan CDN-bewerkingen uit te voeren.  Het eerste wat dat onze methode doet is de lijst met alle profielen en de eindpunten in de resourcegroep en als er een overeenkomst wordt gevonden voor het profiel en eindpunt namen opgegeven in onze constanten maakt een notitie van deze in voor later, zodat we niet probeert te maken van dubbele waarden.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profielen en -eindpunten maken
Vervolgens maken we een profiel.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Zodra het profiel is gemaakt, maken we een eindpunt.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> Het bovenstaande voorbeeld wijst u het eindpunt een oorsprong met de naam *Contoso* met een hostnaam `www.contoso.com`.  U moet dit om te verwijzen naar de hostnaam van uw eigen oorsprong wijzigen.
> 
> 

## <a name="purge-an-endpoint"></a>Een eindpunt leegmaken
Ervan uitgaande dat het eindpunt is gemaakt, is een veelvoorkomende taak die we in ons programma uitvoert de inhoud in ons eindpunt voor verwijderen.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> In het bovenstaande voorbeeld wordt de tekenreeks `/*` geeft aan dat ik wil alles in de hoofdmap van het eindpuntpad leegmaken.  Dit is gelijk aan het controleren van **alle opschonen** in de Azure-portal 'wissen' dialoogvenster. In de `CreateCdnProfile` methode, ik heb gemaakt onze profiel als een **Azure CDN van Verizon** profiel met de code `Sku = new Sku(SkuName.StandardVerizon)`, dus dit voltooid is.  Echter, **Azure CDN van Akamai** profielen bieden geen ondersteuning voor **alle opschonen**, dus als ik een Akamai-profiel is voor deze zelfstudie gebruikt, moet ik specifieke paden opgeven om te verwijderen.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profielen en -eindpunten verwijderen
De laatste methoden worden onze eindpunt en profiel verwijderd.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Het programma uitvoeren
We kunnen nu worden gecompileerd en voer het programma uit door te klikken op de **Start** knop in Visual Studio.

![Programma wordt uitgevoerd](./media/cdn-app-dev-net/cdn-program-running-1.png)

Wanneer het programma de bovenstaande vraag bereikt, zou het mogelijk om te retourneren aan de resourcegroep in Azure portal en Zie dat het profiel is gemaakt.

![Gelukt!](./media/cdn-app-dev-net/cdn-success.png)

We vervolgens de aanwijzingen voor het uitvoeren van de rest van het programma wilt controleren.

![Programma voltooid](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Volgende stappen
Om te zien van het voltooide project van dit scenario [het voorbeeld downloaden](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Ga voor aanvullende documentatie over Azure CDN Management Library voor .NET, weer de [verwijzingen op MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Beheer uw CDN-resources met [PowerShell](cdn-manage-powershell.md).

