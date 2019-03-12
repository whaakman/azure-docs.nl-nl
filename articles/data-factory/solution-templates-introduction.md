---
title: Overzicht van sjablonen voor Azure Data Factory | Microsoft Docs
description: Leer hoe u een vooraf gedefinieerde sjabloon gebruiken om te snel aan de slag met Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/04/2019
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: c684d275052e011d965210c132e09bab679cf166
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57576244"
---
# <a name="templates"></a>Sjablonen

Sjablonen zijn vooraf gedefinieerde Azure Data Factory-pijplijnen waarmee u snel aan de slag met Data Factory. Sjablonen zijn nuttig wanneer u geen ervaring hebt met Data Factory en u snel aan de slag wilt. Deze sjablonen worden de ontwikkelingstijd voor het bouwen van projecten voor gegevensintegratie waardoor de productiviteit van ontwikkelaars.

## <a name="create-data-factory-pipelines-from-templates"></a>Data Factory-pijplijnen maken met sjablonen

U kunt aan de slag maken van een Data Factory-pijplijn met een sjabloon in de volgende twee manieren:

1.  Selecteer **pijplijn maken met sjabloon** op de pagina overzicht te openen van de galerie met sjablonen.

    ![Open de galerie met sjablonen op de pagina overzicht](media/solution-templates-introduction/templates-intro-image1.png)

1.  Selecteer op het tabblad maken in Resource Explorer, **+**, klikt u vervolgens **pijplijn van sjabloon** openen van de galerie met sjablonen.

    ![De galerie met sjablonen openen vanuit het tabblad auteur](media/solution-templates-introduction/templates-intro-image2.png)

## <a name="template-gallery"></a>Galerie met sjablonen

![De galerie met sjablonen](media/solution-templates-introduction/templates-intro-image3.png)

### <a name="out-of-the-box-data-factory-templates"></a>Buiten het vak Data Factory-sjablonen

Data Factory maakt gebruik van Azure Resource Manager-sjablonen voor het opslaan van data factory-pijplijn sjablonen. U kunt zien dat alle Resource Manager-sjablonen, samen met het manifestbestand gebruikt voor buiten de sjablonen van de Data Factory vak de [officiële Azure Data Factory Github-opslagplaats](https://github.com/Azure/Azure-DataFactory/tree/master/templates). De vooraf gedefinieerde sjablonen die door Microsoft geleverd, omvatten maar zijn niet beperkt tot de volgende items:

-   Kopieer de sjablonen:

    -   [Bulksgewijs kopiëren uit Database](solution-template-bulk-copy-with-control-table.md)

    -   [Meerdere bestand containers tussen winkels op basis van een bestand kopiëren](solution-template-copy-files-multiple-containers.md)

    -   [Delta-kopie van de Database](solution-template-delta-copy-with-control-table.md)

    -   Kopiëren van \<bron\> naar \<bestemming\>

        -   Vanaf Amazon S3 naar Azure Data Lake Store van generatie 2

        -   Generatie 2 uit Google Big Query naar Azure Data Lake Store

        -   Van HDF naar Azure Data Lake Store Gen 2

        -   Van Netezza naar Azure Data Lake Store Gen 1

        -   Uit SQL Server on-premises naar Azure SQL Database

        -   Uit SQL Server on-premises naar Azure SQL Data Warehouse

        -   Van Oracle on-premises naar Azure SQL Data Warehouse

-   SSIS-sjablonen

    -   Azure-SSIS Integration Runtime voor het uitvoeren van SSIS-pakketten plannen

-   Sjablonen transformeren

    -   [ETL met Azure Databricks](solution-template-databricks-notebook.md)

### <a name="my-templates"></a>Mijn sjablonen

U kunt ook een pijplijn opslaan als sjabloon door het selecteren van **opslaan als sjabloon** op het tabblad pijplijn.

![Een pijplijn opslaan als een sjabloon](media/solution-templates-introduction/templates-intro-image4.png)

Vindt u pijplijnen die zijn opgeslagen als sjablonen in de **Mijn sjablonen** sectie van de galerie met sjablonen. U kunt ook zien in de **sjablonen** sectie in Resource Explorer.

![Mijn sjablonen](media/solution-templates-introduction/templates-intro-image5.png)

> [!NOTE]
> U moet GIT-integratie inschakelen voor het gebruik van de functie Mijn sjablonen. Zowel Azure DevOps GIT en GitHub worden ondersteund.
