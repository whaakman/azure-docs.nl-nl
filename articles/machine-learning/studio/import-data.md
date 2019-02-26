---
title: Gegevens importeren uit diverse bronnen
titleSuffix: Azure Machine Learning Studio
description: Hoe u uw gegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen. Meer informatie over welke gegevenstypen en opmaak van gegevens worden ondersteund.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/01/2019
ms.openlocfilehash: 0e2073efc0611e4e36c673f05aea0523a57d3aa7
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823247"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Uw trainingsgegevens vanuit verschillende gegevensbronnen importeren in Azure Machine Learning Studio

Als u uw eigen gegevens in Machine Learning Studio wilt ontwikkelen en trainen van een predictive analytics-oplossing, kunt u gegevens uit: 

* **Lokaal bestand** -lokale gegevens vooraf laden van de harde schijf te maken van een gegevensset-module in uw werkruimte
* **Online gegevensbronnen** -gebruik de [importgegevens] [ import-data] module voor toegang tot gegevens op basis van een van verschillende online gegevensbronnen terwijl uw experiment wordt uitgevoerd
* **Machine Learning Studio-experiment** -gegevens gebruiken die is opgeslagen als een gegevensset in Machine Learning Studio
* [**On-premises SQL Server-database** ](use-data-from-an-on-premises-sql-server.md) -gegevens uit een on-premises SQL Server-database gebruiken zonder dat het handmatig kopiëren van gegevens

> [!NOTE]
> Er zijn een aantal voorbeeldgegevenssets beschikbaar in Machine Learning Studio die u voor het trainen van gegevens gebruiken kunt. Zie voor meer informatie over deze [de voorbeeldgegevenssets gebruiken in Azure Machine Learning Studio](use-sample-datasets.md).

## <a name="prepare-data"></a>Gegevens voorbereiden

Machine Learning Studio is ontworpen voor gebruik met rechthoekige of tabellaire gegevens, zoals tekstgegevens die is gescheiden of gestructureerde gegevens uit een database, maar in sommige gevallen niet-rechthoekige gegevens kunnen worden gebruikt.

Het is raadzaam als uw gegevens relatief opgeschoond zijn voordat u deze in Studio importeert. Bijvoorbeeld, moet u voorzichtig zijn van problemen zoals zonder aanhalingstekens tekenreeksen.

Er zijn echter modules beschikbaar in Studio waarmee manipuleren van gegevens in uw experiment nadat u uw gegevens hebt geïmporteerd. Afhankelijk van de machine learning-algoritmen die u wilt gebruiken, moet u mogelijk om te bepalen hoe u gegevens structurele problemen zoals ontbrekende waarden en sparse gegevens kunt verwerken en -modules die u bij die helpen kunnen er zijn. Zoeken in de **gegevenstransformatie** gedeelte van het modulepalet voor modules die deze functies uitvoeren.

U kunt op elk gewenst moment in uw experiment weergeven of downloaden van de gegevens die wordt geproduceerd door een module door te klikken op de uitvoerpoort. Afhankelijk van de module, kunnen er verschillende Downloadinstellingen beschikbaar of kunt u mogelijk de gegevens in uw webbrowser in Studio visualiseren.

## <a name="supported-data-formats-and-data-types"></a>Ondersteunde gegevensindelingen en gegevenstypen

U kunt verschillende soorten gegevens importeren in uw experiment, afhankelijk van welke mechanisme die u voor het importeren van gegevens en waar deze vandaan gebruiken:

