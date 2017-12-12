---
title: Gegevens importeren in Machine Learning Studio uit gegevensbronnen voor online | Microsoft Docs
description: Hoe uw trainingsgegevens Azure Machine Learning Studio importeert uit verschillende online bronnen.
keywords: gegevens, gegevensindeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: c6185cd240d1c040c993e581c27624e1f170f709
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Gegevens importeren in Azure Machine Learning Studio van verschillende online gegevensbronnen met de module Gegevens importeren
In dit artikel beschrijft de ondersteuning voor het importeren van online gegevens uit diverse bronnen en de benodigde informatie voor het verplaatsen van gegevens van deze bronnen naar een Azure Machine Learning-experiment.

> [!NOTE]
> In dit artikel bevat algemene informatie over de [importgegevens] [ import-data] module. Zie voor meer informatie over de typen gegevens die u toegang hebt tot, indelingen, parameters en antwoorden op veelgestelde vragen het naslagonderwerp voor de module voor de [importgegevens] [ import-data] module.
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Inleiding
Met behulp van de [importgegevens] [ import-data] -module, u kunt toegang tot gegevens van een van verschillende gegevensbronnen voor online terwijl uw experiment wordt uitgevoerd [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* Een met behulp van HTTP-URL
* Hadoop HiveQL gebruiken
* Azure BLOB-opslag
* Azure-tabel
* Azure SQL database of SQL Server op Azure VM
* Lokale SQL Server-database.
* Een gegevensfeed momenteel OData-provider
* Azure Cosmos DB

Voor toegang tot bronnen in uw experiment Studio online, voeg de [gegevens importeren] [ import-data] module die u wilt uw, selecteer de **gegevensbron**, en geef vervolgens de parameters die toegang nodig is voor de gegevens. De online gegevensbronnen die worden ondersteund zijn gespecificeerd in de onderstaande tabel. Deze tabel ziet u ook voor de parameters die worden gebruikt voor toegang tot de gegevens en bestandsindelingen die worden ondersteund.

Houd er rekening mee dat omdat deze trainingsgegevens wordt geopend terwijl uw experiment wordt uitgevoerd, alleen beschikbaar in deze experiment is. Vergelijking: gegevens die zijn opgeslagen in een module gegevensset zijn beschikbaar voor alle experimenten in uw werkruimte.

> [!IMPORTANT]
> Op dit moment wordt de [importgegevens] [ import-data] en [gegevens exporteren] [ export-data] modules kunnen lezen en schrijven van gegevens alleen via gemaakt met behulp van de klassieke Azure-opslag implementatiemodel. Met andere woorden, is het nieuwe type uit Azure Blob Storage-account dat een hot storage-toegangslaag of cool storage-toegangslaag biedt nog niet ondersteund. 
> 
> In het algemeen een Azure storage-accounts die u mogelijk hebt gemaakt voordat u deze serviceoptie beschikbaar zijn geworden moet niet worden beïnvloed. 
> Als u een nieuw account maakt moet, selecteert u **klassieke** voor de implementatie model, of gebruik van resourcemanager en selecteer **algemeen** plaats **Blob storage** voor  **Account kind**. 
> 
> Zie voor meer informatie [Azure Blob Storage: Hot en Cool Opslaglagen](../../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Ondersteunde gegevensbronnen voor online
Azure Machine Learning **importgegevens** module biedt ondersteuning voor de volgende gegevensbronnen:

| Gegevensbron | Beschrijving | Parameters |
| --- | --- | --- |
| Web-URL via HTTP |Leest de gegevens in de door komma's gescheiden waarden (CSV), door tabs gescheiden waarden (TSV), kenmerkrelatie bestandsindeling (ARFF) en indelingen ondersteuning Vector Machines (SVM licht), uit een web-URL die HTTP gebruikt |<b>URL</b>: Hiermee geeft u de volledige naam van het bestand, met inbegrip van de site-URL en de bestandsnaam extensie. <br/><br/><b>Gegevensindeling</b>: Hiermee geeft u een van de gegevens van ondersteunde indelingen: CSV, TSV, ARFF of SVM licht. Als de gegevens een rij met kolomkoppen heeft, wordt deze gebruikt om toe te wijzen kolomnamen. |
| Hadoop/HDFS |Leest de gegevens van gedistribueerde opslag in Hadoop. U opgeven de gegevens die u wilt met behulp van HiveQL, een SQL-achtige querytaal. HiveQL kan ook worden gebruikt voor statistische gegevens en het uitvoeren van de gegevens filteren voordat u de gegevens aan Machine Learning Studio toevoegen. |<b>Hive-databasequery</b>: Hiermee geeft u de Hive-query gebruikt voor het genereren van de gegevens.<br/><br/><b>HCatalog server-URI </b> : de naam van het cluster met de indeling opgegeven  *&lt;uw clusternaam&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop-gebruikersaccountnaam</b>: Hiermee geeft u de Hadoop-gebruikersaccountnaam die wordt gebruikt voor het inrichten van het cluster.<br/><br/><b>Het wachtwoord voor gebruikersaccount Hadoop</b> : Hiermee geeft u de referenties die worden gebruikt bij het inrichten van het cluster. Zie voor meer informatie [maken Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Locatie van uitvoergegevens</b>: Hiermee geeft u op of de gegevens worden opgeslagen in een Hadoop distributed file system (HDFS) of in Azure. <br/><ul>Als u de uitvoergegevens in HDFS opslaat, geeft u de URI van de HDFS-server. (Zorg ervoor dat de naam van het HDInsight-cluster zonder het voorvoegsel HTTPS:// gebruiken). <br/><br/>Als u de uitvoergegevens in Azure opslaat, moet u de Azure storage-account, toegangssleutel voor opslag en opslag containernaam opgeven.</ul> |
| SQL Database |Leest de gegevens die zijn opgeslagen in een Azure SQL database of in een SQL Server-database die is uitgevoerd op een virtuele machine van Azure. |<b>Naam van de databaseserver</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><ul>Voer de naam van de server die wordt gegenereerd in geval van een Azure SQL Database. Deze heeft meestal het formulier  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Voer in het geval van een SQL server gehost op een virtuele Azure-machine *tcp:&lt;DNS-naam van virtuele Machine&gt;, 1433*</ul><br/><b>Databasenaam </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>De accountnaam van de gebruiker server</b>: Hiermee geeft u een gebruikersnaam voor een account met toegangsmachtigingen voor de database. <br/><br/><b>Het wachtwoord voor gebruikersaccount server</b>: Hiermee geeft u het wachtwoord voor het gebruikersaccount.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| On-premises SQL-database |Leest de gegevens die zijn opgeslagen in een lokale SQL-database. |<b>Gegevensgateway</b>: Hiermee geeft u de naam van de Data Management Gateway is geïnstalleerd op een computer waarop deze toegang hebben tot uw SQL Server-database. Zie voor meer informatie over het instellen van de gateway [advanced analytics met Azure Machine Learning met gegevens uit een lokale SQL server uitvoeren](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Naam van de databaseserver</b>: Hiermee geeft u de naam van de server waarop de database wordt uitgevoerd.<br/><br/><b>Databasenaam </b>: Hiermee geeft u de naam van de database op de server. <br/><br/><b>De accountnaam van de gebruiker server</b>: Hiermee geeft u een gebruikersnaam voor een account met toegangsmachtigingen voor de database. <br/><br/><b>Gebruikersnaam en wachtwoord</b>: klik op <b>Voer waarden in</b> de databasereferenties van uw in te voeren. U kunt Windows-verificatie of SQL Server-verificatie, al naar gelang hoe uw lokale SQL Server is geconfigureerd.<br/><br/><b>Databasequery</b>: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. |
| Azure-tabel |Leest de gegevens uit de tabelservice in Azure Storage.<br/><br/>Als u grote hoeveelheden gegevens onregelmatig lezen, moet u de Azure-tabel-Service gebruiken. Het biedt een flexibele, niet-relationele (NoSQL), sterk schaalbare, goedkope en maximaal beschikbare opslagoplossing. |De opties in de **importgegevens** wijzigen, afhankelijk van of u toegang tot openbare gegevens of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> die de waarde van 'PublicOrSAS' of 'Account', die elk een eigen set parameters heeft kunnen hebben. <br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: de parameters zijn:<br/><br/><ul><b>Tabel URI</b>: Hiermee geeft u de openbare of SAS-URL voor de tabel.<br/><br/><b>Hiermee geeft u de rijen om te scannen op de namen van eigenschappen</b>: de waarden zijn <i>TopN</i> voor het scannen van het opgegeven aantal rijen, of <i>ScanAll</i> ophalen van alle rijen in de tabel. <br/><br/>Als de gegevens homogene en voorspelbaar is, wordt aanbevolen dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die, afhankelijk van de diepte variëren en positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Hierdoor wordt de integriteit van de resulterende eigenschap en metagegevens van conversie.<br/><br/></ul><b>Persoonlijke Opslagaccount</b>: de parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account met de tabel om te lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de opslagsleutel die is gekoppeld aan het account.<br/><br/><b>Tabelnaam</b> : Hiermee geeft u de naam van de tabel met de gegevens te lezen.<br/><br/><b>Rijen om te scannen op de namen van eigenschappen</b>: de waarden zijn <i>TopN</i> voor het scannen van het opgegeven aantal rijen, of <i>ScanAll</i> ophalen van alle rijen in de tabel.<br/><br/>Als de gegevens homogene en voorspelbaar worden, raden wij aan dat u selecteert *TopN* en voer een getal voor N. Voor grote tabellen, kan dit resulteren in sneller lezen tijden.<br/><br/>Als de gegevens is opgebouwd met sets van eigenschappen die, afhankelijk van de diepte variëren en positie van de tabel, kiest u de *ScanAll* optie voor het scannen van alle rijen. Hierdoor wordt de integriteit van de resulterende eigenschap en metagegevens van conversie.<br/><br/> |
| Azure Blob Storage |Leest de gegevens die zijn opgeslagen in de Blob-service in Azure Storage, met inbegrip van afbeeldingen, niet-gestructureerde tekst of binaire gegevens.<br/><br/>U kunt de Blob-service gegevens openbaar toegankelijk te maken, of voor het opslaan van toepassingsgegevens privé gebruiken. U kunt toegang tot uw gegevens vanaf elke locatie met behulp van HTTP of HTTPS-verbindingen. |De opties in de **importgegevens** module wijzigen, afhankelijk van of u toegang tot openbare gegevens of een persoonlijke opslagaccount waarvoor aanmeldingsreferenties. Dit wordt bepaald door de <b>verificatietype</b> dit kan een waarde van 'PublicOrSAS' of 'Account' hebben.<br/><br/><b>Openbare of Shared Access Signature (SAS) URI</b>: de parameters zijn:<br/><br/><ul><b>URI</b>: Hiermee geeft u de openbare of SAS-URL voor de storage-blob.<br/><br/><b>Bestandsindeling</b>: Hiermee geeft u de indeling van de gegevens in de Blob-service. De ondersteunde indelingen zijn CSV TSV en ARFF.<br/><br/></ul><b>Persoonlijke Opslagaccount</b>: de parameters zijn: <br/><br/><ul><b>Accountnaam</b>: Hiermee geeft u de naam van het account met de blob die u wilt lezen.<br/><br/><b>Accountsleutel</b>: Hiermee geeft u de opslagsleutel die is gekoppeld aan het account.<br/><br/><b>Pad naar de container, map of blob </b> : Hiermee geeft u de naam van de blob met de gegevens te lezen.<br/><br/><b>BLOB-bestandsindeling</b>: Hiermee geeft u de indeling van de gegevens in de blob-service. De van ondersteunde gegevensindelingen zijn CSV, TSV, ARFF, CSV met een opgegeven codering en Excel. <br/><br/><ul>Als de indeling CSV of TSV is, zorg er dan voor dat aangeeft of het bestand een veldnamenrij bevat.<br/><br/>De Excel-optie kunt u gegevens uit Excel-werkmappen te lezen. In de <i>Excel gegevensindeling</i> optie, geef aan of de gegevens in een Excel-werkblad voor het bereik of in een Excel-tabel is. In de <i>Excel-werkblad of ingesloten tabel </i>optie, geef de naam van het blad of de tabel die u lezen wilt uit.</ul><br/> |
| Feed-gegevensprovider |Leest de gegevens van een ondersteunde provider van de feed. Op dit moment alleen de Open Data Protocol (OData)-indeling wordt ondersteund. |<b>Gegevens inhoudstype</b>: Hiermee geeft u de OData-indeling.<br/><br/><b>Bron-URL</b>: Hiermee geeft u de volledige URL voor de gegevensfeed. <br/>De volgende URL leest bijvoorbeeld uit de voorbeelddatabase Northwind: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Volgende stappen

[Azure ML webservices die gebruikmaken van gegevens importeren en exporteren van gegevens modules implementeren](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
