---
title: 'Azure Data Factory: Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Data Factory.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: shlo
ms.openlocfilehash: cf98bb7fab4942955287e8e211e98b9da59472f6
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory Veelgestelde vragen
In dit artikel is van toepassing op versie 2 van de Azure Data Factory-service. Dit vindt u antwoorden op veelgestelde vragen over Data Factory.  

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Veelgestelde vragen over Data Factory versie 1](v1/data-factory-faq.md).

## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory? 
Data Factory is een volledig beheerde, cloud-gebaseerde gegevensintegratie-service waarmee de verplaatsing en transformatie van gegevens worden geautomatiseerd. Zoals een factory die wordt uitgevoerd apparatuur grondstoffen te transformeren in eindproducten, ingedeeld Azure Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie. 

Met behulp van Azure Data Factory, kunt u gegevensgestuurde werkstromen om gegevens te verplaatsen tussen on-premises en cloud gegevensarchieven. U kunt verwerken en transformatie-gegevens met behulp van compute-services zoals Azure HDInsight Azure Data Lake Analytics en SQL Server Integration Services (SSIS) integratie-runtime. 

Met Data Factory, kunt u uw gegevensverwerking uitvoeren op een op basis van een Azure cloudservice of in uw eigen host zichzelf compute-omgeving, zoals SSIS-, SQL Server- of Oracle. Nadat u een pijplijn die de actie uitgevoerd die u nodig hebt gemaakt, kunt u deze uit te voeren regelmatig (bijvoorbeeld per uur, dagelijks of wekelijks) of activeren van de pijplijn vanuit een exemplaar van de gebeurtenis kunt plannen. Zie voor meer informatie [Inleiding tot Azure Data Factory](introduction.md).

## <a name="whats-different-in-version-2"></a>Wat is er anders in versie 2?
Versie 2 van Azure Data Factory is gebaseerd op de oorspronkelijke gegevensverplaatsings- en transformatieservice Azure Data Factory. De service is uitgebreid tot een bredere set van scenario's voor gegevensintegratie in de cloud. Azure Data Factory versie 2 biedt de volgende mogelijkheden:

- Besturingselement stromen en schalen
- Implementeren en uitvoeren van SSIS-pakketten in Azure

Na de release van versie 1 wordt herkend dat klanten moeten ontwerp complex is of hybride gegevensintegratie scenario's waarvoor zowel de verplaatsing van gegevens en de verwerking in de cloud, on-premises en cloud virtuele machines (VM's). Deze vereisten Breng wilt overdragen en verwerken van gegevens binnen een beveiligde virtuele netwerkomgevingen en scale-out met verwerkingskracht op aanvraag.

Als gegevenspijplijnen steeds belangrijker van een strategie voor business analytics, hebben we waargenomen dat deze activiteiten zijn vereist voor flexibele planning om incrementele gegevens geladen en gebeurtenis geactiveerd uitvoeringen te ondersteunen. Als de complexiteit toeneemt, dus te heeft de vereiste voor de service voor ondersteuning van de algemene werkstroom paradigma's met vertakkingen, lussen en voorwaardelijke verwerking.

Met versie 2 kunt u ook bestaande SSIS-pakketten naar de cloud migreren. Deze actie liftonderhoud en SSIS verschuift als een Azure-service die wordt beheerd in de Data Factory, die gebruikmaakt van een nieuwe functie van integratie runtime. Door een runtime SSIS-integratie in versie 2 draaien, kunt u uitvoeren, beheren, controleren en bouwen van SSIS-pakketten in de cloud.

### <a name="control-flows-and-scale"></a>Besturingselement stromen en schalen 
Data Factory heeft een nieuwe, flexibele-pipeline-gegevensmodel dat niet langer aan timeseries gegevens gebonden is ingeschakeld ter ondersteuning van de diverse integratie stromen en patronen in het moderne-datawarehouse. U kunt met deze release model voorwaardelijke, vertakking in de stroom van het besturingselement van een pijplijn gegevens en parameters binnen en tussen deze stromen expliciet doorgeven.

