---
title: Azure data factory's visueel bewaken | Microsoft Docs
description: Meer informatie over het Azure Data Factory's visueel te controleren
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
ms.openlocfilehash: df684860cd3d1b6a002a300682ca4c6398461ba6
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54409959"
---
# <a name="visually-monitor-azure-data-factories"></a>Azure data factory's visueel bewaken
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken.

In deze snelstartgids leert u hoe u Data Factory-pijplijnen visueel bewaken zonder één regel code te schrijven.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="monitor-data-factory-pipelines"></a>Data Factory-pijplijnen bewaken

De pijplijn bewaken en de activiteit wordt uitgevoerd met een eenvoudige lijstweergave-interface. Alle uitvoeringen worden weergegeven in de lokale tijdzone. U kunt de tijdzone wijzigen en alle datum-/ tijdvelden uitlijnen op de geselecteerde tijdzone.  

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
3. Navigeer naar de gemaakte data factory-blade in Azure portal en klik op de tegel 'Controleren en beheren' om te starten van de Data Factory bewakingservaring voor de visual.

## <a name="monitor-pipeline-runs"></a>Pijplijnuitvoeringen controleren
Lijstweergave geeft elke pijplijnuitvoering voor uw v2-pijplijnen van uw gegevensfactory duidelijk weer. Opgenomen kolommen:

| **De naam van kolom** | **Beschrijving** |
| --- | --- |
| Pijplijnnaam | Naam van de pijplijn. |
| Acties | Één actie beschikbaar om uitvoeringen van activiteit weer te geven. |
| Uitvoering starten | Pijplijn uitvoeren start datum en tijd (MM/DD/JJJJ uu: mm: SS AM/PM) |
| Duur | Uitvoeringsduur (uu: mm:) |
| Geactiveerd door | Handmatige triggers, trigger voor schema |
| Status | Is mislukt, is voltooid, wordt uitgevoerd |
| Parameters | Pijplijnuitvoering parameters (naam, waarde-paren) |
| Fout | Pijplijnuitvoering fout (indien/any) |
| Run-id | ID van de pijplijnuitvoering |

