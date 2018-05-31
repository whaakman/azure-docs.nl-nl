---
title: Aan de slag met Azure Data Lake Analytics en Azure Portal
description: Informatie over het gebruik van Azure Portal voor het maken van een Data Lake Analytics-account, het maken van een Data Lake Analytics-taak met U-SQL, en het verzenden van de taak.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.topic: hero-article
ms.workload: big-data
ms.date: 03/21/2017
ms.openlocfilehash: 63fc4272dc4b26e9eb4846118bc57cd6aa5bf361
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358282"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Aan de slag met Azure Data Lake Analytics met Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van Azure Portal voor het maken van Azure Data Lake Analytics-accounts, het definiëren van taken in [U-SQL](data-lake-analytics-u-sql-get-started.md), en het verzenden van taken naar de Data Lake Analytics-service. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u een **Azure-abonnement** hebben. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

U maakt nu tegelijkertijd een Data Lake Analytics- en een Data Lake Store-account.  Deze stap is eenvoudig en duurt maar ongeveer 60 seconden om te voltooien.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Een resource maken** >  **Gegevens en analyses** > **Data Lake Analytics**.
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

1. Selecteer **New Job** vanuit het Data Lake Analytics-account.
2. Plak de tekst van het bovenstaande U-SQL-script. Geef een naam op voor de taak. 
3. Selecteer **Submit** om de taak te starten.   
4. Controleer de **status** van de taak en wacht totdat de taak de status **Succeeded** heeft.
5. Selecteer het tabblad **Data** en vervolgens het tabblad **Output**. Selecteer het uitvoerbestand met de naam `data.csv` en bekijk de uitvoergegevens.

## <a name="see-also"></a>Zie ook

* Zie [U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) om aan de slag te gaan met het ontwikkelen van U-SQL-toepassingen.
* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.
