---
title: Aan de slag met de Azure CDN-SDK voor Node.js | Microsoft Docs
description: Informatie over het schrijven van Node.js-toepassingen in Azure CDN beheren.
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 70bae1558860b763d17d04e10d5d926b39300101
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321242"
---
# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN-ontwikkeling
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

U kunt de [Azure CDN SDK voor Node.js](https://www.npmjs.com/package/azure-arm-cdn) voor het automatiseren van het maken en beheren van CDN-profielen en -eindpunten.  Deze zelfstudie leidt u door het maken van een eenvoudige Node.js-consoletoepassing die ziet u enkele van de beschikbare bewerkingen.  In deze zelfstudie is niet bedoeld om alle aspecten van de Azure CDN-SDK voor Node.js in detail beschreven.

Voor deze zelfstudie, moet u al hebben [Node.js](http://www.nodejs.org) **4.x.x** of hoger is geïnstalleerd en geconfigureerd.  U kunt een teksteditor die u wilt maken van uw Node.js-toepassing gebruiken.  Voor het schrijven van deze zelfstudie, ik gebruikt [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> De [voltooid project uit deze zelfstudie](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is beschikbaar voor downloaden op MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Maken van uw project en NPM-afhankelijkheden toevoegen
Nu dat we hebben een resourcegroep gemaakt voor onze CDN-profielen en toestemming van onze Azure AD-toepassing voor het beheren van CDN-profielen en eindpunten in die groep, kunnen we beginnen met het maken van onze toepassing.

Maak een map voor het opslaan van uw toepassing.  Vanaf de console met de Node.js-hulpprogramma's in het huidige pad, instellen van uw huidige locatie naar deze nieuwe map en het initialiseren van uw project door uit te voeren:

    npm init

U wordt vervolgens weergegeven een reeks vragen stelt uw project initialiseren.  Voor **toegangspunt**, deze zelfstudie wordt gebruikgemaakt van *app.js*.  U kunt de andere opties in het volgende voorbeeld kunt zien.

![NPM init uitvoer](./media/cdn-app-dev-node/cdn-npm-init.png)

Dit project is nu wordt geïnitialiseerd met een *packages.json* bestand.  Dit project is gaan sommige Azure-bibliotheken die zijn opgenomen in de NPM-pakketten gebruiken.  We gebruiken de Runtime van de Client Azure voor Node.js (ms-rest-azure) en de Azure CDN-clientbibliotheek voor Node.js (azure-arm-cd).  Laten we deze toevoegen aan het project als afhankelijkheden.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Nadat u klaar bent met de pakketten installeert, de *package.json* bestand moet er ongeveer als volgt in dit voorbeeld (versie getallen kunnen afwijken):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Ten slotte uw teksteditor gebruiken, maak een leeg tekstbestand en sla deze in de hoofdmap van de projectmap als *app.js*.  We zijn nu gereed om te beginnen met het schrijven van code.

## <a name="requires-constants-authentication-and-structure"></a>Is vereist, constanten, verificatie en -structuur
Met *app.js* openen in de editor, laten we nu de basisstructuur van ons programma geschreven ophalen.

1. Voeg de 'vereist' voor de NPM-pakketten aan de bovenkant met het volgende:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. We moeten sommige constanten die onze methoden gebruikt definiëren.  Voeg het volgende toe.  Zorg ervoor dat u de plaatsaanduidingen vervangt, met inbegrip van de  **&lt;punthaken&gt;**, door uw eigen waarden indien nodig.
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Nu we exemplaar maken van de CDN-management-client en wijs hieraan de referenties.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Als u van verificatie van de individuele gebruiker gebruikmaakt, is deze twee regels er enigszins anders.
   
   > [!IMPORTANT]
   > Gebruik dit codevoorbeeld alleen als u ervoor kiest om de verificatie van de afzonderlijke gebruiker in plaats van een service-principal.  Zorg ervoor dat uw afzonderlijke gebruikersreferenties bewaken en houd ze geheim.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Zorg ervoor dat u het vervangen van de items in **&lt;punthaken&gt;** met de juiste gegevens.  Voor `<redirect URI>`, gebruikt u de omleidings-URI die u hebt ingevoerd toen u de toepassing in Azure AD hebt geregistreerd.
4. Onze Node.js-consoletoepassing gaat duren voordat sommige opdrachtregelparameters.  Laten we valideren ten minste één parameter is doorgegeven.
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. Hiermee zijn we aan het belangrijkste onderdeel van ons programma, waar we vertakking uit andere functies op basis van welke parameters zijn doorgegeven.
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. Op verschillende plaatsen in ons programma moet u om te controleren of het juiste aantal parameters zijn doorgegeven en hulp weergegeven als ze niet bevallen.  We gaan maken functies om dit te doen.
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. Ten slotte worden de functies die worden gebruikt op de CDN-management-client zijn asynchroon, daarom moeten ze een methode om aan te roepen wanneer ze klaar bent.  Laten we dat kunnen weergeven van de uitvoer van de CDN management-client (indien aanwezig) en het programma zonder problemen afsluiten.
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Nu dat de basisstructuur van ons programma is geschreven, maken we de functies die worden aangeroepen op basis van onze parameters.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst met CDN-profielen en -eindpunten
Laten we beginnen met code om onze bestaande profielen en de eindpunten weer te geven.  Mijn codeopmerkingen bevatten de syntaxis van de verwachte zodat we weten waar elke parameter gaat.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profielen en -eindpunten maken
Vervolgens schrijft we de functies voor het maken van profielen en de eindpunten.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Een eindpunt leegmaken
Ervan uitgaande dat het eindpunt is gemaakt, is een veelvoorkomende taak die we willen kunnen uitvoeren in ons programma inhoud in ons eindpunt voor verwijderen.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profielen en -eindpunten verwijderen
De laatste functie we nemen Hiermee verwijdert u eindpunten en -profielen.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Het programma uitvoeren
We kunnen onze Node.js-programma met behulp van onze favoriete debugger nu uitvoeren of op de console.

> [!TIP]
> Als u Visual Studio Code als het foutopsporingsprogramma gebruikt, moet u het instellen van uw omgeving om door te geven in de opdrachtregelparameters.  Visual Studio Code gebeurt op de **launch.json** bestand.  Zoeken naar een eigenschap met de naam **argumenten** en toevoegen van een matrix van tekenreeksen voor de parameters, zodat het er ongeveer als volgt uitziet: `"args": ["list", "profiles"]`.
> 
> 

Laten we beginnen door onze profielen weer te geven.

![Lijst met profielen](./media/cdn-app-dev-node/cdn-list-profiles.png)

Wij weer een lege matrix.  Omdat we geen profielen in de resourcegroep, waarvan wordt verwacht.  We gaan nu een profiel maken.

![Profiel maken](./media/cdn-app-dev-node/cdn-create-profile.png)

Nu gaan we een eindpunt toevoegen.

![Eindpunt maken](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Ten slotte gaan we onze profiel verwijderen.

![Profiel verwijderen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Volgende stappen
Om te zien van het voltooide project van dit scenario [het voorbeeld downloaden](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Als u wilt zien de verwijzing voor de Azure CDN-SDK voor Node.js, geven de [verwijzing](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Voor aanvullende documentatie over de Azure SDK voor Node.js, geven de [volledige verwijzing](http://azure.github.io/azure-sdk-for-node/).

Beheer uw CDN-resources met [PowerShell](cdn-manage-powershell.md).

