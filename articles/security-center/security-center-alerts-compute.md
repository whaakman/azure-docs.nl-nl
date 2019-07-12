---
title: Bedreigingen voor cloud systeemeigen rekenkracht in Azure Security Center | Microsoft Docs
description: In dit onderwerp worden de cloud systeemeigen rekenkracht-waarschuwingen die beschikbaar zijn in Azure Security Center. in Azure Security Center.
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
ms.author: monhaber
ms.openlocfilehash: bc3cb66d43e71777e06c6bd63dcff35e2ff19df8
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571684"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Detectie van bedreigingen voor cloud-native compute in Azure Security Center

Als een cloudprovider, Security Center maakt gebruik van de unieke zichtbaarheid die voor interne logboeken analyseren en identificeren van de aanval methodologie voor meerdere doelen heeft. In dit onderwerp geeft de waarschuwingen die beschikbaar zijn voor de volgende Azure-services:

* [Azure App Service](#app-services)
* [Containers](#azure-containers) 

## Azure App Service <a name="app-services"></a>

Security Center maakt gebruik van de schaal van de cloud voor het identificeren van aanvallen die gericht zijn op klanten toepassingen die worden uitgevoerd via Azure App Service. Met webtoepassingen wordt vrijwel in een moderne netwerk-, test aanvallers om te zoeken van deze en zwakke punten. Voordat het wordt doorgestuurd naar specifieke omgevingen, gaat u aanvragen voor toepassingen die in Azure worden uitgevoerd via verschillende gateways waar ze zijn gecontroleerd en vastgelegd. Deze gegevens wordt vervolgens gebruikt om te identificeren van aanvallen, aanvallers, en voor meer informatie over nieuwe patronen die later worden gebruikt.

Door gebruik te maken van de zichtbaarheid met Azure als een cloudprovider, analyseert Security Center interne logboeken van de App Service voor het identificeren van de aanval methodologie voor meerdere doelen. Bijvoorbeeld, wijdverbreid scannen en gedistribueerde aanvallen. Dit type aanval wordt meestal afkomstig is van een kleine subset van IP-adressen en vertoont patronen van verkennen met vergelijkbare eindpunten op meerdere hosts, te zoeken naar een kwetsbaar pagina of -invoegtoepassing. Dit kan worden gedetecteerd met behulp van de Cloud, maar kan niet worden geïdentificeerd vanuit het oogpunt van één host.

Security Center heeft ook toegang tot de onderliggende sandboxes en virtuele machines. Samen met geheugen forensische gegevens, kan de infrastructuur de boodschap, van een nieuwe aanval die in de natuur naar compromissen in machines van klanten. Daarom kan Security Center aanvallen tegen de webtoepassingen lang detecteren nadat worden misbruikt.

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Verdachte WordPress thema aanroep gedetecteerd**|Het activiteitenlogboek van Azure App Service wijst op een mogelijk code-injectie activiteit voor uw App Service-resource.<br/> Deze verdachte activiteit lijkt op activiteit, die een WordPress-thema ter ondersteuning van server-side-uitvoering van code, gevolgd door een aanvraag voor directe web om aan te roepen van het themabestand gezelschapsdieren wordt bewerkt. Dit type activiteit heeft in het verleden zijn waargenomen als onderdeel van een aanval via WordPress.|
|**Verbinding met de webpagina van afwijkende IP-adres gedetecteerd**|Het activiteitenlogboek van Azure App Service geeft een verbinding met een gevoelige webpagina van een bronadres die nooit met het eerder verbonden. Dit kan betekenen dat iemand een brute force-aanval in uw Web-App beheerprogramma's probeert. Het resultaat van een nieuw IP-adres wordt gebruikt door een geldige gebruiker kan ook zijn.|
|**Een IP-adres dat met uw Azure App Service-Interface met FTP verbonden is gevonden in Bedreigingsinformatie**|Analyse van Azure App Service-FTP-Logboeken is een verbinding van een bronadres dat is gevonden in de feed Bedreigingsinformatie gedetecteerd. Tijdens deze verbinding van een gebruiker toegang krijgen tot de pagina's hieronder vermeld.|
|**Web fingerprinting gedetecteerd**|Het activiteitenlogboek van Azure App Service geeft een mogelijke web fingerprinting activiteit voor uw App Service-resource. <br/>Deze verdachte activiteit gedetecteerd is gekoppeld aan een hulpprogramma Blind olifanten genoemd. Het hulpprogramma vingerafdrukken webservers en er wordt geprobeerd om de geïnstalleerde toepassingen en versies van hun te detecteren. Dit hulpprogramma aanvallers vaak gebruikt voor de web-apps scannen op beveiligingsproblemen zoeken.|
|**Verdachte toegang tot mogelijk kwetsbaar webpagina gedetecteerd**|Het activiteitenlogboek van Azure App Service geeft aan dat een webpagina die lijkt te worden gevoelige is geopend. <br/>Deze verdachte activiteit afkomstig is van een bron-adres waarvan toegangspatroon lijkt op die van een web-scanner. Dit soort activiteit is vaak gekoppeld aan een poging door een aanvaller om uw netwerk om te kunnen toegang krijgen tot gevoelige of kwetsbaar webpagina's te scannen.|
|**PHP-bestand in Uploadmap**|Het activiteitenlogboek van Azure App Service geeft aan dat er iets is geopend op een verdachte PHP-pagina zich in de uploadmap. <br/>Dit type map bevat meestal geen PHP-bestanden. Een licentie te profiteren van de mogelijkheid om willekeurige bestanden uploaden beveiligingsproblemen kan wijzen op de aanwezigheid van dit type bestand.|
|**Een poging tot Linux-opdrachten uitvoeren op een Windows-App-Service**|Analyse van App Service-processen heeft gedetecteerd dat een poging tot een Linux-opdracht uitvoeren op een Windows-App-Service. Deze actie is uitgevoerd door de web-App. Dit gedrag is vaak gezien tijdens de campagnes die gebruikmaakt van een kwetsbaarheid in een algemene webtoepassing.|
|**Uitvoering van verdachte PHP gedetecteerd**|Machinelogboek geeft aan die een verdachte PHP-proces wordt uitgevoerd. De actie opgenomen een poging tot het OS opdrachten of PHP-code uitvoeren vanaf de opdrachtregel met behulp van het PHP-proces. Hoewel dit gedrag legitieme zijn kan, in webtoepassingen dit gedrag is ook het geval is bij schadelijke activiteiten, zoals pogingen tot het infecteren van websites met web shells.|
|**Uitvoering van het proces van tijdelijke map**|Analyse van App Service-processen heeft de uitvoering van een proces van de tijdelijke map van de app gedetecteerd. Hoewel dit probleem legitieme in webtoepassingen die dit gedrag is ook het geval is bij schadelijke activiteiten zijn kan.|
|**Poging om uit te voeren opdracht van hoge bevoegdheid gevonden.**|Analyse van App Service-processen aangetroffen een poging een opdracht waarvoor hoge bevoegdheden uit te voeren. De opdracht is uitgevoerd in de context van de web-toepassing. Hoewel dit probleem legitieme in webtoepassingen die dit gedrag is ook het geval is bij schadelijke activiteiten zijn kan.|

> [!NOTE]
> Detectie van Security Center bedreigingen voor App Service is momenteel niet beschikbaar in Azure government en onafhankelijke cloud-regio's.

Voor meer informatie over App Service-bedreigingsdetectie waarschuwingen gaat u naar App Service beveiligen met Azure Security Center en bekijk het inschakelen van controle en beveiliging van uw App Service-workloads.

## Containers <a name="azure-containers"></a>

Security Center biedt realtime detectie van bedreigingen voor uw containers op Linux-machines op basis van de auditd-framework. De waarschuwingen identificeren verdachte Docker-activiteiten, zoals het maken van een bevoegde container op een host, een indicatie van Secure Shell (SSH)-server die worden uitgevoerd binnen een Docker-container of het gebruik van de cryptografische miners. U kunt deze informatie gebruiken om snel beveiligingsproblemen op te lossen en om de beveiliging van uw containers te verbeteren. Naast het Linux-detectie biedt Security Center ook analytics die specifiek voor implementaties van containers.
