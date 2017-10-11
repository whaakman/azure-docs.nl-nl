---
title: Aan de slag met de Azure CDN-SDK voor Node.js | Microsoft Docs
description: Informatie over het schrijven van Node.js-toepassingen voor het beheren van Azure CDN.
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 46ae8cd9775432d126cbde856c1fb06ea319297e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN-ontwikkeling
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

U kunt de [Azure CDN-SDK voor Node.js](https://www.npmjs.com/package/azure-arm-cdn) te maken en beheren van CDN-profielen en eindpunten te automatiseren.  Deze zelfstudie helpt bij het maken van een eenvoudige Node.js-consoletoepassing die u laat zien dat verschillende van de beschikbare bewerkingen.  Deze zelfstudie is niet bedoeld voor alle aspecten van de Azure CDN-SDK voor Node.js in detail beschrijven.

Voor deze zelfstudie hebt voltooid, moet u al hebben [Node.js](http://www.nodejs.org) **4.x.x** of hoger geïnstalleerd en geconfigureerd.  U kunt een teksteditor die u wilt maken van uw Node.js-toepassing kunt gebruiken.  Voor het schrijven van deze zelfstudie gebruikt ik [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> De [voltooid project uit deze zelfstudie](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is beschikbaar voor downloaden op MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Maken van uw project en NPM afhankelijkheden toevoegen
Nu dat we hebben een resourcegroep gemaakt voor onze CDN-profielen en toestemming van onze Azure AD-toepassing voor het beheren van CDN-profielen en eindpunten binnen die groep, kunnen we beginnen met het maken van de toepassing.

Maak een map voor het opslaan van uw toepassing.  Vanuit een console met de Node.js-hulpprogramma's in uw huidige pad instellen van uw huidige locatie naar deze map en het initialiseren van uw project door te voeren:

    npm init

U wordt weergegeven een reeks vragen om uw project te initialiseren.  Voor **toegangspunt**, deze zelfstudie wordt gebruikgemaakt van *app.js*.  U kunt mijn andere opties in het volgende voorbeeld kunt zien.

![NPM init-uitvoer](./media/cdn-app-dev-node/cdn-npm-init.png)

Onze project nu is geïnitialiseerd met een *packages.json* bestand.  Onze project gaat een aantal Azure-bibliotheken die zijn opgenomen in de NPM-pakketten gebruiken.  We gebruiken de Azure-Client-Runtime voor Node.js (ms-rest-azure) en de Azure CDN-clientbibliotheek voor Node.js (arm-azure-cd).  Laten we deze toevoegen aan het project als afhankelijkheden.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Nadat u klaar bent met de pakketten installeren, de *package.json* bestand zijn vergelijkbaar met het volgende voorbeeld (versie getallen kunnen verschillen):

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

Ten slotte met uw teksteditor een leeg tekstbestand maken en opslaan in de hoofdmap van onze projectmap als *app.js*.  We zijn nu gereed om te beginnen met het schrijven van code.

## <a name="requires-constants-authentication-and-structure"></a>Vereist, constanten, verificatie en -structuur
Met *app.js* openen in onze editor, gaan we aan de basisstructuur van onze programma dat is geschreven.

1. Voeg de 'vereist' voor onze NPM pakketten aan de bovenkant met de volgende opties:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. We moeten sommige constanten die onze methoden gebruikt definiëren.  Voeg het volgende toe.  Zorg ervoor dat u de tijdelijke aanduidingen, met inbegrip van de  **&lt;punthaken&gt;**, met uw eigen waarden zo nodig.
   
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
3. Vervolgens we exemplaar maken van de CDN-management-client en wijs hieraan onze referenties.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Als u afzonderlijke gebruikersverificatie gebruikt, is deze twee regels er iets anders.
   
   > [!IMPORTANT]
   > Dit voorbeeld alleen gebruiken als u ervoor kiest om de verificatie van afzonderlijke gebruikers in plaats van een service-principal.  Zorg ervoor dat uw afzonderlijke gebruikersreferenties bewaken en geheime houden.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Zorg ervoor dat u de items in  **&lt;punthaken&gt;**  met de juiste gegevens.  Voor `<redirect URI>`, gebruikt u de omleidings-URI die u hebt opgegeven bij de registratie van de toepassing in Azure AD.
4. Onze Node.js-consoletoepassing zal duren voordat sommige opdrachtregelparameters.  Laten we valideren ten minste één parameter is doorgegeven.
   
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
5. Ons heeft die in het hoofdgedeelte van het programma, waar we vertakking uit naar andere functies op basis van welke parameters zijn doorgegeven.
   
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
6. Op verschillende plaatsen in onze programma moet u om te controleren of het juiste aantal parameters zijn doorgegeven en hulp weergegeven als ze niet juist zoeken.  Laten we functies hiertoe maken.
   
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
7. Tot slot worden de functies die worden gebruikt op de CDN-management-client zijn asynchroon, daarom ze een methode aan te roepen moeten wanneer ze klaar.  Zorg dat u kunt de uitvoer van de CDN management-client (indien aanwezig) weergeven en het programma afgesloten.
   
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

Nu dat de basisstructuur van het programma is geschreven, maken we de functies die worden aangeroepen op basis van onze parameters.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst met CDN-profielen en -eindpunten
U begint met de code voor een lijst met onze bestaande profielen en -eindpunten.  Mijn codeopmerkingen beschikt u over de syntaxis van de verwachte zodat we weten waar elke parameter gaat.

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

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten maken
We schrijft vervolgens de functies voor het maken van profielen en -eindpunten.

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
Ervan uitgaande dat het eindpunt is gemaakt, is een algemene taak die we wilt uitvoeren in onze programma inhoud in onze eindpunt opschonen.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten verwijderen
De laatste functie opgeroepen we nemen verwijdert eindpunten en -profielen.

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
We ons gebruik van onze favoriete debugger Node.js-programma kan nu worden uitgevoerd of bij de console.

> [!TIP]
> Als u Visual Studio Code als uw debugger gebruikt, moet u uw omgeving instellen om op te geven de opdrachtregelparameters.  Visual Studio Code doet dit de **lanuch.json** bestand.  Zoeken naar een eigenschap met de naam **args** en toevoegen van een matrix van tekenreekswaarden voor uw-parameters, zodat het er ongeveer als volgt uitziet: `"args": ["list", "profiles"]`.
> 
> 

Laten we beginnen met een overzicht van onze profielen.

![Lijst met profielen](./media/cdn-app-dev-node/cdn-list-profiles.png)

Wij terug lege matrix.  Aangezien er zijn momenteel geen profielen in de resourcegroep, waarvan wordt verwacht.  We gaan nu een profiel maken.

![Profiel maken](./media/cdn-app-dev-node/cdn-create-profile.png)

Nu gaan we een eindpunt toevoegen.

![Eindpunt maken](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Tot slot gaan we onze profiel verwijderen.

![Profiel verwijderen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Volgende stappen
Om te zien van de voltooide project van dit scenario [het voorbeeld downloaden](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Geef weer voor de verwijzing voor de Azure CDN-SDK voor Node.js de [verwijzing](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Aanvullende documentatie over de Azure SDK voor Node.js vindt geven de [volledige verwijzing](http://azure.github.io/azure-sdk-for-node/).

Uw CDN-resources beheren met [PowerShell](cdn-manage-powershell.md).

