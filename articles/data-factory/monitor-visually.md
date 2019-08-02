---
title: Azure-gegevens fabrieken visueel bewaken | Microsoft Docs
description: Meer informatie over het visueel bewaken van Azure-gegevens fabrieken
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: shlo
ms.openlocfilehash: 53ead1caed47ae442670f0b6bcd54cd84956a759
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720619"
---
# <a name="visually-monitor-azure-data-factories"></a>Azure-gegevens fabrieken visueel bewaken
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken.

In deze Quick Start leert u hoe u Data Factory pijp lijnen visueel kunt bewaken zonder dat u maar één regel code hoeft te schrijven.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="monitor-data-factory-pipelines"></a>Data Factory pijp lijnen bewaken

Bewaak pijp lijn-en activiteit uitvoeringen met een eenvoudige lijst weergave interface. Alle uitvoeringen worden weer gegeven in de tijd zone van de lokale browser. U kunt de tijd zone en alle datum-en tijd velden op de geselecteerde tijd zone uitlijnen.  

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
3. Navigeer naar de Blade gemaakt data factory in Azure Portal en klik op de tegel & beheer controleren om de Data Factory visuele bewakings ervaring te starten.

## <a name="monitor-pipeline-runs"></a>Pijplijnuitvoeringen controleren
Lijstweergave geeft elke pijplijnuitvoering voor uw v2-pijplijnen van uw gegevensfactory duidelijk weer. Opgenomen kolommen:

| **Kolom naam** | **Beschrijving** |
| --- | --- |
| Naam van pijp lijn | Naam van de pijplijn. |
| Acties | Er is één actie beschikbaar om de uitvoering van de activiteit weer te geven. |
| Start uitvoeren | Begin datum van de pijplijn uitvoering (MM/DD/JJJJ, uu: MM: SS AM/PM) |
| Duration | Uitvoerings duur (UU: MM: SS) |
| Geactiveerd door | Hand matige trigger, Schedule trigger |
| Status | Mislukt, geslaagd, in behandeling |
| Parameters | Para meters voor pijplijn uitvoeringen (naam, waardeparen) |
| Fout | Fout bij uitvoering van pijp lijn (indien/geen) |
| Run-id | ID van de pijplijn uitvoering |