* Tekst zonder opmaak (*.txt)
* Door komma's gescheiden waarden (CSV met een koptekst (.csv) of zonder) (. nh.csv)
* Door tabs gescheiden waarden (TSV met een koptekst (.tsv) of zonder) (. nh.tsv)
* Excel-bestand
* Azure-tabel
* Hive-tabel
* SQL-databasetabel
* OData-waarden
* SVMLight gegevens (.svmlight) (Zie de [SVMLight definitie](http://svmlight.joachims.org/) voor informatie over de)
* Kenmerk Relation File Format (ARFF)-gegevens (.arff) (Zie de [ARFF definitie](http://weka.wikispaces.com/ARFF) voor informatie over de)
* ZIP-bestand (.zip)
* R-object of de werkruimte-bestand (. RData)

Als u gegevens in een indeling zoals ARFF met metagegevens importeert, Studio deze metagegevens gebruikt voor het definiëren van de kop en het gegevenstype van elke kolom.

Als u gegevens zoals TSV- of CSV-indeling die deze metagegevens bevat geen importeert, Studio het gegevenstype voor elke kolom door steekproeven van de gegevens. Als de gegevens ook geen kolomkoppen, biedt Studio standaardnamen.

U kunt expliciet opgeven of wijzigen van de koppen en gegevenstypen voor kolommen met behulp van de [metagegevens bewerken] [ edit-metadata] module.

De volgende gegevenstypen worden herkend door de Studio:

* String
* Geheel getal
* Double
* Booleaans
* DateTime
* TimeSpan

Studio maakt gebruik van een interne gegevenstype met de naam ***gegevenstabel*** om door te geven van gegevens tussen modules. U kunt uw gegevens expliciet converteren naar gegevens tabel opmaken met de [converteren naar gegevensset] [ convert-to-dataset] module.

Een module die dan gegevenstabel hebben converteert de tabel aan gegevens op de achtergrond voordat deze wordt doorgegeven aan de volgende module.

Indien nodig, kunt u de tabel gegevensindeling naar CSV, TSV, ARFF of SVMLight indeling met behulp van andere modules conversie kunt converteren.
Zoeken in de **indeling Gegevensconversies** gedeelte van het modulepalet voor modules die deze functies uitvoeren.

## <a name="data-capacities"></a>Gegevenscapaciteit

Modules in Machine Learning Studio ondersteunen gegevenssets tot 10 GB aan compacte numerieke gegevens voor algemeen gebruik. Als een module meer dan één invoer heeft, is de waarde 10 GB de totale invoergrootte. U kunt nemen uit grotere gegevenssets steekproef met behulp van query's van Hive of Azure SQL Database, of kunt u Learning by Counts vooraf voordat u importeert de gegevens.  

U kunt de volgende typen gegevens in grotere gegevenssets opnemen tijdens het normaliseren van kenmerken, tot maximaal 10 GB:

* Sparse
* Categorische gegevens
* Tekenreeksen
* Binaire gegevens

De volgende modules zijn beperkt tot gegevenssets die kleiner zijn dan 10 GB:

* Aanbevelingsmodules
* De module Synthetic Minority Oversampling Technique (SMOTE)
* Scriptmodules: R, Python, SQL
* Modules waarbij de grootte van de uitvoer groter is dan invoergegevens, zoals Join- of hash-functies
* Kruisvalidatie, Tune Model Hyperparameters, ordinale regressie en One-vs-All-multiklasse, wanneer het aantal herhalingen groot is

Voor gegevenssets die groter dan een paar GB zijn moet de gegevens uploaden naar Azure Storage of Azure SQL Database of gebruik van Azure HDInsight, in plaats van rechtstreeks vanuit een lokaal bestand te uploaden.

U vindt meer informatie over image-gegevens in de [afbeeldingen importeren](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/import-images#bkmk_Notes) -Moduleverwijzing voorkomen.

## <a name="import-from-a-local-file"></a>Importeren uit een lokaal bestand

U kunt een bestand met uploaden van de harde schijf om te gebruiken als trainingsgegevens in Studio. Wanneer u een gegevensbestand importeert, maakt u een gegevensset module klaar voor gebruik in experimenten in uw werkruimte.

Als u wilt gegevens importeren uit een lokale vaste schijf, het volgende doen:

1. Klik op **+ nieuw** aan de onderkant van de Studio-venster.
2. Selecteer **GEGEVENSSET** en **vanuit het lokale bestand**.
3. In de **uploaden van een nieuwe gegevensset** dialoogvenster, blader naar het bestand dat u wilt uploaden.
4. Voer een naam, het gegevenstype te identificeren en optioneel een beschrijving invoeren. Een beschrijving wordt aanbevolen: Hiermee kunt u om vast te leggen van alle kenmerken van de gegevens die u wilt Houd er rekening mee bij het gebruik van de gegevens in de toekomst.
5. Het selectievakje **dit is de nieuwe versie van een bestaande gegevensset** kunt u een bestaande gegevensset bijwerken met nieuwe gegevens. Om dit te doen, klikt u op dit selectievakje in en geef vervolgens de naam van een bestaande gegevensset.

![Upload een nieuwe gegevensset](./media/import-data/upload-dataset-from-local-file.png)

Uploaden tijd is afhankelijk van de grootte van uw gegevens en de snelheid van de verbinding met de service. Als u weet dat het bestand wordt een lange tijd in beslag nemen, kunt u andere dingen in Studio doen terwijl u wacht. Echter, de browser te sluiten voordat het uploaden van gegevens voltooid is, wordt het uploaden mislukt.

Nadat uw gegevens is geüpload, wordt opgeslagen in een gegevensset-module en is beschikbaar voor een experiment in uw werkruimte.

Wanneer u een experiment bewerkt, vindt u de gegevenssets die u hebt geüpload in de **mijn gegevenssets** lijst onder de **opgeslagen gegevenssets** lijst in het modulepalet. U kunt slepen en neerzetten van de gegevensset naar het experimentcanvas wanneer u wilt gebruiken van de gegevensset voor verdere analyse en machine learning.

## <a name="import-from-online-data-sources"></a>Importeren uit online-gegevensbronnen

Met behulp van de [gegevens importeren] [ import-data] -module, uw experiment kunt gegevens importeren uit verschillende online gegevensbronnen tijdens het experiment uitvoeren.

> [!NOTE]
> In dit artikel bevat algemene informatie over de [importgegevens] [ import-data] module. Voor meer informatie over de typen gegevens die u kunt gedetailleerde, indelingen, parameters en antwoorden op veelgestelde vragen, Raadpleeg het naslagonderwerp voor de module voor de [importgegevens] [ import-data] module.

Met behulp van de [importgegevens] [ import-data] -module, u kunt toegang tot gegevens van een van verschillende online gegevensbronnen terwijl uw experiment wordt uitgevoerd:

* Een Web-URL met behulp van HTTP
* Hadoop met behulp van HiveQL
* Azure BLOB-opslag
* Azure-tabel
* Azure SQL database of SQL Server op Azure VM
* On-premises SQL Server-database
* Een data provider, die momenteel OData-feed
* Azure Cosmos DB

Omdat deze trainingsgegevens wordt geopend terwijl uw experiment wordt uitgevoerd, is het alleen beschikbaar in deze experiment. Ter vergelijking: de gegevens die zijn opgeslagen in een gegevensset-module is beschikbaar voor een experiment in uw werkruimte.

Voor toegang tot online gegevensbronnen in de Studio-experiment moet toevoegen de [importgegevens] [ import-data] module naar het experimentcanvas. Selecteer vervolgens **Wizard importeren starten** onder **eigenschappen** voor stapsgewijze begeleide instructies om te selecteren en configureren van de gegevensbron. U kunt ook handmatig selecteren **gegevensbron** onder **eigenschappen** en de parameters die nodig zijn voor toegang tot de gegevens hebt opgegeven.

De online-gegevensbronnen die worden ondersteund in de onderstaande tabel enkele daarvan worden gespecificeerd. Deze tabel ziet u ook de bestandsindelingen die worden ondersteund en de parameters die worden gebruikt voor toegang tot de gegevens.

> [!IMPORTANT]
> Op dit moment de [importgegevens] [ import-data] en [gegevens exporteren] [ export-data] modules kunnen lezen en schrijven van gegevens alleen uit die zijn gemaakt met de klassieke Azure-opslag implementatiemodel. Met andere woorden, is het nieuwe type uit Azure Blob Storage-account dat een hot storage-toegangslaag of cool storage-toegangslaag biedt nog niet ondersteund.
>
> Over het algemeen een Azure storage-accounts die u mogelijk hebt gemaakt voordat deze serviceoptie is beschikbaar geworden moet niet worden beïnvloed.
> Als u een nieuw account maken wilt, selecteert u **klassieke** voor de implementatie van het model, of gebruik van resourcemanager en selecteer **algemeen gebruik** in plaats van **Blob storage** voor  **Soort account**.
>
> Zie voor meer informatie, [Azure Blob-opslag: Hot en Cool Storage-lagen](../../storage/blobs/storage-blob-storage-tiers.md).

### <a name="supported-online-data-sources"></a>Online gegevensbronnen ondersteund
Azure Machine Learning Studio **importgegevens** module biedt ondersteuning voor de volgende gegevensbronnen:

| Gegevensbron | Description | Parameters |
| --- | --- | --- |
| Web-URL via HTTP |Leest de gegevens in door komma's gescheiden waarden (CSV), door tabs gescheiden waarden (TSV), kenmerkrelatie bestand format (ARFF) en indelingen Support Vector Machines (SVM-licht), van een web-URL die gebruikmaakt van HTTP |<b>URL</b>: Hiermee geeft u de volledige naam van het bestand, met inbegrip van de site-URL en de bestandsnaam, met de extensie. <br/><br/><b>Gegevensindeling</b>: Hiermee geeft u een van de van ondersteunde gegevensindelingen: CSV, TSV, ARFF of SVM licht. Als de gegevens een rij met koppen heeft, wordt deze gebruikt voor het toewijzen van de namen van kolommen. |
| Hadoop/HDFS |Leest gegevens uit gedistribueerde opslag in Hadoop. U de gegevens die u wilt met behulp van HiveQL, een SQL-achtige querytaal. HiveQL kan ook worden gebruikt voor statistische gegevens en het uitvoeren van de gegevens filteren voordat u de gegevens aan de Studio toevoegen. |<b>Hive-databasequery</b>: Hiermee geeft u de Hive-query die wordt gebruikt om de gegevens te genereren.<br/><br/><b>HCatalog-server URI </b> : De naam van uw cluster met behulp van de indeling opgegeven  *&lt;de clusternaam van uw&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop-gebruikersaccountnaam</b>: Hiermee geeft u de accountnaam van de Hadoop gebruiker gebruikt voor het inrichten van het cluster.<br/><br/><b>Het wachtwoord voor gebruikersaccount Hadoop</b> : Hiermee geeft u de referenties die worden gebruikt bij het inrichten van het cluster. Zie voor meer informatie, [Hadoop-clusters maken in HDInsight](/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).<br/><br/><b>Locatie van de uitvoergegevens</b>: Hiermee geeft u op of de gegevens worden opgeslagen in een Hadoop distributed file system (HDFS) of in Azure. <br/><ul>Als u de uitvoergegevens in HDFS opslaat, geeft u de HDFS server-URI. (Zorg ervoor dat de naam van het HDInsight-cluster zonder het voorvoegsel HTTPS:// gebruiken). <br/><br/>Als u de uitvoergegevens in Azure opslaat, moet u de Azure storage-accountnaam, de toegangssleutel voor opslag en de naam van de opslagcontainer.</ul> |
| SQL-database |Leest de gegevens die zijn opgeslagen in een Azure SQL database of in een SQL Server-database die wordt uitgevoerd op een virtuele machine van Azure. |<b>Database-servernaam</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><ul>Voer in het geval van Azure SQL Database de naam van de server die wordt gegenereerd. Dit heeft meestal het formulier  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Voer in het geval van een SQL-server die wordt gehost op een virtueel Azure-machine *tcp:&lt;DNS-naam van virtuele Machine&gt;, 1433*</ul><br/><b>De naam van database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met voor de database toegangsmachtigingen. <br/><br/><b>Het wachtwoord voor gebruikersaccount server</b>: Hiermee geeft u het wachtwoord voor het gebruikersaccount.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| On-premises SQL-database |Leest de gegevens die zijn opgeslagen in een on-premises SQL-database. |<b>Gegevensgateway</b>: Hiermee geeft u de naam van de Data Management Gateway is geïnstalleerd op een computer waarop deze toegang heeft tot uw SQL Server-database. Zie voor meer informatie over het instellen van de gateway [advanced analytics met Azure Machine Learning Studio met gegevens uit een on-premises SQL server uitvoeren](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Database-servernaam</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><br/><b>De naam van database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met voor de database toegangsmachtigingen. <br/><br/><b>Gebruikersnaam en wachtwoord</b>: Klik op <b>waarden invoeren</b> de databasereferenties van uw in te voeren. U kunt Windows geïntegreerde verificatie of SQL Server-verificatie, afhankelijk van hoe uw on-premises SQL Server is geconfigureerd.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| Azure Table |Leest gegevens uit de tabelservice in Azure Storage.<br/><br/>Als u grote hoeveelheden gegevens niet regelmatig worden gelezen, gebruikt u de Azure Table-Service. Het biedt een flexibel, niet-relationele (NoSQL), zeer schaalbare, goedkope en maximaal beschikbare oplossing. |De opties in de **importgegevens** veranderen, afhankelijk van of u toegang tot de gegevens van openbare of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> die waarde van 'PublicOrSAS' of 'Account', die allemaal een eigen set parameters kan hebben. <br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: De parameters zijn:<br/><br/><ul><b>Tabel URI</b>: Hiermee geeft u de openbare of SAS-URL voor de tabel.<br/><br/><b>Hiermee geeft u de rijen die u wilt zoeken naar namen van eigenschappen</b>: De waarden zijn <i>TopN</i> om te scannen op het opgegeven aantal rijen, of <i>ScanAll</i> om op te halen van alle rijen in de tabel. <br/><br/>Als de gegevens homogene en voorspelbaar is, is het raadzaam dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die op basis van de diepte variëren en de positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/></ul><b>Private Storage-Account</b>: De parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account dat met de tabel om te lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de toegangssleutel van het opslagaccount dat is gekoppeld aan het account.<br/><br/><b>Tabelnaam</b> : Hiermee geeft u de naam van de tabel met de gegevens te lezen.<br/><br/><b>Rijen die u wilt zoeken naar namen van eigenschappen</b>: De waarden zijn <i>TopN</i> om te scannen op het opgegeven aantal rijen, of <i>ScanAll</i> om op te halen van alle rijen in de tabel.<br/><br/>Als de gegevens homogene en voorspelbare, raden wij aan dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die op basis van de diepte variëren en de positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/> |
| Azure Blob Storage |Leest de gegevens die zijn opgeslagen in de Blob-service in Azure Storage, waaronder afbeeldingen, ongestructureerde tekst of binaire gegevens.<br/><br/>U kunt de Blob-service gebruiken om gegevens openbaar te maken en om toepassingsgegevens Privé opslaan. U kunt toegang tot uw gegevens vanaf elke locatie met behulp van HTTP of HTTPS-verbindingen. |De opties in de **importgegevens** module wijzigen, afhankelijk van of u toegang tot de gegevens van openbare of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> dat kan een waarde van 'PublicOrSAS' of 'Account' hebben.<br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: De parameters zijn:<br/><br/><ul><b>URI</b>: Hiermee geeft u de openbare of SAS-URL voor de opslag-blob.<br/><br/><b>Bestandsindeling van</b>: Hiermee geeft u de indeling van de gegevens in de Blob-service. De ondersteunde indelingen zijn CSV, TSV en ARFF.<br/><br/></ul><b>Private Storage-Account</b>: De parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account met de blob die u wilt lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de toegangssleutel van het opslagaccount dat is gekoppeld aan het account.<br/><br/><b>Pad naar de container, map of blob </b> : Hiermee geeft u de naam van de blob met de gegevens te lezen.<br/><br/><b>BLOB-bestandsindeling</b>: Hiermee geeft u de indeling van de gegevens in de blob-service. De van ondersteunde gegevensindelingen zijn CSV, TSV, ARFF, CSV met een opgegeven codering en Excel. <br/><br/><ul>Als de indeling CSV-bestand of TSV is, zorg er dan voor dat aangeeft of het bestand een rij met koppen bevat.<br/><br/>U kunt de Excel-optie gebruiken om gegevens te lezen uit Excel-werkmappen. In de <i>Excel gegevensindeling</i> optie, geef aan of de gegevens in een Excel-werkblad voor het bereik, of in een Excel-tabel is. In de <i>Excel-werkblad of ingesloten tabel </i>optie, geef de naam van het werkblad of de tabel die u wilt lezen.</ul><br/> |
| -Gegevensfeedprovider |Leest gegevens uit een ondersteunde provider van de feed. Op dit moment alleen de Open Data Protocol (OData)-indeling wordt ondersteund. |<b>Data type inhoud</b>: Hiermee geeft u de OData-indeling.<br/><br/><b>Bron-URL</b>: Hiermee geeft u de volledige URL voor de gegevensfeed. <br/>Bijvoorbeeld, leest de volgende URL van de voorbeelddatabase: http://services.odata.org/northwind/northwind.svc/ |

## <a name="import-from-another-experiment"></a>Importeren uit een ander experiment

Er zijn tijden wanneer moet u een tussentijds resultaat van een experiment uitvoeren en deze gebruiken als onderdeel van een ander experiment. Om dit te doen, kunt u de module als een gegevensset opslaan:

1. Klik op de uitvoer van de module die u wilt opslaan als een gegevensset.
2. Klik op **opslaan als gegevensset**.
3. Wanneer u wordt gevraagd, typt u een naam en een beschrijving op die manier kunt u eenvoudig identificeren van de gegevensset.
4. Klik op de **OK** vinkje.

Als het opslaan is voltooid, wordt de gegevensset worden beschikbaar voor gebruik in een experiment in uw werkruimte. U vindt deze in de **opgeslagen gegevenssets** lijst in het modulepalet.

## <a name="next-steps"></a>Volgende stappen

[Implementeren van Azure Machine Learning studio-webservices die gebruikmaken van gegevensimport- en gegevensexportmodules](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/


<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
