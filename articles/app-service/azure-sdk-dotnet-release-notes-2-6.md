---
title: Azure SDK voor .NET 2.6 Release-opmerkingen
description: Azure SDK voor .NET 2.6 Release-opmerkingen
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 21817b09440fc98a54dc45c9129d104b01fa387d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK voor .NET 2.6 Release-opmerkingen
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.6-release. 

U kunt service cloudtoepassingen (PaaS) op voorwaarde dat u het doel van .NET Framework handmatig op de functie voor Cloud-Service installeren die gericht is op .NET 4.5.2 of .NET 4.6 ontwikkelen met Azure SDK 2.6. Zie [.NET installeren op een Cloud Service-functie](http://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Service Bus-updates
* Event Hubs: 
  
  * Nu kunt gerichte toegangsbeheer bij het verzenden van gebeurtenissen bij het blootstellen van aanvullende publisher eindpunt voor Event Hubs.
  * Als u meer stabiliteit en verbetering toegevoegd aan de functie Event Hubs.
  * Ondersteuning van Amqp-protocol toe te voegen via WebSocket voor messaging en Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools voor Visual Studio-updates
* **IntelliSense verbetering**: externe metagegevens suggestie
  
    HDInsight Tools voor Visual Studio ondersteunt nu ophalen van externe metagegevens wanneer u uw Hive-script bewerkt. U kunt bijvoorbeeld **Selecteer * FROM** en alle tabelnamen van de wordt weergegeven. De kolomnamen worden ook weergegeven na het opgeven van een tabel.
* **HDInsight-emulator ondersteuning**
  
    HDInsight Tools voor Visual Studio ondersteunen nu verbinding maken met HDInsight-emulator, zodat u kan uw Hive-scripts lokaal ontwikkelt zonder eventuele kosten, vervolgens deze scripts uitvoeren op uw HDInsight-clusters wordt uitgevoerd. 
  
    Raadpleeg voor meer informatie [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **HDInsight Tools voor Visual Studio-ondersteuning voor algemene Hadoop-clusters** (Preview)
  
    HDInsight Tools voor Visual Studio ondersteunen nu algemeen Hadoop-clusters, zodat u de HDInsight Tools voor Visual Studio gebruiken kunt het volgende doen:
  
  * verbinding maken met uw cluster 
  * Hive-query met verbeterde ondersteuning voor IntelliSense/automatisch aanvullen, schrijven 
  * alle taken in het cluster met een intuïtieve gebruikersinterface weergeven. 
    
    Raadpleeg voor meer informatie [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Updates in-Role Cache
* **In-Role Cache** is bijgewerkt voor het gebruik van **Microsoft Azure-opslag-SDK** versie 4.3. Tot nu de **In-Role Cache** Azure-opslag-SDK versie 1.7 werd gebruikt.
  
    Klanten met behulp van Azure SDK 2.5 of hieronder moet bijwerken naar Azure SDK 2.6 en verplaatsen naar de nieuwe versie van Azure-opslag-SDK. 
  
    Op dit moment is Azure Storage versie 2011-08-18 gepland voor 1 augustus 2016 verwijderd. Alle migraties van In-Role Cache van de Azure SDK 2.5 of hieronder moeten 2.6 zijn voltooid op dit tijdstip. Zie voor meer informatie over het buiten gebruik stellen van Azure Storage versie 2011-08-18 [verwijderen Update voor de versie van Microsoft Azure Storage Service: uitbreiding van 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Hier kondigen we de 30 November 2016 buiten gebruik stellen voor Azure Managed Cache Service en Azure In-Role Cache. Het is raadzaam om te migreren naar Azure Redis-Cache in voorbereiding op deze buiten gebruik stellen. Zie voor meer informatie over datums en migratie richtlijnen [die Azure Cache aanbieding is geschikt voor mij?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Hulpprogramma's van Azure App Service
De volgende items zijn bijgewerkt in de Azure SDK 2.6-release.

* Azure publishing uitgebreid met Azure API-Apps als een implementatiedoel.
* API-Apps inrichten functionaliteit waarmee gebruikers met een API-App maken en inrichten van functionaliteit.
* Server Explorer aangepast aan de nieuwe App Service-knooppunt met mobiele, Web en API apps, gegroepeerd per resourcegroep.
* Azure API-App-Client gebaar toegevoegd aan de meeste C#-projecten waarmee automatisch genereren van Swagger ingeschakeld API Apps die worden uitgevoerd in Azure-abonnement van een gebruiker toevoegen.
* API-Apps tooling en App Service-knooppunten in Server Explorer zijn alleen beschikbaar in Visual Studio 2013. 

## <a name="azure-resource-manager-tools-updates"></a>Azure Resource Manager-hulpprogramma's zijn bijgewerkt
De Azure resource manager-hulpprogramma's zijn bijgewerkt om op te nemen van sjablonen voor virtuele Machines, netwerken en opslag. De bewerkingsfuncties JSON is bijgewerkt met een nieuwe overzichtsweergave voor sjablonen en de mogelijkheid om te bewerken van de sjablonen met behulp van de JSON-codefragmenten. Sjablonen die zijn geïmplementeerd vanuit Visual Studio gebruiken een PowerShell-script dat is opgegeven met het project, zodat alle wijzigingen aan het script wordt gebruikt door Visual Studio.

## <a name="diagnostics-improvements-for-cloud-services"></a>Diagnostische gegevens verbeteringen voor Cloudservices
Azure SDK 2.6 brengt terug ondersteuning voor het verzamelen van diagnostische logboeken in de Azure-rekenemulator en overbrengt naar opslag ontwikkeling. Diagnostische logboeken (inclusief application trace Logboeken, het Event Tracing voor Windows (ETW)-Logboeken, prestatiemeteritems, infrastructuur logboeken en windows-gebeurtenislogboeken) gegenereerd wanneer de toepassing wordt uitgevoerd in de emulator kan worden overgebracht naar ontwikkeling opslag om te controleren of de logboekregistratie van diagnostische gegevens op uw lokale computer werkt. 

Het opslagaccount voor diagnostische gegevens kan nu worden opgegeven in het servicebestand serviceconfiguratie (.cscfg) om de verschillende diagnostics storage-accounts gebruiken voor verschillende omgevingen te vereenvoudigen. Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in de Azure SDK 2.4 en Azure SDK 2.6 gegaan. Zie voor meer informatie over het gebruik van de opslagverbindingsreeks diagnostische gegevens en hoe dit heeft gevolgen voor uw projecten [diagnostische gegevens configureren voor Azure Cloud Services](http://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Wijzigingen op te splitsen
### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
* De **Cloud implementatieprojecten** projecttype beschikbaar in de Azure SDK 2.5 is gewijzigd in **Azure-resourcegroep**.
* **Cloud implementatieprojecten** type projecten gemaakt in de Azure SDK 2.5 kan worden gebruikt in 2.6 maar mislukt de implementatie van de sjabloon vanuit Visual Studio. Echter wordt implementeren met het PowerShell-script nog steeds werken zoals eerder.  Voor informatie over het gebruik van **Cloud implementatieprojecten** in 2.6 Lees dit [boeken](http://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Bekende problemen
* Verzamelen van diagnostische logboeken in de emulator vereist een 64-bits besturingssysteem. Diagnostische logboeken worden niet verzameld wanneer op een 32-bits besturingssysteem wordt uitgevoerd. Dit heeft geen invloed op een andere functionaliteit van de emulator. 
* Azure SDK 2.6 uitgebracht op 4-29-2015 was twee problemen: 
  
  * Universele App kan niet worden geladen in Visual Studio 2015 als Azure SDK 2.6 is geïnstalleerd op de computer.
  * Foutopsporing in een Cloud Service-project mislukken in Visual Studio 2013 en Visual Studio 2015, Visual Studio niet meer reageert waarbij loopt vast bij het weergeven van een dialoogvenster met het bericht 'Configuring diagnostische gegevens voor de emulator'.
    
    Een update voor de Azure SDK 2.6 is uitgebracht op 5-18-2015. De bijgewerkte versie is 2.6.30508.1601; het bevat oplossingen voor twee problemen die hierboven worden beschreven. U kunt identificeren aan de build van de SDK van het Configuratiescherm -> programma's en onderdelen -> Microsoft Azure-hulpprogramma's voor Microsoft Visual Studio 2013 – v 2.6. De kolom versie wordt weergegeven het build-nummer.
    
    Als u de bovenstaande problemen nog steeds worden geconfronteerd, installeer de nieuwste versie van de 2.6 Azure SDK voor [tegenover 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) of [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Zie ook
[Ondersteuning en buiten gebruik stellen informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

