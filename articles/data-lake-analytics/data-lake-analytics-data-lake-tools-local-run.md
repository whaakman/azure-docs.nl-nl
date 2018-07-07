---
title: Uitvoering Azure Data Lake U-SQL-script op uw lokale machine | Microsoft Docs
description: Leer hoe u met Azure Data Lake Tools voor Visual Studio kunt U-SQL-taken uitvoeren op uw lokale computer.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a7f43c7e17f36d9b4e0767744eee9604c2628ea8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888963"
---
# <a name="run-u-sql-script-on-your-local-machine"></a>U-SQL-script uitvoeren op uw lokale computer

Bij het ontwikkelen van U-SQL-script, is het gebruikelijk dat U-SQL-script lokaal uitvoeren, kosten en de tijd worden opgeslagen. Azure Data Lake Tools voor Visual Studio biedt ondersteuning voor het uitvoeren van U-SQL-scripts op uw lokale computer. 

## <a name="basic-concepts-for-local-run"></a>Basisbeginselen voor het lokaal uitvoeren

Onderstaande diagram ziet u de onderdelen voor het lokaal uitvoeren en hoe deze onderdelen worden toegewezen aan cloud uitvoeren.

|Onderdeel|Lokaal uitvoeren|Cloud uitvoeren|
|---------|---------|---------|
|Storage|De hoofdmap van lokale gegevens|Standaard Azure Data Lake Store-account|
|Compute|U-SQL-engine met lokaal uitvoeren|Azure Data Lake Analytics-service|
|Uitvoeringsomgeving|Werkmap op de lokale computer|Azure Data Lake Analytics-cluster|

Meer uitleg voor onderdelen lokaal uitvoeren:

### <a name="local-data-root-folder"></a>De hoofdmap van lokale gegevens

De hoofdmap van lokale gegevens is een 'lokale opslag' voor de lokale compute-account. Een willekeurige map in het lokale bestandssysteem op uw lokale computer kan een lokale map in de hoofdmap van de gegevens zijn. Dit is gelijk aan het Azure Data Lake Store-standaardaccount van een Data Lake Analytics-account. Overschakelen naar een andere map in de hoofdmap van de gegevens is net als het overschakelen naar een andere store-standaardaccount. 

De hoofdmap van gegevens wordt gebruikt om:
- Store metagegevens, zoals databases, tabellen, tabelfuncties en assembly's.
- Zoek de invoer- en paden die zijn gedefinieerd als relatieve paden in U-SQL-script. Met behulp van relatieve paden gemakkelijker uw U-SQL-scripts te implementeren in Azure.

### <a name="u-sql-local-run-engine"></a>U-SQL-engine met lokaal uitvoeren