U hebt nu de vrijheid om het model van de stijl van een stroom die is vereist voor gegevensintegratie en die kunnen worden verzonden op aanvraag of herhaaldelijk op een planning. Dit zijn enkele veelvoorkomende stromen die nu mogelijk zijn geworden:   

- Besturingselement stromen:
    - Keten activiteiten in een volgorde binnen een pijplijn.
    - Vertakking activiteiten binnen een pijplijn.
    - Parameters
        - Definieer de parameters op het niveau van de pijplijn en argumenten doorgeven tijdens het starten van de pijplijn op aanvraag of vanuit een trigger.
        - Activiteiten kunnen de argumenten die zijn doorgegeven aan de pijplijn gebruiken.
    - Aangepaste status doorgeven
        - Activiteituitvoer zoals de status kan worden gebruikt door een volgende activiteit in de pijplijn.
    - Herhalende containers
        - For-each 
- Op basis van een trigger stromen:
    - Pijplijnen kunnen worden geactiveerd op aanvraag of wanden klok.
- Delta-stromen:
    - Parameters gebruiken en de bovengrens voor delta-exemplaar definiëren tijdens het verplaatsen van de dimensie of een verwijzing tabellen in een relationeel gegevensarchief, on-premises of in de cloud, met de gegevens in de lake laden. 

