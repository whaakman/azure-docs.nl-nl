---
title: 'Azure Data Factory: Veelgestelde vragen | Microsoft Docs'
description: Antwoorden op veelgestelde vragen over Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: ebe8745db06113d0508d86554bf031a4235c8e44
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045946"
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Data Factory.  

## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory? 
Data Factory is een volledig beheerde, cloud-gebaseerde gegevensintegratie-service waarmee de verplaatsing en transformatie van gegevens worden geautomatiseerd. Zoals een factory die wordt uitgevoerd apparatuur grondstoffen te transformeren in eindproducten, ingedeeld Azure Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie. 

Met behulp van Azure Data Factory, kunt u gegevensgestuurde werkstromen om gegevens te verplaatsen tussen on-premises en cloud gegevensarchieven. U kunt verwerken en transformatie-gegevens met behulp van compute-services zoals Azure HDInsight Azure Data Lake Analytics en SQL Server Integration Services (SSIS) integratie-runtime. 

Met Data Factory, kunt u uw gegevensverwerking uitvoeren op een op basis van een Azure cloudservice of in uw eigen host zichzelf compute-omgeving, zoals SSIS-, SQL Server- of Oracle. Nadat u een pijplijn die de actie uitgevoerd die u nodig hebt gemaakt, kunt u deze vanuit een exemplaar van gebeurtenis plannen uitvoeren regelmatig (bijvoorbeeld per uur, dagelijks of wekelijks), tijd-venster planning of trigger de pijplijn. Zie voor meer informatie [Inleiding tot Azure Data Factory](introduction.md).

### <a name="control-flows-and-scale"></a>Besturingselement stromen en schalen 
Ter ondersteuning van de diverse integratie stromen en patronen in het moderne-datawarehouse, Data Factory maakt flexibele data pipeline modellering met volledige Controlestroom voorwaardelijke wordt uitgevoerd, inclusief paradigma's programming vertakkingen in gegevenspijplijnen, en parameters binnen en tussen deze stromen expliciet doorgeven. Controlestroom omvat ook het omzetten van gegevens via de activiteit verzending naar externe uitvoering motoren en gegevensstroom mogelijkheden, zoals het verplaatsen van gegevens op grote schaal via de Kopieeractiviteit.

Data Factory biedt vrijheid als model voor de stijl van een stroom die is vereist voor gegevensintegratie en die kunnen worden verzonden op aanvraag of herhaaldelijk op een planning. Er zijn enkele algemene stromen die dit model stelt:   

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

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Uw gegevens op grote schaal met code gratis pijplijnen transformeren
De nieuwe ervaring voor de browser gebaseerde tooling biedt code gratis pijplijn ontwerpen en implementeren met een moderne, interactieve ervaring met web.

Voor ontwikkelaars van visuele gegevens en gegevens engineers is de ADF-Webgebruikersinterface de ontwerpomgeving code vrij die u gebruiken wilt voor het bouwen van pijplijnen. Het is volledig geïntegreerd met Visual Studio Online Git en biedt integratie voor CI/CD en iteratieve ontwikkeling met opties voor foutopsporing.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Rich cross-platform SDK's voor ervaren gebruikers
Als u een ervaren gebruikers en zoek naar een programma-interface, ADF V2 een uitgebreide set SDK's die kunnen worden gebruikt om te schrijven biedt, beheren, met behulp van uw favoriete IDE pijplijnen bewaken
1.  Python-SDK
2.  PowerShell CLI
3.  Gebruikers van C#-SDK kunt ook gebruikmaken van het gedocumenteerde REST API's om de interface met ADF V2

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Iteratieve ontwikkeling en foutopsporing met visual tools
Azure Data Factory (ADF) visual-hulpprogramma's kunnen u doen iteratieve ontwikkeling en foutopsporing. U kunt uw pijplijnen en ik wil deze test wordt uitgevoerd met behulp van de mogelijkheden voor foutopsporing in het canvas pijplijn zonder een één regel code te schrijven. U kunt de resultaten van de test wordt uitgevoerd in het venster uitvoer van de pipeline-canvas weergeven. Zodra de test uitgevoerd is geslaagd, kunt u meer activiteiten toevoegen aan uw pijplijn en doorgaan foutopsporing in een herhalende manier. Als ze in uitvoering zijn, kunt u uw testruns annuleren. U bent niet vereist voor het publiceren van uw wijzigingen naar de data factory-service voordat u foutopsporing op. Dit is handig in situaties waar u wilt ervoor zorgen dat de nieuwe toevoegingen of wijzigingen werk zoals verwacht vóór de upgrade van uw data factory-werkstromen in ontwikkeling, testen, of prod-omgevingen. 

### <a name="deploy-ssis-packages-to-azure"></a>SSIS-pakketten implementeren in Azure 
Als u wilt uw SSIS-werkbelastingen verplaatsen, kunt u een Gegevensfactory maken en inrichten van een Azure-SSIS-integratie-runtime. De Azure-SSIS-integratie-runtime is een volledig beheerde cluster met Azure Virtual machines (knooppunten) die zijn toegewezen voor het uitvoeren van SSIS-pakketten in de cloud. Zie voor stapsgewijze instructies de [implementeren SSIS-pakketten naar Azure](tutorial-create-azure-ssis-runtime-portal.md) zelfstudie. 
 
