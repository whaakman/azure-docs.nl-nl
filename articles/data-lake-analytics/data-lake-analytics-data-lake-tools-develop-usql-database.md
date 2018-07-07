---
title: Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database voor Azure Data Lake | Microsoft Docs
description: Informatie over het ontwikkelen van U-SQL-database met behulp van Azure Data Lake Tools voor Visual Studio.
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
ms.openlocfilehash: a8099a8c9bcec8ed5a7bb480dae58d2c077b8fe0
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889542"
---
# <a name="use-u-sql-database-project-to-develop-u-sql-database-for-azure-data-lake"></a>U-SQL database-project voor het ontwikkelen van U-SQL-database voor Azure Data Lake gebruiken

U-SQL-database biedt gestructureerde weergaven de niet-gestructureerde gegevens, gestructureerde gegevens in tabellen beheren en biedt een algemene metagegevens catalogus uw gestructureerde gegevens en aangepaste code in te delen. De database is het concept, dat deze verwante objecten worden gegroepeerd.

Meer informatie over [U-SQL-database en Data Definition Language (DDL)](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/data-definition-language-ddl-statements-u-sql). 

U-SQL database-project is een project in Visual Studio waarmee ontwikkelaars kunnen ontwikkelen, beheren en implementeren van de U-SQL-databases, snel en eenvoudig.

## <a name="create-a-u-sql-database-project"></a>Een U-SQL-database-project maken

Azure Data Lake Tools voor Visual Studio een nieuw projectsjabloon die U-SQL database-project aangeroepen na versie 2.3.3000.0 toegevoegd. Als u wilt een U-SQL-project maakt, gaat u door **bestand > Nieuw > Project**, het Project U-SQL-Database kunt u vinden onder **Azure Data Lake > U-SQL-knooppunt**.

