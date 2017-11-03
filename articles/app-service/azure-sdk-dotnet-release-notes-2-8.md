---
title: Azure SDK voor .NET 2.8 Release-opmerkingen
description: Azure SDK voor .NET 2.8 Release-opmerkingen
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 0b9f55d69c824e86245738a082f95fc529583f58
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK voor .NET 2.8, 2.8.1 en 2.8.2
## <a name="overview"></a>Overzicht
In dit artikel bevat de releaseopmerkingen (met bekende problemen en grote wijzigingen) voor de Azure SDK voor .NET 2.8, 2.8.1 en 2.8.2 worden vrijgegeven. 

Zie voor een volledige lijst met nieuwe functies en wijzigingen in deze release de [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) aankondiging. 

## <a name="azure-sdk-for-net-28"></a>Azure SDK voor .NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>Download de Azure SDK voor .NET 2.8
[Azure SDK voor .NET 2.8 voor Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK voor .NET 2.8 voor Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Ondersteuning voor .NET 4.5.2
#### <a name="known-issues"></a>Bekende problemen
Azure .NET SDK 2.8 kunt u .NET 4.5.2 maken Cloudservice pakketten. Echter 4.5.2 .NET framework wordt niet geïnstalleerd op de standaard installatiekopieën van het Gastbesturingssysteem vanaf januari 2016 Gastbesturingssysteem release. Voordat u met .NET 4.5.2 framework is beschikbaar via een afzonderlijke Gastbesturingssysteem releaseversie – November 2015-02. Zie de [Azure Gast OS Releases en SDK compatibiliteit Matrix](../cloud-services/cloud-services-guestos-update-matrix.md) pagina om te controleren wanneer de installatiekopie worden uitgebracht.  Zodra de November 2015-02-afbeelding wordt vrijgegeven kunt u de afbeelding te gebruiken die door uw Service in de Cloud-configuratiebestand (.cscfg)-bestand bij te werken. In de serviceconfiguratie bestand het kenmerk van het element serviceconfiguration zijn ingesteld op de tekenreeks "WA-GUEST-OS-4.26_201511-02 '. Als u opt-in deze installatiekopie te gebruiken en u geen automatische updates voor het Gastbesturingssysteem ontvangt. De automatische updates downloaden voor de osVersion moet worden ingesteld op ' * ' en .NET 4.5.2 is alleen beschikbaar via Automatische updates van januari 2016.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Bekende problemen
Tijdens een **Data Factory sjabloon** project maken met betrekking tot voorbeeldgegevens azure power shell-script kan mislukken als azure power shell-versie is geïnstalleerd op de computer na 0.9.8.

Om dit type project maken, moet u [azure power shell-versie 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
#### <a name="breaking-changes"></a>Wijzigingen op te splitsen
Het PowerShell-script dat is geleverd door de Azure-resourcegroepproject is bijgewerkt in deze versie om te werken met de nieuwe versie 1.0 van de Azure PowerShell-cmdlets.  Dit nieuwe script werkt niet uit met Visual Studio wanneer u een versie van de SDK voordat 2.8.  

Scripts van de projecten die zijn gemaakt in eerdere versies van de SDK wordt niet uitvoeren vanuit Visual Studio bij gebruik van de SDK 2.8.  Alle scripts blijven werken buiten Visual Studio met de juiste versie van de Azure PowerShell-cmdlets.  

De SDK 2.8 vereist versie 1.0 van de Azure PowerShell-cmdlets.  Alle andere versies van de SDK moeten versie 0.9.8 van de Azure PowerShell-cmdlets.  Zie voor meer informatie [dit](http://go.microsoft.com/fwlink/?LinkID=623011) blog.

### <a name="web-tools-extensions"></a>Web hulpprogramma's voor uitbreidingen
#### <a name="known-issues"></a>Bekende problemen
De volgende bekende problemen in de volgende release verholpen.

* App Service gerelateerde Cloud en het Server Explorer gebaar voor niet-productieve omgevingen (zoals Azure China of Azure Stack klanten) werken niet. Voor klanten in de volgende gebieden betrokken kunt het publicatieprofiel downloaden van de Azure-portal publiceren mogelijkheid. Een toekomstige release wordt gebaren zoals 'Foutopsporingsprogramma koppelen' en 'Weergave Streaming Logs' herstellen voor Azure China en Stack-klanten. 
* Klanten kunnen fouten tijdens het App-Service gemaakt wanneer de App Insights-instantie aan die ze implementeert een andere regio dan VS-Oost wordt zien. In deze situaties wordt een App Service maken in de portal en het downloaden van het publicatieprofiel publishing scenario's mogelijk. 

### <a name="azure-hdinsight-tools"></a>Azure HDInsight-hulpprogramma 's
#### <a name="new-updates"></a>Nieuwe updates
* U kunt uw Hive-query uitvoeren in het cluster via HiveServer2 met bijna geen overhead en de taak wordt geregistreerd in realtime zien.
* De nieuwe Hive Taakuitvoeringsweergave kunt u uw taak diepere nader meer informatie en potentiële problemen te identificeren.

Zie voor informatie [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK voor .NET 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekende problemen voor Visual Studio 2013 en Visual Studio 2015
1. Geactiveerde webtaak publiceert naar sleuven wordt weergeven en de fout en won't set een planning, maar het zendt de webtaak naar Azure. Klanten die worden een geplande taak moeten kunnen vervolgens de Azure Portal gebruiken voor het instellen van de planning voor de webtaak. 
2. Python-klanten kunnen foutopsporingsprogramma problemen. Service-team rolt een oplossing voor dit maar als klanten worden beïnvloed, laat Microsoft in de forums of op de blog aankondiging weet of het gedeelte met opmerkingen opmerkingen bij de release. 
3. Klanten in bepaalde regio's (zoals Zuid-India) krijgen met App Service-fouten bij het inrichten. Dit komt overeen met de portal en klanten die met dit probleem kunnen de Azure portal gebruiken om aan te vragen toegang tot het publiceren naar deze geo-regio's. Nadat ze toegang vragen tot deze regio's met behulp van moet de Azure portal inrichting werken. 

## <a name="azure-sdk-for-net-282"></a>Azure SDK voor .NET 2.8.2
Na de installatie van de 2.8.2 hulpmiddelen voor klanten het volgende probleem kunnen optreden.         

* Als u van Windows 10 gebruikmaakt en Internet Explorer niet hebt geïnstalleerd, verschijnt het foutbericht 'Internet Explorer kan niet worden gevonden'.
  U lost het probleem Installeer Internet Explorer met behulp van het dialoogvenster Windows-onderdelen toevoegen of verwijderen.

Als u dit probleem ziet, moet u de verzenden een glimlach-functie gebruiken om dit te melden.

Zie voor meer informatie [dit](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) plaatsen.

## <a name="other-updates"></a>Andere Updates
Zie voor andere updates [Azure SDK 2.8 aankondiging post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Zie ook
[Azure SDK 2.8 aankondiging post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Ondersteuning en buiten gebruik stellen informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx)

