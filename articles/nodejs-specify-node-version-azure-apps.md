---
title: Een Node.js-versie opgeven
description: Informatie over het opgeven van de versie van Node.js gebruikt door Azure websites en Cloudservices
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: a20179c72b227deb14df442bea7b80cf31728aa7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Een Node.js-versie opgeven in een Azure-toepassing
Bij het hosten van een Node.js-toepassing, is het raadzaam om ervoor te zorgen dat uw toepassing gebruikmaakt van een specifieke versie van Node.js. Er zijn verschillende manieren om dit te bereiken voor toepassingen die worden gehost op Azure.

## <a name="default-versions"></a>Standaardversies
De Node.js-versies die is verstrekt door Azure worden voortdurend bijgewerkt. Tenzij anders vermeld, de standaard-versie die is opgegeven in de `WEBSITE_NODE_DEFAULT_VERSION` omgevingsvariabele wordt gebruikt. U kunt deze standaardwaarde overschrijven, volg de stappen in de volgende secties van dit artikel

> [!NOTE]
> Als u uw toepassing in een Azure Cloud Service (web- of worker-rol) host, en is de eerste keer dat de toepassing is geïmplementeerd, wordt Azure probeert te gebruiken dezelfde versie van Node.js als u op uw ontwikkelingsomgeving hebt geïnstalleerd als deze overeenkomt met een t hij standaardversies beschikbaar zijn op Azure.
>
>

## <a name="versioning-with-packagejson"></a>Versiebeheer voor onderdelen met package.json
De versie van Node.js moet worden gebruikt door het volgende toe te voegen kunt u uw **package.json** bestand:

    "engines":{"node":version}

Waar *versie* is het nummer van de specifieke versie te gebruiken. Kunt u complexere voorwaarden voor de versie, zoals:

    "engines":{"node": "0.6.22 || 0.8.x"}

Omdat 0.6.22 niet een van de versies die beschikbaar zijn in de hosting-omgeving is, wordt de hoogste versie van de 0,8 reeks die beschikbaar zal worden gebruikt in plaats daarvan - 0.8.4.

## <a name="versioning-websites-with-app-settings"></a>Versiebeheer Websites met App-instellingen
Als u de toepassing in een Website host, kunt u de omgevingsvariabele instellen **WEBSITE_NODE_DEFAULT_VERSION** naar de gewenste versie.

## <a name="versioning-cloud-services-with-powershell"></a>Versiebeheer Cloudservices met PowerShell
Als u als host voor de toepassing in een Cloudservice optreden en de toepassing met Azure PowerShell implementeert, kunt u de standaardversie van Node.js overschrijven met behulp van de **Set AzureServiceProjectRole** PowerShell-cmdlet. Bijvoorbeeld:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Noteer dat de parameters in de bovenstaande verklaring zijn hoofdlettergevoelig.  U kunt controleren of de juiste versie van Node.js is geselecteerd door het controleren van de **engines** eigenschap in uw rol **package.json**.

U kunt ook de **Get-AzureServiceProjectRoleRuntime** voor het ophalen van een lijst met Node.js versies beschikbaar voor toepassingen die als Cloudservice wordt gehost.  Controleer altijd of de versie van Node.js afhankelijk van uw project is in deze lijst.

## <a name="using-a-custom-version-with-azure-websites"></a>Met behulp van een aangepaste versie met Azure Websites
Azure biedt verschillende standaardversies zijn van Node.js, maar mogelijk wilt gebruiken een versie die wordt standaard niet opgegeven. Als uw toepassing als een Azure-Website wordt gehost, u kunt dit doen met behulp van de **iisnode.yml** bestand. De volgende stappen doorlopen die het proces van het gebruik van een aangepaste versie van Node.Js met een Azure-Website:

1. Een nieuwe map maken en maak vervolgens een **server.js** bestand in de map. De **server.js** -bestand moet bevatten het volgende:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Hiermee wordt de Node.js-versie die wordt gebruikt wanneer u de website bladeren weergegeven.
2. Maak een nieuwe Website en noteer de naam van de site. Bijvoorbeeld de volgende doeleinden gebruiken de [Azure-opdrachtregelprogramma's] voor het maken van een nieuwe Azure-Website met de naam **MijnWebsite**, en schakel vervolgens een Git-opslagplaats voor de website.

        azure site create mywebsite --git
3. Maak een nieuwe map met de naam **bin** als een onderliggend element van de map waarin de **server.js** bestand.
4. Download de specifieke versie van **node.exe** (de Windows-versie) die u wilt gebruiken met uw toepassing. Bijvoorbeeld de volgende doeleinden **curl** 0.8.1 versie te downloaden:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Sla de **node.exe** bestand naar de **bin** map die eerder is gemaakt.
5. Maak een **iisnode.yml** bestand in dezelfde map als de **server.js** bestand en voeg vervolgens de volgende inhoud naar de **iisnode.yml** bestand:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Dit pad is waar de **node.exe** bestand in uw project worden geplaatst wanneer u uw toepassing naar de Azure-Website hebt gepubliceerd.
6. Uw toepassing publiceren. Bijvoorbeeld: nadat ik een nieuwe website eerder met de parameter--git gemaakt, de volgende opdrachten worden bestanden voor de toepassing toevoegen aan mijn lokale Git-opslagplaats en vervolgens toepassen op de website-opslagplaats:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nadat de toepassing is gepubliceerd, opent u de website in een browser. U ziet een bericht weergegeven ' Hallo van Azure actief knooppunt versie: v0.8.1 '.

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe de versie van Node.js gebruikt door de toepassing opgeven begrijpt, informatie over hoe [werken met modules], [bouwen en implementeren van een Node.js-website](app-service/app-service-web-get-started-nodejs.md), en [het gebruik van de Azure Opdrachtregelprogramma's voor Mac en Linux].

Zie het [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/) voor meer informatie.

[het gebruik van de Azure Opdrachtregelprogramma's voor Mac en Linux]:cli-install-nodejs.md
[Azure-opdrachtregelprogramma's]:cli-install-nodejs.md
[werken met modules]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service/app-service-web-get-started-nodejs.md
