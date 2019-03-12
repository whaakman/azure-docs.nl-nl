---
title: Node.js-toepassing met behulp van Socket.io - Azure
description: Informatie over het gebruik van socket.io in een node.js-toepassing die wordt gehost op Azure.
services: cloud-services
documentationcenter: nodejs
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: jeconnoc
ms.openlocfilehash: 6a45d6366ef3c581c00b084b7ea2e4095eaaba3f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57538116"
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Een Node.js-chattoepassing met Socket.IO bouwen op een Azure Cloud Service

Socket.IO biedt realtime communicatie tussen uw node.js-server en clients. In deze zelfstudie helpt u bij het hosten van een socket. I/o-chattoepassing in Azure. Zie voor meer informatie over Socket.IO [socket.io](https://socket.io).

Een schermafbeelding van de voltooide toepassing lager is dan:

![Een browservenster met de service die wordt gehost op Azure][completed-app]  

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende producten en versies zijn geïnstalleerd om te kunnen voltooien in het voorbeeld in dit artikel:

* [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) installeren
* [Node.js](https://nodejs.org/download/) installeren
* Installeer [Python versie 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Een Cloud Service-Project maken
De volgende stappen maakt u het cloudserviceproject die als voor de toepassing Socket.IO host.

1. Uit de **startmenu** of **startscherm**, zoeken naar **Windows PowerShell**. Ten slotte, met de rechtermuisknop op **Windows PowerShell** en selecteer **als Administrator uitvoeren**.
   
    ![Azure PowerShell-pictogram][powershell-menu]
2. Maak een map genaamd **c:\\knooppunt**. 
   
        PS C:\> md node
3. Wijzig de mappen op de **c:\\knooppunt** directory
   
        PS C:\> cd node
4. Voer de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **chatapp** en een werkrol met de naam **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Hier ziet u het volgende antwoord:
   
    ![De uitvoer van de nieuwe-azureservice en voegen azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>De Chat-voorbeeld downloaden
Voor dit project, gebruiken we de chat-voorbeeld uit de [Socket.IO GitHub-opslagplaats]. Voer de volgende stappen uit om te downloaden van het voorbeeld en toe te voegen aan het project dat u eerder hebt gemaakt.

1. Maken van een lokale kopie van de opslagplaats met behulp van de **kloon** knop. U kunt ook de **ZIP** knop voor het downloaden van het project.
   
   ![De weergave van een browser venster https://github.com/LearnBoost/socket.io/tree/master/examples/chat, met het pictogram voor het downloaden van ZIP gemarkeerd][chat-example-view]
2. De mapstructuur van de lokale opslagplaats navigeren totdat er uitziet de **voorbeelden\\chat** directory. Kopieer de inhoud van deze map op de **C:\\knooppunt\\chatapp\\WorkerRole1** directory eerder hebt gemaakt.
   
   ![Verkenner weergeven van de inhoud van de voorbeelden\\chat directory geëxtraheerd uit het archief][chat-contents]
   
   De gemarkeerde items in de bovenstaande schermafbeelding zijn de bestanden die zijn gekopieerd uit de **voorbeelden\\chat** directory
3. In de **C:\\knooppunt\\chatapp\\WorkerRole1** directory, verwijdert de **server.js** bestand en wijzig de naam van de **app.js** het bestand in **server.js**. Hiermee verwijdert u de standaard **server.js** bestand dat eerder is gemaakt door de **toevoegen AzureNodeWorkerRole** cmdlet en vervangt deze door het bestand van de toepassing uit het voorbeeld chat.

### <a name="modify-serverjs-and-install-modules"></a>Server.js wijzigen en -Modules installeren
Voordat u de toepassing test in de Azure-emulator, moeten we enkele kleine wijzigingen aanbrengen. Voer de volgende stappen uit in het server.js-bestand:

1. Open de **server.js** bestand in Visual Studio of in een teksteditor.
2. Zoek de **Module-afhankelijkheden** sectie aan het begin van server.js en wijzig de regel die **sio require('.. = //.. lib//socket.IO')** naar **sio require('socket.io') =** zoals hieronder wordt weergegeven:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. Om te controleren of de toepassing luistert op de juiste poort, server.js opent in Kladblok of uw favoriete editor en wijzigt u de volgende regel te vervangen **3000** met **process.env.port** zoals hieronder wordt weergegeven:
   
       App.Listen (3000, functie () {//Original       app.listen (process.env.port, functie () {//Updated var adres = app.address();       console.log (' app luisteren op http://' + addr.address + ":" + addr.port);     });

Na het opslaan van de wijzigingen in **server.js**, gebruikt u de volgende stappen uit om vereiste modules te installeren en vervolgens de toepassing testen in de Azure-emulator:

1. Met behulp van **Azure PowerShell**, wijzig de mappen op de **C:\\knooppunt\\chatapp\\WorkerRole1** directory en gebruik de volgende opdracht uit om de modules te installeren vereist voor deze toepassing:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Hiermee installeert u de modules die worden vermeld in het package.json-bestand. Nadat de opdracht is voltooid, ziet u uitvoer die vergelijkbaar is met het volgende:
   
   ![De uitvoer van de npm-installatieopdracht][The-output-of-the-npm-install-command]
2. Omdat in dit voorbeeld oorspronkelijk een deel van de Socket.IO GitHub-opslagplaats is en rechtstreeks verwijst naar de bibliotheek Socket.IO door relatief pad, is niet Socket.IO verwezen in het package.json-bestand, zodat we moet worden geïnstalleerd door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testen en implementeren
1. Start de emulator door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Als u problemen met het starten van emulator, bijv.:      Start-AzureEmulator : Er is een onverwachte fout opgetreden.  Details: Er is een onverwachte fout objekt komunikace, System.ServiceModel.Channels.ServiceChannel, kan niet worden gebruikt voor communicatie, omdat deze zich in de status van de Faulted.
   
      Installeer AzureAuthoringTools v 2.7.1 en AzureComputeEmulator v 2.7: Zorg ervoor dat versie overeenkomt met.
   >
   >


2. Open een browser en navigeer naar **http://127.0.0.1**.
3. Als het browservenster wordt geopend, voer een bijnaam in en klik vervolgens op ENTER drukken.
   Hiermee kunt u berichten plaatsen als een specifieke bijnaam. Test de functionaliteit voor meerdere gebruikers, opent u extra browservensters met dezelfde URL en voer verschillende bijnamen.
   
   ![Twee browservensters chatberichten van Gebruiker1 en Gebruiker2 weergeven](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Na het testen van de toepassing, stopt u de emulator door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. Voor het implementeren van de toepassing in Azure, gebruikt u de **Publish-AzureServiceProject** cmdlet. Bijvoorbeeld:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Zorg ervoor dat u een unieke naam zijn, anders wordt het publicatieproces. Nadat de implementatie is voltooid, wordt de browser opent en navigeert u naar de geïmplementeerde service.
   > 
   > Als u een foutmelding waarin staat dat de naam van het opgegeven abonnement niet in de geïmporteerde publicatieprofiel bestaat ontvangt, moet u het downloaden en importeren van het publicatieprofiel voor uw abonnement voordat u Azure implementeert. Zie de **implementeren van de toepassing naar Azure** sectie van [bouwen en implementeren van een Node.js-toepassing in een Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Een browservenster met de service die wordt gehost op Azure][completed-app]
   
   > [!NOTE]
   > Als u een foutmelding waarin staat dat de naam van het opgegeven abonnement niet in de geïmporteerde publicatieprofiel bestaat ontvangt, moet u het downloaden en importeren van het publicatieprofiel voor uw abonnement voordat u Azure implementeert. Zie de **implementeren van de toepassing naar Azure** sectie van [bouwen en implementeren van een Node.js-toepassing in een Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Uw toepassing wordt nu uitgevoerd in Azure en kan chatberichten tussen verschillende clients met behulp van Socket.IO doorsturen.

> [!NOTE]
> Dit voorbeeld is beperkt tot chatten tussen gebruikers die zijn verbonden met hetzelfde exemplaar voor het gemak. Dit betekent dat als de cloudservice twee instanties van de werkrol maakt, gebruikers worden pas om te chatten met anderen die zijn verbonden met de dezelfde instantie van de worker-rol. U kunt voor het schalen van de toepassing om te werken met meerdere rolexemplaren, een technologie, zoals Service Bus gebruiken voor het delen van de status van de store Socket.IO over instanties. Zie voor voorbeelden van de Service Bus-wachtrijen en onderwerpen gebruik voorbeelden in de [Azure SDK voor Node.js-GitHub-opslagplaats](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd over het maken van een eenvoudige chattoepassing die wordt gehost in een Azure-Cloudservice. Zie voor meer informatie over deze toepassing in een Azure-Website te hosten, [een Node.js-chattoepassing met Socket.IO op een Azure-website bouwen][chatwebsite].

Zie voor meer informatie, ook de [Node.js-ontwikkelaarscentrum](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest).

[chatwebsite]: https://docs.microsoft.com/azure/cloud-services/cloud-services-nodejs-develop-deploy-app

[Azure SLA]: https://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub-opslagplaats]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


