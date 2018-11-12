---
title: Azure SDK voor .NET 2.8 opmerkingen bij de Release
description: Azure SDK voor .NET 2.8 opmerkingen bij de Release
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 6aa2684a900dffecd481d51b8876b0e674c1a6ea
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235527"
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK voor .NET 2.8, 2.8.1 en 2.8.2
## <a name="overview"></a>Overzicht
In dit artikel bevat de opmerkingen bij de release (met bekende problemen en wijzigingen die fouten veroorzaken) voor de Azure SDK voor .NET 2.8, 2.8.1 en 2.8.2 worden vrijgegeven. 

Zie voor een volledig overzicht van nieuwe functies en wijzigingen in deze release de [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) aankondiging. 

## <a name="azure-sdk-for-net-28"></a>Azure SDK voor .NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>Download de Azure SDK voor .NET 2.8
[Azure SDK voor .NET 2.8 voor Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK voor .NET 2.8 voor Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Ondersteuning voor .NET 4.5.2
#### <a name="known-issues"></a>Bekende problemen
Azure .NET SDK 2.8 kunt u .NET 4.5.2 maken Cloud Service-pakketten. Echter 4.5.2 .NET framework niet geïnstalleerd op de Gast-OS-installatiekopieën tot januari 2016 Guest OS release standaard. Voordat u met .NET 4.5.2 framework zijn beschikbaar via een afzonderlijke Gastbesturingssysteem releaseversie – November 2015-02. Zie de [Azure Guest OS Releases en SDK Compatibility Matrix](../cloud-services/cloud-services-guestos-update-matrix.md) pagina om bij te houden wanneer de installatiekopie wordt uitgebracht.  Zodra de November 2015-02-installatiekopie wordt vrijgegeven kunt u het gebruik van die installatiekopie door het bijwerken van uw Cloud Service-configuratiebestand (.cscfg)-bestand. In de serviceconfiguratie bestand het kenmerk voor de osVersion van het element ServiceConfiguration ingesteld op de tekenreeks "WA-GUEST-OS-4.26_201511-02". Als u ervoor kiest om u te registreren voor het gebruik van deze afbeelding en vervolgens u geen automatische updates op het Gastbesturingssysteem krijgt. Om op te halen van de automatische updates de osVersion moeten worden ingesteld op ' * ' en .NET 4.5.2 is alleen beschikbaar via Automatische updates in januari 2016.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Bekende problemen
Tijdens een **sjabloon voor Data Factory** project maken met betrekking tot-voorbeeldgegevens, azure power shell-script kan mislukken als azure power shell-versie is geïnstalleerd op de computer na versie 0.9.8.

Als u wilt maken van dit type project, moet u [azure power shell-versie 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
#### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
De PowerShell-script dat is opgegeven door de Azure-resourcegroep-project is bijgewerkt in deze versie om te werken met de nieuwe versie 1.0 van de Azure PowerShell-cmdlets.  Met dit nieuwe script werkt niet in met Visual Studio bij het gebruik van een versie van de SDK voordat 2.8.  

Scripts uit projecten die zijn gemaakt in eerdere versies van de SDK wordt niet uitvoeren in Visual Studio bij het gebruik van de SDK 2.8.  Alle scripts blijven werken buiten Visual Studio met de juiste versie van de Azure PowerShell-cmdlets.  

De SDK 2.8 vereist versie 1.0 van de Azure PowerShell-cmdlets.  Alle andere versies van de SDK vereisen-versie 0.9.8 van de Azure PowerShell-cmdlets.  Zie voor meer informatie [dit](https://go.microsoft.com/fwlink/?LinkID=623011) blog.

### <a name="web-tools-extensions"></a>Web-hulpprogramma's extensies
#### <a name="known-issues"></a>Bekende problemen
De volgende bekende problemen wordt opgelost in de volgende release.

* App Service gerelateerde Cloud en Server Explorer gebaar voor niet-productieomgevingen (zoals Azure China of Azure Stack-klanten) werken niet. Voor klanten in deze getroffen gebieden kunt het publicatieprofiel downloaden vanuit de Azure-portal publiceren mogelijkheid. Een toekomstige release zal gebaren zoals "Foutopsporingsprogramma koppelen" en 'Streaminglogboeken bekijken' herstellen voor Azure China en de Stack-klanten. 
* Fouten tijdens het App Service gemaakt wanneer de App Insights-instantie die ze implementeren in een andere regio dan VS-Oost is mogelijk zichtbaar voor klanten. In deze scenario's wordt het maken van een App Service in de portal en het downloaden van het publicatieprofiel publishing scenario's inschakelen. 

### <a name="azure-hdinsight-tools"></a>Azure HDInsight-hulpprogramma 's
#### <a name="new-updates"></a>Nieuwe updates
* U kunt uw Hive-query uitvoeren in het cluster via HiveServer2 bijna zonder overhead en de taak beschikbaar zijn in realtime.
* Met behulp van de nieuwe Hive Taakuitvoeringsweergave kunt u uw taak diepere Bestudeer, Kijk voor meer informatie en om mogelijke problemen te identificeren.

Zie voor meer informatie, [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK voor .NET 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekende problemen voor Visual Studio 2013 en Visual Studio 2015
1. Geactiveerde webtaak publiceert naar sleuven zal weergeven en de fout en wordt niet een schema instellen, maar deze stuurt de webtaak naar Azure. Klanten die die moeten worden een geplande taak zijn kunnen vervolgens de Azure Portal gebruiken voor het instellen van het schema voor de webtaak. 
2. Python-klanten kunnen het foutopsporingsprogramma problemen optreden. Service-team een oplossing is beschikbaar voor dit maar als klanten worden beïnvloed, laat Microsoft weten wat in de forums of in de aankondigingsblog of release-opmerkingen bij de sectie met opmerkingen. 
3. Klanten in bepaalde regio's (zoals Zuid-India) krijgen met App Service-fouten bij het inrichten. Dit komt overeen met de portal en klanten die dit probleem doet zich de Azure-portal kunnen gebruiken voor het aanvragen van toegang tot het publiceren naar deze geo-regio's. Nadat zij aanvragen toegang tot deze regio's met behulp van de Azure portal provisioning kunnen worden gebruikt. 

## <a name="azure-sdk-for-net-282"></a>Azure SDK voor .NET 2.8.2
Na de installatie van de 2.8.2 hulpprogramma's, klanten het volgende probleem kunnen optreden.         

* Als u van Windows 10 gebruikmaakt en Internet Explorer niet hebt geïnstalleerd, krijgt u mogelijk een 'Internet Explorer kan niet worden gevonden'-fout.
  Los het probleem, installeer Internet Explorer met behulp van het dialoogvenster Windows-onderdelen toevoegen of verwijderen.

Als u dit probleem ziet, gebruikt u de functie voor Send a smile te rapporteren.

Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) plaatsen.

## <a name="other-updates"></a>Andere Updates
Zie voor andere updates [Azure SDK 2.8 aankondiging post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Zie ook
[Azure SDK 2.8 aankondiging post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Ondersteuning en informatie voor het buiten gebruik stellen voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx)

