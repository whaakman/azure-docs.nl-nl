---
title: Aanbevolen procedures - Azure App Service
description: Informatie over aanbevolen procedures en probleemoplossing voor Azure App Service.
services: app-service
documentationcenter: ''
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
ms.custom: seodec18
ms.openlocfilehash: fc3749a9ebfbf0319a57b471b6fce9f62042ba27
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849772"
---
# <a name="best-practices-for-azure-app-service"></a>Aanbevolen procedures voor Azure App Service
In dit artikel bevat een overzicht van aanbevolen procedures voor het gebruik van [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>CO-locatie
Als Azure-resources voor het samenstellen van een oplossing zoals een web-app en een database zich in verschillende regio's, kan deze de volgende gevolgen hebben:

* Verhoogde latentie in de communicatie tussen resources
* Monetaire kosten voor uitgaande gegevens overdragen interregionale, zoals vermeld in de [Azure pagina met prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

CO-locatie in dezelfde regio wordt aanbevolen voor Azure-resources voor het samenstellen van een oplossing zoals een web-app en een database of storage-account gebruikt voor het opslaan van inhoud of gegevens. Bij het maken van resources, zorg er dan voor dat ze zich in dezelfde Azure-regio, tenzij u specifieke zakelijke hebben of ontwerpen reden voor deze niet te worden. U kunt een App Service-app verplaatsen naar de dezelfde regio bevinden als uw database met behulp van de [App Service-functie voor het klonen](app-service-web-app-cloning.md) momenteel beschikbaar voor Premium App Service-Plan-apps.   

## <a name="memoryresources"></a>Wanneer apps verbruikt meer geheugen dan verwacht
U ziet u wanneer u een app verbruikt meer geheugen dan verwacht als aangegeven middels het aanbevelingen voor bewaking of service, kunt u de [App Service automatische herstelfunctie](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Een van de opties voor de functie voor automatisch herstel duurt aangepaste acties op basis van een drempelwaarde geheugen. Acties omvatten het spectrum van e-mailmeldingen onderzoek via geheugendump op de positie risicobeperking door het werkproces wordt gerecycled. Automatisch herstel kan worden geconfigureerd via web.config en via een gebruiksvriendelijke gebruikersinterface zoals beschreven op deze blogpost voor de [App Service ondersteuning voor de extensie voor Site](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Wanneer apps gebruiken meer CPU nodig is dan verwacht
Wanneer u ziet dat een app verbruikt meer CPU nodig is dan verwacht of ervaringen herhaalde CPU pieken zoals wordt aangegeven via aanbevelingen voor bewaking of service, kunt u omhoog of uitschalen van het App Service-plan. Als uw toepassing stateful is, is omhoog schalen de enige optie, terwijl als uw toepassing is staatloze, vergroten/verkleinen out meer flexibiliteit en mogelijkheden voor hogere schaal biedt. 

Bekijk deze video voor meer informatie over 'stateful' vs 'stateless' toepassingen: [Een schaalbare toepassing met meerdere lagen End-to-End van Azure App Service plan](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Zie voor meer informatie over opties voor het schalen en automatisch schalen van App Service, [een Web-App schalen in Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Wanneer de socket resources zijn uitgeput
Een veelvoorkomende reden voor uitput uitgaande TCP-verbindingen is het gebruik van clientbibliotheken, die niet worden geïmplementeerd om TCP-verbindingen opnieuw te gebruiken, of wanneer een hoger niveau protocol zoals HTTP - keepalive niet wordt gebruikt. Raadpleeg de documentatie voor elk van de bibliotheken waarnaar wordt verwezen door de apps in uw App Service-Plan om te controleren of ze zijn geconfigureerd of geopend in de programmacode voor efficiënte hergebruik van uitgaande verbindingen. Volg ook de bibliotheekhulp-documentatie voor het maken van goede en release of opschoning om te voorkomen dat verbindingen lekken. Hoewel deze bibliotheken client verificaties uitgevoerd worden, kan invloed worden verholpen door uitschalen naar meerdere exemplaren.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js en uitgaande http-aanvragen
Als u werkt met Node.js en veel uitgaande http-aanvragen, is het omgaan met HTTP - keepalive belangrijk. U kunt de [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` pakket zodat u gemakkelijk in uw code.

Altijd verwerken de `http` antwoord, zelfs als u niets in de handler doet. Als u niet het antwoord niet goed verwerkt, wordt uw toepassing uiteindelijk vastgelopen omdat er geen sockets meer beschikbaar zijn.

Bijvoorbeeld, als u werkt met de `http` of `https` pakket:

```javascript
const request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Als u op een machine met meerdere kernen op App Service on Linux uitvoert, wordt een andere best practice is het gebruik van PM2 meerdere Node.js-processen voor het uitvoeren van uw toepassing te starten. U kunt dit doen door een opdracht voor opstarten naar de container op te geven.

Als u bijvoorbeeld vier instanties starten:

```
pm2 start /home/site/wwwroot/app.js --no-daemon -i 4
```

## <a name="appbackup"></a>Wanneer uw app back-up starten mislukt
De twee meest voorkomende redenen waarom app back-up mislukt zijn: ongeldige Opslaginstellingen en ongeldige database-configuratie. Deze fouten gebeurt meestal wanneer er wijzigingen in de opslag- of database-resources of wijzigingen in hoe u toegang tot deze resources (bijvoorbeeld referenties bijgewerkt in verband met de database die is geselecteerd in de back-upinstellingen). Back-ups wordt doorgaans uitgevoerd volgens een schema en toegang tot opslag (voor het uitvoeren van de back-up-bestanden) en databases (voor het kopiëren en lezen van de inhoud moet worden opgenomen in de back-up) nodig hebben. Het resultaat van krijgt geen toegang tot een van deze resources zijn consistent back-upfouten. 

Wanneer er back-ups fouten optreden, bekijk meest recente resultaten voor meer informatie over welk type fout plaatsvindt. Voor opslag toegangsfouten, controleren en bijwerken van de storage-instellingen die in de back-upconfiguratie. Bekijk voor database toegangsfouten, en werk de referentietekenreeksen van uw verbindingen als onderdeel van appinstellingen. vervolgens gaat u verder met het bijwerken van uw back-upconfiguratie voor correct zijn onder meer de vereiste databases. Zie voor meer informatie over back-ups [maakt u een Back-up van een web-app in Azure App Service](manage-backup.md).

## <a name="nodejs"></a>Wanneer nieuwe Node.js-apps zijn geïmplementeerd in Azure App Service
Azure App Service-standaardconfiguratie voor Node.js-apps is bedoeld om het beste de behoeften van de meest voorkomende apps. Als de configuratie voor uw Node.js-app voordeel hebben veel van persoonlijke afstemmen om de prestaties verbeteren of Resourcegebruik voor de CPU/geheugen/netwerkbronnen te optimaliseren, raadpleegt [aanbevolen procedures en gids voor probleemoplossing voor knooppunttoepassingen in Azure-App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md). In dit artikel beschrijft de iisnode-instellingen u mogelijk wilt configureren voor uw Node.js-app, worden de verschillende scenario's beschreven of problemen met uw app kan worden aangesloten, en laat zien hoe u deze problemen op te lossen.