![Pijplijnuitvoeringen controleren](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Uitvoering van activiteiten controleren
Lijstweergave geeft activiteituitvoeringen weer die corresponderen met elke pijplijnuitvoering. Klik op het pictogram **activiteit wordt** weer gegeven onder de kolom **acties** om de uitvoering van de activiteit voor elke pijplijn uitvoering te bekijken. Opgenomen kolommen:

| **Kolom naam** | **Beschrijving** |
| --- | --- |
| Naam activiteit | De naam van de activiteit in de pijp lijn. |
| Type activiteit | Type activiteit, zoals Copy, HDInsightSpark, HDInsightHive, enzovoort. |
| Start uitvoeren | Begin datum van activiteit uitvoering (MM/DD/JJJJ, uu: MM: SS AM/PM) |
| Duration | Uitvoerings duur (UU: MM: SS) |
| Status | Mislukt, geslaagd, in behandeling |
| Invoer | JSON-matrix met een beschrijving van de activiteit invoer |
| Output | JSON-matrix waarin de uitvoer van de activiteit wordt beschreven |
| Fout | Fout bij uitvoeren van activiteit (indien/geen) |

![Uitvoering van activiteiten controleren](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> U moet op het pictogram **vernieuwen** bovenaan klikken om de lijst met pijp lijn-en activiteit uitvoeringen te vernieuwen. Automatisch vernieuwen wordt momenteel niet ondersteund.

![Vernieuwen](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>data factory selecteren die u wilt bewaken
Beweeg de muis aanwijzer op het pictogram **Data Factory** linksboven. Klik op het pictogram met de pijl om een lijst weer te geven met Azure-abonnementen en-gegevens fabrieken die u kunt bewaken.

![Data factory selecteren](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>De lijst weergave configureren

### <a name="apply-rich-ordering-and-filtering"></a>Uitgebreide ordening en filters toep assen

Order pijplijn wordt uitgevoerd in desc/ASC door start-en filter pijplijn uitvoeringen uit te voeren op de volgende kolommen:

| **Kolom naam** | **Beschrijving** |
| --- | --- |
| Naam van pijp lijn | Naam van de pijplijn. De opties bevatten snelle filters voor de afgelopen 24 uur, de afgelopen week, de afgelopen 30 dagen of een aangepaste datum/tijd. |
| Start uitvoeren | Begin datum tijd pijplijn uitvoering |
| Uitvoerings status | Filter wordt uitgevoerd op status-geslaagd, mislukt, in behandeling |

![Filteren](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Kolommen toevoegen of verwijderen
Klik met de rechter muisknop op de koptekst van de lijst weergave en kies kolommen die u wilt weer geven in de lijst weergave

![Kolommen](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Kolom breedten aanpassen
Verg root of verklein de kolom breedten in de lijst weergave door de muis aanwijzer over de kolomkop te bewegen

## <a name="promote-user-properties-to-monitor"></a>Gebruikers eigenschappen promo veren om te bewaken

U kunt elke pijplijn activiteit eigenschap promo veren als een gebruikers eigenschap, zodat deze een entiteit wordt die u kunt bewaken. U kunt bijvoorbeeld de **bron** -en **doel** eigenschappen van de Kopieer activiteit in uw pijp lijn promo veren als gebruikers eigenschappen. U kunt ook **automatisch genereren** selecteren om de eigenschappen van de **bron** -en **doel** gebruiker te genereren voor een Kopieer activiteit.

![Gebruikers eigenschappen maken](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> U kunt Maxi maal 5 eigenschappen van pijplijn activiteit verhogen als gebruikers eigenschappen.

Nadat u de gebruikers eigenschappen hebt gemaakt, kunt u ze in de controle lijst Weergaven controleren. Als de bron voor de Kopieer activiteit een tabel naam is, kunt u de naam van de bron tabel bewaken als een kolom in de lijst weergave voor uitvoeringen van activiteit.

![Lijst met uitgevoerde activiteiten zonder gebruikers eigenschappen](media/monitor-visually/monitor-user-properties-image2.png)

![Kolommen toevoegen voor de lijst met gebruikers eigenschappen aan de uitvoering van de activiteit](media/monitor-visually/monitor-user-properties-image3.png)

![Lijst met uitgevoerde activiteiten met kolommen voor gebruikers eigenschappen](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Activiteiten in een pijp lijn opnieuw uitvoeren

U kunt nu activiteiten in een pijp lijn opnieuw uitvoeren. Klik op uitvoeringen van **activiteit weer geven** en selecteer de activiteit in uw pijp lijn van welk punt u de pijp lijn opnieuw wilt uitvoeren.

![Uitvoeringen van activiteit bekijken](media/monitor-visually/rerun-activities-image1.png)

![Een uitvoering van een activiteit selecteren](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Geschiedenis van opnieuw uitvoeren weer geven

U kunt de geschiedenis van opnieuw uitvoeren weer geven voor alle pijplijn uitvoeringen in de lijst weergave.

![Geschiedenis weergeven](media/monitor-visually/rerun-history-image1.png)

U kunt ook de uitvoerings geschiedenis weer geven voor een bepaalde pijplijn uitvoering.

![Geschiedenis voor een pijplijn uitvoering weer geven](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Gantt-weer gaven

Gebruik Gantt-weer gaven om uw pijp lijnen en activiteiten uitvoeringen snel te visualiseren. U kunt de Gantt-weer gave per pijp lijn of groep bekijken op aantekeningen/Tags die u hebt gemaakt in uw pijp lijnen.

![Gantt-diagram](media/monitor-visually/gantt1.png)

![Aantekeningen in Gantt-diagrammen](media/monitor-visually/gantt2.png)

De lengte van de balk informeert de duur van de pijp lijn. U kunt ook op de balk klikken om meer details weer te geven.

![Duur Gantt-diagram](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Rond leidingen
Klik op het informatie pictogram in de linkerbenedenhoek en klik op begeleide rond leidingen voor stapsgewijze instructies voor het bewaken van uw pijp lijn en de uitvoering van de activiteit.

![Rond leidingen](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Feedback
Klik op het pictogram feedback om ons feedback te geven over de verschillende functies of eventuele problemen die u kunt tegen komen.

![Feedback](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Waarschuwingen

U kunt waarschuwingen activeren over ondersteunde metrische gegevens in Data Factory. Selecteer **monitor-> Waarschuwingen & metrische gegevens** op de pagina Data Factory monitor om aan de slag te gaan.

![](media/monitor-visually/alerts01.png)

Bekijk de volgende video voor een inleiding en demonstratie van zeven minuten voor deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Waarschuwingen maken

1.  Klik op **nieuwe waarschuwings regel** om een nieuwe waarschuwing te maken.

    ![](media/monitor-visually/alerts02.png)

1.  Geef de naam van de regel op en selecteer de **Ernst**van de waarschuwing.

    ![](media/monitor-visually/alerts03.png)

1.  Selecteer de waarschuwings criteria.

    ![](media/monitor-visually/alerts04.png)

    ![](media/monitor-visually/alerts05.png)

1.  De waarschuwings logica configureren. U kunt een waarschuwing maken voor de geselecteerde metrische gegevens voor alle pijp lijnen en bijbehorende activiteiten. U kunt ook een bepaald activiteitstype, de naam van de activiteit, de naam van een pijp lijn of een fout type selecteren.

    ![](media/monitor-visually/alerts06.png)

1.  **E-mail/SMS/push/Voice** -meldingen configureren voor de waarschuwing. Maak of kies een bestaande **actie groep** voor de waarschuwings meldingen.

    ![](media/monitor-visually/alerts07.png)

    ![](media/monitor-visually/alerts08.png)

1.  De waarschuwings regel maken.

    ![](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Volgende stappen

Zie [pijp lijnen programmatisch bewaken en beheren](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) voor meer informatie over het bewaken en beheren van pijp lijnen.