Zie voor meer informatie [zelfstudie: stromen beheren](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>SSIS-pakketten implementeren in Azure 
Als u wilt uw SSIS-werkbelastingen verplaatsen, kunt u een Gegevensfactory versie 2 maken en inrichten van een Azure-SSIS-integratie-runtime. De Azure-SSIS-integratie-runtime is een volledig beheerde cluster met Azure Virtual machines (knooppunten) die zijn toegewezen voor het uitvoeren van SSIS-pakketten in de cloud. Zie voor stapsgewijze instructies de [implementeren SSIS-pakketten naar Azure](tutorial-deploy-ssis-packages-azure.md) zelfstudie. 
 

### <a name="sdks"></a>SDK's
Als u een ervaren gebruikers en zoek naar een programma-interface, versie 2 een uitgebreide set SDK's die u gebruiken kunt om te beheren biedt, ontwerpen en pijplijnen bewaken met behulp van uw favoriete IDE.

- **.NET SDK**: de .NET SDK is bijgewerkt voor versie 2. 
- **PowerShell**: de PowerShell-cmdlets zijn bijgewerkt voor versie 2. De cmdlets voor versie 2 hebben *DataFactoryV2* in de naam. Bijvoorbeeld: *Get-AzureRmDataFactoryV2*. 
- **Python SDK**: deze SDK is nieuw in versie 2.
- **REST API**: de REST-API is bijgewerkt voor versie 2.  

De SDK's die zijn bijgewerkt voor versie 2 zijn niet achterwaarts compatibel met versie 1-clients. 

### <a name="monitoring"></a>Bewaking
In versie 2 kunnen data factory's momenteel alleen worden bewaakt met behulp van SDK's. De portal biedt nog geen ondersteuning voor bewaking van data factory's versie 2. 

## <a name="what-is-integration-runtime"></a>Wat is integratie runtime?
Integratie-runtime is de beheerinfrastructuur die wordt gebruikt door Azure Data Factory om te voorzien in de volgende mogelijkheden voor integratie van verschillende netwerkomgevingen:

- **Gegevensverplaatsing**: gegevens verplaatsen tussen gegevensarchieven op een openbaar netwerk en gegevens opslaat in een particulier netwerk (on-premises of virtueel particulier netwerk). Deze optie biedt ondersteuning voor ingebouwde connectors, indelingsconversie, kolomtoewijzing en hoogwaardige en schaalbare gegevensoverdracht.
- **Verzending van activiteiten**: verzending en monitor transformatie-activiteiten die worden uitgevoerd op tal van compute services, zoals Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server en meer.
- **Uitvoeren van SSIS-pakketten**: systeemeigen SSIS-pakketten in een beheerde Azure compute-omgeving wordt uitgevoerd.

U kunt een of meer exemplaren van integratie runtime als vereist voor het verplaatsen en gegevens transformeren implementeren. Integratie runtime kunt uitvoeren op een openbare Azure-netwerk of op een particulier netwerk (on-premises, Azure Virtual Network of Amazon Web Services virtuele-privécloud [VPC]). 

Zie voor meer informatie [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Wat is de limiet voor het aantal integratie runtimes?
Er is geen vaste limiet voor het aantal exemplaren van integratie runtime die een gegevensfactory kan bevatten. Er is echter een limiet voor het aantal kernen voor VM die de integratie-runtime voor de uitvoering van SSIS-pakket per abonnement kunt gebruiken. Zie voor meer informatie [Data Factory beperkt](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="when-should-i-use-version-2-rather-than-version-1"></a>Wanneer moet ik versie 2 in plaats van versie 1 gebruiken? 
Als u niet bekend met Azure Data Factory bent, start u rechtstreeks met versie 2. Als u al van versie 1 gebruikmaakt, opnieuw opbouwen uw gegevensfactory op versie 2.

> [!WARNING]
> Versie 2 van de Data Factory is een Preview-versie en het is niet in het algemeen beschikbaarheid (GA). Daarom valt deze niet onder de dezelfde Azure-service level agreement (SLA) verbintenissen als versie 1 van de Data Factory, dat zich in de algemene beschikbaarheid bevindt. 

## <a name="what-are-the-top-level-concepts-of-version-2"></a>Wat zijn de op het hoogste niveau concepten van versie 2?
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bevat vier belangrijke onderdelen die samenwerken als een platform waarop u gegevensgestuurde werkstromen met stappen om te gaan en gegevens transformeren kunt samenstellen.

### <a name="pipelines"></a>Pijplijnen
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groepering van activiteiten uit te voeren een werkeenheid. De activiteiten in een pijplijn voeren samen een taak uit. Een pijplijn kan bijvoorbeeld een groep activiteiten die voor het opnemen van gegevens uit een Azure-blobopslag en voer vervolgens een Hive-query op een HDInsight-cluster voor het partitioneren van de gegevens bevatten. Het voordeel is dat u een pijplijn gebruiken kunt voor het beheren van de activiteiten als een set in plaats van elke activiteit afzonderlijk beheren. U kunt keten van de activiteiten in een pipeline om te worden na elkaar worden gebruikt of u ze onafhankelijk, parallel kan werken.

### <a name="activity"></a>Activiteit
Activiteiten vertegenwoordigen een verwerkingsstap in een pijplijn. U kunt bijvoorbeeld een *kopie* activiteit om gegevens te kopiëren uit een gegevensopslag met een ander gegevensarchief. Op deze manier kunt u een Hive-activiteit die wordt uitgevoerd een Hive-query op een Azure HDInsight-cluster te transformeren of analyseren van uw gegevens. Data Factory ondersteunt drie soorten activiteiten: activiteiten voor gegevensverplaatsing, activiteiten voor gegevenstransformatie en controleactiviteiten.

### <a name="data-sets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren binnen de gegevensarchieven die simpelweg wijs of verwijzen naar de gegevens die u in uw activiteiten wilt gebruiken als invoer of uitvoer. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Beschouw deze manier: een gekoppelde service definieert u de verbinding met de gegevensbron en de structuur van de gegevens van een gegevensset vertegenwoordigt. Een gekoppelde Azure Storage-service geeft bijvoorbeeld de verbindingsreeks voor verbinding met de Azure Storage-account. En een Azure Blob-gegevensset geeft de blob-container en de map waarin de gegevens.

Gekoppelde services hebben twee doelen in Data Factory:

- Vertegenwoordigt een *gegevensarchief* die bevat, maar is niet beperkt tot een lokale SQL Server-exemplaar, een Oracle-database-exemplaar, een bestandsshare of een Azure Blob storage-account. Zie voor een lijst van ondersteunde gegevensarchieven [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md).
- Ter vertegenwoordiging van een *rekenresource* die de uitvoering van een activiteit kan hosten. Bijvoorbeeld, de HDInsight Hive-activiteit wordt uitgevoerd op een HDInsight Hadoop-cluster. Zie voor een lijst van activiteiten voor gegevenstransformatie en ondersteunde berekeningsomgevingen [transformeer gegevens in Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Triggers
Triggers vertegenwoordigen eenheden van de verwerking die bepalen wanneer een pipeline-uitvoering wordt gestart. Er zijn verschillende soorten triggers voor verschillende soorten gebeurtenissen. Wij ondersteunen een wanden klok scheduler-trigger voor preview. 


### <a name="pipeline-runs"></a>Pijplijnuitvoeringen
Een pijplijn uitvoeren is een exemplaar van de uitvoering van een pipeline. U instantiëren meestal een pijplijn uitgevoerd door de argumenten doorgegeven aan de parameters die zijn gedefinieerd in de pijplijn. U kunt de argumenten doorgeven handmatig of in de definitie van de trigger.

### <a name="parameters"></a>Parameters
Parameters zijn sleutel / waarde-paren in de configuratie van een alleen-lezen. U parameters definiëren in een pijplijn en u de argumenten voor de gedefinieerde parameters doorgegeven tijdens het uitvoeren van een context uitvoeren. De uitvoering context is gemaakt door een trigger of van een pijplijn die u handmatig uitvoeren. Activiteiten binnen de pijplijn gebruiken de parameterwaarden.

Een gegevensset is een sterk getypeerde parameter en een entiteit die u kunt gebruiken of naar verwijzen. Een activiteit kan verwijzen naar gegevenssets en verbruikt dit de eigenschappen die zijn gedefinieerd in de definitie gegevensset.

Een gekoppelde service is ook een sterk getypeerde parameter met de verbindingsgegevens wilt toevoegen aan een gegevensarchief of een compute-omgeving. Het is ook een entiteit die u kunt gebruiken of naar verwijzen.

### <a name="control-flows"></a>Besturingselement stromen
Besturingselement stromen indelen pijplijn activiteiten activiteiten in een reeks omvatten, vertakkingen, parameters die u op het niveau van de pipeline definieert-koppeling en argumenten die u als u doorgeeft de pijplijn op aanvraag of vanuit een trigger worden aangeroepen. Besturingselement stromen ook aangepaste status doorgeeft en lussen containers (dat wil zeggen, voor elke iterators).


Zie de volgende artikelen voor meer informatie over Data Factory-concepten:

- [Gegevensverzameling en gekoppelde services](concepts-datasets-linked-services.md)
- [Pijplijnen en activiteiten](concepts-pipelines-activities.md)
- [Integration Runtime](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Wat is het prijsmodel voor Data Factory?
Zie voor Azure Data Factory prijsinformatie, [Data Factory prijsinformatie](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="what-regions-support-azure-data-factory-version-2"></a>Welke regio's Azure Data Factory versie 2 ondersteund?
Op dit moment kunt u data Factory van versie 2 in de regio's VS-Oost, VS-Oost 2 en West-Europa. Een gegevensfactory kan integratie runtime echter gebruiken in een andere regio om gegevens te verplaatsen tussen gegevensarchieven, dispatch activiteiten op basis van compute services of verzending SSIS-pakketten. Zie voor meer informatie [Data Factory locaties](concepts-integration-runtime.md#integration-runtime-location).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Hoe kan ik product up-to-date houden met informatie over Data Factory?
Voor de meest actuele informatie over Azure Data Factory, gaat u naar de volgende sites:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Documentatie-startpagina](/azure/data-factory)
- [Product-startpagina](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Technische diepgaand 

### <a name="can-version-1-and-version-2-pipelines-run-side-by-side"></a>Kan versie 1 en versie 2 pijplijnen naast elkaar uitvoeren?
Nee. Versie 2 en data Factory van versie 1 kunnen geen entiteiten (bijvoorbeeld, gekoppelde services, gegevenssets of pijplijnen) van de andere versie bevatten.   

### <a name="do-i-still-need-to-define-data-sets-in-version-2"></a>Moet ik gegevenssets definiëren in versie 2?
Een gegevensset is niet langer een verplichte entiteit voor de meeste activiteiten. Het is vereist voor het kopiëren, machine learning, opzoeken, validatie en aangepaste activiteiten die gebruikmaken van het schema en andere metagegevens in de gegevensset voor transformatie. De rest van de activiteiten niet langer vereist gegevenssets.

### <a name="can-i-chain-two-activities-without-a-data-set-in-version-2"></a>Kan ik twee activiteiten zonder een gegevensset in versie 2 koppelen?
Ja. U kunt de activiteiten in versie 2 koppelen zonder gegevenssets. Keten van activiteiten met behulp van de **dependsOn** eigenschap in de JSON-definitie van de pijplijn. 

### <a name="are-all-the-version-1-activities-supported-in-version-2"></a>De versie 1 activiteiten die worden ondersteund in versie 2 zijn? 
Ja, worden alle activiteiten van versie 1 worden ondersteund in versie 2.

### <a name="how-can-i-schedule-a-version-2-pipeline"></a>Hoe kan ik een pijplijn versie 2 plannen? 
De trigger scheduler kunt u een pijplijn versie 2 plannen. De trigger een kalender wanden klok planning gebruikt en u kunt het plannen van pijplijnen periodiek of met behulp van periodieke patronen op basis van een kalender (bijvoorbeeld wekelijks op maandag op 18: 00 uur en donderdag om 9 uur). Zie [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie.

### <a name="can-i-pass-parameters-to-a-pipeline-run-in-version-2"></a>Kan ik parameters doorgeven aan een pijplijn in versie 2 wordt uitgevoerd
Ja, parameters zijn een concept klas, op het hoogste niveau in versie 2. U kunt de parameters op het niveau van de pijplijn definiëren en argumenten doorgeven als u de pijplijn uitvoeren op aanvraag of met behulp van een trigger uitvoeren.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kan ik de standaardwaarden voor de pipeline-parameters definiëren? 
Ja. U kunt de standaardwaarden voor de parameters definiëren in de pijplijnen. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Kan een activiteit in een pijplijn gebruiken argumenten die worden doorgegeven aan een pijplijn uitvoeren? 
Ja. Elke activiteit in de pijplijn gebruiken de parameterwaarde die is doorgegeven aan de pijplijn en worden uitgevoerd met de `@parameter` samenstellen. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kan de eigenschap van een activiteit uitvoer worden gebruikt in een andere activiteit? 
Ja. De uitvoer van een activiteit kan worden gebruikt in een volgende activiteit met de `@activity` samenstellen.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Hoe ik null-waarden in de uitvoer van een activiteit probleemloos verwerkt? 
U kunt de `@coalesce` maken in de expressies voor het afhandelen van null-waarden probleemloos. 

### <a name="can-i-use-retry-and-timeout-at-the-activity-level-in-version-2"></a>Kan ik opnieuw proberen en time-out op het activiteitenniveau van de in versie 2 gebruiken?
Ja. U kunt voor het bepalen van de uitvoering van activiteiten in versie 2 als in versie 1, opnieuw proberen en time-out configureren op het activiteitenniveau van de. 

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het maken van een gegevensfactory versie 2 van de volgende zelfstudies:

- [Snelstartgids: Een gegevensfactory maken](quickstart-create-data-factory-dot-net.md)
- [Zelfstudie: Gegevens kopiëren in de cloud](tutorial-copy-data-dot-net.md)

