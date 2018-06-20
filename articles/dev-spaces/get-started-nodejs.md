---
title: Een Kubernetes Node.js-ontwikkelomgeving maken in de cloud met VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 0507208e58323fd31bb7c6cdb3a293ec0179cabe
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823908"
---
# <a name="get-started-on-azure-dev-spaces-with-nodejs"></a>Aan de slag in Azure Dev Spaces met behulp van Node.js

[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

U kunt nu een op Kubernetes gebaseerde ontwikkelomgeving maken in Azure.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Azure-CLI installeren
Azure Dev Spaces vereist minimale instellingen voor de lokale computer. De configuratie van uw ontwikkelomgeving wordt grotendeels opgeslagen in de cloud en kan worden gedeeld met andere gebruikers. Begin met het downloaden en uitvoeren van de [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

> [!IMPORTANT]
> Als u de Azure CLI al hebt geïnstalleerd, controleert u of u versie 2.0.33 of hoger gebruikt.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Terwijl u wacht totdat het cluster wordt gemaakt, kunt u beginnen met het schrijven van code.

## <a name="create-a-nodejs-container-in-kubernetes"></a>Een Node.js-container maken in Kubernetes

In deze sectie maakt u een Node.js-web-app en voert u deze uit in een container in Kubernetes.

### <a name="create-a-nodejs-web-app"></a>Een Node.js-web-app maken
Download code vanuit GitHub door naar https://github.com/Azure/dev-spaces te navigeren. Selecteer vervolgens **Clone or Download** om de GitHub-opslagplaats te downloaden naar de lokale omgeving. De code voor deze handleiding bevindt zich in `samples/nodejs/getting-started/webfrontend`.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Een inhoudsbestand bijwerken
Azure Dev Spaces draait niet alleen om het ophalen van code die wordt uitgevoerd in Kubernetes. Het gaat er om dat u de codewijzigingen snel en iteratief toegepast kunt zien in een Kubernetes-omgeving in de cloud.

1. Zoek het bestand `./public/index.html` en bewerk de HTML-code. Wijzig bijvoorbeeld de achtergrondkleur van de pagina in een blauwtint:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

2. Sla het bestand op. Enkele ogenblikken later ziet u in het terminalvenster een bericht met de melding dat een bestand in de actieve container is bijgewerkt.
1. Ga naar de browser en vernieuw de pagina. U ziet nu de bijgewerkte kleur.

Wat is er gebeurd? Voor bewerkingen van inhoudsbestanden, zoals HTML en CSS, is niet vereist dat het Node.js-proces opnieuw wordt opgestart. Door een actieve `azds up`-opdracht worden gewijzigde inhoudsbestanden rechtstreeks automatisch gesynchroniseerd in de actieve container in Azure. Zo kunt u de bewerkingen van uw inhoud snel zien.

### <a name="test-from-a-mobile-device"></a>Testen via een mobiel apparaat
Als u de web-app op een mobiel apparaat opent, ziet u dat de gebruikersinterface niet correct wordt weergegeven op een klein apparaat.

Om dit te verhelpen voegt u een `viewport`-metatag toe:
1. Open het bestand `./public/index.html`
1. Voeg een `viewport`-metatag toe aan het bestaande `head`-element:

    ```html
    <head>
        <!-- Add this line -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    ```

1. Sla het bestand op.
1. Vernieuw de browser op het apparaat. U ziet nu dat de web-app correct wordt weergegeven. 

Dit is een voorbeeld van hoe bepaalde problemen pas worden gevonden als u test op de apparaten waarop de app moet worden gebruikt. Met Azure Dev Spaces kunt u code snel opnieuw uitvoeren en eventuele wijzigingen op doelapparaten valideren.

### <a name="update-a-code-file"></a>Een codebestand bijwerken
Het bijwerken van codebestanden aan de serverzijde vereist iets meer werk, omdat een Node.js-app opnieuw moet worden opgestart.

1. Druk in het terminalvenster op `Ctrl+C` (om `azds up` te stoppen).
1. Open het codebestand met de naam `server.js` en bewerk het hallo-bericht van de service: 

    ```javascript
    res.send('Hello from webfrontend running in Azure!');
    ```

3. Sla het bestand op.
1. Voer `azds up` uit in het terminalvenster. 

Hiermee wordt de installatiekopie van de container opnieuw gebouwd en het Helm-diagram opnieuw geïmplementeerd. Laad de browserpagina om de codewijzigingen door te voeren.

Er bestaat echter een nog *snellere methode* voor het ontwikkelen van code. Deze methode gaat u in de volgende sectie verkennen. 

## <a name="debug-a-container-in-kubernetes"></a>Fouten opsporen in een Kubernetes-container

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]

### <a name="select-the-azds-debug-configuration"></a>Selecteer de AZDS-foutopsporingsconfiguratie
1. Om de foutopsporingsweergave te openen, klikt u op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code.
1. Selecteer **Launch Program (AZDS)** als de actieve foutopsporingsconfiguratie.

![](media/get-started-node/debug-configuration-nodejs.png)

> [!Note]
> Als u geen Azure Dev Spaces-opdrachten ziet in het Opdrachtenpalet, controleert u of u [de VS Code-extensie voor Azure Dev Spaces hebt geïnstalleerd](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code).

### <a name="debug-the-container-in-kubernetes"></a>Fouten opsporen in de container in Kubernetes
Druk op **F5** om fouten in uw code in Kubernetes op te sporen.

Net als bij de opdracht `up` wordt de code gesynchroniseerd met de ontwikkelomgeving wanneer u foutopsporing start en wordt een container gemaakt en geïmplementeerd in Kubernetes. Op dit moment is het foutopsporingsprogramma gekoppeld aan de externe container.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Stel een onderbrekingspunt in een codebestand aan de serverzijde in, bijvoorbeeld binnen de `app.get('/api'...` in `server.js`. Vernieuw de browserpagina of druk op de knop Say It Again. U komt dan op het onderbrekingspunt terecht en kunt de code doorlopen.

U hebt volledige toegang tot foutopsporingsgegevens, net alsof de code lokaal wordt uitgevoerd. Denk hierbij aan de aanroep-stack, lokale variabelen en informatie over uitzonderingen, enzovoort.

### <a name="edit-code-and-refresh-the-debug-session"></a>Code bewerken en de foutopsporingssessie vernieuwen
Breng met het actieve foutopsporingsprogramma een codewijziging aan. Wijzig bijvoorbeeld het hallo-bericht opnieuw:

```javascript
app.get('/api', function (req, res) {
    res.send('**** Hello from webfrontend running in Azure! ****');
});
```

Sla het bestand op en klik in het deelvenster **Debug actions** op de knop **Refresh**. 

![](media/get-started-node/debug-action-refresh-nodejs.png)

In plaats van telkens wanneer codewijzigingen zijn aangebracht een nieuwe containerinstallatiekopie te bouwen en opnieuw te implementeren, wat meestal veel tijd kost, wordt het Node.js-proces tussen foutopsporingssessies opnieuw opgestart in Azure Dev Spaces voor een snellere bewerkings-/foutopsporingslus.

Vernieuw de web-app in de browser of druk op de knop *Say It Again*. U ziet dat uw aangepaste bericht wordt weergegeven in de gebruikersinterface.

### <a name="use-nodemon-to-develop-even-faster"></a>NodeMon gebruiken om nog sneller te ontwikkelen
*Nodemon* is een populair hulpprogramma dat Node.js-ontwikkelaars gebruiken voor snelle ontwikkeling. In plaats van het Node-proces handmatig opnieuw te starten telkens als er een wijziging aan de serverzijde is doorgevoerd, configureren ontwikkelaars hun Node-project vaak om bestandswijzigingen te laten controleren met *nodemon* en het serverproces automatisch opnieuw te starten. Bij deze manier van werken vernieuwt de ontwikkelaar alleen de browser nadat een codewijziging is aangebracht.

Met Azure Dev Spaces kunt u veel dezelfde ontwikkelwerkstromen gebruiken die u ook gebruikt wanneer u lokaal ontwikkelt. Ter illustratie hiervan is het `webfrontend`-voorbeeldproject geconfigureerd voor gebruik van *nodemon* (het is geconfigureerd als een apparaatafhankelijkheid in `package.json`).

Voer de volgende stappen uit:
1. Stop het VS Code-foutopsporingsprogramma.
1. Klik op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code. 
1. Selecteer **Attach (AZDS)** als de actieve foutopsporingsconfiguratie.
1. Druk op F5.

In deze configuratie is de container geconfigureerd voor het starten van *nodemon*. Wanneer servercodewijzigingen worden doorgevoerd, wordt met *nodemon* het Node-proces automatisch opnieuw opgestart, net zoals wanneer u het lokaal ontwikkelt. 
1. Bewerk het hallo-bericht opnieuw in `server.js` en sla het bestand op.
1. Vernieuw de browser of klik op de knop *Say It Again* om de wijzigingen door te voeren.

**U beschikt nu over een methode om code snel te ontwikkelen en foutopsporing rechtstreeks uit te voeren in Kubernetes.** Hierna ziet u hoe u een tweede container kunt maken en aanroepen.

## <a name="call-a-service-running-in-a-separate-container"></a>Een service aanroepen die wordt uitgevoerd in een afzonderlijke container

In deze sectie maakt u een tweede service, `mywebapi`, en laat u deze aanroepen in `webfrontend`. Elke service wordt uitgevoerd in een afzonderlijke container. Vervolgens spoort u fouten op in beide containers.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Voorbeeldcode openen voor *mywebapi*
U beschikt al over voorbeeldcode voor `mywebapi` voor deze handleiding in een map met de naam `samples`. (Zo niet, dan gaat u naar https://github.com/Azure/dev-spaces en selecteert u **Clone or Download** om de GitHub-opslagplaats te downloaden.) De code voor deze sectie bevindt zich in `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>*mywebapi* uitvoeren
1. Open de map `mywebapi` in een *afzonderlijk VS Code-venster*.
1. Druk op F5 en wacht tot de service is gebouwd en geïmplementeerd. Dit proces is voltooid, zodra de VS Code-balk voor foutopsporing wordt weergegeven.
1. Noteer de URL van het eindpunt. Deze ziet er ongeveer als volgt uit: http://localhost:\<portnumber\>. **Tip: op de VS Code-statusbalk wordt een klikbare URL weergegeven.** Het lijkt misschien alsof de container lokaal wordt uitgevoerd, maar dat is niet zo. De container wordt uitgevoerd in de ontwikkelomgeving in Azure. Het localhost-adres is te zien omdat er nog geen openbare eindpunten zijn gedefinieerd in `mywebapi` en toegang daarom alleen mogelijk is binnen het Kubernetes-exemplaar. Voor uw gemak en om interactie met de privésessie mogelijk te maken vanaf de lokale computer wordt in Azure Dev Spaces een tijdelijke SSH-tunnel gemaakt naar de container die wordt uitgevoerd in Azure.
1. Wanneer `mywebapi` klaar is, opent u de browser naar het localhost-adres. U ziet nu een reactie van de `mywebapi`-service. (Hello from mywebapi).


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Verzend een aanvraag van *webfrontend* naar *mywebapi*
Nu gaan we code schrijven in `webfrontend` waarmee een aanvraag wordt verzonden naar `mywebapi`.
1. Schakel over naar het VS Code-venster voor `webfrontend`.
1. Voeg deze coderegels toe boven aan `server.js`:
    ```javascript
    var request = require('request');
    ```

3. *Vervang* de code door de `/api`-GET-handler. Tijdens het verwerken van een aanvraag, wordt vervolgens een aanroep verzonden naar `mywebapi` en worden de resultaten van beide services geretourneerd.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```

In het vorige codevoorbeeld wordt de `azds-route-as`-header van de binnenkomende aanvraag doorgestuurd naar de uitgaande aanvraag. Later ziet u hoe dit teamleden helpt om samen te ontwikkelen.

### <a name="debug-across-multiple-services"></a>Foutopsporing in meerdere services
1. Op dit punt moet `mywebapi` nog steeds worden uitgevoerd met het bijgevoegde foutopsporingsprogramma. Als dit niet het geval is, drukt u op F5 in het `mywebapi`-project.
1. Stel een onderbrekingspunt in de standaard-GET `/`-handler in.
1. Stel in het `webfrontend`-project een onderbrekingspunt in vlak voordat een GET-aanvraag wordt verzonden naar `http://mywebapi`.
1. Druk op F5 in het `webfrontend`-project.
1. Open de web-app en analyseer de code in beide services. Als het goed is, wordt nu een bericht weergegeven dat is samengesteld uit de twee services: Hello from webfrontend en Hello from mywebapi.

Dat is dus gelukt. U hebt nu een toepassing met meerdere containers waarin elke container afzonderlijk kan worden ontwikkeld en geïmplementeerd.

## <a name="learn-about-team-development"></a>Meer informatie over teamontwikkeling

[!INCLUDE[](includes/team-development-1.md)]

Zo ziet het er in de praktijk uit:
1. Ga naar het VS Code-venster voor `mywebapi` en wijzig de code van de standaard-GET `/`-handler, bijvoorbeeld:

    ```javascript
    app.get('/', function (req, res) {
        res.send('mywebapi now says something new');
    });
    ```

[!INCLUDE[](includes/team-development-2.md)]

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]