U-SQL lokaal uitvoeren-engine is een 'lokale compute-account' voor de U-SQL-taken. Gebruikers kunnen U-SQL-taken lokaal uitvoeren via Azure Data Lake Tools voor Visual Studio. Lokale uitvoering wordt ook ondersteund via de opdrachtregel en programming interfaces Azure Data Lake U-SQL-SDK. [Meer informatie over Azure Data Lake U-SQL-SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directory"></a>Werkmap

Wanneer een U-SQL-script is uitgevoerd, wordt een map werkt vereist voor het compileren resultaten, logboekbestanden voor het uitvoeren en caching. In Azure Data Lake Tools voor Visual Studio, de werkmap is van het project U-SQL-werkmap (gewoonlijk te vinden onder `<U-SQL project root path>/bin/debug>`). De werkmap worden verwijderd telkens wanneer een nieuwe geactiveerde uitvoert.

## <a name="local-run-in-visual-studio"></a>Lokaal uitvoeren in Visual Studio

Azure Data Lake Tools voor Visual Studio is een ingebouwde lokale engine worden uitgevoerd, en toont het als lokale compute-account. Uitvoeren van een U-SQL-script lokaal selecteren (lokale computer) of (Local-project)-account in de script-editor marge vervolgkeuzelijst en klikt u op **indienen**.

![Data Lake Tools voor Visual Studio submit script voor het lokale account](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-run-with-local-machine-account"></a>Lokaal uitvoeren met account (lokale computer)

(Lokale computer)-account is een gedeelde lokale compute-account met een één lokale gegevens hoofdmap als het lokale archief-account. De hoofdmap van gegevens is standaard zich bevindt op ' C:\Users\<gebruikersnaam > \AppData\Local\USQLDataRoot ", het is ook worden geconfigureerd via **Extra > Data Lake > Opties en instellingen**.

![Data Lake Tools voor Visual Studio configureren hoofdmap van de lokale gegevens](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Een U-SQL-project is vereist voor het lokaal uitvoeren. De werkmap van de U-SQL-project wordt gebruikt voor de werkmap van U-SQL lokaal uitvoeren. Compileren resultaten, logboekbestanden voor het uitvoeren en andere bestanden met betrekking tot uitvoering van taak zijn gegenereerd en opgeslagen onder de map directory tijdens de uitvoering van de lokale. Houd er rekening mee dat telkens wanneer u het script opnieuw uitvoeren, deze bestanden in de werkmap wordt opgeschoond en wordt opnieuw gegenereerd.

## <a name="local-run-with-local-project-account"></a>Lokaal uitvoeren met account (Local-project)

Rekening (local-project) is een lokale compute-project is geïsoleerd voor elk project met geïsoleerde lokale gegevens-hoofdmap. Elke actieve U-SQL-project in Solution Explorer openen beschikt over een corresponderende `(Local-project: <project name>)` account vermeld beide in Server Explorer en U-SQL-script-editor marge. 

Het account (Local-project) biedt een schone en geïsoleerd ontwikkelomgeving voor ontwikkelaars. In tegenstelling tot (lokale computer)-account met een gedeelde lokale gegevens hoofdmap opslaan van metagegevens en i/o-gegevens voor alle lokale taken, maakt (Local-project)-account een tijdelijke lokale Gegevenshoofdmap map onder de U-SQL project werkmap telkens wanneer een U-SQL-script Met deze eigenschap wordt uitgevoerd. Deze tijdelijke map in de hoofdmap van de gegevens opgehaald opgeschoond als opnieuw samenstellen of opnieuw uitvoeren gebeurt. 

U-SQL project biedt goede ervaring voor het beheren van deze geïsoleerde lokale omgeving uitvoeren via project referentie en de eigenschap. U kunt zowel de invoer gegevensbronnen voor U-SQL-scripts in het project, evenals de omgevingen van de database waarnaar wordt verwezen.

### <a name="manage-input-data-source-for-local-project-account"></a>Invoer van de gegevensbron voor (Local-project)-account beheren

De U-SQL project zorgt dat het lokale Gegevenshoofdmap map maken en gegevens instellen voor account (Local-project). Een tijdelijke map in de hoofdmap van de gegevens is verwijderd en opnieuw worden gemaakt onder de U-SQL project werkmap telkens opnieuw opbouwen en lokaal uitvoeren gebeurt. Alle gegevensbronnen die zijn geconfigureerd door de U-SQL-project worden gekopieerd naar deze tijdelijke lokale gegevens hoofdmap voordat lokale taak wordt uitgevoerd. 

U kunt configureren dat de hoofdmap van uw gegevensbronnen via **Klik met de rechtermuisknop op U-SQL project > eigenschap > gegevensbron testen**. Als U-SQL-script uitgevoerd op (lokale-project)-account, worden alle bestanden en submappen (inclusief bestanden in submappen) in **gegevensbron testen** map worden gekopieerd naar de tijdelijke lokale gegevens hoofdmap. Na het uitvoeren van lokale taak is voltooid, resultaten van de uitvoer kunnen ook te vinden onder de tijdelijke lokale gegevens hoofdmap in de werkmap project. Houd er rekening mee dat alle deze uitvoer wordt verwijderd en gewist wanneer het project wordt opnieuw opgebouwd en opgeschoond. 

![Data Lake Tools voor Visual Studio configureren project test-gegevensbron](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-referenced-database-environment-for-local-project-account"></a>Beheren van de database waarnaar wordt verwezen omgeving voor het account (Local-project) 

Als een U-SQL query gebruikt of query's met U-SQL database-objecten, moet u de databaseomgevingen klaar lokaal voordat dit U-SQL-script lokaal worden uitgevoerd. Voor account (Local-project), kunnen U-SQL-databaseafhankelijkheden worden beheerd door U-SQL projectverwijzingen. U kunt U-SQL-database projectverwijzingen aan uw project U-SQL kunt toevoegen. Voordat U-SQL-scripts worden uitgevoerd op (lokale-project)-account, worden alle databases waarnaar wordt verwezen naar de tijdelijke lokale gegevens hoofdmap geïmplementeerd. En voor elke uitvoering, de tijdelijke map in de hoofdmap van de gegevens worden gewist als een nieuwe geïsoleerde omgeving.

Gerelateerde artikelen:
* [Informatie over het beheren van de definitie van de U-SQL-database via U-SQL database-project](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)
* [Informatie over het beheren van de verwijzing naar de U-SQL-database in U-SQL project](data-lake-analytics-data-lake-tools-develop-usql-database.md)

## <a name="difference-between-local-machine-and-local-project-account"></a>Verschil tussen de (lokale computer) en het account (Local-project)

(Lokale computer)-account is erop gericht om te simuleren van een Azure Data Lake Analytics-account op de lokale computer gebruikers. Deze deelt dezelfde ervaring met Azure Data Lake Analytics-account. Doel (lokale-project) te bieden een gebruiksvriendelijke lokale ontwikkelingsomgeving waarmee gebruikers implementeren databases-verwijzingen en invoergegevens voordat het script lokaal worden uitgevoerd. (Lokale computer) account voorziet in een gedeelde permanente omgeving, die kan worden geopend via alle projecten. (Local-project) account voorziet in een geïsoleerde ontwikkelomgeving voor elk project en voor elke uitvoering wordt vernieuwd. Op basis van bovenstaande, biedt account (Local-project) een snellere ontwikkelervaring door snel nieuwe wijzigingen toepassen.

U vindt meer verschil tussen de (lokale computer) en (Local-project)-account in de grafiek als volgt te werk:

|Verschil hoek|(Lokale computer)|(Local-project)|
|----------------|---------------|---------------|
|Lokale toegang|Kan worden geopend door alle projecten|Alleen het bijbehorende project toegang tot dit account|
|Hoofdmap van de lokale gegevens|Een permanente lokale map. Geconfigureerde via **Tools > Data Lake > Opties en instellingen**|Een tijdelijke map gemaakt voor elke lokale uitvoering onder de U-SQL project werkmap. De map wordt opgeschoond als opnieuw samenstellen of opnieuw uitvoeren gebeurt|
|Invoergegevens voor U-SQL-script|Relatief pad in de permanente lokale gegevens hoofdmap|Stel via **eigenschap van U-SQL project > gegevensbron testen**. Alle bestanden, submappen worden gekopieerd naar de tijdelijke map in de hoofdmap van de gegevens voor lokale uitvoering|
|Uitvoergegevens voor de U-SQL-script|Relatief pad in de permanente lokale gegevens hoofdmap|Output naar de tijdelijke map in de hoofdmap van de gegevens. De resultaten worden opgeschoond als opnieuw samenstellen of opnieuw uitvoeren gebeurt.|
|Implementatie van de database waarnaar wordt verwezen|Waarnaar wordt verwezen, databases worden niet automatisch worden geïmplementeerd met het uitvoeren in (lokale computer) account. Hetzelfde voor het indienen van Azure Data Lake Analytics-account.|Databases waarnaar wordt verwezen worden naar het account (Local-project) automatisch voor lokale uitvoering geïmplementeerd. Alle databaseomgevingen zijn opgeschoond en opnieuw wordt geïmplementeerd als opnieuw samenstellen of opnieuw uitvoeren gebeurt.|

## <a name="local-run-with-u-sql-sdk"></a>Lokaal uitvoeren met U-SQL-SDK

Naast van U-SQL-scripts lokaal uitvoeren in Visual Studio, kunt u ook gebruiken de Azure Data Lake U-SQL-SDK voor de U-SQL-scripts lokaal uitvoeren met de opdrachtregel en programming interfaces. U kunt via deze interfaces automatiseren van U-SQL lokaal uitvoeren en testen.

[Meer informatie over Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md)
- [CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md)
- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
