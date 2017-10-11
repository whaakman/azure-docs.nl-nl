---
title: Gerelateerde gegevens activa weergeven in Azure Data Catalog | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u gerelateerde gegevensassets van een activum geselecteerde gegevens weergeven in Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d45f2cabe712a7982f99a9d280fed4494fc4d377
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Hoe gerelateerde gegevens activa weergeven in Azure Data Catalog?
Azure Data Catalog kunt u gegevensassets die betrekking hebben op een geselecteerde gegevens asset en bekijk de relaties tussen deze twee weergeven. 

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen 
Wanneer u gegevensassets van de volgende gegevensbronnen registreert, worden de metagegevens over join-relaties tussen de geselecteerde gegevensassets Azure Data Catalog automatisch geregistreerd. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

## <a name="view-related-data-assets"></a>Gerelateerde gegevens activa weergeven
Als u wilt weergeven van gegevensassets die zijn gerelateerd aan een geselecteerde gegevensset, gebruiken de **relaties** tabblad zoals weergegeven in de volgende afbeelding: 

![Azure Data Catalog - gerelateerde gegevensassets weergeven](media\data-catalog-how-to-view-related-data-assets\relationships-tab.png)

In dit voorbeeld zijn er twee relaties voor het geselecteerde **ProductSubcategory** gegevensasset: 

- ProductSubcategoryID kolom van de Product-tabel heeft een refererende-sleutelrelatie met ProductSubcategoryID kolom van de geselecteerde ProductSubcategory-tabel. 
- ProductCategoryID kolom van de tabel ProductSubCategory heeft een refererende-sleutelrelatie met ProductCategoryID kolom van de geselecteerde ProductCategory-tabel.

> [!NOTE]
> U ziet de richting van de pijl in de structuurweergave relaties.  

Beweeg de muisaanwijzer over voor meer informatie zoals de volledig gekwalificeerde naam van de kolom, en u ziet een pop-up die vergelijkbaar is met de volgende afbeelding: 

![Azure Data Catalog - relatie pop](media\data-catalog-how-to-view-related-data-assets\relationship-popup.png)

Als u wilt opnemen relaties tussen de activa die al zijn geregistreerd, deze assets opnieuw te registreren.

## <a name="next-steps"></a>Volgende stappen
- [Gegevensassets beheren](data-catalog-how-to-manage.md)