![Pijplijnuitvoeringen controleren](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Uitvoering van activiteiten controleren
Lijstweergave geeft activiteituitvoeringen weer die corresponderen met elke pijplijnuitvoering. Klik op **'Uitvoeringen van activiteit'** pictogram onder de **'Acties'** kolom om weer te geven van de activiteit wordt uitgevoerd voor elke pijplijn-run. Opgenomen kolommen:

| **De naam van kolom** | **Beschrijving** |
| --- | --- |
| Naam activiteit | Naam van de activiteit in de pijplijn. |
| Type activiteit | Het type van de activiteit, zoals kopiëren, HDInsightSpark, HDInsightHive, enzovoort. |
| Uitvoering starten | Uitvoering van activiteit start datum en tijd (MM/DD/JJJJ uu: mm: SS AM/PM) |
| Duur | Uitvoeringsduur (uu: mm:) |
| Status | Is mislukt, is voltooid, wordt uitgevoerd |
| Invoer | JSON-matrix met een beschrijving van de invoer voor de activiteit |
| Uitvoer | JSON-matrix met een beschrijving van de uitvoer voor activiteiten |
| Fout | Fout (indien/any) de uitvoering van activiteiten |

![Uitvoering van activiteiten controleren](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> U moet klikken op **'Vernieuwen'** pictogram bovenaan de lijst van de uitvoering van pijplijn en activiteit te vernieuwen. Automatisch vernieuwen wordt momenteel niet ondersteund.

![Vernieuwen](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>Selecteer een data factory te controleren
Beweeg de muisaanwijzer op de **Data Factory** pictogram links bovenin. Klik op het pictogram 'Pijl' voor een overzicht van azure-abonnementen en gegevensfactory's die u kunt controleren.

![Data factory selecteren](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>De lijstweergave configureren

### <a name="apply-rich-ordering-and-filtering"></a>Uitgebreide volgorde plaatsen en filteren van toepassing

Volgorde pijplijnuitvoeringen in aflopende/oplopende op Run Start en filter pijplijnuitvoeringen door de volgende kolommen:

| **De naam van kolom** | **Beschrijving** |
| --- | --- |
| Pijplijnnaam | Naam van de pijplijn. Opties voor snelle filters bevatten voor 'afgelopen 24 uur', 'Afgelopen week', 'afgelopen 30 dagen' of een aangepaste datum en tijd selecteren. |
| Uitvoering starten | Pijplijnuitvoering starten, datum en tijd |
| Uitvoeringsstatus | Filter wordt uitgevoerd op status - geslaagd, mislukt, wordt uitgevoerd |

![Filteren](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Kolommen toevoegen of verwijderen
Met de rechtermuisknop op de lijstweergavekop en kies de kolommen die u wilt weergeven in de lijstweergave

![Kolommen](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Kolombreedte
Vergroot of verklein de kolombreedtes in lijstweergave door de muiswijzer op de kolomkop

## <a name="promote-user-properties-to-monitor"></a>Eigenschappen van de gebruiker voor het bewaken van promoten

Zodat deze een entiteit die u kunt controleren, kunt u een eigenschap van een pijplijn activiteit als een eigenschap van het promoveren. Bijvoorbeeld, u kunt verhogen de **bron** en **bestemming** eigenschappen van de kopieeractiviteit in de pijplijn als eigenschappen van de gebruiker. U kunt ook selecteren **automatisch genereren** voor het genereren van de **bron** en **bestemming** gebruikerseigenschappen voor een kopieeractiviteit.

![Eigenschappen van de gebruiker maken](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> U kunt maximaal 5 pijplijn activiteitseigenschappen alleen promoveren als eigenschappen van de gebruiker.

Nadat u de eigenschappen van de gebruiker hebt gemaakt, kunt u deze bewaken in de controleweergaven in de lijst. Als de bron voor de kopieerbewerking een tabelnaam wordt opgegeven is, kunt u de naam van de tabel bewaken terwijl een kolom in de activiteit wordt uitgevoerd lijstweergave.

![Lijst zonder gebruikerseigenschappen uitvoeringen van activiteit](media/monitor-visually/monitor-user-properties-image2.png)

![Kolommen voor de eigenschappen van de gebruiker toevoegen aan lijst met uitvoeringen van activiteit](media/monitor-visually/monitor-user-properties-image3.png)

![Uitvoeringen van activiteit lijst met kolommen voor de eigenschappen van de gebruiker](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Activiteiten binnen een pijplijn opnieuw uitvoeren

U kunt nu activiteiten binnen een pijplijn opnieuw. Klik op **uitvoeringen van activiteit weergeven** en selecteer de activiteit in de pijplijn van het moment waarop u wilt uw pijplijn opnieuw uitvoeren.

![Uitvoeringen van activiteit bekijken](media/monitor-visually/rerun-activities-image1.png)

![Selecteer een activiteit die wordt uitgevoerd](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Geschiedenis weergeven opnieuw uitvoeren

U kunt de opnieuw uitvoeren geschiedenis weergeven voor alle de pijplijn wordt uitgevoerd in de lijstweergave.

![Geschiedenis weergeven](media/monitor-visually/rerun-history-image1.png)

U kunt ook geschiedenis weergeven opnieuw uitvoeren voor een specifieke pijplijnuitvoering.

![Geschiedenis weergeven voor een pijplijnuitvoering](media/monitor-visually/rerun-history-image2.png)

## <a name="guided-tours"></a>Rondleidingen
Klik op het 'informatiepictogram' linksonder in en klik op rondleidingen begeleide voor stapsgewijze instructies over het bewaken van de uitvoering van uw pijplijn en activiteit.

![Rondleidingen](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Feedback
Klik op het pictogram 'Feedback' om ons feedback over verschillende functies of eventuele problemen die u kunt te maken krijgen.

![Feedback](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Waarschuwingen

U kunt waarschuwingen op ondersteunde metrische gegevens in Data Factory kunt verhogen. Selecteer **Monitor -> waarschuwingen en metrische gegevens** op de pagina Data Factory Monitor aan de slag.

![](media/monitor-visually/alerts01.png)

Bekijk de volgende video voor een inleiding van 7 minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Waarschuwingen maken

1.  Klik op **nieuwe waarschuwingsregel** om een nieuwe waarschuwing te maken.

    ![](media/monitor-visually/alerts02.png)

1.  Geef de regelnaam en selecteer de waarschuwing **ernst**.

    ![](media/monitor-visually/alerts03.png)

1.  Selecteer de waarschuwingscriteria.

    ![](media/monitor-visually/alerts04.png)

    ![](media/monitor-visually/alerts05.png)

1.  Alert logic configureren. U kunt een waarschuwing voor de geselecteerde metrische gegevens voor alle bijbehorende activiteiten en pijplijnen kunt maken. U kunt ook een bepaalde activiteit-type, naam van de activiteit, de pijplijnnaam van de of een fouttype selecteren.

    ![](media/monitor-visually/alerts06.png)

1.  Configureer **e-mailadres/SMS/Push/stem** meldingen voor de waarschuwing. Maak of kies een bestaande **actiegroep** voor de meldingen van waarschuwingen.

    ![](media/monitor-visually/alerts07.png)

    ![](media/monitor-visually/alerts08.png)

1.  De waarschuwingsregel maken.

    ![](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Volgende stappen

Zie [bewaken en beheren van pijplijnen programmatisch](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) artikel voor meer informatie over het controleren en beheren van pijplijnen.
