---
title: Azure Data Lake Analytics U-SQL-taken met behulp van SSIS plannen
description: Informatie over het gebruik van SQL Server Integration Services om U-SQL-taken te plannen.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/17/2018
ms.openlocfilehash: 7d5630c082c044ac936f555965aec5a2a00f3544
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448717"
---
# <a name="schedule-u-sql-jobs-using-sql-server-integration-services-ssis"></a>U-SQL-taken met behulp van SQL Server Integration Services (SSIS) plannen

In dit document leert u hoe u indelen en maakt U-SQL-taken met behulp van SQL Server Integration Service (SSIS). 

## <a name="prerequisites"></a>Vereisten

[Azure Feature Pack voor integratieservices](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud) biedt de [Azure Data Lake Analytics-taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017) en de [Azure Data Lake Analytics Connection Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017) dat helpt verbinding met Azure Data Lake maken Analytics-service. Voor het gebruik van deze taak, zorg ervoor dat u installeert:

- [Download en installeer SQL Server Data Tools (SSDT) voor Visual Studio](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017)
- [Azure Feature Pack voor de integratieservices (SSIS) installeren](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017)

## <a name="azure-data-lake-analytics-task"></a>Azure Data Lake Analytics-taak

De Azure Data Lake Analytics-taak toestaan dat gebruikers verzenden van U-SQL-taken naar de Azure Data Lake Analytics-account. 

[Meer informatie over het configureren van Azure Data Lake Analytics-taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017).

![Azure Data Lake Analytics-taak in SSIS](./media/data-lake-analytics-schedule-jobs-ssis/data-lake-analytics-azure-data-lake-analytics-task-in-ssis.png)

U kunt de U-SQL-script ophalen uit verschillende plaatsen met behulp van ingebouwde functies voor SSIS en taken, onderstaande scenario's weer te geven hoe kunt u de U-SQL-scripts voor het geval van de andere gebruiker configureren.

## <a name="scenario-1-use-inline-script-call-tvfs-and-stored-procs"></a>Scenario 1-gebruik inline script aanroep tvf's en opgeslagen procedures

Configureren in Azure Data Lake Analytics taak Editor **SourceType** als **DirectInput**, en plaats de U-SQL-instructies in **USQLStatement**.

Voor eenvoudig onderhoud en codebeheer, alleen korte U-SQL-script plaatsen als inline-scripts, bijvoorbeeld, kunt u aanroepen bestaande functies met tabelwaarden en opgeslagen procedures in uw U-SQL-databases. 

![Inline-U-SQL-script in de SSIS-taak bewerken](./media/data-lake-analytics-schedule-jobs-ssis/edit-inline-usql-script-in-ssis.png)

