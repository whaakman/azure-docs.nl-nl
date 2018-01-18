---
title: Aanbevolen procedures voor Azure App Service
description: Informatie over aanbevolen procedures en probleemoplossing voor Azure App Service.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 7c5eb6190d4a4cdfa47779d2c4d7aadac5a2fb80
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="best-practices-for-azure-app-service"></a>Aanbevolen procedures voor Azure App Service
In dit artikel bevat een overzicht van aanbevolen procedures voor het gebruik van [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Plaatsing
Als Azure-resources voor het samenstellen van een oplossing zoals een web-app en een database zich in verschillende regio's, kan deze de volgende gevolgen hebben:

* Wachttijd in de communicatie tussen bronnen
* Financieel kosten voor uitgaande gegevens overbrengen regio-overschrijdende zoals aangegeven in de [Azure pagina met prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

Collocatie in dezelfde regio wordt aanbevolen voor het samenstellen van een oplossing zoals een web-app en een database of opslaggroep account dat wordt gebruikt voor het opslaan van inhoud of Azure-resources. Bij het maken van resources, zorg er dan voor dat ze zich in dezelfde Azure-regio, tenzij u specifieke zakelijke hebt of ontwerpen reden ze niet kunnen worden. Kunt u een App Service-app in dezelfde regio bevinden als de database met behulp van de [App Service-functie voor het klonen](app-service-web-app-cloning.md) momenteel beschikbaar voor Premium-App Service-Plan apps.   

## <a name="memoryresources"></a>Wanneer de meer geheugen hebben dan verwacht voor het gebruiken van apps
Wanneer u een app verbruikt meer geheugen hebben dan verwacht als merken aangegeven middels het controleren of de service aanbevelingen, overweeg dan de [functie-App Service automatisch herstel](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Een van de opties voor de functie automatisch herstel duurt aangepaste acties op basis van een drempelwaarde voor geheugen. Acties omvatten het spectrum van e-mailmeldingen voor onderzoek via geheugendump naar de positie risicobeperking door het werkproces wordt gerecycled. Automatisch herstel kan worden geconfigureerd via web.config en via een beschrijvende gebruikersinterface zoals beschreven op in dit blogbericht voor de [Appuitbreiding Service ondersteuning Site](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Wanneer de meer CPU dan verwacht voor het gebruiken van apps
Wanneer u ziet dat een app verbruikt meer CPU dan verwacht of ervaringen herhaalde CPU pieken aangegeven via bewaking of service aanbevelingen, overweeg dan omhoog schalen of uitbreiden van de App Service-abonnement. Als uw toepassing stateful, is omhoog schalen de enige optie, hoewel als uw toepassing staatloze, scaling out u meer flexibiliteit en hogere schaal potentieel geeft. 

Voor meer informatie over 'stateful' vs 'stateless' toepassingen kunt Bekijk deze video: [Planning van een toepassing met meerdere lagen schaalbare End-to-End voor Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Zie voor meer informatie over opties voor het schalen en automatisch schalen van App Service [schalen van een Web-App in Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Wanneer de socket-resources zijn uitgeput
Een veelvoorkomende reden voor put uitgaande TCP-verbindingen is het gebruik van clientbibliotheken, die niet zijn geïmplementeerd om TCP-verbindingen opnieuw te gebruiken, of wanneer een hoger niveau protocol zoals HTTP - keepalive niet wordt gebruikt. Raadpleeg de documentatie voor elk van de bibliotheken waarnaar wordt verwezen door de apps in uw App Service-abonnement om te controleren of ze zijn geconfigureerd of toegankelijk is in uw code voor efficiënte hergebruik van uitgaande verbindingen. Volgt tevens de bibliotheek documentatie richtlijnen voor het maken van goede en release of cleanup om te voorkomen dat verbindingen lekken. Hoewel deze bibliotheken client onderzoeken uitgevoerd worden, mogelijk gevolgen worden verminderd door uitbreiden naar meerdere exemplaren.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js en uitgaande http-aanvragen
Als u werkt met Node.js en veel uitgaande http-aanvragen, is het omgaan met HTTP - keepalive belangrijk. U kunt de [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` pakket om het gemakkelijker in uw code.

Altijd verwerkt de `http` antwoord, zelfs als u niets in de handler. Als u niet het antwoord niet goed verwerkt, wordt uw toepassing uiteindelijk vastgelopen omdat er geen meer sockets beschikbaar zijn.

Bijvoorbeeld, als u werkt met de `http` of `https` pakket:

```
var request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Als u op een machine met meerdere kernen op op Linux-App Service uitvoert, wordt een andere aanbevolen procedure is het gebruik van PM2 meerdere Node.js-processen voor het uitvoeren van uw toepassing te starten. U kunt dit doen door op te geven van een opstartopdracht naar de container.

Als u bijvoorbeeld vier exemplaren starten:

```
pm2 start /home/site/wwwroot/app.js --no-daemon -i 4
```

## <a name="appbackup"></a>Wanneer uw app back-up begint mislukt
De twee meest voorkomende redenen waarom app back-up mislukt zijn: instellingen van het opslagaccount is ongeldig en ongeldige database-configuratie. Deze fouten worden doorgaans gebeuren wanneer er wijzigingen in de opslaggroep of database resources of wijzigingen voor toegang tot deze bronnen (bijvoorbeeld referenties voor de database die is geselecteerd in de back-upinstellingen bijgewerkt). Back-ups worden doorgaans volgens een planning wordt uitgevoerd en toegang tot opslag (voor het uitvoeren van de back-up-bestanden) en databases vereisen (voor het kopiëren en lezen van de inhoud moet worden opgenomen in de back-up). Het resultaat van de toegankelijkheid van een van deze resources zijn consistent back-upfouten. 

Wanneer back-fouten optreden, bekijkt u de meest recente resultaten om te begrijpen welke type van de fout zich heeft voorgedaan. Voor toegang Opslagfouten, controleren en bijwerken van de instellingen voor de opslag in de back-upconfiguratie gebruikt. Voor toegang databasefouten, controleren en bijwerken van uw verbindingen tekenreeksen als onderdeel van de appinstellingen van de. gaat u verder met het bijwerken van de back-configuratie correct zijn onder meer de vereiste databases. Zie voor meer informatie over back-ups van app [Back-up van een web-app in Azure App Service](web-sites-backup.md).

## <a name="nodejs"></a>Wanneer nieuwe Node.js-apps zijn geïmplementeerd in Azure App Service
Azure App Service-standaardconfiguratie voor Node.js-apps is bedoeld om het best past bij de behoeften van de meest voorkomende apps. Als de configuratie voor uw Node.js-app wilt profiteren van persoonlijke afstemmen om de prestaties verbeteren of Resourcegebruik voor CPU/geheugen/netwerkbronnen te optimaliseren, Zie [Best practices en de gids voor probleemoplossing voor knooppunt toepassingen in de Azure-App voor Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md). In dit artikel beschrijft de iisnode-instellingen wilt configureren voor uw Node.js-app, beschrijft de verschillende scenario's of problemen met uw app kan worden geconfronteerd, en laat zien hoe deze problemen op te lossen.

