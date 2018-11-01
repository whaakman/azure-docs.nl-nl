---
title: Aanbevolen procedures voor het beheren van U-SQL-assembly's in CI/CD-pijplijn voor Azure Data Lake
description: Informatie over de aanbevolen procedures voor het beheren van U-SQL C# assembly's in CI/CD-pijplijn met Azure DevOps.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634008"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>Aanbevolen procedures voor het beheren van U-SQL-assembly's in CI/CD-pijplijn

In dit artikel leert u hoe u voor het beheren van de broncode van U-SQL-assembly met het nieuwe project dat geïntroduceerd U-SQL-database. U kunt ook meer informatie over het instellen van een continue integratie en implementatiepijplijn (CI/CD) voor de assembly-registratie met behulp van Azure DevOps.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Het project U-SQL-database gebruiken voor het beheren van assembly-broncode

[Het project U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md) is een projecttype in Visual Studio waarmee ontwikkelaars kunnen ontwikkelen, beheren en implementeren van de U-SQL-databases, snel en eenvoudig. Alle objecten van U-SQL-database (met uitzondering van referenties) kan worden beheerd met het project U-SQL-database. 

De aanbevolen manier van het beheer van de C# assembly bron code en de assembly-registratie DDL U-SQL-scripts is:

* Gebruik **U-SQL-Database Project** voor het beheren van assembly-registratie van U-SQL-scripts.
* Gebruik **Class Library (voor U-SQL-toepassing)** voor het beheren van de C# source code en afhankelijkheden voor de gebruiker gedefinieerde operators, functies en lezers (UDO's / UDF/UDAGs).
* Gebruik het project U-SQL-database om te verwijzen naar de klassebibliotheekproject. 

Een U-SQL-Database-Project kunt verwijzen naar een Klassebibliotheekproject (voor U-SQL-toepassing). Assembly's die zijn geregistreerd in de U-SQL-database kunnen worden gemaakt met waarnaar wordt verwezen C# broncode van dit Project Class Library (voor U-SQL-toepassing).

Volg de onderstaande stappen voor het maken van projecten en verwijzingen toe te voegen:
1. Maak een project Class Library (voor U-SQL-toepassing) via **bestand > Nieuw > Project**. Het project is onder **Azure Data Lake > U-SQL** knooppunt.
   ![Data Lake Tools voor Visual Studio--maken C# -klassebibliotheekproject](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. Voeg uw door de gebruiker gedefinieerde C# code in het project (voor U-SQL-toepassing)-klassenbibliotheek vormt. 
3. Maak een U-SQL-project via **bestand > Nieuw > Project**. Het project is onder **Azure Data Lake > U-SQL** knooppunt.
   ![Data Lake Tools voor Visual Studio - databaseproject voor maakt U-SQL](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. Verwijzing toevoegen aan de C# -klassebibliotheekproject voor het project U-SQL-database.

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools voor Visual Studio--toevoegen U-SQL-database projectverwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. Maken van een assembly-script in het project U-SQL-database via **Klik met de rechtermuisknop op het project > Add New Item**.
   ![Data Lake Tools voor Visual Studio - assembly script toevoegen](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. Open het script assembly in de ontwerpweergave van de assembly, kiest u de assembly waarnaar wordt verwezen in **assembly maken van verwijzing** vervolgkeuzelijst.

    ![Data Lake Tools voor Visual Studio - maken assembly van verwijzing](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. Toevoegen **beheerde afhankelijkheden** en **aanvullende bestanden** indien aanwezig. Wanneer u extra bestanden toevoegen, gebruikt het hulpprogramma voor het relatieve pad om te controleren of dat deze later kunt terugvinden de assembly's op uw lokale computer en de build-machine. 

**@_DeployTempDirectory** in de editor is venster onder een vooraf gedefinieerde variabele die het hulpprogramma naar de uitvoermap build verwijst. Onder de uitvoermap build heeft elke assembly een submap met de naam van de assembly. Alle dll's en andere bestanden zijn in de submap. 

## <a name="build-a-u-sql-database-project"></a>Bouw een U-SQL-database-project

De uitvoer voor een project U-SQL-database de implementatiepakket voor een U-SQL-database is, met de naam met het achtervoegsel build `.usqldbpack`. De `.usqldbpack` pakket is een ZIP-bestand met alle DDL-componenten in een enkel U-SQL-script in de **DDL** map en alle gemaakte dll's en extra bestanden voor assembly's in de **Temp** map.

## <a name="deploy-a-u-sql-database"></a>Een U-SQL-database implementeren

De `.usqldbpack` pakket kan worden geïmplementeerd naar een lokaal account of een Azure Data Lake Analytics-account met behulp van Visual Studio of de SDK-implementatie. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Implementeren van een U-SQL-database in Visual Studio

U kunt een U-SQL-database via een U-SQL-database-project implementeren of een `.usqldbpack` -pakket in Visual Studio.

#### <a name="deploy-through-a-u-sql-database-project"></a>Implementeren via een U-SQL-database-project

1.  Met de rechtermuisknop op het project U-SQL-database en selecteer vervolgens **implementeren**.
2.  In de **U-SQL-Database-Wizard implementeren**, selecteer de **ADLA account** waarop u wilt implementeren van de database. Zowel lokale accounts als ADLA-accounts worden ondersteund.
3.  **Bron van de database** wordt automatisch ingevuld en uitvoermap verwijst naar het pakket .usqldbpack in van het project te bouwen.
4.  Voer een naam in **databasenaam** om een database te maken. Als een database met die dezelfde naam al in de doel-Azure Data Lake Analytics-account bestaat, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.
5.  Voor het implementeren van de U-SQL-database, selecteert u **indienen**. Alle resources (verzamelingen en aanvullende bestanden) worden geüpload en een U-SQL-taak met alle DDL-componenten is verzonden.

    ![Data Lake Tools voor Visual Studio - databaseproject voor implementeert U-SQL](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools voor Visual Studio, implementeert U-SQL database-projectwizard](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>U-SQL-database in Azure DevOps implementeren

`PackageDeploymentTool.exe` biedt de programmering en opdrachtregelinterfaces die helpen bij het implementeren van U-SQL-databases. De SDK is opgenomen in de [U-SQL SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), dat zich bevindt op `build/runtime/PackageDeploymentTool.exe`.

In Azure DevOps, kunt u vanaf de opdrachtregel-taak en deze SDK om het instellen van automation-pijplijn voor het vernieuwen van de U-SQL-database. [Meer informatie over de SDK en hoe u een CI/CD-pijplijn voor de implementatie van de U-SQL-database instellen](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>Volgende stappen

* [Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
* [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
* [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
