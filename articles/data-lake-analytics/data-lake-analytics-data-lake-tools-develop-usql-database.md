---
title: Een project U-SQL-database gebruiken voor het ontwikkelen van een U-SQL-database voor Azure Data Lake | Microsoft Docs
description: Informatie over het ontwikkelen van een U-SQL-database met behulp van Azure Data Lake Tools voor Visual Studio.
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
ms.openlocfilehash: 1fee28ad185629b0cb26abb54cc2e196fca791e5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364046"
---
# <a name="use-a-u-sql-database-project-to-develop-a-u-sql-database-for-azure-data-lake"></a>Een project U-SQL-database gebruiken voor het ontwikkelen van een U-SQL-database voor Azure Data Lake

U-SQL-database biedt een gestructureerde weergaven via niet-gestructureerde gegevens en beheerde gestructureerde gegevens in tabellen. Het biedt ook een systeem voor de productcatalogus algemene metagegevens voor het ordenen van uw gestructureerde gegevens en aangepaste code. De database is het concept, dat deze verwante objecten worden gegroepeerd.

Meer informatie over [U-SQL-database en Data Definition Language (DDL)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

Het project U-SQL-database is een project in Visual Studio waarmee ontwikkelaars kunnen ontwikkelen, beheren en implementeren van de U-SQL-databases, snel en eenvoudig.

## <a name="create-a-u-sql-database-project"></a>Een U-SQL-database-project maken

Azure Data Lake Tools voor Visual Studio een nieuw projectsjabloon die U-SQL database-project aangeroepen na versie 2.3.3000.0 toegevoegd. Voor het maken van een U-SQL-project, selecteer **bestand > Nieuw > Project**. Het Project U-SQL-Database kunt u vinden onder **Azure Data Lake > U-SQL-knooppunt**.

![Data Lake Tools voor Visual Studio - maken project voor U-SQL-database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-by-using-a-database-project"></a>U-SQL database-objecten ontwikkelen met behulp van een database-project

Met de rechtermuisknop op het project U-SQL-database. Selecteer **toevoegen > Nieuw item**. U vindt alle nieuwe ondersteunde objecttypen die in de **Add New Item** Wizard. 

Een nieuw U-SQL-script voor een niet-assembly-object (bijvoorbeeld een tabelwaardefunctie) is gemaakt nadat u een nieuw item toevoegen. U kunt beginnen met het ontwikkelen van de DDL-instructie voor dat object in de editor.

Het hulpprogramma biedt voor een assembly-object, een gebruiksvriendelijke gebruikersinterface-editor die helpt u bij het registreren van de assembly en DLL-bestanden en andere aanvullende bestanden implementeren. De volgende stappen laten zien hoe u een definitie van een assembly-object toevoegen aan het project U-SQL-database:

1.  Verwijzingen naar de C#-project met de UDO/UDAG/UDF voor het project U-SQL-database toevoegen.

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Kies in de ontwerpweergave van het assembly de assembly waarnaar wordt verwezen vanuit **assembly maken van verwijzing** vervolgkeuzelijst.

    ![Data Lake Tools voor Visual Studio - maken assembly van verwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Toevoegen **beheerde afhankelijkheden** en **aanvullende bestanden** indien aanwezig. Wanneer u extra bestanden toevoegen, gebruikt het hulpprogramma voor het relatieve pad om te controleren of dat deze later kunt terugvinden de assembly's op uw lokale computer en de build-machine. 

@_DeployTempDirectory is een vooraf gedefinieerde variabele die het hulpprogramma naar de uitvoermap build verwijst. Onder de uitvoermap build heeft elke assembly een submap met de naam van de assembly. Alle dll's en andere bestanden zijn in de submap. 
 
## <a name="build-a-u-sql-database-project"></a>Bouw een U-SQL-database-project

De uitvoer voor een project U-SQL-database de implementatiepakket voor een U-SQL-database is, met de naam met het achtervoegsel build `.usqldbpack`. De `.usqldbpack` pakket is een ZIP-bestand met alle DDL-componenten in een enkel U-SQL-script in de **DDL** map en alle dll's en extra bestanden voor assembly's in de **Temp** map.

Meer informatie over [hoe het bouwen van een U-SQL-database-project met de MSBuild vanaf de opdrachtregel en een Visual Studio Team Services taak bouwen](data-lake-analytics-cicd-overview.md).

## <a name="deploy-a-u-sql-database"></a>Een U-SQL-database implementeren

Het pakket .usqldbpack kan worden geïmplementeerd op een lokaal account of een Azure Data Lake Analytics-account met behulp van Visual Studio of de SDK-implementatie. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Implementeren van een U-SQL-database in Visual Studio

U kunt een U-SQL-database via een project U-SQL-database of een pakket .usqldbpack in Visual Studio implementeren.

#### <a name="deploy-through-a-u-sql-database-project"></a>Implementeren via een U-SQL-database-project

1.  Met de rechtermuisknop op het project U-SQL-database en selecteer vervolgens **implementeren**.
2.  In de **U-SQL-Database-Wizard implementeren**, selecteer de **ADLA account** waarop u wilt implementeren van de database. Zowel lokale accounts als ADLA-accounts worden ondersteund.
3.  **Bron van de database** wordt automatisch ingevuld en uitvoermap verwijst naar het pakket .usqldbpack in van het project te bouwen.
4.  Voer een naam in **databasenaam** om een database te maken. Als een database met die dezelfde naam al in de doel-Azure Data Lake Analytics-account bestaat, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.
5.  Voor het implementeren van de U-SQL-database, selecteert u **indienen**. Alle resources (verzamelingen en aanvullende bestanden) worden geüpload en een U-SQL-taak met alle DDL-componenten is verzonden.

    ![Data Lake Tools voor Visual Studio - databaseproject voor implementeert U-SQL](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools voor Visual Studio, implementeert U-SQL database-projectwizard](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-a-u-sql-database-deployment-package"></a>Implementeren via een implementatiepakket van U-SQL-database

1.  Open **Server Explorer**. Vouw vervolgens de **Azure Data Lake Analytics-account** waarop u wilt implementeren van de database.
2.  Klik met de rechtermuisknop **U-SQL-Databases**, en kies vervolgens **Deploy Database**.
3.  Stel **databasebron** aan het pad voor de U-SQL database-implementatie-pakket (.usqldbpack-bestand).
4.  Voer de **databasenaam** om een database te maken. Als er een database met dezelfde naam al in de doel-Azure Data Lake Analytics-account bestaat, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.

    ![Data Lake Tools voor Visual Studio, implementeert U-SQL databasepakket](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Data Lake Tools voor Visual Studio, implementeert U-SQL database-wizard pakket](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-by-using-the-sdk"></a>U-SQL-database implementeren met behulp van de SDK

`PackageDeploymentTool.exe` biedt de programmering en opdrachtregelinterfaces die helpen bij het implementeren van U-SQL-databases. De SDK is opgenomen in de [U-SQL SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), dat zich bevindt op `build/runtime/PackageDeploymentTool.exe`.

[Meer informatie over de SDK en hoe u een CI/CD-pijplijn voor de implementatie van de U-SQL-database instellen](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>Verwijzen naar een U-SQL-database-project

Een U-SQL-project kunt verwijzen naar een project U-SQL-database. De verwijzing van invloed is op twee workloads:

- *Build project*: de database waarnaar wordt verwezen-omgevingen instellen voor het bouwen van de U-SQL-scripts. 
- *Lokaal uitvoeren op (een lokale-project) account*: de database waarnaar wordt verwezen-omgevingen zijn geïmplementeerd op (een lokale-project) account voordat U-SQL-script wordt uitgevoerd. [Meer informatie over lokaal wordt uitgevoerd en het verschil tussen (de lokale computer) en (een lokale-project) account hier](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-a-u-sql-database-reference"></a>De verwijzing naar een U-SQL-database toevoegen

1. Met de rechtermuisknop op het project U-SQL in **Solution Explorer**, en kies vervolgens **U-SQL-Database-verwijzing toevoegen...** .

    ![Data Lake Tools voor Visual Studio - toevoegen database projectverwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Configureer de verwijzing naar een database in een U-SQL-database-project in de huidige oplossing of in een pakketbestand van de U-SQL-database.
3. Geef de naam voor de database.

    ![Data Lake Tools voor Visual Studio toevoegen project referentie-databasewizard](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Volgende stappen

- [Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
