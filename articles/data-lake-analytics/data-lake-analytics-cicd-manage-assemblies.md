---
title: Aanbevolen procedures voor het beheren van U-SQL-assembly's in een CI/CD-pijplijn voor Azure Data Lake
description: Informatie over de aanbevolen procedures voor het beheren van U-SQL C# assembly's in een CI/CD-pijplijn met Azure DevOps.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0d9192e5ca4dba202ca5287481072bb0f8ae5621
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53598516"
---
# <a name="best-practices-for-managing-u-sql-assemblies-in-a-cicd-pipeline"></a>Aanbevolen procedures voor het beheren van U-SQL-assembly's in een CI/CD-pijplijn

In dit artikel leert u hoe u voor het beheren van de broncode van U-SQL-assembly met de geïntroduceerde project van de U-SQL-database. U leert ook hoe u een continue integratie en implementatie (CI/CD) pijplijn voor assembly-registratie instellen met behulp van Azure DevOps.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Het project U-SQL-database gebruiken voor het beheren van assembly-broncode

[Het project U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md) is een projecttype in Visual Studio waarmee ontwikkelaars kunnen ontwikkelen, beheren en implementeren van de U-SQL-databases, snel en eenvoudig. U kunt alle U-SQL database-objecten (met uitzondering van referenties) aan het project U-SQL-database kunt beheren. 

Voor het beheren van de C# assembly-broncode en assembly-registratie DDL U-SQL-scripts, gebruikt u de:

* U-SQL database-project voor het beheren van assembly-registratie van U-SQL-scripts.
* Klassebibliotheek (voor U-SQL-toepassing) voor het beheren van de C# source code en afhankelijkheden voor de gebruiker gedefinieerde operators, functies en lezers (UDO's, UDF's en UDAGs).
* U-SQL database-project om te verwijzen naar de project-klassenbibliotheek vormt. 

Een U-SQL-database-project kunt verwijzen naar een klassebibliotheekproject (voor U-SQL-toepassing). U kunt maken van assembly's die zijn geregistreerd in de U-SQL-database met behulp van waarnaar wordt verwezen C# broncode van dit project Class Library (voor U-SQL-toepassing).

Volg deze stappen voor het maken van projecten en voeg verwijzingen toe.
1. Maak een project Class Library (voor U-SQL-toepassing) selecteren **bestand** > **nieuw** > **Project**. Het project is onder de **Azure Data Lake > U-SQL** knooppunt.

   ![Data Lake Tools voor Visual Studio--maken C# -klassebibliotheekproject](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
1. Voeg uw door de gebruiker gedefinieerde C# code in het project (voor U-SQL-toepassing)-klassenbibliotheek vormt.

1. Maak een U-SQL project selecteren **bestand** > **nieuw** > **Project**. Het project is onder de **Azure Data Lake** > **U-SQL** knooppunt.

   ![Data Lake Tools voor Visual Studio - databaseproject voor maakt U-SQL](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
1. Voeg een verwijzing naar de C# -klassebibliotheekproject voor het project U-SQL-database.

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)

5. Een assembly-script in het project U-SQL-database maken met de rechtermuisknop op het project en selecteert u **Add New Item**.

   ![Data Lake Tools voor Visual Studio - assembly script toevoegen](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)

1. Open het assembly-script in de ontwerpweergave van de assembly. Selecteer de assembly waarnaar wordt verwezen in de **assembly maken van verwijzing** vervolgkeuzelijst.

    ![Data Lake Tools voor Visual Studio - maken assembly van verwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Voeg **beheerde afhankelijkheden** en **aanvullende bestanden**, indien aanwezig. Wanneer u extra bestanden toevoegen, gebruikt het hulpprogramma voor het relatieve pad om te controleren of dat deze later kunt terugvinden de assembly's op uw lokale computer en op de build-machine. 

**@_DeployTempDirectory** in de editor is venster onder een vooraf gedefinieerde variabele die het hulpprogramma naar de uitvoermap build verwijst. Onder de uitvoermap build heeft elke assembly een submap met de naam van de assembly. Alle dll's en andere bestanden zijn in de submap. 

## <a name="build-a-u-sql-database-project"></a>Bouw een U-SQL-database-project

De uitvoer van de build voor een project U-SQL-database is een implementatiepakket van U-SQL-database. De naam ervan met het achtervoegsel `.usqldbpack`. De `.usqldbpack` pakket is een ZIP-bestand met alle DDL-componenten in een enkel U-SQL-script in de DDL-map. Alle gemaakte dll-bestanden en extra bestanden voor assembly's zijn in de map Temp.

## <a name="deploy-a-u-sql-database"></a>Een U-SQL-database implementeren

De `.usqldbpack` pakket kan worden geïmplementeerd op een lokaal account of een Azure Data Lake Analytics-account. Gebruik Visual Studio of de SDK-implementatie. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Implementeren van een U-SQL-database in Visual Studio

U kunt een U-SQL-database implementeren met behulp van een project U-SQL-database of een `.usqldbpack` -pakket in Visual Studio.

#### <a name="deploy-by-using-a-u-sql-database-project"></a>Implementeren met behulp van een project U-SQL-database

1.  Met de rechtermuisknop op het project U-SQL-database en selecteer vervolgens **implementeren**.
2.  In de **U-SQL-Database implementeren** wizard selecteert u de **ADLA Account** waarop u wilt implementeren van de database. Zowel lokale accounts als ADLA-accounts worden ondersteund.
3.  **Bron van de database** wordt automatisch ingevuld. Deze verwijst naar het pakket .usqldbpack in de map voor uitvoer van het project-build.
4.  Voer een naam in **databasenaam** om een database te maken. Als een database met die dezelfde naam al in de doel-Azure Data Lake Analytics-account bestaat, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.
5.  Voor het implementeren van de U-SQL-database, selecteert u **indienen**. Alle resources, zoals assembly's en aanvullende bestanden worden geüpload. Een U-SQL-taak met alle DDL-componenten wordt verzonden.

    ![Data Lake Tools voor Visual Studio - databaseproject voor implementeert U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools voor Visual Studio, implementeert U-SQL database-projectwizard](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-a-u-sql-database-in-azure-devops"></a>Implementeren van een U-SQL-database in Azure DevOps

`PackageDeploymentTool.exe` biedt de programmering en opdrachtregelinterfaces die helpen bij het implementeren van U-SQL-databases. De SDK is opgenomen in de [U-SQL SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), dat zich bevindt op `build/runtime/PackageDeploymentTool.exe`.

In Azure DevOps, kunt u een opdrachtregeltaak en deze SDK gebruiken voor het instellen van een automation-pijplijn voor het vernieuwen van de U-SQL-database. [Meer informatie over de SDK en over het instellen van een CI/CD-pijplijn voor de implementatie van de U-SQL-database](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Volgende stappen

* [Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
* [Testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
* [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
