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
ms.openlocfilehash: d704c32ee7417c6460ad6cc880e451adddfa61de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345751"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory? 
Data Factory is een volledig beheerde, cloud-gebaseerde, gegevensintegratie-service waarmee de verplaatsing en transformatie van gegevens worden geautomatiseerd. Zoals een factory die wordt uitgevoerd apparatuur wordt gebruikt om grondstoffen te transformeren in eindproducten, beheert Azure Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie. 

Met behulp van Azure Data Factory, kunt u gegevensgestuurde werkstromen voor het verplaatsen van gegevens tussen on-premises en cloud gegevensarchieven. En u kunt verwerken en transformeren van gegevens met behulp van rekenservices zoals Azure HDInsight, Azure Data Lake Analytics en de SQL Server Integration Services (SSIS) integratieruntime. 

U kunt uw gegevensverwerking met Data Factory kunt uitvoeren op een Azure-gebaseerde cloudservice of in uw eigen zelf-hostend compute-omgeving, zoals SSIS, SQL Server of Oracle. Nadat u een pijplijn die de actie die u nodig hebt maken, kunt u plannen dat het periodiek (per uur, dagelijks of wekelijks, bijvoorbeeld), tijdvenster planning wordt uitgevoerd of de pijplijn van het exemplaar van een gebeurtenis activeren. Zie voor meer informatie [Inleiding tot Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Besturingselement stromen en schaal 
Ter ondersteuning van de diverse integratiestromen en -patronen in het moderne datawarehouse, Data Factory maakt flexibele data pipeline-modellen. Dit houdt in dat volledige Controlestroom paradigma's, waaronder conditionele uitvoering programmeren, vertakkingen maken in de gegevens-pipelines en de mogelijkheid om expliciet parameters doorgeven binnen en tussen deze stromen. Controlestroom omvat ook het transformeren van gegevens via verzending van de activiteit voor uitvoering van externe-engines en gegevens flow-mogelijkheden, met inbegrip van de verplaatsing van gegevens op schaal, via de Copy-activiteit.

Data Factory biedt vrijheid om modellen te alle stroomstijlen die is vereist voor de integratie van gegevens en die kunnen worden verzonden op aanvraag of herhaaldelijk op basis van een schema. Er zijn enkele veelvoorkomende stromen die dit model kunt:   

- Besturingselement stromen:
    - Activiteiten kunnen een keten worden samengevoegd in een volgorde binnen een pijplijn koppelen.
    - Activiteiten kunnen worden vertakt binnen een pijplijn koppelen.
    - Parameters:
        - Parameters kunnen worden gedefinieerd op pijplijnniveau en argumenten kunnen worden doorgegeven tijdens het starten van de pijplijn op aanvraag of vanuit een trigger.
        - Activiteiten kunnen de argumenten die zijn doorgegeven aan de pijplijn gebruiken.
    - Aangepaste status doorgeven:
        - Uitvoer voor activiteiten, zoals de status, kunnen worden gebruikt door een volgende activiteit in de pijplijn.
    - Herhalende containers:
        - De foreach-activiteit wordt herhaald voor een opgegeven verzameling van activiteiten in een lus. 
- Op triggers gebaseerde stromen:
    - Pijplijnen kunnen worden geactiveerd op aanvraag of door de kloktijd.
- Deltastromen:
    - Parameters kunnen worden gebruikt voor het definiëren van de bovengrens op voor deltakopieerbewerkingen tijdens het verplaatsen van dimensie of verwijzingstabellen in een relationele opslagplaats, on-premises of in de cloud, met de gegevens in de lake laden. 

Zie [Zelfstudie: Stromen beheren](tutorial-control-flow.md).

### <a name="data-transformed-at-scale-with-code-free-pipelines"></a>Gegevens getransformeerd op schaal met pijplijnen zonder code
De nieuwe ervaring voor de browser gebaseerde hulpprogramma's biedt zonder code pijplijn ontwerpen en implementeren met een moderne, interactieve webgebaseerde ervaring bieden.

Voor visuele gegevens ontwikkelaars en data-engineers is de web-UI van Data Factory de ontwerpomgeving zonder code die u gebruiken wilt voor het bouwen van pijplijnen. Het volledig geïntegreerd met Visual Studio Online Git en zorgt voor integratie voor CI/CD en iteratieve ontwikkeling met opties voor foutopsporing.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Uitgebreide cross-platform-SDK's voor ervaren gebruikers
Data Factory V2 biedt een uitgebreide set SDK's die kunnen worden gebruikt voor het ontwerpen, beheren en controleren van pijplijnen met behulp van uw favoriete IDE, met inbegrip van:
* Python-SDK
* PowerShell CLI
* C# SDK

Gebruikers kunnen ook de gedocumenteerde REST-API's gebruiken voor interactie met Data Factory V2.

### <a name="iterative-development-and-debugging-by-using-visual-tools"></a>Iteratieve ontwikkeling en foutopsporing met behulp van visuele hulpmiddelen
Azure Data Factory visuele hulpprogramma's inschakelen iteratieve ontwikkeling en foutopsporing. U kunt uw pijplijnen maken en ik wil deze test wordt uitgevoerd met behulp van de **Debug** mogelijkheden op het pijplijncanvas zonder één regel code te schrijven. Vindt u de resultaten van de test wordt uitgevoerd in de **uitvoer** venster van de pijplijncanvas. Nadat uw Testuitvoering is voltooid, kunt u meer activiteiten toevoegen aan uw pijplijn en doorgaan foutopsporing in een iteratief manier. U kunt ook uw testuitvoeringen annuleren nadat ze uitgevoerd zijn. 

U bent niet verplicht om uw wijzigingen publiceren naar de data factory-service voordat u selecteert **Debug**. Dit is handig in situaties waar u om ervoor te zorgen dat de nieuwe toevoegingen of wijzigingen werkt zoals verwacht voordat u uw data factory-werkstromen in ontwikkeling, tests of productie-omgevingen worden bijgewerkt. 

### <a name="ability-to-deploy-ssis-packages-to-azure"></a>Mogelijkheid tot het implementeren van SSIS-pakketten naar Azure 
Als u uw SSIS-werkbelastingen verplaatsen wilt, kunt u een Data Factory maken en een Azure-SSIS-integratieruntime inrichten. Een Azure-SSIS integratieruntime is een volledig beheerd cluster met virtuele Azure-machines (knooppunten) die zijn toegewezen voor het uitvoeren van uw SSIS-pakketten in de cloud. Zie voor stapsgewijze instructies, de [implementeren van SSIS-pakketten naar Azure](tutorial-create-azure-ssis-runtime-portal.md) zelfstudie. 
 
### <a name="sdks"></a>SDK's
Als u een ervaren gebruiker bent en op zoek naar een programma-interface, Data Factory een uitgebreide set SDK's die u gebruiken kunt om te ontwerpen biedt, beheren of bewaken van pijplijnen met behulp van uw favoriete IDE. Taalondersteuning bevat .NET, PowerShell, Python en REST.

### <a name="monitoring"></a>Bewaking
U kunt uw Data Factory's via PowerShell, SDK of de Visual controlehulpprogramma's in de gebruikersinterface van de browser controleren. U kunt controleren en op aanvraag, op basis van een trigger en klok gebaseerde aangepaste stromen op een efficiënte en effectieve manier beheren. Bestaande taken annuleren, Zie fouten in een oogopslag zich gedetailleerde foutberichten en fouten opsporen in de problemen, allemaal met een enkel glazen zonder context overschakelt of heen en tussen schermen navigeren Zoom. 

### <a name="new-features-for-ssis-in-data-factory"></a>Nieuwe functies voor SSIS in Data Factory
Omdat de eerste openbare preview-versie in 2017, is de volgende functies in Data Factory toegevoegd voor SSIS:

-   Ondersteuning voor drie meer configuraties/varianten van Azure SQL Database voor het hosten van de SSIS-database (SSISDB) van projecten /-pakketten:
-   SQL-Database met virtual network-service-eindpunten
-   Beheerd exemplaar
-   Elastische pool
-   Ondersteuning voor een Azure Resource Manager-netwerk boven op een klassiek virtueel netwerk om te worden in de toekomst afgeschaft waarmee u invoeren/join uw Azure-SSIS integratieruntime met een virtueel netwerk dat is geconfigureerd voor SQL-Database met virtual network-service toegang tot eindpunten/MI/on-premises gegevens. Zie voor meer informatie ook [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).
-   Ondersteuning voor verificatie van Azure Active Directory (Azure AD) en SQL-verificatie verbinding maken met de SSISDB, zodat Azure AD-verificatie met uw Data Factory beheerde identiteit voor Azure-resources
-   Ondersteuning om uw eigen on-premises SQL Server-licentie om te winnen aanzienlijke kostenbesparingen uit de optie Azure Hybrid Benefit
-   Ondersteuning voor Enterprise-editie van de Azure-SSIS integratieruntime waarmee u gebruik geavanceerde/premium-functies, een aangepaste installatie-interface voor het installeren van extra onderdelen/extensies en een partnerecosysteem. Zie voor meer informatie ook [Enterprise Edition, aangepaste installatie en 3e partij uitbreidbaarheid voor SSIS in ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/). 
-   Diepere integratie van SSIS in Data Factory waarmee u eersteklas SSIS-pakket uitvoeren-activiteiten in Data Factory-pijplijnen aanroepen/trigger en ze via SSMS te plannen. Zie voor meer informatie ook [Modernize en uitbreiden van uw ETL/ELT-werkstromen met SSIS-activiteiten in ADF pijplijnen](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/).


## <a name="what-is-the-integration-runtime"></a>Wat is de integratieruntime?
De integratieruntime is de rekeninfrastructuur die Azure Data Factory gebruikt voor de volgende mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen:

- **Gegevensverplaatsing**: Voor gegevensverplaatsing verplaatst integratieruntime de gegevens tussen de bron- en doelserver, terwijl er ondersteuning wordt geboden voor ingebouwde connectors, Indelingsconversie, kolomtoewijzing en goed presterende en schaalbare gegevensoverdracht.
- **Verzending van activiteiten**: Voor transformatie biedt integratieruntime mogelijkheid voor het uitvoeren van systeemeigen SSIS-pakketten.
- **Uitvoeren van SSIS-pakketten**: De integratieruntime voert systeemeigen SSIS-pakketten in een beheerde Azure compute-omgeving. De integratieruntime biedt ook ondersteuning voor transformatieactiviteiten voor verzending en bewaking die worden uitgevoerd op diverse compute-services, zoals Azure HDInsight, Azure Machine Learning, SQL-Database en SQL Server.

U kunt een of meer instanties van de integratieruntime implementeren die nodig is om de verplaatsing en transformatie van gegevens. De integratieruntime kan worden uitgevoerd op een openbare Azure-netwerk of in een particulier netwerk (on-premises, Azure Virtual Network of Amazon Web Services virtual private cloud [VPC]). 

Zie voor meer informatie [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Wat is de limiet voor het aantal integratieruntimes?
Er is geen vaste limiet voor het aantal instanties van integration runtime die kan er in een data factory. Er is echter een limiet voor het aantal VM-kernen dat de integratieruntime per abonnement voor de uitvoering van SSIS-pakket gebruiken kunt. Zie voor meer informatie, [Data Factory-limieten](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Wat zijn de op het hoogste niveau concepten van Azure Data Factory?
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bevat vier hoofdonderdelen die samenwerken als een platform waarop u gegevensgestuurde werkstromen met stappen voor het verplaatsen en transformeren van gegevens kunt maken.

### <a name="pipelines"></a>Pijplijnen
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groepering van activiteiten uit te voeren een werkeenheid. De activiteiten in een pijplijn voeren samen een taak uit. Een pijplijn kan bijvoorbeeld een groep activiteiten die gegevens uit een Azure-blob opnemen en vervolgens een Hive-query uitvoeren op een HDInsight-cluster voor het partitioneren van de gegevens bevatten. Het voordeel is dat u een pijplijn gebruiken kunt voor het beheren van de activiteiten als een set in plaats van dat elke activiteit afzonderlijk beheren. U kunt keten van de activiteiten in een pijplijn worden na elkaar worden gebruikt, of u ze onafhankelijk van elkaar, parallel kan werken.

### <a name="activities"></a>Activiteiten
Activiteiten vertegenwoordigen een verwerkingsstap in een pijplijn. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren van één gegevensarchief naar een ander gegevensarchief. Op deze manier kunt u een Hive-activiteit, waarmee een Hive-query wordt uitgevoerd op een Azure HDInsight-cluster om te transformeren of analyseren van uw gegevens. Data Factory ondersteunt drie soorten activiteiten: activiteiten voor gegevensverplaatsing, activiteiten voor gegevenstransformatie en controleactiviteiten.

### <a name="datasets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Het op deze manier bekijkt: Een gekoppelde service definieert de verbinding met de gegevensbron en een gegevensset vertegenwoordigt de structuur van de gegevens. Een gekoppelde Azure Storage-service geeft bijvoorbeeld de verbindingsreeks aan om verbinding maken met de Azure Storage-account. En een Azure blob-gegevensset specificeert de blobcontainer en de map die de gegevens bevat.

Gekoppelde services beschikken over twee doeleinden in Data Factory:

- Vertegenwoordigt een *gegevensarchief* die bevat, maar is niet beperkt tot een on-premises SQL Server-exemplaar, een Oracle database-exemplaar, een bestandsshare of een Azure Blob storage-account. Zie voor een lijst met ondersteunde gegevensarchieven, [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md).
- Ter vertegenwoordiging van een *rekenresource* die de uitvoering van een activiteit kan hosten. Bijvoorbeeld, de HDInsight Hive-activiteit wordt uitgevoerd op een HDInsight Hadoop-cluster. Zie voor een lijst met transformatieactiviteiten en ondersteunde rekenomgevingen, [transformeren van gegevens in Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Triggers
Triggers vertegenwoordigen eenheden van de verwerking die bepalen wanneer een pijplijnuitvoering wordt gestart. Er zijn verschillende soorten triggers voor verschillende soorten gebeurtenissen. 

### <a name="pipeline-runs"></a>Pijplijnuitvoeringen
De uitvoering van een pijplijn is een exemplaar van een pijplijnuitvoering. U instantiëren meestal een pijplijnuitvoering doordat argumenten worden doorgegeven aan de parameters die zijn gedefinieerd in de pijplijn. U kunt de argumenten doorgeven handmatig of in de definitie van de trigger.

### <a name="parameters"></a>Parameters
Parameters zijn sleutel-waardeparen in een alleen-lezen-configuratie. U parameters definiëren in een pijplijn en u de argumenten voor de gedefinieerde parameters doorgeven tijdens het uitvoeren van een context uitvoeren. De uitvoeringscontext wordt gemaakt door een trigger of van een pijplijn die u handmatig uitvoeren. Activiteiten binnen de pijplijn gebruiken de parameterwaarden.

Een gegevensset is een algemeen type parameter en een entiteit die u kunt gebruiken of verwijzen naar. Een activiteit kan verwijzen naar gegevenssets en kan deze de eigenschappen die zijn gedefinieerd in het definitie van de gegevensset gebruiken.

Een gekoppelde service is ook een algemeen type parameter met de verbindingsgegevens voor een gegevensarchief of een compute-omgeving. Het is ook een entiteit die u kunt gebruiken of verwijzen naar.

### <a name="control-flows"></a>Stromen beheren
Besturingselement stromen pipeline-activiteiten met koppelen van activiteiten in een reeks, vertakkingen, parameters die u op het pijplijnniveau van de definieert, indelen en argumenten die u als u doorgeeft de pijplijn op aanvraag of vanuit een trigger aanroepen. Besturingselement stromen ook aangepaste status doorgeven en luscontainers (dat wil zeggen, foreach iterators).


Zie de volgende artikelen voor meer informatie over Data Factory-concepten:

- [Gegevenssets en gekoppelde services](concepts-datasets-linked-services.md)
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
U kunt de scheduler-trigger of tijd venster trigger gebruiken voor het plannen van een pijplijn. De trigger gebruikt een agenda wandklokschema, waarmee pijplijnen kan worden gepland periodiek of in de kalender op basis van periodieke patronen bijvoorbeeld op (maandag om 6:00 uur) en donderdag om 21:00 uur. Zie [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie.

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Kan ik parameters doorgeven aan een pijplijnuitvoering te starten?
Ja, parameters zijn een concept eersteklas, op het hoogste niveau in Data Factory. U kunt parameters definiëren op pijplijnniveau en de argumenten doorgeven bij het uitvoeren van de pijplijn op aanvraag of met behulp van een trigger.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kan ik de standaardwaarden voor de pijplijnparameters definiëren? 
Ja. U kunt de standaardwaarden voor de parameters definiëren in de pijplijnen. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Kan een activiteit in een pijplijn argumenten die worden doorgegeven aan de uitvoering van een pijplijn gebruiken? 
Ja. Elke activiteit in de pijplijn kan de waarde van de parameter die is doorgegeven aan de pijplijn en uitvoeren met gebruiken de `@parameter` maken. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kan de eigenschap van een activiteit uitvoer worden gebruikt in een andere activiteit? 
Ja. De uitvoer van een activiteit kan worden gebruikt in een volgende activiteit met de `@activity` maken.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Hoe ga ik null-waarden in de uitvoer van een activiteit zonder problemen om? 
U kunt de `@coalesce` maken in de expressies voor het afhandelen van null-waarden zonder problemen. 

## <a name="mapping-data-flows"></a>Gegevensoverdrachten van toewijzing

### <a name="which-data-factory-version-do-i-use-to-create-data-flows"></a>Welke versie van Data Factory moet ik gebruiken om te gegevensstromen maken?
De Data Factory V2-versie gebruiken om gegevensstromen te maken.
  
### <a name="i-was-a-previous-private-preview-customer-who-used-data-flows-and-i-used-the-data-factory-v2-preview-version-for-data-flows"></a>Ik was vorige privépreview klant die gegevensstromen en kan ik de Data Factory V2 preview-versie voor gegevensoverdrachten gebruikt.
Deze versie is nu verouderd. Gebruik Data Factory V2 voor gegevensoverdrachten.
  
### <a name="what-has-changed-from-private-preview-to-limited-public-preview-in-regard-to-data-flows"></a>Wat is gewijzigd van beperkte Preview-versie voor beperkte openbare preview voor gegevensoverdrachten?
U hebt niet meer om uw eigen Azure Databricks-clusters. Data Factory beheert het maken van clusters en tear omlaag. BLOB-gegevenssets en Azure Data Lake Storage Gen2 gegevenssets worden verdeeld in tekst met scheidingstekens en Apache Parquet-gegevenssets. U kunt nog steeds Data Lake Storage Gen2 en Blob-opslag gebruiken voor het opslaan van deze bestanden. Gebruik de juiste gekoppelde service voor de opslag-engines.

### <a name="can-i-migrate-my-private-preview-factories-to-data-factory-v2"></a>Kan ik mijn privépreview factory's migreren naar Data Factory V2?

Ja. [Volg de instructies](https://www.slideshare.net/kromerm/adf-mapping-data-flow-private-preview-migration).

### <a name="i-need-help-troubleshooting-my-data-flow-logic-what-info-do-i-need-to-provide-to-get-help"></a>Ik moet informatie over het oplossen van mijn gegevensstroom logica. Welke gegevens heb ik nodig voor hulp bij?

Wanneer Microsoft biedt hulp of oplossen van problemen met gegevensstromen, geeft u het plan DSL-code. Voer de volgende stappen uit om dit te doen:

1. Selecteer in de Flow-ontwerper, **Code** in de rechterbovenhoek. Hiermee wordt de bewerkbare JSON-code voor de gegevensstroom weergegeven.
2. Selecteer in de codeweergave **plannen** in de rechterbovenhoek. Deze in-/ uitschakelen wordt overgeschakeld van JSON naar het kenmerk alleen-lezen opgemaakte DSL-script-abonnement.
3. Kopieer en plak dit script of opslaan in een tekstbestand.

### <a name="how-do-i-access-data-by-using-the-other-80-dataset-types-in-data-factory"></a>Hoe krijg ik toegang tot gegevens met behulp van de andere 80 gegevenssettypen in Data Factory?

De functie voor toewijzing gegevensstroom kan momenteel dat Azure SQL Database, Azure SQL Data Warehouse gescheiden tekstbestanden uit Azure Blob storage of Azure Data Lake Storage Gen2 en Parquet-bestanden uit Blob storage of Data Lake Storage Gen2 systeemeigen voor de bron en sink. 

Met de kopieeractiviteit gegevens te Faseren van een van de andere connectors en uitvoeren van een gegevensstroom activiteit voor het transformeren van gegevens nadat deze is tijdelijk worden opgeslagen. Bijvoorbeeld, de pijplijn wordt eerst naar Blob-opslag kopiëren en vervolgens een gegevensstroom activiteit een gegevensset wordt gebruikt in de bron om die gegevens te transformeren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van een data factory:

- [Snelstart: Een data factory maken](quickstart-create-data-factory-dot-net.md)
- [Zelfstudie: Kopiëren van gegevens in de cloud](tutorial-copy-data-dot-net.md)