### <a name="sdks"></a>SDK's
Als u een ervaren gebruikers en zoek naar een programma-interface, ADF een uitgebreide set SDK's die u gebruiken kunt om te beheren biedt, ontwerpen en pijplijnen bewaken met behulp van uw favoriete IDE. Taalondersteuning bevat .NET, PowerShell, Python en REST.

### <a name="monitoring"></a>Bewaking
U kunt uw Gegevensfactory via PowerShell, SDK of de visuele controlehulpprogramma's in de gebruikersinterface van de browser kunt bewaken. U kunt bewaken en beheren op aanvraag, trigger basis en klok aangestuurd aangepaste stromen op een efficiënte en doeltreffende wijze. Bestaande taken annuleren, Zie fouten in een oogopslag inzoomen naar gedetailleerde foutberichten te krijgen, en problemen met de alle vanaf een door één venster zonder context overschakelen of heen en weer navigeren tussen schermen voor foutopsporing. 

### <a name="new-features-for-ssis-in-adf"></a>Nieuwe functies voor SSIS in ADF
Data Factory is de volgende functies toegevoegd voor SSIS sinds de eerste openbare Preview-versie in 2017:

-   Ondersteuning voor drie meer configuraties/varianten van Azure SQL Database (database) naar host SSIS-catalogus van projecten/pakketten (SSISDB):
-   Azure SQL database met VNet service-eindpunten
-   Beheerde exemplaar (MI)
-   Elastische pool
-   Ondersteuning voor Azure Resource Manager-netwerk (VNet) boven op het klassieke VNet dat in de toekomst – afgeschaft Hiermee kunt u uw Azure-SSIS integratie Runtime (IR) naar een VNet dat is geconfigureerd voor Azure SQL DB met VNet service-eindpunten/MI injecteren/join / lokale toegang tot gegevens, Zie: https://docs.microsoft.com/en-us/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network 
-   Ondersteuning voor verificatie van Azure Active Directory (AAD) boven op de SQL-verificatie verbinding maken met de SSISDB - Hiermee kunt u gebruikmaken van AAD-verificatie met uw ADF beheerde Service identiteit (MSI)
-   Ondersteuning voor uw eigen lokale SQL Server-licentie om te winnen aanzienlijke kostenbesparingen met de optie Azure hybride voordeel (AHB) te brengen
-   Ondersteuning voor Enterprise-editie van Azure SSIS IR waarmee u gebruik van geavanceerde/Premiumfuncties, aangepaste setup te installeren van extra onderdelen/extensies en 3e partij ecosysteem, Zie: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/ 
-   Betere integratie van SSIS in ADF waarmee u uitstekende SSIS-pakket uitvoeren activiteiten in ADF pijplijnen aanroepen/trigger en ze te plannen via SSMS, Zie: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>Wat is integratie runtime?
Integratie-runtime is de beheerinfrastructuur die wordt gebruikt door Azure Data Factory om te voorzien in de volgende mogelijkheden voor integratie van verschillende netwerkomgevingen:

- **Gegevensverplaatsing**: integratie Runtime worden voor verplaatsing van gegevens, de gegevens verplaatst tussen de gegevensarchieven bron- en doelserver tijdens het bieden van ondersteuning voor ingebouwde verbindingslijnen, conversie van de indeling, kolomtoewijzing, en zodat en schaalbare gegevensoverdracht.
- **Verzending van activiteiten**: voor transformatie, integratie Runtime bieden de mogelijkheid om uit te voeren systeemeigen SSIS-pakketten.
- **Uitvoeren van SSIS-pakketten**: systeemeigen SSIS-pakketten in een beheerde Azure compute-omgeving wordt uitgevoerd. Integration Runtime biedt ook ondersteuning voor transformatieactiviteiten voor verzending en bewaking die worden uitgevoerd in diverse compute-services zoals Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server en meer.

U kunt een of meer exemplaren van integratie runtime als vereist voor het verplaatsen en gegevens transformeren implementeren. Integratie runtime kunt uitvoeren op een openbare Azure-netwerk of op een particulier netwerk (on-premises, Azure Virtual Network of Amazon Web Services virtuele-privécloud [VPC]). 

