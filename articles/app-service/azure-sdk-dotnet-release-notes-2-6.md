---
title: Azure SDK voor .NET 2.6 opmerkingen bij de Release
description: Azure SDK voor .NET 2.6 opmerkingen bij de Release
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: dfc1fe223dbff178c35a969e0273ed80fb4c8be9
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016417"
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK voor .NET 2.6 opmerkingen bij de Release
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.6 release. 

U kunt cloud service-toepassingen (PaaS) dat die gericht is op .NET 4.5.2 of .NET 4.6 voorwaarde dat u het doel van .NET Framework handmatig op de functie van Cloud Service installeren ontwikkelen met Azure SDK 2.6. Zie [.NET installeren op een Cloudservicerol](https://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Service Bus-updates
* Eventhubs: 
  
  * Nu kunt gerichte toegangsbeheer bij het verzenden van gebeurtenissen bij het blootstellen van aanvullende publisher-eindpunt voor Event Hubs.
  * Extra stabiliteit en verbetering toegevoegd aan de functie Event Hubs.
  * Ondersteuning van Amqp-protocol via WebSocket toe te voegen voor messaging en Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools voor Visual Studio-updates
* **IntelliSense uitbreiding**: externe metagegevens suggestie
  
    HDInsight Tools voor Visual Studio ondersteunt nu het ophalen van de externe metagegevens wanneer u het Hive-script bewerkt. Bijvoorbeeld, u kunt typen **selecteren * FROM** en alle tabelnamen van de worden weergegeven. De kolomnamen worden ook weergegeven nadat u een tabel hebt opgegeven.
* **Ondersteuning voor HDInsight emulator**
  
    HDInsight Tools voor Visual Studio ondersteunen nu verbinding maken met HDInsight emulator, zodat u kunt uw Hive-scripts lokaal ontwikkelen zonder eventuele kosten, klikt u vervolgens deze scripts op basis van uw HDInsight-clusters uitvoeren. 
  
    Raadpleeg voor meer informatie, [deze handleiding](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **Ondersteuning voor algemene Hadoop-clusters voor HDInsight Tools voor Visual Studio** (Preview)
  
    HDInsight Tools voor Visual Studio ondersteunen nu algemene Hadoop-clusters, zodat u HDInsight Tools voor Visual Studio gebruiken kunt om het volgende te doen:
  
  * verbinding maken met uw cluster. 
  * Hive-query met uitgebreide ondersteuning voor IntelliSense/automatisch aanvullen, schrijven 
  * alle taken in uw cluster met een intuïtieve gebruikersinterface weergeven. 
    
    Raadpleeg voor meer informatie, [deze handleiding](https://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>In-Role Cache-updates
* **In-Role Cache** is bijgewerkt voor het gebruik van **Microsoft Azure Storage SDK** versie 4.3. Tot op heden, de **In-Role Cache** is met behulp van Azure Storage SDK versie 1.7.
  
    Klanten met behulp van Azure SDK 2.5 of hieronder moet bijwerken naar de Azure SDK 2.6 en verplaatsen naar de nieuwe versie van Azure Storage-SDK. 
  
    Op dit moment is Azure Storage, versie 2011-08-18 gepland worden op 1 augustus 2016 verwijderd. Alle migraties van In-Role Cache van Azure SDK 2.5 of lager moeten naar 2.6 zijn voltooid op dit moment zijn opgelost. Zie voor meer informatie over het buiten gebruik stellen van Azure Storage, versie 2011-08-18 [Microsoft Azure Storage-Service versie verwijderen Update: uitbreiding van 2016](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Microsoft kondigt de 30 November 2016 buiten gebruik stellen voor Azure Managed Cache Service en Azure In-Role Cache. Het is raadzaam dat u naar Azure Cache voor Redis ter voorbereiding op buiten gebruik stellen migreren. Zie voor meer informatie over de datums en hulp bij de migratie [welke Azure Cache-aanbieding is juist voor mij?](../azure-cache-for-redis/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Hulpprogramma's voor Azure App Service
De volgende items zijn bijgewerkt in de Azure SDK 2.6-release.

* Azure-publicatie uitgebreid met Azure API Apps als een implementatiedoel.
* API-Apps inrichten functionaliteit waarmee gebruikers met een API-App maken en inrichten van functionaliteit.
* Server Explorer gewijzigd om nieuwe App Service-knooppunt met Web, mobiele Apps en API apps die zijn gegroepeerd op resourcegroep weer te geven.
* Toevoegen van Azure API App Client gebaar toegevoegd aan de meeste C# projecten waarmee automatisch genereren van Swagger-functionaliteit API Apps die worden uitgevoerd in Azure-abonnement van een gebruiker.
* Hulpprogramma's voor API-Apps en App Service-knooppunten in Server Explorer zijn alleen beschikbaar in Visual Studio 2013. 

## <a name="azure-resource-manager-tools-updates"></a>Updates van de Azure Resource Manager-hulpprogramma 's
De Azure resource manager-hulpprogramma's zijn bijgewerkt om op te nemen van sjablonen voor virtuele Machines, netwerken en opslag. De JSON bewerken is bijgewerkt om op te nemen van een nieuwe overzichtsweergave voor sjablonen en de mogelijkheid om te bewerken van de sjablonen met behulp van JSON-fragmenten. Sjablonen die zijn geïmplementeerd vanuit Visual Studio gebruiken een PowerShell-script dat bij het project, zodat alle wijzigingen aan het script wordt gebruikt door Visual Studio.

## <a name="diagnostics-improvements-for-cloud-services"></a>Verbeteringen voor diagnosefuncties in voor Cloud Services
Azure SDK 2.6 brengt terug ondersteuning voor het verzamelen van diagnostische logboeken in de Azure-rekenemulator en ze overbrengen naar opslag. Een diagnostische logboeken (inclusief Toepassingstracering Logboeken, Event Tracing voor Windows (ETW)-Logboeken, prestatiemeteritems, infrastructuur logboeken en windows-gebeurtenislogboeken) die worden gegenereerd wanneer de toepassing wordt uitgevoerd in de emulator kunt overbrengen naar opslag om te controleren of de logboekregistratie van diagnostische gegevens op uw lokale computer werkt. 

Het opslagaccount voor diagnostische gegevens kan nu worden opgegeven in de service (.cscfg) configuratiebestand gemakkelijker met diagnostische gegevens van andere storage-accounts voor verschillende omgevingen. Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in de Azure SDK 2.4 en Azure SDK 2.6 gewerkt. Zie voor meer informatie over het gebruik van de verbindingsreeks voor opslag van diagnostische gegevens en hoe dit invloed heeft op uw projecten [diagnostische gegevens configureren voor Azure Cloud Services](https://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
* De **implementatieprojecten van Cloud** projecttype beschikbaar in de Azure SDK 2.5 is gewijzigd in **Azure-resourcegroep**.
* **Implementatie-projecten in de cloud** type projecten in de Azure SDK 2.5 kan worden gebruikt in 2.6 maar mislukt de implementatie van de sjabloon vanuit Visual Studio. Implementeren met de PowerShell-script wordt nog steeds werken echter als deze eerder hebt gedaan.  Voor meer informatie over het gebruik van **implementatieprojecten van Cloud** in 2.6 Lees dit [boeken](https://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Bekende problemen
* Verzamelen van diagnostische logboeken in de emulator is, vereist een 64-bits besturingssysteem. Wanneer u een 32-bits besturingssysteem worden geen logboeken met diagnostische gegevens verzameld. Dit heeft geen invloed op alle andere functionaliteit van de emulator. 
* Azure SDK 2.6 uitgebracht op 29-4-2015 hadden twee problemen: 
  
  * Universele App kan niet worden geladen in Visual Studio 2015 als Azure SDK 2.6 is geïnstalleerd op de computer.
  * Foutopsporing in een Cloud Service-project in Visual Studio 2013 en Visual Studio 2015 waar Visual Studio niet meer reageert en loopt vast bij het weergeven van een dialoogvenster met het bericht 'Configureren diagnostische gegevens voor de emulator', mislukken.
    
    Een update van Azure SDK 2.6 is uitgebracht op 18-5/2015. De bijgewerkte versie is 2.6.30508.1601; bevat oplossingen voor twee problemen die hierboven worden beschreven. U kunt identificeren aan de build van de SDK van het Configuratiescherm -> programma's en onderdelen -> Microsoft Azure-hulpprogramma's voor Microsoft Visual Studio 2013 – v 2.6. De kolom versie wordt weergegeven het build-nummer.
    
    Als u nog steeds de bovenstaande problemen ondervindt, installeert u de nieuwste versie van de Azure 2.6 SDK voor [VS 2012](https://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](https://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) of [VS 2015](https://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Zie ook
[Ondersteuning en informatie voor het buiten gebruik stellen voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

