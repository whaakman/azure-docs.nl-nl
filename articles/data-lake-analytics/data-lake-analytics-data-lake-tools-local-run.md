---
title: Uitvoering Azure Data Lake U-SQL-scripts op uw lokale computer
description: Leer hoe u met Azure Data Lake Tools voor Visual Studio kunt U-SQL-taken uitvoeren op uw lokale computer.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 8f110a88558b4479d8fdadb2967c7dedeca60ce9
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43043554"
---
# <a name="run-u-sql-scripts-on-your-local-machine"></a>U-SQL-scripts uitvoeren op uw lokale computer

Wanneer u U-SQL-scripts ontwikkelen, bespaart u tijd en geld door de scripts lokaal worden uitgevoerd. Azure Data Lake Tools voor Visual Studio biedt ondersteuning voor actieve U-SQL-scripts op uw lokale computer. 

## <a name="basic-concepts-for-local-runs"></a>Basisbeginselen voor het lokaal wordt uitgevoerd

Het volgende diagram ziet u de onderdelen voor het lokaal uitvoeren en hoe deze onderdelen worden toegewezen aan cloud uitvoeren.

|Onderdeel|Lokaal uitvoeren|Cloud uitvoeren|
|---------|---------|---------|
|Storage|Hoofdmap van de lokale gegevens|Standaard Azure Data Lake Store-account|
|Compute|U-SQL-engine met lokaal uitvoeren|Azure Data Lake Analytics-service|
|Omgeving worden uitgevoerd|Werkmap op de lokale computer|Azure Data Lake Analytics-cluster|

De volgende secties geven informatie over de onderdelen van lokaal uitvoeren.

### <a name="local-data-root-folders"></a>Hoofdmappen van lokale gegevens

De hoofdmap van een lokale gegevens is een **lokale archief** voor de lokale compute-account. Een willekeurige map in het lokale bestandssysteem op uw lokale computer is de hoofdmap van een lokale gegevens. Dit is hetzelfde als het Azure Data Lake Store-standaardaccount van een Data Lake Analytics-account. Overschakelen naar de hoofdmap van een andere gegevens is net als het overschakelen naar een andere store-standaardaccount. 

De hoofdmap van de gegevens wordt als volgt gebruikt:
- Metagegevens Store. Voorbeelden zijn databases, tabellen, tabelfuncties en assembly's.
- Zoek de invoer- en paden die zijn gedefinieerd als relatieve paden in U-SQL-scripts. Met behulp van relatieve paden, is het gemakkelijker te implementeren van uw U-SQL-scripts in Azure.

### <a name="u-sql-local-run-engines"></a>U-SQL lokaal uitvoeren engines

