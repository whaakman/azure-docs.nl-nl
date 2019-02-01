---
title: Gegevens importeren in Machine Learning Studio van online-gegevensbronnen
titleSuffix: Azure Machine Learning Studio
description: Dit artikel beschrijft de ondersteuning voor online-gegevens importeren uit verschillende bronnen en de informatie die nodig zijn voor het verplaatsen van gegevens uit deze bronnen in een Azure Machine Learning Studio-experiment.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 21b0cceabaf9cd7d284865e87086d695280df2bb
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486568"
---
# <a name="import-data-into-azure-machine-learning-studio-from-online-data-sources"></a>Gegevens importeren in Azure Machine Learning Studio uit online-gegevensbronnen
Dit artikel beschrijft de ondersteuning voor online-gegevens importeren uit verschillende bronnen en de informatie die nodig zijn voor het verplaatsen van gegevens uit deze bronnen in een Azure Machine Learning Studio-experiment.

> [!NOTE]
> In dit artikel bevat algemene informatie over de [importgegevens] [ import-data] module. Voor meer informatie over de typen gegevens die u kunt gedetailleerde, indelingen, parameters en antwoorden op veelgestelde vragen, Raadpleeg het naslagonderwerp voor de module voor de [importgegevens] [ import-data] module.
>
>

