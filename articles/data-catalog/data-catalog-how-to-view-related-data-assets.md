---
title: Gerelateerde gegevensassets weergeven in Azure Data Catalog
description: In dit artikel wordt uitgelegd hoe u gerelateerde gegevensassets van een geselecteerde gegevensasset weergeven in Azure Data Catalog.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: e68dc22943b7a53693320528f91670d9229b806f
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958329"
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Hoe kan ik gerelateerde gegevensassets weergeven in Azure Data Catalog?
Azure Data Catalog kunt u weergeven die betrekking hebben op een geselecteerde gegevens asset en bekijk de relaties tussen deze gegevensassets. 

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen 
Wanneer u gegevensassets uit de volgende gegevensbronnen hebt geregistreerd, wordt in Azure Data Catalog automatisch metagegevens over de joinrelaties tussen de geselecteerde gegevensassets geregistreerd. 

- SQL Server
- Azure SQL Database
- MySQL
- Oracle

> [!NOTE]
> Voor Data Catalog voor het importeren van de relatie tussen twee gegevensassets, moet u zowel de elementen op hetzelfde moment te registreren. Als u had een van deze afzonderlijk toegevoegd, is deze opnieuw op en de andere gegevensasset voor het importeren van de relatie tussen deze toevoegen.

## <a name="view-related-data-assets"></a>Gerelateerde gegevensassets weergeven
Als u wilt weergeven van gegevensassets die gerelateerd zijn aan een geselecteerde gegevensset, gebruikt u de **relaties** tabblad zoals wordt weergegeven in de volgende afbeelding: 

![Azure Data Catalog - gerelateerde gegevensassets weergeven](media/data-catalog-how-to-view-related-data-assets/relationships-tab.png)

In dit voorbeeld zijn er twee relaties voor de geselecteerde **ProductSubcategory** gegevensasset: 

- ProductSubcategoryID kolom van de tabel Product heeft een refererende-sleutelrelatie met ProductSubcategoryID kolom van de geselecteerde tabel ProductSubcategory. 
- ProductCategoryID kolom van de tabel ProductSubCategory heeft een refererende-sleutelrelatie met ProductCategoryID kolom van de geselecteerde tabel ProductCategory.

> [!NOTE]
> U ziet de richting van de pijl in de structuurweergave van relaties.  

Voor meer informatie, zoals de volledig gekwalificeerde naam van de kolom, Beweeg de muisaanwijzer over en ziet u een pop-upvenster die vergelijkbaar is met de volgende afbeelding: 

![Azure Data Catalog - relatie pop-upvenster](media/data-catalog-how-to-view-related-data-assets/relationship-popup.png)

Als u wilt opnemen relaties tussen de elementen die al zijn geregistreerd, deze assets opnieuw te registreren.

## <a name="next-steps"></a>Volgende stappen
- [Gegevensassets beheren](data-catalog-how-to-manage.md)