Zie voor meer informatie [Integration Runtime in Azure Data Factory](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Wat is de limiet voor het aantal integratie runtimes?
Er is geen vaste limiet voor het aantal exemplaren van integratie runtime die een gegevensfactory kan bevatten. Er is echter een limiet voor het aantal kernen voor VM die de integratie-runtime voor de uitvoering van SSIS-pakket per abonnement kunt gebruiken. Zie voor meer informatie [Data Factory beperkt](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Wat zijn de op het hoogste niveau concepten van Azure Data Factory?
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bevat vier belangrijke onderdelen die samenwerken als een platform waarop u gegevensgestuurde werkstromen met stappen om te gaan en gegevens transformeren kunt samenstellen.

### <a name="pipelines"></a>Pijplijnen
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groepering van activiteiten uit te voeren een werkeenheid. De activiteiten in een pijplijn voeren samen een taak uit. Een pijplijn kan bijvoorbeeld een groep activiteiten die voor het opnemen van gegevens uit een Azure-blobopslag en voer vervolgens een Hive-query op een HDInsight-cluster voor het partitioneren van de gegevens bevatten. Het voordeel is dat u een pijplijn gebruiken kunt voor het beheren van de activiteiten als een set in plaats van elke activiteit afzonderlijk beheren. U kunt keten van de activiteiten in een pipeline om te worden na elkaar worden gebruikt of u ze onafhankelijk, parallel kan werken.

### <a name="activity"></a>Activiteit
Activiteiten vertegenwoordigen een verwerkingsstap in een pijplijn. U kunt bijvoorbeeld een *kopie* activiteit om gegevens te kopiëren uit een gegevensopslag met een ander gegevensarchief. Op deze manier kunt u een Hive-activiteit die wordt uitgevoerd een Hive-query op een Azure HDInsight-cluster te transformeren of analyseren van uw gegevens. Data Factory ondersteunt drie soorten activiteiten: activiteiten voor gegevensverplaatsing, activiteiten voor gegevenstransformatie en controleactiviteiten.

### <a name="datasets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Beschouw deze manier: een gekoppelde service definieert u de verbinding met de gegevensbron en de structuur van de gegevens van een gegevensset vertegenwoordigt. Een gekoppelde Azure Storage-service geeft bijvoorbeeld de verbindingsreeks voor verbinding met de Azure Storage-account. En een Azure Blob-gegevensset geeft de blob-container en de map waarin de gegevens.

Gekoppelde services hebben twee doelen in Data Factory:

- Vertegenwoordigt een *gegevensarchief* die bevat, maar is niet beperkt tot een lokale SQL Server-exemplaar, een Oracle-database-exemplaar, een bestandsshare of een Azure Blob storage-account. Zie voor een lijst van ondersteunde gegevensarchieven [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md).
- Ter vertegenwoordiging van een *rekenresource* die de uitvoering van een activiteit kan hosten. Bijvoorbeeld, de HDInsight Hive-activiteit wordt uitgevoerd op een HDInsight Hadoop-cluster. Zie voor een lijst van activiteiten voor gegevenstransformatie en ondersteunde berekeningsomgevingen [transformeer gegevens in Azure Data Factory](transform-data.md).

### <a name="triggers"></a>Triggers
Triggers vertegenwoordigen eenheden van de verwerking die bepalen wanneer een pipeline-uitvoering wordt gestart. Er zijn verschillende soorten triggers voor verschillende soorten gebeurtenissen. 

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

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Hoe kan ik op de hoogte blijven met informatie over Data Factory?
Voor de meest actuele informatie over Azure Data Factory, gaat u naar de volgende sites:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Documentatie-startpagina](/azure/data-factory)
- [Product-startpagina](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Technische diepgaand 

### <a name="how-can-i-schedule-a-pipeline"></a>Hoe kan ik een pijplijn plannen? 
U kunt de planner trigger of tijd venster trigger plannen van een pijplijn. De trigger een kalender wanden klok planning gebruikt en u kunt het plannen van pijplijnen periodiek of met behulp van periodieke patronen op basis van een kalender (bijvoorbeeld wekelijks op maandag op 18: 00 uur en donderdag om 9 uur). Zie [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie.

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Kan ik parameters doorgeven aan een pipeline-uitvoering
Ja, parameters zijn een concept klas, op het hoogste niveau in ADF. U kunt de parameters op het niveau van de pijplijn definiëren en argumenten doorgeven als u de pijplijn uitvoeren op aanvraag of met behulp van een trigger uitvoeren.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Kan ik de standaardwaarden voor de pipeline-parameters definiëren? 
Ja. U kunt de standaardwaarden voor de parameters definiëren in de pijplijnen. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Kan een activiteit in een pijplijn gebruiken argumenten die worden doorgegeven aan een pijplijn uitvoeren? 
Ja. Elke activiteit in de pijplijn gebruiken de parameterwaarde die is doorgegeven aan de pijplijn en worden uitgevoerd met de `@parameter` samenstellen. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Kan de eigenschap van een activiteit uitvoer worden gebruikt in een andere activiteit? 
Ja. De uitvoer van een activiteit kan worden gebruikt in een volgende activiteit met de `@activity` samenstellen.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Hoe ik null-waarden in de uitvoer van een activiteit probleemloos verwerkt? 
U kunt de `@coalesce` maken in de expressies voor het afhandelen van null-waarden probleemloos. 

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het maken van een gegevensfactory, de volgende zelfstudies:

- [Snelstartgids: Een gegevensfactory maken](quickstart-create-data-factory-dot-net.md)
- [Zelfstudie: Gegevens kopiëren in de cloud](tutorial-copy-data-dot-net.md)