![Data Lake Tools voor Visual Studio maken u-sql-database-project](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-using-database-project"></a>Ontwikkelen van U-SQL database-objecten met behulp van de database-project

Met de rechtermuisknop op het project U-SQL-database, klikt u op **toevoegen > Nieuw item**, alle ondersteunde objecttypen die kunnen worden gevonden in de Wizard Nieuw Item toevoegen. 

1.  Voor niet-assembly-object, bijvoorbeeld: table valued function, een nieuw U-SQL-script wordt gemaakt na het toevoegen van nieuwe item. U kunt beginnen met het ontwikkelen van de DDL-instructie voor dat object in de editor.
2.  Het hulpprogramma biedt voor de assembly-object, een gebruiksvriendelijke gebruikersinterface-editor die helpt u bij het registreren van de assembly en aanvullende bestanden en .dll-implementeren. Volg de onderstaande stappen voor het toevoegen van de definitie van een assembly-object aan het project U-SQL-database:

1.  Voeg verwijzingen van het C#-project met de UDO/UDAG/UDF aan het project U-SQL-database toe.

    ![Data Lake Tools voor Visual Studio toevoegen-project naslaginformatie van u-sql-database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools voor Visual Studio toevoegen-project naslaginformatie van u-sql-database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Kies in de ontwerpweergave van de assembly, de assembly waarnaar wordt verwezen vanuit **assembly maken van verwijzing** vervolgkeuzelijst.

    ![Data Lake Tools voor Visual Studio maken assembly van verwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  Voeg **beheerde afhankelijkheden** en **aanvullende bestanden** indien van toepassing. Wanneer u bestanden toe te voegen, wordt het hulpprogramma voor het relatieve pad gebruiken om te controleren of dat de assembly's zowel op uw lokale computer en de machine build later kan worden gevonden. @_DeployTempDirectory is een vooraf gedefinieerde variabele die het hulpprogramma naar de uitvoermap build verwijst. Onder de uitvoer van de build heeft elke assembly een submap met de naam omdat de assembly-naam, alle dll's, en extra bestanden staan er in de submap. 
 
## <a name="build-u-sql-database-project"></a>U-SQL database-project maken

De uitvoer voor de U-SQL database-project is de implementatiepakket voor een U-SQL-database, met de naam met het achtervoegsel build `.usqldbpack`. De `.usqldbpack` pakket is een zip-bestand bevat alle DDL-componenten in een enkel U-SQL-script in **DDL** map en alle dll's en extra bestanden voor assembly's in **Temp** map.

Meer informatie over [over het bouwen van U-SQL database-project met MSBuild vanaf de opdrachtregel en Visual Studio Team Service-opbouwtaak](data-lake-analytics-cicd-overview.md#build-u-sql-database-project).

## <a name="deploy-u-sql-database"></a>U-SQL-database implementeren

Het pakket .usqldbpack kan worden geïmplementeerd op zowel lokaal account of Azure Data Lake Analytics-account met behulp van Visual Studio of de SDK-implementatie. 

### <a name="deploy-u-sql-database-in-visual-studio"></a>U-SQL-database in Visual Studio implementeren

U kunt een U-SQL-database via een project U-SQL-database of een pakket .usqldbpack in Visual Studio implementeren.

#### <a name="deploy-through-u-sql-database-project"></a>Implementeren via U-SQL database-project

1.  Met de rechtermuisknop op het project U-SQL-database en kies **implementeren**.
2.  Selecteer in de wizard Deploy U-SQL-Database, de **ADLA account** u wilt de database te implementeren. Zowel (Local)-account als ADLA-account worden ondersteund.
3.  **Bron van de database** zijn ingevuld automatisch aan te wijzen aan het pakket .usqldbpack in van het project bouwen uitvoermap
4.  Voer **databasenaam** voor het maken van een database. Als er een database met dezelfde bestaande in de doel-Azure Data Lake Analytics-account, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.
5.  Klik op **indienen** de U-SQL-database implementeren. Alle resources (verzamelingen en aanvullende bestanden) worden geüpload en een U-SQL-taak bevat alle DDL-componenten zijn verzonden.

    ![Data Lake Tools voor Visual Studio implementeren project voor de u-sql-database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools voor Visual Studio implementeren-projectwizard van u-sql database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-u-sql-database-deployment-package"></a>Implementeren via U-SQL-database-implementatiepakket

1.  Open **Server Explorer**, vouw de **Azure Data Lake Analytics-account** u wilt de database te implementeren.
2.  Klik met de rechtermuisknop op U-SQL-Databases en kies **Deploy Database**.
3.  Stel **databasebron** aan het pad voor de U-SQL database-implementatie-pakket (.usqldbpack-bestand).
4.  Voer de **databasenaam** voor het maken van een database. Als er een database met dezelfde bestaande in de doel-Azure Data Lake Analytics-account, worden alle objecten die zijn gedefinieerd in de database-project gemaakt zonder de database opnieuw te maken.

    ![Data Lake Tools voor Visual Studio implementeren u-sql-databasepakket](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Data Lake Tools voor Visual Studio implementeren wizard pakket van u-sql-database](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-using-sdk"></a>U-SQL-database met behulp van SDK implementeren

`PackageDeploymentTool.exe` biedt de programmering en opdrachtregelinterfaces die helpen bij het implementeren van U-SQL-databases. De SDK is opgenomen in de [U-SQL SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), zoeken naar op `build/runtime/PackageDeploymentTool.exe`.

[Meer informatie over de SDK en hoe u een CI/CD-pijplijn voor de implementatie van de U-SQL-database instellen](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-visual-studio-team-service).

## <a name="reference-a-u-sql-database-project"></a>Verwijzen naar een U-SQL-database-project

Een U-SQL-project kunt verwijzen naar een project U-SQL-database. De verwijzing van invloed is op twee workloads:

- Build-project: de database waarnaar wordt verwezen-omgevingen zijn ingesteld voor het bouwen van de U-SQL-scripts. 
- Lokaal uitvoeren op basis van de account (Local-project): de database waarnaar wordt verwezen-omgevingen zijn geïmplementeerd op (lokale-project)-account voordat U-SQL-script wordt uitgevoerd. [Meer informatie over lokaal uitvoeren en het verschil tussen de (lokale computer) en (Local-project) account hier](data-lake-analytics-data-lake-tools-local-run.md).

### <a name="how-to-add-u-sql-database-reference"></a>Verwijzing naar de U-SQL-database toevoegen

1. Met de rechtermuisknop op het project U-SQL in **Solution Explorer**, en kies **U-SQL-Database-verwijzing toevoegen...** .

    ![Data Lake Tools voor Visual Studio toevoegen database projectverwijzing](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Verwijzing naar de database in een U-SQL-database-project in de huidige oplossing of een pakketbestand van de U-SQL-database configureren.
3. Geef de naam voor de database.

    ![Data Lake Tools voor Visual Studio toevoegen project referentie-databasewizard](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Volgende stappen

- [CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
