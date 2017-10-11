---
title: Web-App snelle (Node.js) | Microsoft Docs
description: Een zelfstudie die is gebaseerd op de cloud service-zelfstudie en laat zien hoe u de Express-module.
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 54b715695e24786ec4e8dfcabefc648d76179c8b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Een Node.js-webtoepassing met een snelle op een Azure Cloud Service bouwen
Node.js bevat een minimale set van de functionaliteit in de runtime core.
Ontwikkelaars gebruiken vaak 3e partij modules aanvullende functionaliteit kan bieden bij het ontwikkelen van een Node.js-toepassing. In deze zelfstudie maakt u een nieuwe toepassing met de [Express] [ Express] module waarmee een MVC-framework biedt voor het maken van Node.js-webtoepassingen.

Een schermopname van de voltooide toepassing lager is dan:

![Een webbrowser waarin Welkom in Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Een Cloud Service-Project maken
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Voer de volgende stappen voor het maken van een nieuw cloudserviceproject met de naam 'expressapp':

1. Van de **startmenu** of **startscherm**, zoeken naar **Windows PowerShell**. Ten slotte de met de rechtermuisknop op **Windows PowerShell** en selecteer **als Administrator uitvoeren**.
   
    ![Azure PowerShell-pictogram](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Wijzig de mappen op de **c:\\knooppunt** directory en voer de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **expressapp** en een Webrol met de naam **WebRole1**:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Standaard **Add-AzureNodeWebRole** maakt gebruik van een oudere versie van Node.js. De **Set AzureServiceProjectRole** instructie hiervoor Hiermee geeft u Azure v0.10.21 van knooppunt gebruiken.  Noteer dat de parameters zijn hoofdlettergevoelig.  U kunt controleren of de juiste versie van Node.js is geselecteerd door het controleren van de **engines** eigenschap in **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Express installeren
1. De Express-generator installeren door de volgende opdracht:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    De uitvoer van de npm-opdracht zijn vergelijkbaar met het onderstaande resultaat. 
   
    ![Windows PowerShell weergeven van de uitvoer van de npm installeren snelle opdracht.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Wijzig de mappen op de **WebRole1** directory en gebruik de express-opdracht voor het genereren van een nieuwe toepassing:
   
        PS C:\node\expressapp\WebRole1> express
   
    U wordt gevraagd uw eerdere toepassing overschrijven. Voer **y** of **Ja** om door te gaan. Snelle genereert het bestand app.js en de mappenstructuur van een voor het bouwen van uw toepassing.
   
    ![De uitvoer van de snelle opdracht](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Voer de volgende opdracht voor het installeren van extra afhankelijkheden die zijn gedefinieerd in het package.json-bestand:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![De uitvoer van de npm-installatieopdracht](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Gebruik de volgende opdracht om te kopiëren de **bin/www** van het bestand in **server.js**. Dit is de service in de cloud vindt het toegangspunt dat voor deze toepassing.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Nadat u deze opdracht is voltooid, hebt u een **server.js** bestand in de map WebRole1.
5. Wijzig de **server.js** verwijderen van een van de '.' tekens uit de volgende regel.
   
       var app = require('../app');
   
   Na deze wijziging, de regel moet worden weergegeven als volgt.
   
       var app = require('./app');
   
   Deze wijziging is vereist omdat het bestand is verplaatst (voorheen **bin/www**,) in dezelfde map als het appbestand dat vereist is. Nadat u deze wijziging, sla de **server.js** bestand.
6. Gebruik de volgende opdracht voor het uitvoeren van de toepassing in de Azure-emulator:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Een webpagina met Welkom om uit te drukken.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>De weergave wijzigen
Wijzig nu de weergave om het bericht 'Welkom naar Express in Azure' weer te geven.

1. Voer de volgende opdracht om de index.jade-bestand te openen:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![De inhoud van het bestand index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade is de standaard weergave-engine wordt gebruikt door toepassingen van Express. Zie voor meer informatie over Jade weergave-engine [http://jade-lang.com][http://jade-lang.com].
2. De laatste regel van tekst wijzigen door toe te voegen **in Azure**.
   
   ![Het bestand index.jade de laatste regel leest: p Welkom bij de \#{title} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Sla het bestand op en sluit u Kladblok af.
4. Vernieuw de browser en ziet u uw wijzigingen.
   
   ![Een browservenster met de pagina bevat Welkom bij de snelle in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Na de toepassing testen, gebruiken de **Stop AzureEmulator** cmdlet om te stoppen van de emulator.

## <a name="publishing-the-application-to-azure"></a>Publiceren van de toepassing in Azure
In het venster Azure PowerShell gebruiken de **Publish-AzureServiceProject** cmdlet voor het implementeren van de toepassing naar een cloudservice

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Zodra de implementatiebewerking is voltooid, wordt uw browser openen en de webpagina weergegeven.

![Een webbrowser om de Express-pagina weer te geven. De URL geeft aan dat het nu wordt gehost op Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Volgende stappen
Zie het [Node.js Developer Center](/develop/nodejs/) voor meer informatie.

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


