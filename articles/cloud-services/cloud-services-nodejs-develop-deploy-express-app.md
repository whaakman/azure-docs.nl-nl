---
title: Bouw en implementeer een Node.js Express-app in Azure Cloud Services
description: Ontwikkel en implementeer een toepassing Express.js in Node.js op Azure Cloud Services
services: cloud-services
documentationcenter: nodejs
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: jeconnoc
ms.openlocfilehash: 260c63ed55b5cb2535567038f27626aa7a085550
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574603"
---
# <a name="build-and-deploy-a-nodejs-web-application-using-express-on-an-azure-cloud-services"></a>Bouw en implementeer een Node.js-webtoepassing met Express op een Azure Cloud Services

Node.js bevat een minimale set functionaliteit in de core-runtime.
Ontwikkelaars gebruiken vaak 3e partij modules bieden extra functionaliteit bij het ontwikkelen van een Node.js-toepassing. In deze zelfstudie maakt u een nieuwe toepassing met de [Express](https://github.com/expressjs/express) module waarmee een MVC-framework voor het maken van Node.js-webtoepassingen.

Een schermafbeelding van de voltooide toepassing lager is dan:

![Een webbrowser waarin Welkom in Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Een Cloud Service-Project maken
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Voer de volgende stappen uit voor het maken van een nieuwe cloud service-project met de naam 'expressapp':

1. Uit de **startmenu** of **startscherm**, zoeken naar **Windows PowerShell**. Ten slotte, met de rechtermuisknop op **Windows PowerShell** en selecteer **als Administrator uitvoeren**.
   
    ![Azure PowerShell-pictogram](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Wijzig de mappen op de **c:\\knooppunt** directory en voer de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **expressapp** en een Webrol die met de naam **WebRole1**:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Standaard **Add-AzureNodeWebRole** maakt gebruik van een oudere versie van Node.js. De **Set AzureServiceProjectRole** instructie hiervoor wordt gebruikt door Azure te gebruiken v0.10.21 van knooppunt.  Houd er rekening mee dat de parameters zijn hoofdlettergevoelig.  U kunt controleren of de juiste versie van Node.js is geselecteerd door het controleren van de **engines** eigenschap in **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Express installeren
1. Installeer de Express-generator met behulp van de volgende opdracht uit:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    De uitvoer van de npm-opdracht moet naar het volgende resultaat als volgt uitzien. 
   
    ![Snelle opdracht installeren Windows PowerShell voor het weergeven van de uitvoer van de npm](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Wijzig de mappen op de **WebRole1** directory en gebruikt u de snelle opdracht voor het genereren van een nieuwe toepassing:
   
        PS C:\node\expressapp\WebRole1> express
   
    U wordt gevraagd naar uw oudere toepassing worden overschreven. Voer **y** of **Ja** om door te gaan. Snelle genereert het app.js-bestand en de mappenstructuur van een voor het bouwen van uw toepassing.
   
    ![De uitvoer van de snelle opdracht](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Voer de volgende opdracht voor het installeren van extra afhankelijkheden die zijn gedefinieerd in het package.json-bestand:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![De uitvoer van de npm-installatieopdracht](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Gebruik de volgende opdracht uit om te kopiëren de **bin/www** van het bestand in **server.js**. Dit is de service in de cloud vindt het beginpunt voor deze toepassing.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Nadat u deze opdracht is voltooid, hebt u een **server.js** bestand in de WebRole1-map.
5. Wijzig de **server.js** verwijderen van een van de '.' tekens uit de volgende regel.
   
       var app = require('../app');
   
   Na deze wijziging, moet de regel er als volgt uitzien.
   
       var app = require('./app');
   
   Deze wijziging is vereist omdat we het bestand verplaatst (voorheen **bin/www**,) naar dezelfde map als het appbestand dat is vereist. Nadat u deze wijziging, sla de **server.js** bestand.
6. Gebruik de volgende opdracht voor het uitvoeren van de toepassing in de Azure-emulator:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Een webpagina met Welkom om uit te drukken.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>De weergave wijzigen
Wijzig nu de weergave om het bericht 'Welkom aan Express in Azure' weer te geven.

1. Voer de volgende opdracht om de index.jade-bestand te openen:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![De inhoud van de index.jade-bestand.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade is de standaard-weergave-engine wordt gebruikt door toepassingen van Express. Zie voor meer informatie over Jade weergave-engine [ http://jade-lang.com ] [ http://jade-lang.com].
2. De laatste regel van tekst wijzigen door toe te voegen **in Azure**.
   
   ![Index.jade-bestand, de laatste regel leest: p Welkom bij \#{title} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Sla het bestand op en sluit u Kladblok af.
4. Vernieuw uw browser en ziet u uw wijzigingen.
   
   ![Een browservenster bevat de pagina Welkom bij Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Nadat de toepassing testen, gebruikt u de **Stop-AzureEmulator** cmdlet voor het stoppen van de emulator.

## <a name="publishing-the-application-to-azure"></a>Publiceren van de toepassing in Azure
In de Azure PowerShell-venster, gebruikt u de **Publish-AzureServiceProject** cmdlet om de toepassing naar een cloudservice te implementeren

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Nadat de implementatiebewerking is voltooid, wordt uw browser openen en weergeven van de webpagina wordt weergegeven.

![Een webbrowser waarin de Express wordt weergegeven. De URL geeft aan dat het nu wordt gehost op Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Volgende stappen
Zie het [Node.js Developer Center](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest) voor meer informatie.

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


