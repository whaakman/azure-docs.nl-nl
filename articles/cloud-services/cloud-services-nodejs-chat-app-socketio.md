---
title: Node.js-toepassing met Socket.io - Azure
description: Informatie over het gebruik van socket.io in een node.js-toepassing die wordt gehost op Azure.
services: cloud-services
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 186cf5e22468b7abf58d6366ca0dec616be23cc6
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Het bouwen van een Node.js-chattoepassing met Socket.IO op een Azure Cloud Service

Socket.IO biedt realtime-communicatie tussen uw node.js-server en clients. Deze zelfstudie leert u die als host fungeert voor een socket. I/o gebaseerd chatprogramma op Azure. Zie voor meer informatie over Socket.IO [socket.io](http://socket.io).

Een schermopname van de voltooide toepassing lager is dan:

![Een browservenster met de service die wordt gehost op Azure][completed-app]  

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende producten en versies zijn geïnstalleerd om te voltooien in het voorbeeld in dit artikel:

* [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) installeren
* [Node.js](https://nodejs.org/download/) installeren
* Installeer [Python versie 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Een Cloud Service-Project maken
De volgende stappen uit maken het cloudserviceproject die als host voor de toepassing Socket.IO fungeert.

1. Van de **startmenu** of **startscherm**, zoeken naar **Windows PowerShell**. Ten slotte de met de rechtermuisknop op **Windows PowerShell** en selecteer **als Administrator uitvoeren**.
   
    ![Azure PowerShell-pictogram][powershell-menu]
2. Maak een map met de naam **c:\\knooppunt**. 
   
        PS C:\> md node
3. Wijzig de mappen op de **c:\\knooppunt** directory
   
        PS C:\> cd node
4. Voer de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **chatapp** en een werkrol toe met de naam **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Hier ziet u het volgende antwoord:
   
    ![De uitvoer van de nieuwe-azureservice en voeg azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>In het voorbeeld Chat downloaden
Voor dit project, gebruiken we het voorbeeld chat uit de [Socket.IO GitHub-opslagplaats]. Voer de volgende stappen uit om te downloaden van het voorbeeld en toe te voegen aan het project dat u eerder hebt gemaakt.

1. Maken van een lokale kopie van de opslagplaats via de **kloon** knop. U kunt ook de **ZIP** knop voor het downloaden van het project.
   
   ![Een browservenster https://github.com/LearnBoost/socket.io/tree/master/examples/chat, weergeven met het pictogram van het ZIP downloaden gemarkeerd][chat-example-view]
2. De mapstructuur van de lokale opslagplaats navigeren totdat u aankomt bij de **voorbeelden\\chat** directory. Kopieer de inhoud van deze map bij de **C:\\knooppunt\\chatapp\\WorkerRole1** directory eerder hebt gemaakt.
   
   ![Verkenner weergeven van de inhoud van de voorbeelden\\chat directory opgehaald uit het archief][chat-contents]
   
   De gemarkeerde items in de bovenstaande schermafbeelding zijn de bestanden die zijn gekopieerd uit de **voorbeelden\\chat** directory
3. In de **C:\\knooppunt\\chatapp\\WorkerRole1** directory, verwijdert de **server.js** bestand en wijzig de naam van de **app.js** het bestand in **server.js**. Hiermee verwijdert u de standaard **server.js** bestand dat eerder is gemaakt door de **toevoegen AzureNodeWorkerRole** cmdlet en vervangen door het bestand van de toepassing uit het chat-voorbeeld.

### <a name="modify-serverjs-and-install-modules"></a>Wijzigen van Server.js en -Modules installeren
Voordat u de toepassing test in de Azure-emulator, maken we een aantal kleine wijzigingen. Voer de volgende stappen voor het bestand server.js:

1. Open de **server.js** -bestand in Visual Studio of een teksteditor.
2. Zoek de **Module afhankelijkheden** sectie aan het begin van server.js en wijzigt u de regel die **sio require('.. = //.. lib//socket.IO')** naar **sio require('socket.io') =** zoals hieronder wordt weergegeven:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. Om te controleren of de toepassing luistert op de juiste poort, server.js geopend in Kladblok of uw favoriete editor en wijzig vervolgens de volgende regel door te vervangen **3000** met **process.env.port** zoals hieronder wordt weergegeven:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

Na het opslaan van de wijzigingen in **server.js**, gebruik de volgende stappen voor het installeren van vereiste modules en vervolgens de toepassing in de Azure-emulator te testen:

1. Met behulp van **Azure PowerShell**, wijzig de mappen op de **C:\\knooppunt\\chatapp\\WorkerRole1** directory en gebruik de volgende opdracht de modules installeren door deze toepassing vereist:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Hiermee installeert u de modules die worden vermeld in de package.json-bestand. Nadat u de opdracht is voltooid, ziet u uitvoer ziet er als volgt:
   
   ![De uitvoer van de npm-installatieopdracht][The-output-of-the-npm-install-command]
2. Aangezien dit voorbeeld oorspronkelijk een onderdeel van de Socket.IO GitHub-opslagplaats was en rechtstreeks verwijst naar de bibliotheek Socket.IO door relatief pad, Socket.IO is niet waarnaar wordt verwezen in de package.json-bestand, dus er moet worden geïnstalleerd door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testen en implementeren
1. Start de emulator door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Als u problemen ondervindt met bijvoorbeeld emulator te starten.: begin AzureEmulator: Er is een onverwachte fout opgetreden.  Worden details: Aangetroffen is een onverwachte fout het communicatieobject, System.ServiceModel.Channels.ServiceChannel, kan niet gebruikt voor communicatie omdat het de status Faulted heeft.
   
      Installeer AzureAuthoringTools v 2.7.1 en AzureComputeEmulator v 2.7: Zorg ervoor dat de versie die overeenkomt met.
   >
   >


2. Open een browser en Ga naar **http://127.0.0.1**.
3. Wanneer het browservenster wordt geopend, voer een bijnaam en klik vervolgens op ENTER drukken.
   Hierdoor kunt u om berichten te posten als een specifieke bijnaam. Test de functionaliteit voor meerdere gebruikers, extra browservensters met dezelfde URL te openen en voer verschillende bijnaam.
   
   ![Twee browservensters chatberichten van Gebruiker1 als Gebruiker2 weergeven](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Na de toepassing testen, stopt u de emulator door de volgende opdracht:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. Gebruik voor de implementatie van de toepassing in Azure, de **Publish-AzureServiceProject** cmdlet. Bijvoorbeeld:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Zorg ervoor dat u een unieke naam, anders mislukt de het publicatieproces. Nadat de implementatie is voltooid, wordt de browser openen en navigeer naar de geïmplementeerde service.
   > 
   > Als u een foutmelding weergegeven dat de naam van het opgegeven abonnement niet in de geïmporteerde publicatieprofiel bestaat ontvangt, moet u het downloaden en importeer het publicatieprofiel voor uw abonnement voordat u Azure implementeert. Zie de **implementeren van de toepassing in Azure** sectie van [bouwen en implementeren van een Node.js-toepassing naar een Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Een browservenster met de service die wordt gehost op Azure][completed-app]
   
   > [!NOTE]
   > Als u een foutmelding weergegeven dat de naam van het opgegeven abonnement niet in de geïmporteerde publicatieprofiel bestaat ontvangt, moet u het downloaden en importeer het publicatieprofiel voor uw abonnement voordat u Azure implementeert. Zie de **implementeren van de toepassing in Azure** sectie van [bouwen en implementeren van een Node.js-toepassing naar een Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Uw toepassing wordt nu uitgevoerd in Azure, en kan chatberichten tussen verschillende clients met Socket.IO doorsturen.

> [!NOTE]
> Dit voorbeeld is voor het gemak, beperkt tot chatten tussen gebruikers verbonden met hetzelfde exemplaar. Dit betekent dat als de cloudservice twee exemplaren van worker-rol maakt, gebruikers is alleen mogelijk om te chatten met anderen verbonden met dezelfde worker rolinstantie. Als u wilt schalen van de toepassing met meerdere exemplaren van de functie werkt, kunt u een technologie zoals Service Bus gebruiken voor het delen van de status van de store Socket.IO over exemplaren. Zie voor voorbeelden van de Service Bus-wachtrijen en onderwerpen gebruik voorbeelden in de [Azure SDK voor Node.js GitHub-opslagplaats](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd het maken van een eenvoudige chattoepassing die wordt gehost in een Azure Cloud Service. Zie voor meer informatie over deze toepassing in een Azure-Website te hosten, [een Node.js-chattoepassing met Socket.IO op een Azure-website bouwen][chatwebsite].

Zie voor meer informatie, ook de [Node.js Developer Center](/develop/nodejs/).

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
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


