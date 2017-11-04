---
title: Azure SDK voor .NET 2.5.1 Release-opmerkingen
description: Azure SDK voor .NET 2.5.1 Release-opmerkingen
services: app-service
documentationcenter: .net,nodejs,java
author: Juliako
manager: erikre
editor: 
ms.assetid: 8d3d815f-bb58-447e-8ff0-f9b9603c7b00
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/10/2016
ms.author: juliako
ms.openlocfilehash: 0a422b02623a18ac6a1eef460bbada681672e69f
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 Release-opmerkingen
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.5.1 release. 

## <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 release-opmerkingen
Het volgende zijn nieuwe functies en -updates in de Azure SDK voor .NET 2.5.1.

* Nieuwe features\scenarios betrekking hebben op **Web extra Extensions**. 
  
  * Azure Websites is gewijzigd in Azure App Service. 
  * Ondersteuning van Azure API-Apps (Preview) is toegevoegd zodat klanten ASP.NET-projecten als API-Apps publiceren en gebruik vervolgens de toevoegen > gebaar van de Azure-API-App-Client in C#-project voor het genereren van code op basis van de structuur van de geïmplementeerde API-App. 
  * Het knooppunt Websites in Server Explorer is afgeschaft in plaats van het Azure App Service-knooppunt, waarin ondersteuning voor Resource groepering van Azure API-Apps, Mobile Apps en Web-Apps op basis van groep.
  * Ondersteuning van Azure Mobile Apps (Preview) is toegevoegd zodat klanten kunnen maken van nieuwe Mobile Apps-projecten, Mobile Apps-domeincontrollers toevoegt, de projecten publiceren en op afstand fouten opsporen in toepassingen.
  * Toevoegen > gebaar van de Azure-API-App-Client biedt nu ondersteuning voor lokale Swagger JSON-bestanden, zodat ontwikkelaars van Web-API van derden NuGets zoals Swashbuckle gebruiken kunnen voor het genereren van Swagger of handmatig maken. Op deze manier client ontwikkelaars kunnen het genereren van code-functies gebruiken van een willekeurig eindpunt Swagger in C#-projecten gebruiken. 
  * Web-App en het API-App publiceren dialoogvensters zijn uitgebreid ter ondersteuning van de Azure portal concept van bron groeperen en selectie/maken van Azure-resourcegroepen en de App Service-abonnementen worden weergegeven in het nieuwe Web-Apps en API-App inrichten dialoogvenster. 
  * Azure API-App Server Explorer knooppunten bevatten koppelingen naar de dieptekoppeling naar de API-Apps in de Azure-portal, evenals andere functies zoals logboek Streaming en foutopsporing op afstand.
    
    Voor bekende problemen en de huidige beperkingen in Azure SDK .NET 2.5.1 [dit](app-service-release-notes.md#known_issues_2_5_1) hieronder.
* Nieuwe features\scenarios betrekking hebben op **HDInsight Tools** in Visual Studio zijn ingeschakeld in deze release. 
  
  * Lokale validatie van hive-scripts. Klik op de knop valideren script in de werkbalk om te zien of er fouten zijn opgetreden in het script. 
  * Verbeterd foutopsporing van Hive-taken. U kunt nu fouten opsporen Hive-taken via Yarn-Logboeken in Visual Studio. Als uw toepassing prestatieproblemen heeft, geven onderzoeken YARN-logboeken nuttige informatie...
  * (Openbare Preview) Sleutelwoord automatisch aanvullen en IntelliSense ondersteuning voor Hive. Bij het ontwerpen van Hive-scripts, HDInsight Tools voor Visual Studio toegevoegd sleutelwoord automatisch aanvullen en IntelliSense-ondersteuning voor Hive.
  * Storm-ondersteuning. U kunt nu HDInsight Tools voor Visual Studio gebruiken voor het ontwikkelen van Storm-topologieën/Spouts/Bolts in C#. U kunt de ontwikkelde topologie om een Storm-cluster te verzenden en de status bolt-topologie/spout zien. Kunt u het systeemlogboek in Logboeken en logboeken van de klant uw Storm-topologieën/Bolts/Spouts oplossen. U kunt ook bestaande JAVA-elementen in Storm op HDInsight.
    
    Zie voor meer informatie [aan de slag met HDInsight Hadoop-hulpprogramma's voor Visual Studio](../hdinsight/hadoop/apache-hadoop-visual-studio-tools-get-started.md).

## <a id="known_issues_2_5_1"></a>Azure SDK voor .NET 2.5.1 bekende problemen en beperkingen
* Azure API-Apps wordt weergegeven als een implementatiedoel voor mobiele Apps. Web Apps moet het enige doel voor Mobile Apps tot en met een latere release. 
* Azure API-App inrichten kan leiden tot succes maar afwisselend niet bijwerken van de voortgang in het venster Azure App Service-activiteit. Tijdelijke oplossing is om de status van de nieuwe Azure API-App in de Azure portal te controleren. 
* Bestand > Nieuw Project > API-App > F5 ervaring resulteert in een HTTP-fout, omdat er geen default/index.html. Tijdelijke oplossing is het handmatig Blader naar de URL /api/values. 
* Server Explorer pictogrammen weergegeven en met tussenpozen platte. Dit probleem wordt opgelost tegenover opnieuw te starten. 
* Als een uitzondering is opgetreden tijdens het inrichten van Web-App of API-App (zoals quotum overschreden fouten of dubbele naam van de Azure API-App-gateway), wordt in de fouten sommige onbewerkte JSON-tekst weergeven. 
* Maken van onregelmatige projecten problemen bij het Application Insights is ingeschakeld tijdens het project maken.
* In sommige gevallen kan de gegenereerde clientcode van Azure API-App ontbreekt naamruimten, ze moeten worden opgenomen handmatig (of automatisch geïmporteerd via Visual Studio-hints) voor de code kan worden gecompileerd. 
* Mobiele App projecten moeten worden gepubliceerd op web-apps, maar u moet een site die u als een mobiele App in de Azure portal hebt gemaakt, omdat een database is vereist voor mobiele App projecten kiezen. 
* De startpagina voor mobiele Apps wordt de term 'mobiele service' in plaats van 'mobiele apps' gebruikt. 
* Mobiele App-project maken kan een minuut duren maken. 
* Tijdens de API-App weer inrichten (in sommige gevallen) een fout van de Azure-API opgetreden bij het weergeven dat de machtigingen niet juist kunnen worden ingesteld terwijl de API-App juist is ingericht en klaar voor gebruik is. U kunt handmatig met de Azure portal machtigingen instellen.
* Application Insights wordt niet ondersteund voor sjablonen van de API-App en sjablonen van de mobiele App.
* API-App-projecten kunnen niet worden gebruikt in combinatie met Cloud Service-projecten.
* API-App-projectsjablonen zijn alleen beschikbaar in C#.
* API-App-verbruik via het snelmenu 'Azure API-App-Client toevoegen' wordt alleen ondersteund in C#.

