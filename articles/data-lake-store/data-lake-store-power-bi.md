---
title: Analyseren van gegevens in Azure Data Lake Storage Gen1 met behulp van Power BI | Microsoft Docs
description: Power BI gebruiken voor het analyseren van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b873d90dbc33aff27cd2fc3b5099b08eacf929b6
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880063"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen1-by-using-power-bi"></a>Analyseren van gegevens in Azure Data Lake Storage Gen1 met behulp van Power BI
In dit artikel leert u hoe u Power BI Desktop gebruiken om te analyseren en visualiseren van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **De account van een Data Lake Storage Gen1**. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md). In dit artikel wordt ervan uitgegaan dat u een Data Lake Storage Gen1-account, met de naam al hebt gemaakt **myadlsg1**, en een eenvoudig gegevensbestand geüpload (**Drivers.txt**) toe. Dit voorbeeldbestand is beschikbaar voor downloaden van [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. U kunt dit downloaden van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI Desktop
1. Start Power BI Desktop op uw computer.
2. Uit de **Start** lint, klikt u op **gegevens ophalen**, en klik vervolgens op meer. In de **gegevens ophalen** in het dialoogvenster, klikt u op **Azure**, klikt u op **Azure Data Lake Store**, en klik vervolgens op **Connect**.
   
    ![Verbinding maken met Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account.png "verbinding maken met Data Lake Storage Gen1")
3. Als u een dialoogvenster over de connector wordt in een ontwikkelingsfase bevindt ziet, kunt u kiezen om door te gaan.
4. In de **Azure Data Lake Store** in het dialoogvenster, geeft u de URL naar uw Data Lake Storage Gen1-account en klik vervolgens op **OK**.
   
    ![URL voor Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL voor Data Lake Storage Gen1")
5. Klik in het volgende dialoogvenster op **aanmelden** zich aanmelden bij de Gen1 van Data Lake Storage-account. U wordt omgeleid naar de aanmeldingspagina van uw organisatie. Volg de aanwijzingen voor het aanmelden bij het account.
   
    ![Meld u aan bij de Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Meld u aan bij de Data Lake Storage Gen1")
6. Nadat u hebt aangemeld, klikt u op **Connect**.
   
    ![Verbinding maken met Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "verbinding maken met Data Lake Storage Gen1")
7. Het volgende dialoogvenster ziet u het bestand dat u hebt geüpload naar uw Data Lake Storage Gen1-account. Controleer of de gegevens en klik vervolgens op **Load**.
   
    ![Laden van gegevens uit Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "laden van gegevens uit Data Lake Storage Gen1")
8. Nadat de gegevens zijn geladen in Power BI, ziet u de volgende velden in de **velden** tabblad.
   
    ![Velden geïmporteerd](./media/data-lake-store-power-bi/imported-fields.png "velden geïmporteerd")
   
    Voor het visualiseren en analyseren van de gegevens, liever we echter de gegevens worden per de volgende velden beschikbaar
   
    ![Gewenste velden](./media/data-lake-store-power-bi/desired-fields.png "gewenste velden")
   
    In de volgende stappen wordt de query voor het converteren van de geïmporteerde gegevens in de gewenste indeling bijgewerkt.
9. Uit de **Start** lint, klikt u op **query's bewerken**.
   
    ![Query's bewerken](./media/data-lake-store-power-bi/edit-queries.png "query's bewerken")
10. In de Query-Editor, onder de **inhoud** kolom, klikt u op **binaire**.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query1.png "query's bewerken")
11. Ziet u een bestandspictogram, waarmee de **Drivers.txt** -bestand dat u hebt geüpload. Met de rechtermuisknop op het bestand en klik op **CSV**.    
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query2.png "query's bewerken")
12. U ziet uitvoer zoals hieronder wordt weergegeven. Uw gegevens is nu beschikbaar in een indeling die u gebruiken kunt om visualisaties te maken.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query3.png "query's bewerken")
13. Uit de **Start** lint, klikt u op **sluiten en toepassen**, en klik vervolgens op **sluiten en toepassen**.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/load-edited-query.png "query's bewerken")
14. Nadat de query is bijgewerkt, de **velden** tabblad ziet u de nieuwe velden beschikbaar voor visualisatie.
    
    ![Bijgewerkte velden](./media/data-lake-store-power-bi/updated-query-fields.png "bijgewerkte velden")
15. Laat het ons een cirkeldiagram om weer te geven van de stuurprogramma's in elke plaats voor een bepaald land/regio maken. Om dit te doen, moet u de volgende selecties maken.
    
    1. Klik op het tabblad visualisaties op het symbool voor een cirkeldiagram.
       
        ![Maken van de cirkeldiagram](./media/data-lake-store-power-bi/create-pie-chart.png "cirkeldiagram maken")
    2. De kolommen die we gaan gebruiken **kolom 4** (naam van de stad) en **kolom 7** (naam van het land/regio). Sleep deze kolommen uit **velden** tab naar **visualisaties** tabblad zoals hieronder wordt weergegeven.
       
        ![Visualisaties maken](./media/data-lake-store-power-bi/create-visualizations.png "visualisaties maken")
    3. Het cirkeldiagram moet nu lijken op, zoals hieronder wordt weergegeven.
       
        ![Cirkeldiagram](./media/data-lake-store-power-bi/pie-chart.png "visualisaties maken")
16. Als u een bepaald land van filters op niveau van de pagina selecteert, kunt u nu het aantal stuurprogramma's in elke plaats van het geselecteerde land zien. Bijvoorbeeld onder de **visualisaties** tabblad onder **filters op paginaniveau**, selecteer **Brazilië**.
    
    ![Selecteer een land/regio](./media/data-lake-store-power-bi/select-country.png "een land/regio selecteren")
17. Het cirkeldiagram wordt automatisch bijgewerkt om de stuurprogramma's in de steden van Brazilië weer te geven.
    
    ![Stuurprogramma's in een land/regio](./media/data-lake-store-power-bi/driver-per-country.png "stuurprogramma's per land/regio")
18. Uit de **bestand** menu, klikt u op **opslaan** om op te slaan van de visualisatie als een Power BI Desktop-bestand.

## <a name="publish-report-to-power-bi-service"></a>Rapport publiceren naar Power BI-service
Nadat u de visualisaties in Power BI Desktop hebt gemaakt, kunt u deze delen met anderen door deze te publiceren naar Power BI-service. Zie voor instructies over hoe u dat doet, [publiceren vanuit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Zie ook
* [Analyseren van gegevens in Data Lake Storage Gen1 met Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