## <a name="introduction"></a>Inleiding
Met behulp van de [importgegevens] [ import-data] -module, u kunt toegang tot gegevens van een van verschillende online gegevensbronnen terwijl uw experiment wordt uitgevoerd [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* Een Web-URL met behulp van HTTP
* Hadoop met behulp van HiveQL
* Azure BLOB-opslag
* Azure-tabel
* Azure SQL database of SQL Server op Azure VM
* On-premises SQL Server-database
* Een data provider, die momenteel OData-feed
* Azure Cosmos DB

Voor toegang tot bronnen in uw experiment Studio online-gegevens, toevoegen de [gegevens importeren] [ import-data] module uw, selecteert u de **gegevensbron**, en geef vervolgens de parameters die nodig zijn voor toegang tot de gegevens. De online-gegevensbronnen die worden ondersteund in de onderstaande tabel enkele daarvan worden gespecificeerd. Deze tabel ziet u ook de bestandsindelingen die worden ondersteund en de parameters die worden gebruikt voor toegang tot de gegevens.

Houd er rekening mee dat omdat deze trainingsgegevens wordt geopend terwijl uw experiment wordt uitgevoerd, alleen beschikbaar in deze experiment is. Ter vergelijking: de gegevens die zijn opgeslagen in een gegevensset-module zijn beschikbaar voor een experiment in uw werkruimte.

> [!IMPORTANT]
> Op dit moment de [importgegevens] [ import-data] en [gegevens exporteren] [ export-data] modules kunnen lezen en schrijven van gegevens alleen uit die zijn gemaakt met de klassieke Azure-opslag implementatiemodel. Met andere woorden, is het nieuwe type uit Azure Blob Storage-account dat een hot storage-toegangslaag of cool storage-toegangslaag biedt nog niet ondersteund.
>
> Over het algemeen een Azure storage-accounts die u mogelijk hebt gemaakt voordat deze serviceoptie is beschikbaar geworden moet niet worden beïnvloed.
> Als u een nieuw account maken wilt, selecteert u **klassieke** voor de implementatie van het model, of gebruik van resourcemanager en selecteer **algemeen gebruik** in plaats van **Blob storage** voor  **Soort account**.
>
> Zie voor meer informatie, [Azure Blob-opslag: Hot en Cool Storage-lagen](../../storage/blobs/storage-blob-storage-tiers.md).
>
>

## <a name="supported-online-data-sources"></a>Online gegevensbronnen ondersteund
Azure Machine Learning **importgegevens** module biedt ondersteuning voor de volgende gegevensbronnen:

| Gegevensbron | Description | Parameters |
| --- | --- | --- |
| Web-URL via HTTP |Leest de gegevens in door komma's gescheiden waarden (CSV), door tabs gescheiden waarden (TSV), kenmerkrelatie bestand format (ARFF) en indelingen Support Vector Machines (SVM-licht), van een web-URL die gebruikmaakt van HTTP |<b>URL</b>: Hiermee geeft u de volledige naam van het bestand, met inbegrip van de site-URL en de bestandsnaam, met de extensie. <br/><br/><b>Gegevensindeling</b>: Hiermee geeft u een van de van ondersteunde gegevensindelingen: CSV, TSV, ARFF of SVM licht. Als de gegevens een rij met koppen heeft, wordt deze gebruikt voor het toewijzen van de namen van kolommen. |
| Hadoop/HDFS |Leest gegevens uit gedistribueerde opslag in Hadoop. U de gegevens die u wilt met behulp van HiveQL, een SQL-achtige querytaal. HiveQL kan ook worden gebruikt voor statistische gegevens en het uitvoeren van de gegevens filteren voordat u de gegevens aan Machine Learning Studio toevoegen. |<b>Hive-databasequery</b>: Hiermee geeft u de Hive-query die wordt gebruikt om de gegevens te genereren.<br/><br/><b>HCatalog-server URI </b> : De naam van uw cluster met behulp van de indeling opgegeven  *&lt;de clusternaam van uw&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop-gebruikersaccountnaam</b>: Hiermee geeft u de accountnaam van de Hadoop gebruiker gebruikt voor het inrichten van het cluster.<br/><br/><b>Het wachtwoord voor gebruikersaccount Hadoop</b> : Hiermee geeft u de referenties die worden gebruikt bij het inrichten van het cluster. Zie voor meer informatie, [Hadoop-clusters maken in HDInsight](../../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Locatie van de uitvoergegevens</b>: Hiermee geeft u op of de gegevens worden opgeslagen in een Hadoop distributed file system (HDFS) of in Azure. <br/><ul>Als u de uitvoergegevens in HDFS opslaat, geeft u de HDFS server-URI. (Zorg ervoor dat de naam van het HDInsight-cluster zonder het voorvoegsel HTTPS:// gebruiken). <br/><br/>Als u de uitvoergegevens in Azure opslaat, moet u de Azure storage-accountnaam, de toegangssleutel voor opslag en de naam van de opslagcontainer.</ul> |
| SQL-database |Leest de gegevens die zijn opgeslagen in een Azure SQL database of in een SQL Server-database die wordt uitgevoerd op een virtuele machine van Azure. |<b>Database-servernaam</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><ul>Voer in het geval van Azure SQL Database de naam van de server die wordt gegenereerd. Dit heeft meestal het formulier  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Voer in het geval van een SQL-server die wordt gehost op een virtueel Azure-machine *tcp:&lt;DNS-naam van virtuele Machine&gt;, 1433*</ul><br/><b>De naam van database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met voor de database toegangsmachtigingen. <br/><br/><b>Het wachtwoord voor gebruikersaccount server</b>: Hiermee geeft u het wachtwoord voor het gebruikersaccount.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| On-premises SQL-database |Leest de gegevens die zijn opgeslagen in een on-premises SQL-database. |<b>Gegevensgateway</b>: Hiermee geeft u de naam van de Data Management Gateway is geïnstalleerd op een computer waarop deze toegang heeft tot uw SQL Server-database. Zie voor meer informatie over het instellen van de gateway [advanced analytics met Azure Machine Learning met gegevens uit een on-premises SQL server uitvoeren](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Database-servernaam</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><br/><b>De naam van database </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>Naam van gebruikersaccount server</b>: Hiermee geeft u een gebruikersnaam voor een account met voor de database toegangsmachtigingen. <br/><br/><b>Gebruikersnaam en wachtwoord</b>: Klik op <b>waarden invoeren</b> de databasereferenties van uw in te voeren. U kunt Windows geïntegreerde verificatie of SQL Server-verificatie, afhankelijk van hoe uw on-premises SQL Server is geconfigureerd.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| Azure Table |Leest gegevens uit de tabelservice in Azure Storage.<br/><br/>Als u grote hoeveelheden gegevens niet regelmatig worden gelezen, gebruikt u de Azure Table-Service. Het biedt een flexibel, niet-relationele (NoSQL), zeer schaalbare, goedkope en maximaal beschikbare oplossing. |De opties in de **importgegevens** veranderen, afhankelijk van of u toegang tot de gegevens van openbare of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> die waarde van 'PublicOrSAS' of 'Account', die allemaal een eigen set parameters kan hebben. <br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: De parameters zijn:<br/><br/><ul><b>Tabel URI</b>: Hiermee geeft u de openbare of SAS-URL voor de tabel.<br/><br/><b>Hiermee geeft u de rijen die u wilt zoeken naar namen van eigenschappen</b>: De waarden zijn <i>TopN</i> om te scannen op het opgegeven aantal rijen, of <i>ScanAll</i> om op te halen van alle rijen in de tabel. <br/><br/>Als de gegevens homogene en voorspelbaar is, is het raadzaam dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die op basis van de diepte variëren en de positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/></ul><b>Private Storage-Account</b>: De parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account dat met de tabel om te lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de toegangssleutel van het opslagaccount dat is gekoppeld aan het account.<br/><br/><b>Tabelnaam</b> : Hiermee geeft u de naam van de tabel met de gegevens te lezen.<br/><br/><b>Rijen die u wilt zoeken naar namen van eigenschappen</b>: De waarden zijn <i>TopN</i> om te scannen op het opgegeven aantal rijen, of <i>ScanAll</i> om op te halen van alle rijen in de tabel.<br/><br/>Als de gegevens homogene en voorspelbare, raden wij aan dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die op basis van de diepte variëren en de positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Dit zorgt ervoor dat de integriteit van de resulterende eigenschap en metagegevens conversie.<br/><br/> |
| Azure Blob Storage |Leest de gegevens die zijn opgeslagen in de Blob-service in Azure Storage, waaronder afbeeldingen, ongestructureerde tekst of binaire gegevens.<br/><br/>U kunt de Blob-service gebruiken om gegevens openbaar te maken en om toepassingsgegevens Privé opslaan. U kunt toegang tot uw gegevens vanaf elke locatie met behulp van HTTP of HTTPS-verbindingen. |De opties in de **importgegevens** module wijzigen, afhankelijk van of u toegang tot de gegevens van openbare of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> dat kan een waarde van 'PublicOrSAS' of 'Account' hebben.<br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: De parameters zijn:<br/><br/><ul><b>URI</b>: Hiermee geeft u de openbare of SAS-URL voor de opslag-blob.<br/><br/><b>Bestandsindeling van</b>: Hiermee geeft u de indeling van de gegevens in de Blob-service. De ondersteunde indelingen zijn CSV, TSV en ARFF.<br/><br/></ul><b>Private Storage-Account</b>: De parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account met de blob die u wilt lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de toegangssleutel van het opslagaccount dat is gekoppeld aan het account.<br/><br/><b>Pad naar de container, map of blob </b> : Hiermee geeft u de naam van de blob met de gegevens te lezen.<br/><br/><b>BLOB-bestandsindeling</b>: Hiermee geeft u de indeling van de gegevens in de blob-service. De van ondersteunde gegevensindelingen zijn CSV, TSV, ARFF, CSV met een opgegeven codering en Excel. <br/><br/><ul>Als de indeling CSV-bestand of TSV is, zorg er dan voor dat aangeeft of het bestand een rij met koppen bevat.<br/><br/>U kunt de Excel-optie gebruiken om gegevens te lezen uit Excel-werkmappen. In de <i>Excel gegevensindeling</i> optie, geef aan of de gegevens in een Excel-werkblad voor het bereik, of in een Excel-tabel is. In de <i>Excel-werkblad of ingesloten tabel </i>optie, geef de naam van het werkblad of de tabel die u wilt lezen.</ul><br/> |
| -Gegevensfeedprovider |Leest gegevens uit een ondersteunde provider van de feed. Op dit moment alleen de Open Data Protocol (OData)-indeling wordt ondersteund. |<b>Data type inhoud</b>: Hiermee geeft u de OData-indeling.<br/><br/><b>Bron-URL</b>: Hiermee geeft u de volledige URL voor de gegevensfeed. <br/>Bijvoorbeeld, leest de volgende URL van de voorbeelddatabase: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Volgende stappen

[Implementeren van Azure Machine Learning studio-webservices die gebruikmaken van gegevensimport- en gegevensexportmodules](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
