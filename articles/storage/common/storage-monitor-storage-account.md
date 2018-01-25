---
title: Een Azure Storage-account controleren | Microsoft Docs
description: Informatie over het bewaken van een opslagaccount in Azure met behulp van de Azure-portal.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: tamram
ms.openlocfilehash: 20cb425b64bfba06d64b3c6c41f9ef496395ad8e
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Een opslagaccount in de Azure portal controleren

[Azure Storage Analytics](../storage-analytics.md) voorziet in maatstaven voor alle storage-services en logboeken voor blobs, wachtrijen en tabellen. U kunt de [Azure-portal](https://portal.azure.com) configureren welke metrische gegevens en de logboeken worden geregistreerd voor uw account en grafieken die visuele weergaven van de metrische gegevens bieden configureren.

> [!NOTE]
> Er worden kosten in verband met het onderzoeken van controlegegevens in de Azure portal. Zie voor meer informatie [Storage Analytics en facturering](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Azure Files momenteel ondersteunt Opslaganalyse metrische gegevens, maar logboekregistratie nog niet ondersteunt.
>
> Storage-accounts met een replicatietype van de Zone-redundante opslag (ZRS) ondersteuning voor metrische gegevens en logboekregistratie. De vorige ZRS-accounts die zijn gewijzigd ZRS klassieke, kunt u geen ondersteuning voor metrische gegevens logboekregistratie. Zie voor meer informatie over ZRS [Zone-redundante opslag](storage-redundancy.md#zone-redundant-storage). 
> 
> Zie voor een gedetailleerde uitleg over het gebruik van Storage Analytics en andere hulpprogramma's om te bepalen, onderzoeken en oplossen van problemen met Azure Storage met [Monitor, vaststellen en oplossen van Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Bewaking configureren voor een opslagaccount

1. In de [Azure-portal](https://portal.azure.com), selecteer **opslagaccounts**, klikt u vervolgens de opslagaccountnaam om de account-dashboard te openen.
1. Selecteer **Diagnostics** in de **bewaking** sectie van de blade menu.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Selecteer de **type** van metrische gegevens voor elke **service** u wilt bewaken, en de **bewaarbeleid** voor de gegevens. U kunt ook uitschakelen door in te stellen bewaking **Status** naar **uit**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   Er zijn twee soorten metrische gegevens die kunt u inschakelen voor elke service, die beide zijn standaard ingeschakeld voor nieuwe storage-accounts:

   * **Cumulatieve**: metrische gegevens zoals inkomend en uitgaand, beschikbaarheid, latentie en geslaagd percentages verzamelt. Deze metrische gegevens worden voor de blob, queue, table en Bestandsservices samengevoegd.
   * **Per API**: naast de cumulatieve metrische gegevens, verzamelt u dezelfde set van metrische gegevens voor elke opslagbewerking in de API van Azure Storage-service.

   Om in te stellen het bewaarbeleid voor gegevens, gaan de **bewaartermijn (dagen)** schuifregelaar of Voer het aantal dagen aan gegevens te behouden tussen 1 en 365. De standaardwaarde voor nieuwe opslagaccounts is zeven dagen. Als u niet een bewaarbeleid instellen wilt, voert u nul. Als er geen bewaarbeleid, is het tot u de bewakingsgegevens verwijderen.

   > [!WARNING]
   > U in rekening worden gebracht wanneer u metrische gegevens voor het handmatig verwijderen. Verouderde analytische gegevens (gegevens die ouder zijn dan het bewaarbeleid) is verwijderd door het systeem zonder kosten. Het is raadzaam om een bewaarbeleid voor afhankelijk van hoe lang u wilt behouden storage analytics-gegevens voor uw account instellen. Zie [welke kosten komen er gemaakt bij het inschakelen van metrische gegevens storage?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) voor meer informatie.
   >

1. Wanneer u klaar bent met de configuratie van de bewaking, selecteert u **opslaan**.

Een standaardset metrische gegevens wordt weergegeven in de grafieken op de blade opslagaccount, evenals de blades afzonderlijke service (blob, queue, table en -bestand). Nadat u metrische gegevens voor een service hebt ingeschakeld, kan deze gegevens worden weergegeven in de grafieken een uur duren. U kunt selecteren **bewerken** op een metrische grafiek voor [configureren welke metrische gegevens](#how-to-customize-metrics-charts) worden weergegeven in de grafiek.

U kunt metrische gegevens verzamelen en logboekregistratie uitschakelen door in te stellen **Status** naar **uit**.

> [!NOTE]
> Azure Storage maakt gebruik van [tabel opslag](../common/storage-introduction.md#table-storage) voor het opslaan van de metrische gegevens voor uw opslagaccount en slaat de metrische gegevens in de tabellen in uw account. Zie voor meer informatie. [Hoe metrische gegevens worden opgeslagen](../common/storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Metrische gegevens grafieken aanpassen

Gebruik de volgende procedure om te kiezen welke opslag metrische gegevens om weer te geven in een grafiek metrische gegevens. 

1. Start een metrische grafiek opslag in de Azure-portal weergegeven. U kunt grafieken vinden op de **blade opslagaccount** en in de **metrische gegevens** blade voor een afzonderlijke service (blob, queue, table, bestand).

   In dit voorbeeld werken we met het volgende diagram dat wordt weergegeven op de **blade opslagaccount**:

   ![Grafiekselectie in Azure-portal](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Klik vervolgens op een willekeurige plaats in de grafiek openen de **metriek** blade. Selecteer **grafiek bewerken** openen de **grafiek bewerken** blade.

   ![Knop grafiek op de blade grafiek bewerken](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. Op de **grafiek bewerken** blade, selecteer de **tijdsbereik** van de metrische gegevens om weer te geven in de grafiek en de **service** (blob, queue, tabel, bestand) waarvan metrische gegevens die u wilt weergeven. Er is hier voor gekozen om de afgelopen week metrische gegevens voor de blob-service weer te geven:

   ![Bereik en service tijdzone in de blade grafiek bewerken](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Selecteer de afzonderlijke **metrische gegevens** moest u zoals weergegeven in de grafiek, klikt u op **OK**. Bijvoorbeeld, hier we hebt gekozen om weer te geven de *ContainerCount* en *ObjectCount* metrische gegevens:

   ![Afzonderlijke metrische selectie in de blade grafiek bewerken](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

De grafiekinstellingen van uw hebben geen invloed op de verzameling, aggregatie of opslag van gegevens in het opslagaccount, alleen de weergave van metrische gegevens van bewaking.

### <a name="metrics-availability-in-charts"></a>Beschikbaarheid van de metrische gegevens in grafieken

De lijst met beschikbare metrische gegevens wijzigingen op basis van welke service die u hebt gekozen in de vervolgkeuzelijst en het eenheidstype van de grafiek die u wilt bewerken. Bijvoorbeeld, kunt u percentage metrische gegevens zoals *PercentNetworkError* en *PercentThrottlingError* alleen als u een diagram waarin eenheden percentage bewerkt:

![Aanvraag voor fout percentage grafiek in de Azure-portal](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Omzetting van de metrische gegevens

De metrische gegevens die u hebt geselecteerd in de Diagnostics bepaalt de resolutie van de metrische gegevens die beschikbaar voor uw account zijn:

* **Cumulatieve** voorziet in maatstaven zoals inkomend en uitgaand, beschikbaarheid, latentie en geslaagd percentages voor bewaking. Deze metrische gegevens worden van de blob, table, bestand en wachtrijservices samengevoegd.
* **Per API** biedt betere oplossing met metrische gegevens beschikbaar voor afzonderlijke opslagbewerkingen, naast de serviceniveau-statistische functies.

## <a name="configure-metrics-alerts"></a>Metrische gegevens waarschuwingen configureren

U kunt waarschuwingen om u te waarschuwen wanneer drempels hebt bereikt voor metrische gegevens van storage resource maken.

1. Openen de **waarschuwingsregels blade**, schuif omlaag naar de **bewaking** sectie van de **Menu blade** en selecteer **waarschuwing regels**.
1. Selecteer **waarschuwing toevoegen** openen de **een waarschuwingsregel toevoegen** blade
1. Selecteer een **Resource** (blob, bestand, in de wachtrij, tabel) in de vervolgkeuzelijst en voer een **naam** en **beschrijving** voor uw nieuwe waarschuwingsregel.
1. Selecteer de **metriek** voor die u wilt een waarschuwing, wordt een waarschuwing toevoegen **voorwaarde**, en een **drempelwaarde**. De eenheid drempelwaarde Typ wijzigingen, afhankelijk van de metrische gegevens die u hebt gekozen. 'Aantal' is bijvoorbeeld het eenheidstype voor *ContainerCount*, terwijl de eenheid voor de *PercentNetworkError* meetwaarde is een percentage.
1. Selecteer de **periode**. Metrische gegevens die bereikt of overschrijdt de drempelwaarde binnen een periode geeft een waarschuwing.
1. (Optioneel) Configureer **e** en **Webhook** meldingen. Zie voor meer informatie over webhooks [een webhook configureren op een Azure metrische waarschuwing](../../monitoring-and-diagnostics/insights-webhooks-alerts.md). Als u geen e-mailadres of webhook meldingen configureert, wordt alleen in de Azure portal waarschuwingen weergegeven.

![Blade 'Een waarschuwingsregel toevoegen' in de Azure portal](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Metrische gegevens grafieken toevoegen aan het portaldashboard

U kunt grafieken van Azure Storage metrische gegevens voor een van uw storage-accounts toevoegen aan uw portal-dashboard.

1. Selecteer Klik **bewerken dashboard** tijdens het bekijken van het dashboard in de [Azure-portal](https://portal.azure.com).
1. In de **tegel galerie**, selecteer **zoeken door tegels** > **Type**.
1. Selecteer **Type** > **opslagaccounts**.
1. In **Resources**, selecteer het opslagaccount waarvan metrische gegevens die u wilt toevoegen aan het dashboard.
1. Selecteer **categorieën** > **bewaking**.
1. Slepen en neerzetten van de grafiek tegel op uw dashboard voor de metrische gegevens die u wilt weergegeven. Herhaal van alle metrische gegevens die u wilt weergegeven op het dashboard. De grafiek 'Blobs - totale aanvragen' is gemarkeerd als voorbeeld in de volgende afbeelding, maar alle grafieken zijn beschikbaar voor plaatsing op uw dashboard.

   ![Tegel galerie in Azure-portal](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Selecteer **gedaan aanpassen** boven aan het dashboard wanneer u grafieken toe te voegen bent klaar.

Zodra u grafieken hebt toegevoegd aan uw dashboard, kunt u verder deze aanpassen zoals beschreven in [grafieken van de metrische gegevens aanpassen](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Logboekregistratie configureren

U kunt opgeven dat Azure Storage Sla de logboeken van de diagnostische gegevens voor lezen, schrijven en aanvragen voor de blob, table en queue-services verwijderen. Het bewaarbeleid voor gegevens u geldt ook voor deze logboeken.

> [!NOTE]
> Azure Files momenteel ondersteunt Opslaganalyse metrische gegevens, maar logboekregistratie nog niet ondersteunt.
>

1. In de [Azure-portal](https://portal.azure.com), selecteer **opslagaccounts**, klikt u vervolgens de naam van het storage-account openen van de blade opslagaccount.
1. Selecteer **Diagnostics** in de **bewaking** sectie van de blade menu.

    ![Diagnostische gegevens menu-item onder controle in de Azure portal.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Zorg ervoor dat **Status** is ingesteld op **op**, en selecteer de **services** voor die u wilt inschakelen van logboekregistratie.

    ![Logboekregistratie in de Azure portal configureren.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. Klik op **Opslaan**.

De logboeken met diagnostische gegevens worden opgeslagen in een blob-container met de naam $logs in uw opslagaccount. U kunt de logboekgegevens met een Opslagverkenner zoals weergeven de [Microsoft Opslagverkenner](http://storageexplorer.com), of programmatisch met behulp van de storage-clientbibliotheek of PowerShell.

Zie voor meer informatie over het openen van de container $logs [opslag vastleggen inschakelen en toegang tot logboekgegevens](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie vinden over [metrische gegevens en de registratie en facturering](../storage-analytics.md) voor Storage Analytics.
* [Inschakelen van Azure Storage metrische gegevens en bekijk metrische gegevens](../storage-enable-and-view-metrics.md) met behulp van PowerShell en programmatisch met C#.
