---
title: Azure data factory's visueel te bewaken | Microsoft Docs
description: Informatie over het Azure Data Factory's visueel te bewaken
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: shlo
ms.openlocfilehash: 60d7897caebcb138180d99358ec754c1b9594fe5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="visually-monitor-azure-data-factories"></a>Azure data factory's visueel te bewaken
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken.
In deze snel starten leert u visueel data factory-v2-pijplijnen bewaken zonder een één regel code te schrijven.
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [bewaken en beheren van pijplijnen in Data Factory version1 gedefinieerd](v1/data-factory-monitor-manage-app.md).

## <a name="monitor-data-factory-v2-pipelines"></a>Data factory-v2-pijplijnen bewaken

1. Start **Microsoft Edge** of **Google Chrome** webbrowser. Op dit moment wordt Data Factory-gebruikersinterface alleen ondersteund in Microsoft Edge en Google Chrome webbrowsers.
2. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
3. Navigeer naar de gemaakte data factory-blade in Azure-portal en klik op de tegel 'Monitor & beheren'. Hierdoor wordt de ADF v2 visual bewakingservaring gestart.

## <a name="list-view-monitoring"></a>Lijstweergave bewaking

De pijplijn bewaken en de activiteit wordt uitgevoerd met een eenvoudige lijst weergave-interface. Alle uitvoeringen worden weergegeven in de lokale tijdzone van de browser. U kunt de tijdzone wijzigen en alle datum-/tijdvelden springen naar de geselecteerde tijdzone.  

#### <a name="monitoring-pipeline-runs"></a>Bewaking van de pijplijn wordt uitgevoerd
Lijstweergave geeft elke pijplijnuitvoering voor uw v2-pijplijnen van uw gegevensfactory duidelijk weer. Opgenomen kolommen:

| **Kolomnaam** | **Beschrijving** |
| --- | --- |
| De naam van de pijplijn | Naam van de pijplijn. |
| Acties | Één actie beschikbaar is voor het weergeven van de activiteit wordt uitgevoerd. |
| Begin uitvoeren | Uitvoering starten met pipeline datum / tijd (MM/DD/JJJJ uu: mm: SS AM/PM) |
| Duur | Uitvoeren van de duur (: mm: SS) |
| Geactiveerd door | Handmatige worden geactiveerd, planning trigger |
| Status | Is mislukt, is voltooid, wordt uitgevoerd |
| Parameters | Pijplijn parameters (naam, waarde-paren) uitvoeren |
| Fout | Pipeline-fout (indien/van toepassing) uitvoeren |
| Run-id | Id van de pijplijn uitvoeren |

![Pijplijnuitvoeringen controleren](media/monitor-visually/pipeline-runs.png)

#### <a name="monitoring-activity-runs"></a>Bewaking van de activiteit wordt uitgevoerd
Lijstweergave geeft activiteituitvoeringen weer die corresponderen met elke pijplijnuitvoering. Klik op **'Activiteit wordt uitgevoerd'** pictogram onder de **'Acties'** kolom om weer te geven van de activiteit wordt uitgevoerd voor elke pijplijn uitvoeren. Opgenomen kolommen:

| **Kolomnaam** | **Beschrijving** |
| --- | --- |
| Naam activiteit | De naam van de activiteit in de pijplijn. |
| Type activiteit | Type van de activiteit dat wil zeggen kopiëren, HDInsightSpark, HDInsightHive enzovoort. |
| Begin uitvoeren | Activiteit die wordt uitgevoerd, start datum / tijd (MM/DD/JJJJ uu: mm: SS AM/PM) |
| Duur | Uitvoeren van de duur (: mm: SS) |
| Status | Is mislukt, is voltooid, wordt uitgevoerd |
| Invoer | JSON-matrix met een beschrijving van de activiteitinvoer |
| Uitvoer | JSON-matrix met een beschrijving van de uitvoer voor activiteiten |
| Fout | Activiteit die wordt uitgevoerd fout (indien/van toepassing) |

![Uitvoering van activiteiten controleren](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> U moet op **'Vernieuwen'** pictogram bovenaan de lijst van de pijplijn en activiteiten wordt uitgevoerd te vernieuwen. Automatisch vernieuwen wordt momenteel niet ondersteund.
>

![Vernieuwen](media/monitor-visually/refresh.png)

## <a name="features"></a>Functies

#### <a name="rich-ordering-and-filtering"></a>Uitgebreid op volgorde plaatsen en filteren

Volgorde-pijplijn wordt uitgevoerd in desc/asc door uitvoeren starten en filter pipeline wordt door de volgende kolommen:

| **Kolomnaam** | **Beschrijving** |
| --- | --- |
| De naam van de pijplijn | Naam van de pijplijn. Opties zijn snelle filters voor de afgelopen 24 uur 'Afgelopen week', 'afgelopen 30 dagen' of selecteert u een aangepaste datum-tijd. |
| Begin uitvoeren | Pijplijn uitvoeren start datum / tijd |
| Uitvoeringsstatus | Filter wordt uitgevoerd door de status dat wil zeggen geslaagd, mislukt, wordt uitgevoerd |

![Filteren](media/monitor-visually/filter.png)

#### <a name="addremove-columns-to-list-view"></a>Kolommen toevoegen aan/verwijderen uit lijstweergave
Klik met de rechtermuisknop op de koptekst van de weergave lijst en kies kolommen die u wilt weergeven in de lijstweergave

![Kolommen](media/monitor-visually/columns.png)

#### <a name="reorder-column-widths-in-list-view"></a>De kolombreedte in de lijstweergave rangschikken
Verhogen en de kolombreedte in de lijstweergave verlagen door de gewoon muiswijzer op de kolomkop

#### <a name="select-data-factory"></a>Data factory selecteren
Beweeg de muisaanwijzer op het pictogram 'Data Factory' linksboven op. Klik op het pictogram 'Pijl' voor een overzicht van azure-abonnementen en gegevens factory's die u kunt bewaken.

![Data factory selecteren](media/monitor-visually/select-datafactory.png)

#### <a name="guided-tours"></a>Rondleidingen
Klik op het 'informatiepictogram' linksonder in en klik op 'Excursies' voor stapsgewijze instructies over het bewaken van de pijplijn en activiteiten wordt uitgevoerd.

![Rondleidingen](media/monitor-visually/guided-tours.png)

#### <a name="feedback"></a>Feedback
Klik op het pictogram 'Feedback' Geef ons feedback over de verschillende functies of eventuele problemen die kunnen worden aangesloten.

![Feedback](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Volgende stappen

Zie [bewaken en beheren van pijplijnen programmatisch](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) artikel voor meer informatie over het controleren en beheren van pijplijnen
