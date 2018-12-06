---
title: Azure SDK voor .NET 2.7 en .NET 2.7.1 opmerkingen bij de Release
description: Azure SDK voor .NET 2.7 en .NET 2.7.1 opmerkingen bij de Release
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 4c5a4dfcdde91d1bd0c2728ff9d071d4c2f73f0e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969767"
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK voor .NET 2.7 en .NET 2.7.1 opmerkingen bij de Release
## <a name="overview"></a>Overzicht
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.7 release. 

Het document bevat ook de opmerkingen bij de release voor de Azure SDK voor .NET 2.7.1 release.

Azure SDK 2.7 wordt alleen ondersteund in Visual Studio 2015 en Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) is de laatste SDK voor Visual Studio 2012 ondersteund.

Zie voor gedetailleerde informatie over deze release [Azure SDK 2.7 aankondiging post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) en [Azure SDK 2.7.1 aankondiging post](https://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK voor .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Meld u aan verbeteringen voor Visual Studio 2015
Azure SDK 2.7 voor Visual Studio 2015 biedt ondersteuning voor de nieuwe functies voor identiteit in Visual Studio 2015.  Dit biedt ondersteuning voor accounts die toegang tot Azure via op rollen gebaseerd toegangsbeheer, Cloud Solution Providers, DreamSpark en andere typen-account en abonnement.

Opgenomen in de Azure SDK 2.7 verbeteringen zijn alleen beschikbaar in Visual Studio 2015. Ondersteuning voor Visual Studio 2013 is opgenomen in de Azure SDK 2.7.1.

### <a name="mobile-sdk"></a>Mobiele SDK
Bijgewerkt **Mobile Apps** sjablonen in overeenstemming met nieuwste [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) en -instellingen te openen.

### <a name="service-bus"></a>Service Bus
Algemene fouten opgelost en verbeteringen. Raadpleeg voor informatie over updates en functies, de opmerkingen bij de release van de meest recente [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
In deze release zijn de volgende updates zijn aangebracht. Deze updates zijn in preview. Zie voor meer informatie, [deze blog](https://go.microsoft.com/fwlink/?LinkId=619108).

* Hive-grafieken voor Hive op Tez-taken
* Volledige ondersteuning van Hive DML-IntelliSense
* Ondersteuning voor pig-sjabloon
* Storm-sjablonen voor Azure-services

#### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
* Oude **Storm** project moet worden bijgewerkt wanneer u deze versie van de hulpprogramma's. Zie voor meer informatie, [deze blog](https://go.microsoft.com/fwlink/?LinkId=619108).
* Visual Studio Web Express wordt niet meer ondersteund. Zie voor meer informatie, [deze blog](https://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Hulpprogramma's voor Azure App Service
In deze release zijn de volgende updates zijn aangebracht in extensies voor Web-hulpprogramma's. Zie voor meer informatie [dit](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog. 

* Ondersteuning voor DreamSpark-accounts die zijn toegevoegd
* Volledige Azure-hulpprogramma's gewijzigd voor de ondersteuning van de nieuwe Azure Resource Management API 's
* Ondersteuning voor Azure App Service toegevoegd aan [Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Bekende problemen
Web-App-implementatie sleuf knooppunten worden niet weergegeven onder het knooppunt sleuven in Server Explorer en Web-App-implementatie sleuf onderliggende knooppunten onder Cloud Explorer niet laden. Dit probleem is opgelost en voorbereid voor de volgende release van SDK. 

### <a name="cloud_explorer"></a>Cloud Explorer voor Visual Studio 2015
Azure SDK 2.7 bevat Cloud Explorer voor Visual Studio 2015 waarmee u uw Azure-resources bekijken, controleren van de eigenschappen en voert u belangrijke ontwikkelaar acties op basis van Visual Studio. 

Cloud explorer ondersteunt het volgende:

* Resourcegroep en het resourcetype weergaven van uw Azure-resources 
* Zoeken naar resources met de naam (beschikbaar in de weergave van de Resource-Type)
* Ondersteuning voor abonnementen en resources waarvoor rollen gebaseerd toegangsbeheer (RBAC) toegepast 
* Geïntegreerde deelvenster actie aan die ontwikkelaars acties specifiek zijn voor de geselecteerde resources bevat. Bijvoorbeeld: extern foutopsporingsprogramma koppelen voor virtuele Machines die zijn gemaakt met behulp van de Resource Manager-Stack, diagnostische gegevens voor virtuele Machines enz weergeven.
* Geïntegreerde deelvenster met eigenschappen geeft ontwikkelaars eigenschappen vaak nodig tijdens het ontwikkelen en testen 
* Snel overschakelen van het account moet worden gebruikt bij het inventariseren van resources (opdracht instellingen op de werkbalk gebruiken) 
* Het filteren van het abonnement moet worden gebruikt bij het inventariseren van resources (opdracht instellingen op de werkbalk gebruiken) 
* Dieptekoppelingen naar de Azure-Portal voor het beheren van resources en resourcegroepen 

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's
De Azure Resource Manager-hulpprogramma's zijn bijgewerkt om te werken met op rollen gebaseerd toegangsbeheer (RBAC) en nieuwe abonnementstypen.  Met deze wijzigingen worden opgenomen, is de mogelijkheid nieuwe storage-accounts, naast klassieke opslag gebruiken voor het opslaan van artefacten tijdens de implementatie.  

Als u een Azure-resourcegroep-project van een eerdere versie van de SDK gebruikt met de SDK 2.7, wordt een nieuwe implementatiescript nodig om te implementeren met behulp van een nieuw opslagaccount in plaats van klassieke opslag.  U wordt gevraagd voordat wijzigingen worden aangebracht aan uw project naar het nieuwe script toevoegen.  De naam van het oude script zal worden gewijzigd en moet u handmatig geen wijzigingen aanbrengen in het nieuwe script.

### <a name="storage-explorer-tools"></a>Hulpprogramma's van Storage Explorer
* Ondersteuning voor het weergeven van toevoeg-Blobs. Meer informatie in [dit blogbericht](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Ondersteuning voor het weergeven van Premium Storage-accounts via Server Explorer. Server Explorer wordt alleen weergegeven voor pagina-blobs voor premium storage-accounts als ze het enige ondersteunde type voor premium storage-accounts zijn.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory-hulpprogramma's voor Visual Studio
Maak kennis met **Azure Data Factory-hulpprogramma's** voor Visual Studio. Hieronder vindt u de functies zijn ingeschakeld. Zie [deze blog](https://go.microsoft.com/fwlink/?LinkId=617530) voor meer informatie.

* **Sjabloon op basis van authoring**: gebruik indeling selecteren op basis van sjablonen, data movement sjablonen of sjablonen implementeren een end-to-end oplossing voor gegevensintegratie en aan de slag met Data Factory snel praktische gegevensverwerking. 
* **Integratie met Solution Explorer voor het ontwerpen en implementeren van Data Factory-entiteiten**: maken en implementeren van pijplijnen en gerelateerde entiteiten als Visual Studio-projecten. 
* **Integratie met diagramweergave voor visuele interactie bij het ontwerpen van**: pijplijnen en gegevenssets met steun van de diagramweergave visueel ontwerpen. 
* **Integratie met Server explorer voor het bekijken en interactie met de reeds geïmplementeerde entiteiten**: gebruikmaken van de Server Explorer om te bladeren al geïmplementeerd data factory's en bijbehorende entiteiten. Een geïmplementeerde data factory of een entiteit (Pipeline, gekoppelde Service, gegevenssets) importeren in uw project. 
* **JSON bewerken met schema-validatie en uitgebreide intellisense**: efficiënt te configureren en JSON-documenten van Data Factory-entiteiten met uitgebreide validatie van intellisense en het schema bewerken 
* **Meerdere omgeving publiceren**: publiceren die leiden tot dev-, test- of Prod omgeving opgesteld door het maken van afzonderlijke configuratiebestanden voor elke omgeving.
* **Pig, Hive en .net op basis van ondersteuning voor gegevensverwerking**: ondersteuning voor Pig en Hive-Scripts in Data Factory-project. Ondersteuning voor verwijzende C# Project voor het beheren van .net activiteit.

## <a name="azure-sdk-for-net-271"></a>Azure SDK voor .NET 2.7.1
De volgende sectie bevat updates die zijn geïntroduceerd in de Azure SDK voor .NET 2.7.1 release.

### <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
Zie voor meer informatie over updates voor HDInsight-hulpprogramma's gedetailleerde, [deze blog](https://go.microsoft.com/fwlink/?LinkId=623831).

* Hive-weergave van de Operator taak (een nieuwe functie)
  
    Meer inzicht in uw Hive-query beter, is de functie voor weergave van Hive Operator toegevoegd. Als u wilt zien van alle operators in een hoekpunt, dubbelklikt u op de hoekpunten van de taakgrafiek. Voor meer informatie van een bepaalde operator, houd de muis boven deze operator.
* Hive-fout-markering (een nieuwe functie)
  
    Om te bekijken van de grammaticafouten onmiddellijk is de Hive-fout markering-functie toegevoegd. Ook foutberichten zijn uitgebreid en u kunt gedetailleerde grammaticafouten nu direct zien (tot en met deze release, had u indienen van een Hive-script aan het cluster en wacht enige tijd voordat u meer informatie over het foutbericht).  
* Storm-topologie Graph (een nieuwe functie)
  
    Het is heel belangrijk om te visualiseren wanneer u wenst te zien of uw topologie werkt zoals verwacht. In deze release hebben we visualisatie voor Storm grafieken toegevoegd. U kunt de belangrijke metrische gegevens visualiseren voor uw topologie (bijvoorbeeld een kleur geeft weer een bepaalde Bolt 'bezet' is of niet). U kunt ook dubbelklikt u op de Bolt/Spout om meer details weer te geven.
* Ondersteuning voor HDInsight-clusters die zijn gemaakt in de Azure-Portal (een opgelost probleem)
  
    U kunt nu Visual Studio gebruiken om te bekijken en verzenden van taken met uw HDInsight-clusters, ongeacht waar het cluster zijn gemaakt.
* Meer ondersteuning voor IntelliSense en snellere Hive-metagegevens van het laden van (een verbetering)
  
    We hebben de IntelliSense verbeterd door meer beschrijvende suggesties toe te voegen. Bijvoorbeeld, kan tabelalias nu ook worden voorgesteld in IntelliSense, zodat u uw query eenvoudiger kunt schrijven. We hebben ook de Hive-metagegevens laden, zodat het duurt slechts enkele seconden aan de lijst met alle databases, tabellen en kolommen in uw Hive-metastore verbeterd.

Zie voor meer informatie over updates voor HDInsight-hulpprogramma's gedetailleerde, [deze blog](https://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Verbeteringen in Visual Studio 2013
* Azure SDK 2.7.1 kunnen Visual Studio 2013 voor toegang tot Azure-accounts en abonnementen via op rollen gebaseerd toegangsbeheer, Cloud Solution Providers en Dreamspark.
* Met Azure SDK 2.7.1 is het nieuwe venster van de Cloud Explorer-hulpprogramma nu ook beschikbaar in Visual Studio 2013.

### <a name="known-issues"></a>Bekende problemen
Installatie van de Azure SDK 2.6 of 2.7.1 voor Visual Studio Community 2013 op een niet - Engelse OS wordt een waarschuwing weergegeven dat de Engelse en niet-Engelse resources van Visual Studio mogelijk overeen. Deze waarschuwing kan veilig worden genegeerd. Dit gebeurt alleen als de computer geen een eerdere installatie van Visual Studio Community 2013 bevat en u de SDK op een niet - Engelse OS installeert. De waarschuwing wordt weergegeven nadat het taalpakket geldt de RTM-bronnen voor Visual Studio, maar voordat deze Update 4 wordt toegepast. De waarschuwing negeren kunt het taalpakket doorgaan en voltooien met het toepassen van de Update 4-versie van de inhoud van language pack.

LightSwitch projecten zijn niet compatibile met deze release. Dit probleem is opgelost met de volgende release van SDK.

## <a name="also-see"></a>Zie ook
[Azure SDK 2.7.1 aankondiging post](https://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 aankondiging post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Ondersteuning en informatie voor het buiten gebruik stellen voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

