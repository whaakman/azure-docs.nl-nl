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
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: da7ac8f323f9b2ec6303fbc85aa570373a538bcf
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Hoe gerelateerde gegevens activa weergeven in Azure Data Catalog?
Azure Data Catalog kunt u gegevensassets die betrekking hebben op een geselecteerde gegevens asset en bekijk de relaties tussen deze twee weergeven. 

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen 
Wanneer u gegevensassets van de volgende gegevensbronnen registreert, worden de metagegevens over join-relaties tussen de geselecteerde gegevensassets Azure Data Catalog automatisch geregistreerd. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

> [!NOTE]
> Voor Data Catalog voor het importeren van de relatie tussen twee gegevensassets, moet u zowel de elementen op hetzelfde moment te registreren. Als u een van deze afzonderlijk had toegevoegd, is deze opnieuw op en de andere gegevensasset voor het importeren van de relatie tussen deze twee toevoegen.

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