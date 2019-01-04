---
title: 'Azure Data Factory: Veelgestelde vragen | Microsoft Docs'
description: Vind antwoorden op veelgestelde vragen over Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: be0cdeed81c66e1a848b44d2429c1c67bce9b4f3
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024090"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory? 
Data Factory is een volledig beheerde, cloud-gebaseerde, gegevensintegratie-service waarmee de verplaatsing en transformatie van gegevens worden geautomatiseerd. Zoals een factory die wordt uitgevoerd apparatuur wordt gebruikt om grondstoffen te transformeren in eindproducten, beheert Azure Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie. 

Met behulp van Azure Data Factory, kunt u gegevensgestuurde werkstromen voor het verplaatsen van gegevens tussen on-premises en cloud gegevensarchieven. En u kunt verwerken en transformeren van gegevens met behulp van rekenservices zoals Azure HDInsight, Azure Data Lake Analytics en SQL Server Integration Services (SSIS)-integratie-runtime. 

U kunt uw gegevensverwerking met Data Factory kunt uitvoeren op een Azure-gebaseerde cloudservice of in uw eigen zelf-hostend compute-omgeving, zoals SSIS, SQL Server of Oracle. Nadat u een pijplijn die de actie die u nodig hebt maken, kunt u deze wilt uitvoeren regelmatig (bijvoorbeeld per uur, dagelijks of wekelijks), tijd-venster planning of trigger de pijplijn plannen vanuit een exemplaar van de gebeurtenis. Zie voor meer informatie [Inleiding tot Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Besturingselement stromen en schaal 
Ter ondersteuning van de diverse integratiestromen en -patronen in het moderne datawarehouse, Data Factory voorziet flexibele pijplijn gegevensmodellering met volledig beheer stroom programmeren paradigma's waaronder conditionele uitvoering, vertakkingen maken in een gegevens-pipelines en expliciet parameters doorgeven binnen en tussen deze stromen. Controlestroom omvat ook het transformeren van gegevens via verzending van de activiteit voor uitvoering van externe-engines en gegevens flow-mogelijkheden, met inbegrip van de verplaatsing van gegevens op schaal, via de Copy-activiteit.

Data Factory biedt vrijheid om modellen te alle stroomstijlen die is vereist voor de integratie van gegevens en die kunnen worden verzonden op aanvraag of herhaaldelijk op basis van een schema. Er zijn enkele veelvoorkomende stromen die dit model kunt:   

- Besturingselement stromen:
    - Keten activiteiten in een volgorde binnen een pijplijn koppelen.
    - Vertakking activiteiten binnen een pijplijn.
    - Parameters
        - Parameters definiëren op pijplijnniveau en argumenten doorgeven tijdens het starten van de pijplijn op aanvraag of vanuit een trigger.
        - Activiteiten kunnen de argumenten die zijn doorgegeven aan de pijplijn gebruiken.
    - Aangepaste status doorgeven
        - Activiteituitvoer zoals de status kan worden gebruikt door een volgende activiteit in de pijplijn.
    - Herhalende containers
        - For-each 
- Op triggers gebaseerde stromen:
    - Pijplijnen kunnen worden geactiveerd op aanvraag of door de kloktijd.
- Deltastromen:
    - Gebruik parameters en geef de bovengrens op voor deltakopieerbewerkingen tijdens het verplaatsen van dimensie of verwijzingstabellen in een relationele opslagplaats, on-premises of in de cloud, met de gegevens in de lake laden. 

Zie [Zelfstudie: Stromen beheren](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Transformeer uw gegevens op schaal met code gratis pijplijnen
De nieuwe ervaring voor de browser gebaseerde hulpprogramma's biedt zonder code pijplijn ontwerpen en implementeren met een moderne, interactieve webgebaseerde ervaring bieden.

Voor visuele gegevens ontwikkelaars en data-engineers is de ADF-Webgebruikersinterface de ontwerpomgeving zonder code die u gebruiken wilt voor het bouwen van pijplijnen. Het is volledig geïntegreerd met Visual Studio Online Git en zorgt voor integratie voor CI/CD en iteratieve ontwikkeling met opties voor foutopsporing.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Uitgebreide cross-platform-SDK's voor ervaren gebruikers
Als u een ervaren gebruiker bent en op zoek naar een programma-interface, ADF V2 een uitgebreide set SDK's die kunnen worden gebruikt voor het ontwerpen biedt, beheren, bewaken van pijplijnen met behulp van uw favoriete IDE
1.  Python-SDK
2.  Powershell CLI
3.  C# SDK gebruikers kunnen ook gebruikmaken van de gedocumenteerde REST API's om de interface met ADF V2

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Iteratieve ontwikkeling en foutopsporing met visual tools
Azure Data Factory (ADF) visuele hulpprogramma's kunnen u doen iteratieve ontwikkeling en foutopsporing. U kunt uw pijplijnen maken en ik wil deze test wordt uitgevoerd met behulp van de mogelijkheden voor foutopsporing op het pijplijncanvas zonder één regel code te schrijven. U kunt de resultaten van uw testuitvoeringen weergeven in het uitvoervenster van het pijplijncanvas. Zodra uw Testuitvoering is geslaagd, kunt u meer activiteiten toevoegen aan uw pijplijn en doorgaan foutopsporing in een iteratief manier. U kunt ook uw testuitvoeringen annuleren nadat ze uitgevoerd zijn. U bent niet verplicht om uw wijzigingen publiceren naar de data factory-service voordat u foutopsporing op. Dit is handig in situaties waar u om ervoor te zorgen dat de nieuwe toevoegingen of wijzigingen werk zoals verwacht voordat het bijwerken van uw data factory-werkstromen in ontwikkelen, testen, of prod omgevingen. 

### <a name="deploy-ssis-packages-to-azure"></a>SSIS-pakketten implementeren in Azure 
Als u uw SSIS-werkbelastingen verplaatsen wilt, kunt u een Data Factory maken en een Azure-SSIS-integratieruntime inrichten. De Azure-SSIS integratieruntime is een volledig beheerd cluster met virtuele Azure-machines (knooppunten) die zijn toegewezen voor het uitvoeren van uw SSIS-pakketten in de cloud. Zie voor stapsgewijze instructies, de [implementeren van SSIS-pakketten naar Azure](tutorial-create-azure-ssis-runtime-portal.md) zelfstudie. 
 
### <a name="sdks"></a>SDK's
Als u een ervaren gebruiker bent en op zoek naar een programma-interface, ADF een uitgebreide set SDK's die u gebruiken kunt om te ontwerpen biedt, beheren of bewaken van pijplijnen met behulp van uw favoriete IDE. Taalondersteuning bevat .NET, PowerShell, Python en REST.

### <a name="monitoring"></a>Bewaking
U kunt uw Data Factory's via PowerShell, SDK of de Visual controlehulpprogramma's in de gebruikersinterface van de browser controleren. U kunt controleren en op aanvraag, trigger op basis van en klok gestuurde aangepaste stromen op een efficiënte en effectieve manier beheren. Bestaande taken annuleren, Zie fouten in een oogopslag zich gedetailleerde foutberichten en spoor fouten voor de op via een enkel glazen zonder context overschakelt of heen en tussen schermen navigeren Zoom. 

### <a name="new-features-for-ssis-in-adf"></a>Nieuwe functies voor SSIS in ADF
Data Factory heeft de volgende functies toegevoegd voor SSIS sinds de eerste openbare Preview-versie in 2017:

-   Ondersteuning voor drie meer configuraties/varianten van Azure SQL Database (database) naar host SSIS-catalogus van projecten /-pakketten (SSISDB):
-   Azure SQL-database met VNet-service-eindpunten
-   Beheerd exemplaar (MI)
-   Elastische pool
-   Ondersteuning voor Azure Resource Manager-netwerk (VNet) boven op klassieke VNet dat in de toekomst – worden afgeschaft Hiermee kunt u uw Azure-SSIS Integration Runtime (IR) met een VNet dat is geconfigureerd voor Azure SQL-database met VNet-service-eindpunten/MI invoeren/join / on-premises gegevenstoegang, Zie: https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network 
-   Ondersteuning voor verificatie met Azure Active Directory (AAD) boven op SQL-verificatie verbinding maken met de SSISDB - Hiermee kunt u AAD-verificatie met uw ADF beheerde identiteit voor de Azure-resources
-   Ondersteuning om uw eigen on-premises SQL Server-licentie om te winnen aanzienlijke kostenbesparingen via de optie Azure Hybrid Benefit (AHB)
-   Ondersteuning voor Enterprise-editie van Azure-SSIS IR waarmee u gebruik geavanceerde/premium-functies, aangepaste setup te installeren van extra onderdelen/extensies en 3e partij-ecosysteem, Zie: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/ 
-   Diepere integratie van SSIS in ADF waarmee u eersteklas SSIS-pakket uitvoeren-activiteiten in ADF pijplijnen aanroepen/trigger en ze te plannen via SSMS, Zie: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>Wat is een integratieruntime?
De integratieruntime is de rekeninfrastructuur die door Azure Data Factory wordt gebruikt voor de volgende mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen:

- **Gegevensverplaatsing**: Voor gegevensverplaatsing verplaatst Integration Runtime de gegevens tussen de bron- en doelgegevensarchieven, terwijl er ondersteuning wordt geboden voor ingebouwde connectors, indelingsconversie, kolomtoewijzing en goed presterende en schaalbare gegevensoverdracht.
- **Verzending van activiteiten**: Voor transformatie biedt Integration Runtime de mogelijkheid om intern SSIS-pakketten uit te voeren.
- **Uitvoeren van SSIS-pakketten**: Systeemeigen uitvoert SSIS-pakketten in een beheerde Azure compute-omgeving. Integration Runtime biedt ook ondersteuning voor transformatieactiviteiten voor verzending en bewaking die worden uitgevoerd in diverse compute-services zoals Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server en meer.

U kunt een of meer instanties van integratieruntime implementeren die nodig is om de verplaatsing en transformatie van gegevens. Integratieruntime kan worden uitgevoerd op een openbare Azure-netwerk of op een particulier netwerk (on-premises, Azure Virtual Network of Amazon Web Services virtual private cloud [VPC]). 

Zie voor meer informatie [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Wat is de limiet voor het aantal integratieruntimes?
Er is geen vaste limiet voor het aantal instanties van integration runtime die kan er in een data factory. Er is echter een limiet voor het aantal VM-kernen dat de integratieruntime per abonnement voor de uitvoering van SSIS-pakket gebruiken kunt. Zie voor meer informatie, [Data Factory-limieten](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Wat zijn de op het hoogste niveau concepten van Azure Data Factory?
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bevat vier hoofdonderdelen die samenwerken als een platform waarop u gegevensgestuurde werkstromen met stappen voor het verplaatsen en transformeren van gegevens kunt maken.

### <a name="pipelines"></a>Pijplijnen
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groepering van activiteiten uit te voeren een werkeenheid. De activiteiten in een pijplijn voeren samen een taak uit. Een pijplijn kan bijvoorbeeld een groep activiteiten die gegevens uit een Azure-blob opnemen en vervolgens een Hive-query uitvoeren op een HDInsight-cluster voor het partitioneren van de gegevens bevatten. Het voordeel is dat u een pijplijn gebruiken kunt voor het beheren van de activiteiten als een set in plaats van dat elke activiteit afzonderlijk beheren. U kunt keten van de activiteiten in een pijplijn worden na elkaar worden gebruikt, of u ze onafhankelijk van elkaar, parallel kan werken.

### <a name="activity"></a>Activiteit
Activiteiten vertegenwoordigen een verwerkingsstap in een pijplijn. Bijvoorbeeld, kunt u een *kopie* activiteit voor het kopiëren van gegevens van één gegevensarchief naar een ander gegevensarchief. Op deze manier kunt u een Hive-activiteit, waarmee een Hive-query wordt uitgevoerd op een Azure HDInsight-cluster om te transformeren of analyseren van uw gegevens. Data Factory ondersteunt drie soorten activiteiten: activiteiten voor gegevensverplaatsing, activiteiten voor gegevenstransformatie en controleactiviteiten.

### <a name="datasets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Het op deze manier bekijkt: definieert de gekoppelde service de verbinding met de gegevensbron en een gegevensset de structuur van de gegevens vertegenwoordigt. Een gekoppelde Azure Storage-service geeft bijvoorbeeld de verbindingsreeks aan om verbinding maken met de Azure Storage-account. En een Azure Blob-gegevensset specificeert de blobcontainer en de map die de gegevens bevat.

Gekoppelde services beschikken over twee doeleinden in Data Factory:

- Vertegenwoordigt een *gegevensarchief* die bevat, maar is niet beperkt tot een on-premises SQL Server-exemplaar, een Oracle database-exemplaar, een bestandsshare of een Azure Blob storage-account. Zie voor een lijst met ondersteunde gegevensarchieven, [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md).
- Ter vertegenwoordiging van een *rekenresource* die de uitvoering van een activiteit kan hosten. Bijvoorbeeld, de HDInsight Hive-activiteit wordt uitgevoerd op een HDInsight Hadoop-cluster. Zie voor een lijst met transformatieactiviteiten en ondersteunde rekenomgevingen, [transformeren van gegevens in Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Triggers
Triggers vertegenwoordigen eenheden van de verwerking die bepalen wanneer een pijplijnuitvoering wordt gestart. Er zijn verschillende soorten triggers voor verschillende soorten gebeurtenissen. 

### <a name="pipeline-runs"></a>Pijplijnuitvoeringen
De uitvoering van een pijplijn is een exemplaar van een pijplijnuitvoering. U instantiëren meestal een pijplijnuitvoering doordat argumenten worden doorgegeven aan de parameters die zijn gedefinieerd in de pijplijn. U kunt de argumenten doorgeven handmatig of in de definitie van de trigger.

### <a name="parameters"></a>Parameters
Parameters zijn sleutel-waardeparen in een alleen-lezen-configuratie. U parameters definiëren in een pijplijn en u de argumenten voor de gedefinieerde parameters doorgeven tijdens het uitvoeren van een context uitvoeren. De uitvoeringscontext wordt gemaakt door een trigger of van een pijplijn die u handmatig uitvoeren. Activiteiten binnen de pijplijn gebruiken de parameterwaarden.

Een gegevensset is een algemeen type parameter en een entiteit die u kunt gebruiken of verwijzen naar. Een activiteit kan verwijzen naar gegevenssets en kan deze de eigenschappen die zijn gedefinieerd in de definitie van de gegevensset gebruiken.

Een gekoppelde service is ook een algemeen type parameter met de verbindingsgegevens voor een gegevensarchief of een compute-omgeving. Het is ook een entiteit die u kunt gebruiken of verwijzen naar.

### <a name="control-flows"></a>Stromen beheren
Besturingselement stromen pipeline-activiteiten met koppelen van activiteiten in een reeks, vertakkingen, parameters die u op het pijplijnniveau van de definieert, indelen en argumenten die u als u doorgeeft de pijplijn op aanvraag of vanuit een trigger aanroepen. Besturingselement stromen ook aangepaste status doorgeven en luscontainers (dat wil zeggen, voor elke iterators).


Zie de volgende artikelen voor meer informatie over Data Factory-concepten:

- [Gegevensset en gekoppelde services](concepts-datasets-linked-services.md)
- [Pijplijnen en activiteiten](concepts-pipelines-activities.md)
- [Integration Runtime](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Wat is het prijsmodel voor Data Factory?
Zie voor Azure Data Factory prijsinformatie [Data Factory prijsinformatie](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Hoe kan ik op de hoogte blijven met informatie over Data Factory?
Voor de meest actuele informatie over Azure Data Factory, gaat u naar de volgende sites:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Startpagina voor documentatie](/azure/data-factory)
- [Product-startpagina](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Technische details 

### <a name="how-can-i-schedule-a-pipeline"></a>Hoe kan ik een pijplijn plannen? 
U kunt de scheduler-trigger of tijd venster trigger gebruiken voor het plannen van een pijplijn. De trigger gebruikt een wandklokplanning agenda en u deze kunt gebruiken voor het plannen van pijplijnen periodiek of met behulp van de agenda op basis van periodieke patronen (bijvoorbeeld elke week op maandag op 18: 00 uur en donderdag om 21: 00). Zie [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie.

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Kan ik parameters doorgeven aan een pijplijnuitvoering te starten?
Ja, parameters zijn een concept eersteklas, op het hoogste niveau in ADF. U kunt parameters definiëren op pijplijnniveau en de argumenten doorgeven bij het uitvoeren van de pijplijn op aanvraag of met behulp van een trigger.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kan ik de standaardwaarden voor de pijplijnparameters definiëren? 
Ja. U kunt de standaardwaarden voor de parameters definiëren in de pijplijnen. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Kan een activiteit in een pijplijn argumenten die worden doorgegeven aan de uitvoering van een pijplijn gebruiken? 
Ja. Elke activiteit in de pijplijn kan de waarde van de parameter die is doorgegeven aan de pijplijn en uitvoeren met gebruiken de `@parameter` maken. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kan de eigenschap van een activiteit uitvoer worden gebruikt in een andere activiteit? 
Ja. De uitvoer van een activiteit kan worden gebruikt in een volgende activiteit met de `@activity` maken.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Hoe ga ik null-waarden in de uitvoer van een activiteit zonder problemen om? 
U kunt de `@coalesce` maken in de expressies voor het afhandelen van null-waarden zonder problemen. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van een data factory:

- [Snelstart: Een data factory maken](quickstart-create-data-factory-dot-net.md)
- [Zelfstudie: Kopiëren van gegevens in de cloud](tutorial-copy-data-dot-net.md)
