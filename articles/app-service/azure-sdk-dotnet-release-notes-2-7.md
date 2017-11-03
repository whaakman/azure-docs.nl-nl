---
title: Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen
description: Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 9a69253129cdedc4f5d7e736d5bd8d6a68f95a1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen
## <a name="overview"></a>Overzicht
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.7 release. 

Het document bevat ook de release-opmerkingen voor de Azure SDK voor .NET 2.7.1 release.

Azure SDK 2.7 wordt alleen ondersteund in Visual Studio 2015 en Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) is de laatste SDK voor Visual Studio 2012 ondersteund.

Zie voor gedetailleerde informatie over deze release [Azure SDK 2.7 aankondiging post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) en [Azure SDK 2.7.1 aankondiging post](http://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK voor .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Meld u aan de verbeteringen voor Visual Studio 2015
2.7 van Azure SDK voor Visual Studio 2015 ondersteunt de nieuwe functies voor identiteit in Visual Studio 2015.  Dit biedt ondersteuning voor de accounts toegang krijgen tot Azure via op rollen gebaseerd toegangsbeheer, Cloud Solution Providers, DreamSpark en andere typen account en abonnement.

Inbegrepen bij de Azure SDK 2.7 verbeteringen zijn alleen beschikbaar in Visual Studio 2015. Ondersteuning voor Visual Studio 2013 is opgenomen in de Azure SDK 2.7.1.

### <a name="mobile-sdk"></a>Mobiele SDK
Bijgewerkt **Mobile Apps** sjablonen in overeenstemming met nieuwste [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) en -instellingen te openen.

### <a name="service-bus"></a>Service Bus
Algemene oplossingen voor problemen en verbeteringen. Raadpleeg voor informatie over updates en functies, de opmerkingen bij de release van de meest recente [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
In deze release zijn de volgende updates zijn aangebracht. Deze updates zijn in preview. Zie voor meer informatie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108).

* Hive-grafieken voor Hive op Tez-taken
* Volledige Hive DML IntelliSense-ondersteuning
* Ondersteuning voor pig-sjabloon
* Storm-sjablonen voor Azure-services

#### <a name="breaking-changes"></a>Wijzigingen op te splitsen
* Oude **Storm** project moet worden bijgewerkt wanneer u deze versie van de hulpprogramma's. Zie voor meer informatie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108).
* Web Visual Studio Express is niet langer ondersteund. Zie voor meer informatie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Hulpprogramma's van Azure App Service
In deze release zijn de volgende updates zijn aangebracht in Web extra extensies. Zie voor meer informatie [dit](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog. 

* Ondersteuning voor DreamSpark-accounts die zijn toegevoegd
* Volledige wijziging Azure-hulpprogramma's die zijn aangebracht ter ondersteuning van de nieuwe Azure Resource Management API 's
* Ondersteuning voor Azure App Service toegevoegd aan [Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Bekende problemen
Web-App-implementatiesleuf knooppunten worden niet weergegeven onder het knooppunt sleuven in Server Explorer en Web-App-implementatiesleuf onderliggende knooppunten onder Cloud Explorer niet laden. Dit probleem is opgelost en voorbereid voor de volgende release van de SDK. 

### <a name="cloud_explorer"></a>Cloud Explorer voor Visual Studio 2015
Azure SDK 2.7 omvat Cloud Explorer voor Visual Studio 2015 waarmee u uw Azure-resources bekijken, controleren van de eigenschappen en uitvoeren van acties van de belangrijkste ontwikkelaars uit vanuit Visual Studio. 

Cloud explorer ondersteunt het volgende:

* Resourcegroep en het resourcetype weergaven van uw Azure-resources 
* Resources zoeken op naam (beschikbaar in de weergave van brontype)
* Ondersteuning voor abonnementen en -resources met rol gebaseerd toegangsbeheer (RBAC een) toegepast 
* Geïntegreerde actie deelvenster waarin ontwikkelaars gerichte acties specifiek zijn voor de geselecteerde resources. Bijvoorbeeld: externe foutopsporingsprogramma koppelen voor virtuele Machines die zijn gemaakt met behulp van de Resource Manager-Stack diagnostics-gegevens voor virtuele Machines, enzovoort weergeven.
* Geïntegreerde eigenschappen Configuratiescherm waarin ontwikkelaars gerichte eigenschappen vaak nodig tijdens ontwikkelen en testen 
* Snelle overschakelen van de account moet worden gebruikt bij het opsommen van de resources (opdracht instellingen op de werkbalk gebruiken) 
* Voor het filteren van de abonnementen voor gebruik bij het opsommen van de resources (opdracht instellingen op de werkbalk gebruiken) 
* Dieptekoppelingen naar de Azure-Portal voor het beheren van resources en resourcegroepen 

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
De Azure Resource Manager-hulpprogramma's zijn bijgewerkt om te werken met rollen gebaseerd toegangsbeheer (RBAC) en het nieuwe abonnement typen.  Die deel uitmaakt van deze wijzigingen, is de mogelijkheid nieuwe storage-accounts, naast de klassieke opslag gebruiken voor het opslaan van artefacten tijdens de implementatie.  

Als u een Azure-resourcegroepproject van een eerdere versie van de SDK met de SDK-2.7 gebruikt, wordt het script voor een nieuwe implementatie nodig om te implementeren met behulp van een nieuw opslagaccount in plaats van klassieke opslag.  U wordt gevraagd voordat de wijzigingen zijn aangebracht aan uw project in het nieuwe script toevoegen.  Het script van de oude naam wordt gewijzigd en moet u handmatig eventuele wijzigingen aanbrengen in het nieuwe script.

### <a name="storage-explorer-tools"></a>Hulpprogramma's voor Storage Explorer
* Ondersteuning voor het weergeven van toevoeg-Blobs. Meer informatie over de in [dit blogbericht](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Ondersteuning voor het weergeven van Premium Storage-accounts via Server Explorer. Server Explorer worden alleen weergegeven voor pagina-blobs voor premium-opslagaccounts omdat dit het enige ondersteunde type voor premium storage-accounts.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory-hulpprogramma's voor Visual Studio
Inleiding tot **Azure Data Factory-hulpprogramma's** voor Visual Studio. Hieronder vindt u de ingeschakelde functies. Zie [deze blog](http://go.microsoft.com/fwlink/?LinkId=617530) voor meer informatie.

* **Sjabloon gebaseerd ontwerpen**: gebruik geïntegreerd selecteren op basis van sjablonen, data movement sjablonen of sjablonen voor het implementeren van een oplossing end-to-end-integratie en aan de slag met Data Factory snel verwerken van gegevens. 
* **Integratie met Solution Explorer voor het ontwerpen en implementeren van de Data Factory-entiteiten**: maken en implementeren van de pijplijnen en gerelateerde entiteiten als Visual Studio-projecten. 
* **Integratie met de diagramweergave voor visual interactie bij het ontwerpen van**: pijplijnen en gegevenssets met hulp van de diagramweergave visueel te ontwerpen. 
* **Integratie met Server explorer voor het bekijken en interactie met de reeds geïmplementeerde entiteiten**: gebruikmaken van de Server Explorer gegevensfactory's bladeren al geïmplementeerd en de bijbehorende entiteiten. Een geïmplementeerde gegevensfactory of een entiteit (gegevenssets Pipeline, de gekoppelde Service) in uw project importeren. 
* **JSON bewerken met schema-validatie en geavanceerde intellisense**: efficiënt te configureren en bewerken van JSON-documenten van de Data Factory-entiteiten met uitgebreide intellisense en schema-validatie 
* **Publicatie van een omgeving met meerdere**: publiceren pijplijnen om te ontwikkelen, testen of Prod omgeving opgesteld door het maken van afzonderlijke configuratiebestanden voor elke omgeving.
* **Pig, Hive en .net op basis van gegevensverwerking ondersteuning**: ondersteuning voor Pig en Hive-Scripts in Data Factory-project. Ondersteuning voor C#-Project voor het beheren van .net verwijzende activiteit.

## <a name="azure-sdk-for-net-271"></a>Azure SDK voor .NET 2.7.1
De volgende sectie bevat updates die zijn geïntroduceerd in de Azure SDK voor .NET 2.7.1 release.

### <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
Zie voor meer uitleg over updates van HDInsight-hulpprogramma's gedetailleerde, [deze blog](http://go.microsoft.com/fwlink/?LinkId=623831).

* Hive-weergave van de Operator taak (een nieuwe functie)
  
    Meer informatie over uw Hive-query beter, is de functie Hive Operator-weergave toegevoegd. Als u wilt zien alle operators in een hoekpunt, dubbelklikt u op de hoekpunten van de taakgrafiek. Meer informatie over een bepaalde operator, houd de muis boven deze operator weer.
* Hive-fout-markering (een nieuwe functie)
  
    Zodat u de grammaticafouten weergeven onmiddellijk is de fout markering Hive-functie toegevoegd. Ook foutberichten zijn uitgebreid en u ziet nu gedetailleerde grammaticafouten onmiddellijk (totdat deze release, moest u een Hive-script voor het cluster te verzenden en wacht enige tijd voordat u meer informatie over het foutbericht).  
* Storm-topologie grafiek (een nieuwe functie)
  
    Visualiseren is heel belangrijk als u weten wilt of uw topologie werkt zoals verwacht. In deze release toegevoegd we visualisatie voor Storm-grafieken. U kunt de belangrijke metrische gegevens voor uw topologie visualiseren (bijvoorbeeld een kleur geeft weer een bepaalde Bolt 'bezet' is of niet). U kunt ook Dubbelklik de Bolt/Spout om meer details te bekijken.
* Ondersteuning voor HDInsight-clusters die zijn gemaakt in de Azure-Portal (een bug fix)
  
    U kunt nu Visual Studio gebruiken om te bekijken en verzenden van taken aan uw HDInsight-clusters ongeacht waar het cluster zijn gemaakt.
* Meer ondersteuning voor IntelliSense & sneller Hive-metagegevens bij het laden van (een verbetering)
  
    We hebben de IntelliSense verbeterd door meer gebruikersvriendelijker suggesties toe te voegen. Bijvoorbeeld, kan tabelalias nu ook worden voorgesteld in IntelliSense zodat u kunt eenvoudig uw query schrijven. Ook hebben we de Hive-metagegevens laden zodat het duurt slechts enkele seconden voor een lijst met alle databases, tabellen en kolommen van uw Hive-metastore verbeterd.

Zie voor meer uitleg over updates van HDInsight-hulpprogramma's gedetailleerde, [deze blog](http://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Verbeteringen in Visual Studio 2013
* Azure SDK 2.7.1 kunt Visual Studio 2013 voor toegang tot Azure-accounts en -abonnementen via op rollen gebaseerd toegangsbeheer, Cloud Solution Providers en Dreamspark.
* Met Azure SDK 2.7.1 is het nieuwe venster van de Cloud Explorer-hulpprogramma nu ook beschikbaar in Visual Studio 2013.

### <a name="known-issues"></a>Bekende problemen
Installatie van de Azure SDK 2.6 of 2.7.1 voor Visual Studio Community 2013 op een niet - Engelse OS wordt een waarschuwing weergegeven dat de Engelse en niet-Engelse resources van Visual Studio misschien overeen. Deze waarschuwing kan veilig worden genegeerd. Dit gebeurt alleen als de computer heeft geen een eerdere installatie van Visual Studio Community 2013 en u de SDK op een niet - Engelse OS installeert. De waarschuwing wordt weergegeven nadat het taalpakket geldt de RTM-resources voor Visual Studio, maar voordat deze Update 4 van toepassing is. De waarschuwing genegeerd, krijgt het taalpakket doorgaan en voltooien met het toepassen van de Update 4-versie van de inhoud van language pack.

LightSwitch-projecten zijn niet compatibile met deze release. Dit probleem is opgelost met de volgende release van de SDK.

## <a name="also-see"></a>Zie ook
[Azure SDK 2.7.1 aankondiging post](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 aankondiging post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Ondersteuning en buiten gebruik stellen informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

