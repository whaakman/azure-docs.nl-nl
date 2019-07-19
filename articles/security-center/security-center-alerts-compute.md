---
title: Detectie van bedreigingen voor de eigen Cloud Compute in Azure Security Center | Microsoft Docs
description: Dit onderwerp bevat de eigen Cloud Compute-waarschuwingen die beschikbaar zijn in Azure Security Center. in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 14433806a28e31cef1a278e16cb69e7c9b1a2458
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295849"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Detectie van bedreigingen voor de systeem eigen Cloud Compute in Azure Security Center

Als Cloud provider Security Center maakt gebruik van de unieke zicht baarheid van het analyseren van interne logboeken en het identificeren van een aanvals methodologie op meerdere doelen. Dit onderwerp bevat de waarschuwingen die beschikbaar zijn voor de volgende Azure-Services:

* [Azure App Service](#app-services)
* [Containers](#azure-containers) 

## Azure App Service<a name="app-services"></a>

Security Center maakt gebruik van de schaal van de cloud om aanvallen te identificeren die gericht zijn op toepassingen van klanten die meer dan Azure App Service. Met webtoepassingen die praktisch zijn in elk moderne netwerk, testen aanvallers om deze zwakke plekken te vinden. Aanvragen voor toepassingen die in Azure worden uitgevoerd, gaan vóór de route naar specifieke omgevingen via verschillende gateways, waar ze worden geïnspecteerd en geregistreerd. Deze gegevens worden vervolgens gebruikt voor het identificeren van aanvallen, aanvallers en het leren van nieuwe patronen die later zullen worden gebruikt.

Door gebruik te maken van de zicht baarheid van Azure als een Cloud provider, Security Center analyseert App Service interne Logboeken om een aanvals methodologie op meerdere doelen te identificeren. Bijvoorbeeld verbreide scans en gedistribueerde aanvallen. Dit type aanval is doorgaans afkomstig uit een kleine subset van IP-adressen en vertoont patronen van het verkennen naar vergelijk bare eind punten op meerdere hosts. er wordt gezocht naar een kwets bare pagina of invoeg toepassing. Dit kan worden gedetecteerd met behulp van de Cloud, maar kan niet vanuit het oogpunt van één host worden geïdentificeerd.

Security Center heeft ook toegang tot de onderliggende sandboxes en virtuele machines. Samen met het geheugen forensische kan de infra structuur het verhaal vertellen, van een nieuwe aanval die in het wild wordt circuleren tot inbreuk op de klant machines. Daarom kunnen Security Center voor het detecteren van aanvallen op webtoepassingen lang duren nadat ze zijn misbruikt.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Aanroep van verdacht WordPress-thema gedetecteerd**|Het Azure App Service activiteiten logboek duidt op een mogelijke code-injectie activiteit op uw App Service-Resource.<br/> Deze verdachte activiteit lijkt op een activiteit die een WordPress-thema bewerkt ter ondersteuning van de uitvoering van code aan de server zijde, gevolgd door een directe webaanvraag om het gemanipuleerde thema bestand aan te roepen. Dit type activiteit is in het verleden gezien als onderdeel van een aanvals campagne via WordPress.|
|**Verbinding met webpagina van afwijkend IP-adres gedetecteerd**|Het Azure App Service activiteiten logboek duidt op een verbinding met een gevoelige webpagina van een bron adres dat nooit eerder verbinding heeft gemaakt met deze pagina. Dit kan erop wijzen dat iemand een beveiligings aanval probeert uit te voeren in de beheer pagina's van uw web-app. Dit kan ook het gevolg zijn van een nieuw IP-adres dat wordt gebruikt door een rechtmatige gebruiker.|
|**Er is een IP-adres gevonden dat is verbonden met uw Azure App Service FTP-interface in bedreigings informatie**|Azure App Service FTP-logboeken analyse heeft een verbinding gedetecteerd van een bron adres dat in de bedreigings informatie feed is gevonden. Tijdens deze verbinding heeft een gebruiker toegang tot de pagina's die hieronder worden weer gegeven.|
|**Webvingerafdruking gedetecteerd**|Het Azure App Service activiteiten logboek duidt op een mogelijke Web-vingerafdruk activiteit op uw App Service-bron. <br/>Deze verdachte activiteit is gekoppeld aan een hulp programma met de naam blind Elephant. Het hulp programma vervingerafdrukt webservers en probeert de geïnstalleerde toepassingen en hun versies te detecteren. Aanvallers gebruiken dit hulp programma vaak om de webtoepassingen te zoeken om beveiligings problemen op te sporen.|
|**Verdachte toegang tot mogelijk kwets bare webpagina gedetecteerd**|In het Azure App Service activiteiten logboek wordt aangegeven dat een webpagina die vertrouwelijk lijkt te zijn geopend. <br/>Deze verdachte activiteit is afkomstig van een bron adres waarvan het toegangs patroon op een webscanner lijkt. Dit soort activiteit is vaak gekoppeld aan een poging van een aanvaller om uw netwerk te scannen om te proberen toegang te krijgen tot gevoelige of kwets bare webpagina's.|
|**PHP-bestand in uploadmap**|Het Azure App Service activiteiten logboek geeft aan dat er een verdachte PHP-pagina is geopend in de uploadmap. <br/>Dit type map bevat doorgaans geen PHP-bestanden. Het bestaan van dit type bestand kan duiden op een voor deel dat er sprake is van een probleem met het uploaden van wille keurige bestanden.|
|**Een poging om Linux-opdrachten uit te voeren op een Windows-App Service**|Tijdens de analyse van App Service processen is een poging tot het uitvoeren van een Linux-opdracht op een Windows-App Service gedetecteerd. Deze actie is uitgevoerd door de webtoepassing. Dit gedrag wordt vaak weer gegeven tijdens campagnes die misbruik maken van een beveiligingslek in een gemeen schappelijke webtoepassing.|
|**Verdachte PHP-uitvoering gedetecteerd**|Computer logboeken geven aan dat een verdacht PHP-proces wordt uitgevoerd. De actie bevat een poging om besturingssysteem opdrachten of PHP-code uit te voeren vanaf de opdracht regel met behulp van het PHP-proces. Hoewel dit gedrag legitiem kan zijn, wordt dit gedrag in webtoepassingen ook waargenomen in schadelijke activiteiten, zoals pogingen om websites te infecteren met webshells.|
|**Uitvoering van tijdelijke map verwerken**|App Service analyse van processen heeft een uitvoering van een proces in de tijdelijke map van de app gedetecteerd. Hoewel dit gedrag legitiem kan zijn, wordt dit gedrag in webtoepassingen ook waargenomen in schadelijke activiteiten.|
|**Poging tot het uitvoeren van een opdracht met hoge bevoegdheden gedetecteerd**|Tijdens de analyse van App Service processen is een poging tot het uitvoeren van een opdracht met hoge bevoegdheden gedetecteerd. De opdracht is uitgevoerd in de context van de webtoepassing. Hoewel dit gedrag legitiem kan zijn, wordt dit gedrag in webtoepassingen ook waargenomen in schadelijke activiteiten.|

> [!NOTE]
> Security Center detectie van bedreigingen voor App Service is momenteel niet beschikbaar in azure Government en soevereine Cloud regio's.

Ga voor meer informatie over App Service waarschuwingen voor detectie van bedreigingen naar Beveilig App Service met Azure Security Center en lees hoe u bewaking en beveiliging van uw App Service workloads inschakelt.

## Containers<a name="azure-containers"></a>

Security Center biedt real-time detectie van bedreigingen voor uw containers op Linux-machines op basis van het gecontroleerde Framework. De waarschuwingen identificeren verschillende verdachte docker-activiteiten, zoals het maken van een geprivilegieerde container op een host, een indicatie van een SSH-server (Secure Shell) die binnen een docker-container of het gebruik van crypto grafie-Miners. U kunt deze informatie gebruiken om snel beveiligingsproblemen op te lossen en om de beveiliging van uw containers te verbeteren. Naast Linux-detecties biedt Security Center ook analyses die specifiek zijn voor implementaties van containers.
