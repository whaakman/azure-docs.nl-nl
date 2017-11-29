---
title: Gegevens in Data Lake Store te analyseren met Power BI | Microsoft Docs
description: Power BI gebruiken voor het analyseren van gegevens die zijn opgeslagen in Azure Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 0c77ccd29e3e22005c3339ed4e89dccfed725fa0
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Gegevens in Data Lake Store te analyseren met Power BI
In dit artikel leert u hoe u Power BI Desktop om te analyseren en visualiseren van gegevens die zijn opgeslagen in Azure Data Lake Store.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-account**. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md). In dit artikel wordt ervan uitgegaan dat u hebt al een Data Lake Store-account genoemd gemaakt **mybidatalakestore**, en een voorbeeldbestand gegevens geüpload (**Drivers.txt**) aan. Dit voorbeeldbestand is beschikbaar voor downloaden van [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. U kunt dit downloaden van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI Desktop
1. Power BI Desktop op uw computer starten.
2. Van de **Start** lint, klikt u op **gegevens ophalen**, en klik vervolgens op meer. In de **gegevens ophalen** in het dialoogvenster, klikt u op **Azure**, klikt u op **Azure Data Lake Store**, en klik vervolgens op **Connect**.
   
    ![Verbinding maken met Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "verbinding maken met Data Lake Store")
3. Als er een dialoogvenster over de connector wordt in een ontwikkelingsfase bevindt, kunt u kiezen om door te gaan.
4. In de **Microsoft Azure Data Lake Store** in het dialoogvenster en geef de URL naar uw Data Lake Store-account en klik vervolgens op **OK**.
   
    ![URL voor Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL voor Data Lake Store")
5. Klik in het volgende dialoogvenster op **aanmelden** zich aanmeldt bij de Data Lake Store-account. U wordt omgeleid naar de aanmeldingspagina van uw organisatie. Volg de aanwijzingen voor het aanmelden bij het account.
   
    ![Meld u aan bij de Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Meld u aan bij de Data Lake Store")
6. Nadat u bent aangemeld, klikt u op **Connect**.
   
    ![Verbinding maken met Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "verbinding maken met Data Lake Store")
7. Het volgende dialoogvenster ziet u het bestand dat u hebt geüpload naar uw Data Lake Store-account. Controleer of de gegevens en klik vervolgens op **Load**.
   
    ![Gegevens laden in Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "laden van gegevens uit Data Lake Store")
8. Nadat de gegevens zijn geladen in Power BI, ziet u de volgende velden in de **velden** tabblad.
   
    ![Velden geïmporteerd](./media/data-lake-store-power-bi/imported-fields.png "velden geïmporteerd")
   
    Voor het visualiseren en analyseren van de gegevens, liever we echter de gegevens worden per de volgende velden beschikbaar
   
    ![Velden gewenst](./media/data-lake-store-power-bi/desired-fields.png "gewenst velden")
   
    In de volgende stappen wordt de query om de geïmporteerde gegevens in de gewenste indeling te converteren bijgewerkt.
9. Van de **Start** lint, klikt u op **query's bewerken**.
   
    ![Query's bewerken](./media/data-lake-store-power-bi/edit-queries.png "query's bewerken")
10. De Query-Editor, onder de **inhoud** kolom, klikt u op **binaire**.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query1.png "query's bewerken")
11. Ziet u een bestandspictogram, waarmee de **Drivers.txt** -bestand dat u hebt geüpload. Met de rechtermuisknop op het bestand en klik op **CSV**.    
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query2.png "query's bewerken")
12. U ziet een uitvoer zoals hieronder wordt weergegeven. Uw gegevens is nu beschikbaar in een indeling die u gebruiken kunt voor het maken van visualisaties.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/convert-query3.png "query's bewerken")
13. Van de **Start** lint, klikt u op **sluiten en toepassen**, en klik vervolgens op **sluiten en toepassen**.
    
    ![Query's bewerken](./media/data-lake-store-power-bi/load-edited-query.png "query's bewerken")
14. Nadat de query wordt bijgewerkt, de **velden** tabblad wordt de nieuwe velden beschikbaar voor visualisatie weergegeven.
    
    ![Bijgewerkte velden](./media/data-lake-store-power-bi/updated-query-fields.png "velden bijgewerkt")
15. Laat het ons een cirkeldiagram te vertegenwoordigen de stuurprogramma's in verschillende steden voor een bepaald land maken. U doet dit door de volgende selecties te maken.
    
    1. Klik op het tabblad visualisaties op het symbool voor een cirkeldiagram.
       
        ![Maken van het cirkeldiagram](./media/data-lake-store-power-bi/create-pie-chart.png "cirkeldiagram maken")
    2. De kolommen die we gaan zijn **kolom 4** (naam van de plaats) en **kolom 7** (naam van het land). Sleep deze kolommen uit **velden** tab naar **visualisaties** tabblad zoals hieronder wordt weergegeven.
       
        ![Maak visualisaties](./media/data-lake-store-power-bi/create-visualizations.png "visualisaties maken")
    3. Het cirkeldiagram ziet er nu zoals hieronder weergegeven.
       
        ![Cirkeldiagram](./media/data-lake-store-power-bi/pie-chart.png "visualisaties maken")
16. Als u een bepaald land van de paginafilters niveau selecteert, kunt u het aantal stuurprogramma's in elke plaats van het geselecteerde land nu zien. Bijvoorbeeld onder de **visualisaties** tabblad onder **niveau filters pagina**, selecteer **Brazilië**.
    
    ![Selecteer een land](./media/data-lake-store-power-bi/select-country.png "land selecteren")
17. Het cirkeldiagram wordt automatisch bijgewerkt om de stuurprogramma's in de steden van Brazilië weer te geven.
    
    ![Stuurprogramma's in een land](./media/data-lake-store-power-bi/driver-per-country.png "stuurprogramma's per land")
18. Van de **bestand** menu, klikt u op **opslaan** de visualisatie opslaan als Power BI Desktop-bestand.

## <a name="publish-report-to-power-bi-service"></a>Rapport naar Power BI-service publiceren
Zodra u de visualisaties in Power BI Desktop hebt gemaakt, kunt u het delen met anderen door deze te publiceren naar de Power BI-service. Zie voor instructies over hoe u dat doet, [publiceren vanuit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Zie ook
* [Gegevens analyseren in Data Lake Store met Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

