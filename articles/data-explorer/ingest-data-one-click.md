---
title: Gebruik een opname met één klik om gegevens op te nemen in azure Data Explorer
description: Meer informatie over het opnemen (laden) van gegevens in azure Data Explorer met één klik op opname.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: b1ce2d9efe44021b4e3191739bd2f922e34c44cb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69520047"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Gebruik een opname met één klik om gegevens op te nemen in azure Data Explorer

In dit artikel wordt beschreven hoe u met één klik kunt opnemen in een nieuwe tabel in JSON-of CSV-indelingen van opslag in azure Data Explorer. Zodra de gegevens zijn opgenomen, kunt u de tabel bewerken en query's uitvoeren met de webgebruikersinterface.

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* Meld u aan bij [de toepassing](https://dataexplorer.azure.com/).
* [Een Azure Data Explorer-cluster en-data base](create-cluster-database-portal.md) maken
* Bron van de gegevens in Azure Storage.

## <a name="ingest-new-data"></a>Nieuwe gegevens opnemen

1. Klik met de rechter muisknop op de naam van de *Data Base* en selecteer **nieuwe gegevens opnemen (preview-versie)**

    ![Selecteer een opname in de Web-UI](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. In het venster **gegevens opname (preview)** , in het tabblad **bron** , voltooit u de **Project Details**:

    * Voer een nieuwe **tabel naam**in. 
    * Selecteer **opname type** > **uit opslag**.
    * Voer de **koppeling naar de opslag** -URL in op opslag. Gebruik de SAS-URL van de BLOB voor privé opslag accounts. 
    * **Schema bewerken** selecteren
 
    ![Eén Klik op opname bron Details](media/ingest-data-one-click/one-click-ingestion-source.png) 

1. Op het tabblad **schema** selecteert u **gegevens indeling** in vervolg keuzelijst > **JSON** of **CSV**. 
   
   Als u **CSV**selecteert:
    * Schakel het selectie vakje **koptekst negeren** om rij van het CSV-bestand te negeren in.    
    * De **toewijzings naam** wordt automatisch ingesteld, maar kan worden bewerkt.

    ![Eén Klik op opname CSV-indeling schema. png](media/ingest-data-one-click/one-click-csv-format.png)

   Als **JSON**selecteren:
    * **JSON-niveaus**selecteren: 1-10 van vervolg keuzelijst. De niveaus in het JSON-bestand worden weer gegeven in de tabel aan de rechter kant. 
    * De **toewijzings naam** wordt automatisch ingesteld, maar kan worden bewerkt.

    ![schema met één klik voor de JSON-indeling van opname](media/ingest-data-one-click/one-click-json-format.png)  

1. Selecteerin de editor **V** aan de rechter kant om de editor te openen. In de editor kunt u de automatische query's weer geven en kopiëren die zijn gegenereerd op basis van uw invoer. 

1.  In de tabel aan de rechter kant: 
    * Selecteer **V** rechts van kolom om **kolom naam te wijzigen**, **kolom verwijderen**, oplopend **sorteren**of aflopend **sorteren**
    * Dubbel klik op de kolom naam die u wilt bewerken.
    * Selecteer het pictogram links van de kolom naam om het gegevens type te wijzigen. 

1. Selecteer **opname starten** om tabel, toewijzing maken en gegevens opname te maken.
 
## <a name="query-data"></a>Querygegevens

1. In het venster **gegevens opname voltooid** worden alle drie de stappen gemarkeerd met een groen vinkje als de gegevens opname is voltooid. 
 
    ![Eén Klik op gegevens opname is voltooid](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Selecteer **V** om query te openen. Kopiëren naar de Web-UI om de query te bewerken.

1. Het menu aan de rechter kant bevat **snelle query's** en **hulpprogram ma's**. 

    * **Snelle query's** bevatten koppelingen naar de Web-UI met voorbeeld query's.
    * **Hulpprogram ma's** bevatten koppelingen naar de Web-UI met **Drop-opdrachten** waarmee u problemen kunt oplossen door `.drop` de relevante opdracht uit te voeren.

    > [!TIP]
    > Gegevens kunnen verloren gaan met `.drop` behulp van opdrachten. Gebruik deze zorgvuldig.

## <a name="next-steps"></a>Volgende stappen

* [Query's uitvoeren op gegevens in azure Data Explorer web-UI](web-query-data.md)
* [Query's schrijven voor Azure Data Explorer met behulp van de Kusto-query taal](write-queries.md)
