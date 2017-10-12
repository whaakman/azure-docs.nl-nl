---
title: Aan de slag met Azure Data Lake Analytics met Azure Portal | Microsoft Docs
description: 'Informatie over het gebruik van Azure Portal voor het maken van een Data Lake Analytics-account, het maken van een Data Lake Analytics-taak met U-SQL, en het verzenden van de taak. '
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Aan de slag met Azure Data Lake Analytics met Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van Azure Portal voor het maken van Azure Data Lake Analytics-accounts, het definiëren van taken in [U-SQL](data-lake-analytics-u-sql-get-started.md), en het verzenden van taken naar de Data Lake Analytics-service. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u een **Azure-abonnement** hebben. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

U maakt nu tegelijkertijd een Data Lake Analytics- en een Data Lake Store-account.  Deze stap is eenvoudig en duurt maar ongeveer 60 seconden om te voltooien.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** >  **Gegevens + analyse** > **Data Lake Analytics**.
3. Selecteer waarden voor de volgende items:
   * **Naam**: geef uw Data Lake Analytics-account een naam (alleen kleine letters en cijfers zijn toegestaan).
   * **Abonnement**: kies het Azure-abonnement dat u gebruikt voor het Analytics-account.
   * **Resourcegroep**. Selecteer een bestaande Azure-resourcegroep of maak een nieuwe.
   * **Locatie**. Selecteer een Azure-datacenter voor het Data Lake Analytics-account.
   * **Data Lake Store**: volg de instructies voor het maken van een nieuw Data Lake Store-account of selecteer een bestaand account. 
4. U kunt ervoor kiezen om een prijscategorie te selecteren voor uw Data Lake Analytics-account.
5. Klik op **Create**. 


## <a name="your-first-u-sql-script"></a>Uw eerste U-SQL-script

De volgende tekst is een zeer eenvoudig U-SQL-script. Het scrip is alleen bedoeld om een kleine gegevensset in het script mee te definiëren en deze gegevensset vervolgens naar de standaard Data Lake Store te schrijven als een bestand met de naam `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>Een U-SQL-taak verzenden

1. Klik in het Data Lake Analytics-account op **New Job**.
2. Plak de tekst van het hierboven weergegeven U-SQL-script. 
3. Klik op **Taak verzenden**.   
4. Wacht tot de taakstatus verandert in **Succeeded**.
5. Zie [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md) (Data Lake Analytics-taken bewaken en fouten oplossen) als de taak is mislukt.
6. Klik op het tabblad **Output** en klik vervolgens op `data.csv`. 

## <a name="see-also"></a>Zie ook

* Zie [U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) om aan de slag te gaan met het ontwikkelen van U-SQL-toepassingen.
* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.