Verwant artikel: [Parameter doorgeven aan de opgeslagen procedures](#scenario-6-pass-parameters-to-u-sql-script)

## <a name="scenario-2-use-u-sql-files-in-azure-data-lake-store"></a>Scenario 2: gebruik U-SQL-gegevensbestanden in Azure Data Lake Store

U kunt ook U-SQL-bestanden in de Azure Data Lake Store gebruiken met behulp van **systeemtaak van Azure Data Lake Store-bestand** in Azure Feature Pack. Deze aanpak kunt u de scripts die zijn opgeslagen op de cloud gebruiken.

Volg de onderstaande stappen voor het instellen van de verbinding tussen Azure Data Lake Store-bestand systeemtaak en Azure Data Lake Analytics-taak.

### <a name="set-task-control-flow"></a>Controlestroom set-taak

In de ontwerpweergave van SSIS-pakket toevoegen een **systeemtaak van Azure Data Lake Store-bestand**, een **Foreach-lus Container** en een **Azure Data Lake Analytics-taak** in de Foreach-lus De container. De Azure Data Lake Store-bestand systeemtaak helpt bij het downloaden van U-SQL-bestanden in uw ADLS-account naar een tijdelijke map. De Foreach-lus-Container en de Azure Data Lake Analytics-taak te verzenden om U-SQL-bestand onder de tijdelijke map op het Azure Data Lake Analytics-account als een U-SQL-taak.

![U-SQL-bestanden gebruiken in Azure Data Lake Store](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-data-lake-store.png)

### <a name="configure-azure-data-lake-store-file-system-task"></a>Azure Data Lake Store-bestand systeemtaak configureren

1. Stel **bewerking** naar **CopyFromADLS**.
2. Instellen van **AzureDataLakeConnection**, meer informatie over [Azure Data Lake Store Connection Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017).
3. Set **AzureDataLakeDirectory**. Verwijzen naar de map voor het opslaan van uw U-SQL-scripts. Relatief pad ten opzichte van de hoofdmap van de Azure Data Lake Store-account gebruiken.
4. Stel **bestemming** naar een map die de gedownloade U-SQL-scripts in de cache opslaat. Pad naar deze map wordt gebruikt in Foreach-lus Container voor het indienen van U-SQL-taak. 

![Azure Data Lake Store-bestand systeemtaak configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-azure-data-lake-store-file-system-task.png)

[Meer informatie over Azure Data Lake Store-bestand systeemtaak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-store-file-system-task?view=sql-server-2017).

### <a name="configure-foreach-loop-container"></a>Foreach-lus Container configureren

1. In **verzameling** pagina, stelt u **Enumerator** naar **Foreach bestand Enumerator**.

2. Stel **map** onder **enumerator voor configuratie** groep naar de tijdelijke map met de gedownloade U-SQL-scripts.

3. Stel **bestanden** onder **enumerator voor configuration** naar `*.usql` zodat de container lus alleen de bestanden die eindigen vangt op `.usql`.

    ![Foreach-lus Container configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-collection.png)

4. In **variabele toewijzingen** pagina, voegt u een variabele van de gebruiker gedefinieerd voor de bestandsnaam op voor elk bestand dat U-SQL. Stel de **Index** op 0 om op te halen van de bestandsnaam. In dit voorbeeld definieert u een variabele met de naam `User::FileName`. Deze variabele wordt worden gebruikt om dynamisch verbinding U-SQL-script-bestanden ophalen en instellen van de naam van de U-SQL-taak in Azure Data Lake Analytics-taak.

    ![Foreach-lus Container om op te halen bestandsnaam configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-variable-mapping.png)

### <a name="configure-azure-data-lake-analytics-task"></a>Azure Data Lake Analytics-taak configureren 

1. Stel **SourceType** naar **FileConnection**.

2. Stel **FileConnection** naar het bestand-verbinding die naar het bestandsobjecten verwijst geretourneerd uit Foreach-lus Container.
    
    Dit bestand om verbinding te maken:

    1. Kies **<New Connection...>** in FileConnection instelling.
    2. Stel **gebruikstype** te **bestaand bestand**, en stel de **bestand** naar een bestaand bestand bestandspad.

        ![Foreach-lus Container configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

    3. In **Connection Managers** weergeven, met de rechtermuisknop op de verbinding van de bestanden die zojuist hebt gemaakt en kies **eigenschappen**.

    4. In de **eigenschappen** venster, vouw **expressies**, en stel **ConnectionString** aan de variabele die is gedefinieerd in Foreach-lus Container, bijvoorbeeld `@[User::FileName]`.

        ![Foreach-lus Container configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-property-for-foreach-loop-container.png)

3. Stel **AzureDataLakeAnalyticsConnection** bij het Azure Data Lake Analytics-account dat u wilt verzenden van taken naar. Meer informatie over [Azure Data Lake Analytics Connection Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017).

4. Andere configuraties taak instellen. [Meer informatie](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017).

5. Gebruik **expressies** dynamisch instellen U-SQL-Taaknaam:

    1. In **expressies** pagina toevoegen van een nieuwe expressie sleutel / waarde-paar voor **JobName**.
    2. Stel de waarde voor JobName aan de variabele die is gedefinieerd in Foreach-lus Container, bijvoorbeeld `@[User::FileName]`.
    
        ![SSIS-expressie voor de naam van de U-SQL-taak configureren](./media/data-lake-analytics-schedule-jobs-ssis/configure-expression-for-u-sql-job-name.png)

## <a name="scenario-3-use-u-sql-files-in-azure-blob-storage"></a>Scenario 3: gebruik U-SQL-gegevensbestanden in Azure Blob Storage

U kunt U-SQL-bestanden in Azure Blob Storage gebruiken met behulp van **Azure Blob downloaden taak** in Azure Feature Pack. Deze methode kunt u met behulp van de scripts op de cloud.

De stappen zijn vergelijkbaar met [Scenario 2: U-SQL-bestanden gebruiken in Azure Data Lake Store](#scenario-2-use-u-sql-files-in-azure-data-lake-store). Wijzigt de systeemtaak is Azure Data Lake Store-bestand in Azure Blob downloadtaak. [Meer informatie over Azure Blob downloaden taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task?view=sql-server-2017).

De Controlestroom is, zoals hieronder.

![U-SQL-bestanden gebruiken in Azure Data Lake Store](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-blob-storage.png)

## <a name="scenario-4-use-u-sql-files-on-the-local-machine"></a>Scenario 4: gebruik U-SQL-gegevensbestanden op de lokale computer

Naast van het gebruik van U-SQL-bestanden die zijn opgeslagen in de cloud, kunt u ook gebruiken op uw lokale computer of bestanden die zijn geïmplementeerd met uw SSIS-pakketten.

1. Met de rechtermuisknop op **Connection Managers** in SSIS-project en kies **nieuwe Connection Manager**.

2. Selecteer **bestand** typt en op **toevoegen...** .

3. Stel **gebruikstype** naar **bestaand bestand**, en stel de **bestand** naar het bestand op de lokale computer.

    ![Bestand-verbinding toevoegen aan het lokale bestand](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

4. Voeg **Azure Data Lake Analytics** taak en:
    1. Stel **SourceType** naar **FileConnection**.
    2. Stel **FileConnection** naar de bestand-verbinding zojuist hebt gemaakt.

5. Andere configuraties voor Azure Data Lake Analytics-taak worden voltooid.

## <a name="scenario-5-use-u-sql-statement-in-ssis-variable"></a>Scenario 5-gebruik U-SQL-instructie in de variabele SSIS

In sommige gevallen moet u mogelijk dynamisch genereren de U-SQL-instructies. U kunt **SSIS variabele** met **SSIS expressie** en andere SSIS-taken, zoals de taak Script, kunt u de U-SQL-instructie dynamisch genereren.

1. Open variabelen werkvenster via **SSIS > variabelen** menu op het hoogste niveau.

2. Een SSIS-variabele toevoegen en stel de waarde rechtstreeks of gebruik een **expressie** voor het genereren van de waarde.

3. Voeg **Azure Data Lake Analytics-taak** en:
    1. Stel **SourceType** naar **variabele**.
    2. Stel **SourceVariable** naar SSIS variabele zojuist hebt gemaakt.

4. Andere configuraties voor Azure Data Lake Analytics-taak worden voltooid.

## <a name="scenario-6-pass-parameters-to-u-sql-script"></a>Scenario 6-Geef parameters door aan de U-SQL-script

In sommige gevallen kunt u dynamisch de U-SQL-variabele instellen in de U-SQL-script. **Parametertoewijzing** functie in Azure Data Lake Analytics-taak hulp bij dit scenario. Er zijn doorgaans twee normale gebruiker gevallen:

- Stel de invoer en uitvoer bestand padvariabelen dynamisch op basis van de huidige datum en tijd.
- Stel de parameter voor opgeslagen procedures.

[Meer informatie over het instellen van de parameters voor de U-SQL-script](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017#parameter-mapping-page-configuration).

## <a name="next-steps"></a>Volgende stappen

- [SSIS-pakketten uitvoeren in Azure](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
- [Azure Feature Pack voor de integratieservices (SSIS)](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud)
- [U-SQL-taken met behulp van Azure Data Factory plannen](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics)