Een U-SQL lokaal uitvoeren-engine is een **lokale compute-account** voor U-SQL-taken. Gebruikers kunnen U-SQL-taken lokaal uitvoeren via Azure Data Lake Tools voor Visual Studio. Lokaal wordt uitgevoerd, worden ook ondersteund via de opdrachtregel en programming interfaces voor Azure Data Lake U-SQL-SDK. Meer informatie over de [Azure Data Lake U-SQL-SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directories"></a>Mappen werken

Wanneer u een U-SQL-script uitvoert, wordt een werkmap directory nodig om te compileren resultaten in de cache, uitvoeren van Logboeken en andere functies uitvoeren. In Azure Data Lake Tools voor Visual Studio is de werkmap van het project U-SQL-werkmap. Deze bevindt zich onder `<U-SQL project root path>/bin/debug>`. De werkmap is opgeschoond telkens wanneer een nieuwe uitvoering wordt geactiveerd.

## <a name="local-runs-in-microsoft-visual-studio"></a>Lokaal wordt uitgevoerd in Microsoft Visual Studio

Azure Data Lake Tools voor Visual Studio hebben een ingebouwde lokale uitvoeren-engine. De engine van surface de hulpprogramma's voor als een lokale compute-account. Als u wilt een U-SQL-script lokaal uitvoeren, selecteert u de **Local machine** of **lokale-project** -account in van het script-editor marge in het menu. Selecteer vervolgens **indienen**.

![Indienen van een U-SQL-script voor een lokaal account](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-runs-with-a-local-machine-account"></a>Lokaal wordt uitgevoerd met een lokaal computeraccount

Een **Local machine** -account is een gedeelde lokale compute-account met de hoofdmap van een enkele lokale gegevens als het lokale archief-account. Standaard de hoofdmap van de gegevens zich bevindt op **C:\Users\<gebruikersnaam > \AppData\Local\USQLDataRoot**. Het is ook worden geconfigureerd via **extra** > **Data Lake** > **opties en instellingen**.

![Configureren van de hoofdmap van een lokale gegevens](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Een U-SQL-project is vereist voor een lokaal uitvoeren. De werkmap van de U-SQL-project wordt gebruikt voor de U-SQL lokaal uitvoeren werkmap. Compilatieresultaten, logboeken en andere bestanden met betrekking tot uitvoeren van taak uitvoeren worden gegenereerd en opgeslagen onder de werkmap tijdens het lokaal uitvoeren. Telkens wanneer u het script opnieuw uitvoeren, worden alle bestanden in de werkmap verwijderd en opnieuw gegenereerd.

## <a name="local-runs-with-a-local-project-account"></a>Lokaal wordt uitgevoerd met een account met lokale-project

Een **lokale-project** -account is een project afgeschermd lokale compute-account voor elk project met de hoofdmap van een geïsoleerde lokale gegevens. Elke actieve U-SQL-project dat wordt geopend in Solution Explorer in Visual Studio beschikt over een corresponderende `(Local-project: <project name>)` account. De accounts zijn opgenomen in Server Explorer in Visual Studio en de marge van U-SQL-script-editor.  

De **lokale-project** account voorziet in een schone en geïsoleerde omgeving. Een **Local machine** account heeft de hoofdmap van een gedeelde lokale gegevens waarmee gegevens van metagegevens en invoer en uitvoer voor alle lokale taken worden opgeslagen. Maar een **lokale-project** account de hoofdmap van een tijdelijke lokale gegevens onder de werkmap van een U-SQL-project wordt gemaakt telkens wanneer een U-SQL-script wordt uitgevoerd. Deze hoofdmap tijdelijke gegevens worden gewist wanneer een opnieuw maken of gebeurt opnieuw uitvoeren. 

Een U-SQL project beheert de geïsoleerde lokale uitvoeringsomgeving door middel van een project referentie en de eigenschap. U kunt de invoergegevensbronnen voor U-SQL-scripts configureren in de omgeving waarnaar wordt verwezen, database en het project.

### <a name="manage-the-input-data-source-for-a-local-project-account"></a>Beheren van de bron van de invoergegevens voor een account met lokale-project 

Een U-SQL-project wordt gemaakt van de hoofdmap van een lokale gegevens en stelt u de gegevens voor een **lokale-project** account. De hoofdmap van een tijdelijke gegevens is verwijderd en opnieuw worden gemaakt onder de werkmap van de U-SQL-project telkens wanneer een opnieuw opbouwen en lokaal uitvoeren gebeurt. Alle gegevensbronnen die zijn geconfigureerd met de U-SQL-project worden gekopieerd naar de hoofdmap van deze tijdelijke lokale gegevens voordat de lokale taak wordt uitgevoerd. 

U kunt de hoofdmap van uw gegevensbronnen configureren. Met de rechtermuisknop op **U-SQL project** > **eigenschap** > **gegevensbron testen**. Wanneer u een U-SQL-script uitvoert op een **lokale-project** -account, worden alle bestanden en submappen in de **gegevensbron testen** map worden gekopieerd naar de hoofdmap van de tijdelijke lokale gegevens. Bestanden in submappen worden opgenomen. Nadat een lokale taak is uitgevoerd, resultaten van de uitvoer kunnen ook te vinden onder de hoofdmap van de tijdelijke lokale gegevens in de werkmap project. Alle deze uitvoer is verwijderd en opgeschoond wanneer het project wordt opnieuw opgebouwd en opgeschoond. 

![Configureren van een project test-gegevensbron](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-a-referenced-database-environment-for-a-local-project-account"></a>Beheren van de omgeving van een database waarnaar wordt verwezen voor een **lokale-project** account 

Als een U-SQL query gebruikt of query's met U-SQL database-objecten, moet u de databaseomgevingen klaar lokaal voordat u de U-SQL-script lokaal uitvoeren. Voor een **lokale-project** U-SQL-database afhankelijkheden kunnen worden beheerd door de projectverwijzingen U-SQL-account. U kunt U-SQL-database projectverwijzingen aan uw project U-SQL kunt toevoegen. Voordat U-SQL-scripts worden uitgevoerd op een **lokale-project** -account, alle waarnaar wordt verwezen, databases zijn geïmplementeerd naar de hoofdmap van de tijdelijke lokale gegevens. En voor elke uitvoering, wordt de hoofdmap van de tijdelijke gegevens opgeschoond als een nieuwe geïsoleerde omgeving.

Dit verwante artikel raadplegen:
* Informatie over het beheren van definities van U-SQL-database en verwijzingen in [U-SQL database-projecten](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="the-difference-between-local-machine-and-local-project-accounts"></a>Het verschil tussen **Local machine** en **lokale-project** accounts

Een **Local machine** account simuleert een Azure Data Lake Analytics-account op lokale computers van gebruikers. Deze deelt dezelfde ervaring met een Azure Data Lake Analytics-account. Een **lokale-project** account voorziet in een gebruiksvriendelijke lokale ontwikkelomgeving. Deze omgeving helpt gebruikers van databaseverwijzingen en invoergegevens voordat ze lokaal uitvoeren van scripts implementeren. Een **Local machine** account voorziet in een gedeelde permanente omgeving die kan worden benaderd via alle projecten. Een **lokale-project** account voorziet in een geïsoleerde ontwikkelomgeving voor elk project. Voor elke uitvoering wordt deze vernieuwd. Een **lokale-project** account biedt een snellere ontwikkelingservaring door snel nieuwe wijzigingen toepassen.

Meer verschillen tussen **Local machine** en **lokale-project** -accounts worden weergegeven in de volgende tabel:

|Verschil hoek|Lokale machine|Lokale-project|
|----------------|---------------|---------------|
|Lokale toegang|Kunnen worden geopend door alle projecten.|Alleen het bijbehorende project hebben toegang tot dit account.|
|Hoofdmap van de lokale gegevens|Een permanente lokale map. Geconfigureerde via **extra** > **Data Lake** > **opties en instellingen**.|Een tijdelijke map gemaakt voor elke lokale uitvoering onder de U-SQL project werkmap. De map wordt verwijderd bij het opnieuw opbouwen of voer er gebeurt.|
|De invoergegevens voor een U-SQL-script|Het relatieve pad onder de hoofdmap van de permanente lokale gegevens.|Stel via **U-SQL projecteigenschap** > **gegevensbron testen**. Alle bestanden en submappen worden gekopieerd naar de hoofdmap van de tijdelijke gegevens voordat u een lokaal uitvoeren.|
|Uitvoergegevens voor een U-SQL-script|Relatief pad in de hoofdmap van de permanente lokale gegevens.|Uitvoer naar de hoofdmap van de tijdelijke gegevens. De resultaten worden opgeschoond bij het opnieuw opbouwen of voer er gebeurt.|
|Implementatie van de database waarnaar wordt verwezen|Databases waarnaar wordt verwezen niet worden automatisch geïmplementeerd met het uitvoeren in een **Local machine** account. Dit is hetzelfde voor het indienen van een Azure Data Lake Analytics-account.|Waarnaar wordt verwezen, databases zijn geïmplementeerd op de **lokale-project** account automatisch voordat u een lokaal uitvoeren. Alle databaseomgevingen worden schoongemaakt en opnieuw wordt geïmplementeerd als een opnieuw maken of gebeurt opnieuw uitvoeren.|

## <a name="a-local-run-with-the-u-sql-sdk"></a>Een lokaal uitvoeren met de U-SQL-SDK

U kunt U-SQL-scripts lokaal uitvoeren in Visual Studio en daarnaast de Azure Data Lake U-SQL-SDK gebruiken om het U-SQL-scripts lokaal uitvoeren met de opdrachtregel en programming interfaces. Via deze interfaces, kunt u U-SQL lokaal wordt uitgevoerd en tests automatiseren.

Meer informatie over de [Azure Data Lake U-SQL-SDK](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Volgende stappen

- [Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md).
- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md).
