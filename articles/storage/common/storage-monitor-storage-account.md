---
title: Een Azure Storage-account controleren | Microsoft Docs
description: Informatie over het bewaken van een opslagaccount in Azure met behulp van de Azure-portal.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/31/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: d437c6e2327fc9771f9567a37bfbf8ad82713b2b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452640"
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Een opslagaccount in Azure portal controleren

[Azure Storage Analytics](../storage-analytics.md) biedt metrische gegevens voor alle opslagservices en logboeken voor blobs, wachtrijen en tabellen. U kunt de [Azure-portal](https://portal.azure.com) configureren welke metrische gegevens en logboeken voor uw account worden vastgelegd en grafieken die visuele weergaven van uw metrische gegevens bieden configureren.

> [!NOTE]
> Er worden kosten in verband met het onderzoeken van bewakingsgegevens in de Azure-portal. Zie voor meer informatie, [Storage Analytics en facturering](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Azure Files op dit moment biedt ondersteuning voor Storage Analytics metrics, maar biedt logboekregistratie nog geen ondersteuning.
> 
> Zie voor een uitgebreide handleiding over het gebruik van Storage Analytics en andere hulpprogramma's om te identificeren, onderzoeken en oplossen van problemen met betrekking tot Azure Storage, [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>De bewaking van een storage-account configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer **opslagaccounts**, klikt u vervolgens de storage-accountnaam om de accountdashboard te openen.
1. Selecteer **Diagnostics** in de **bewaking** sectie van de menu-blade.

    ![MonitoringOptions](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)

1. Selecteer de **type** van metrische gegevens voor elke **service** u wilt bewaken, en de **bewaarbeleid** voor de gegevens. U kunt ook uitschakelen voor bewaking door in te stellen **Status** naar **uit**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/storage-enable-metrics-01.png)

   Om in te stellen het bewaarbeleid voor gegevens, gaan de **bewaarperiode (dagen)** schuifregelaar of Voer het aantal dagen aan gegevens te behouden tussen 1 en 365. De standaardwaarde voor de nieuwe storage-accounts is zeven dagen. Als u niet een retentiebeleid instellen wilt, voert u nul. Als er geen bewaarbeleid is, wordt het is aan u de bewakingsgegevens verwijderen.

   > [!WARNING]
   > U betaalt wanneer u metrische gegevens voor het handmatig verwijderen. Verouderde analytics-gegevens (gegevens die ouder zijn dan het bewaarbeleid) is verwijderd door het systeem zonder kosten. Het is raadzaam om een bewaarbeleid dat is gebaseerd op hoe lang u wilt behouden van storage analytics-gegevens voor uw account instellen. Zie [welke kosten moet u rekenen wanneer u metrische opslaggegevens inschakelen?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) voor meer informatie.
   >

1. Wanneer u klaar bent met de configuratie van de bewaking, selecteert u **opslaan**.

Een standaardset van metrische gegevens wordt weergegeven in de grafieken op de blade opslagaccount, evenals de blades van afzonderlijke service (blob, wachtrij, tabel en bestand). Nadat u de metrische gegevens voor een service hebt ingeschakeld, het kan een uur duren voor de gegevens worden weergegeven in de grafieken. U kunt selecteren **bewerken** op een grafiek met metrische gegevens voor [configureren welke metrische gegevens](#how-to-customize-metrics-charts) worden weergegeven in de grafiek.

U kunt metrische gegevens verzamelen en logboekregistratie uitschakelen door in te stellen **Status** naar **uit**.

> [!NOTE]
> Azure Storage maakt gebruik van [tabelopslag](../common/storage-introduction.md#table-storage) voor het opslaan van de metrische gegevens voor uw opslagaccount en slaat de metrische gegevens in tabellen in uw account. Zie voor meer informatie. [Hoe metrische gegevens worden opgeslagen](../common/storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Grafieken met metrische gegevens aanpassen

Gebruik de volgende procedure om te kiezen welke metrische opslaggegevens om in een grafiek met metrische gegevens weer te geven. 

1. Gestart door een grafiek met metrische gegevens opslag in Azure portal weer te geven. U kunt grafieken vinden op de **blade opslagaccount** en in de **metrische gegevens** blade voor een afzonderlijke service (blob, wachtrij, tabel, -bestand).

   In dit voorbeeld gebruikt u het volgende diagram dat wordt weergegeven op de **blade opslagaccount**:

   ![Grafiekselectie in Azure portal](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Klik ergens in de grafiek aan de grafiek bewerken.

1. Selecteer vervolgens de **tijdsbereik** van de metrische gegevens om weer te geven in de grafiek en de **service** (blob, wachtrij, tabel en bestand) waarvan metrische gegevens die u wilt weergeven. Hier, zijn metrische gegevens over de afgelopen week geselecteerd om weer te geven voor de blob-service:

   ![Tijd bereik en de service-selectie in de blade van de grafiek bewerken](./media/storage-monitor-storage-account/storage-edit-metric-time-range.png)

1. Selecteer de afzonderlijke **metrische gegevens** u was zoals weergegeven in de grafiek, klikt u op **OK**.

   ![Afzonderlijke meetwaarde selecteren in de blade van de grafiek bewerken](./media/storage-monitor-storage-account/storage-edit-metric-selections.png)

De grafiekinstellingen niet van invloed op de verzameling, aggregeren of opslag van gegevens in de storage-account te controleren.

### <a name="metrics-availability-in-charts"></a>Metrische gegevens over beschikbaarheid in grafieken

De lijst met beschikbare metrische gegevens-wijzigingen op basis van welke service die u hebt gekozen in de vervolgkeuzelijst en het eenheidstype van de grafiek die u wilt bewerken. Bijvoorbeeld, kunt u percentage metrische gegevens zoals *percentnetworkerror aan* en *percentthrottlingerror aan* alleen als u een diagram waarin eenheden in een percentage bewerkt:

![Aanvraag voor fout percentage grafiek in de Azure-portal](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Metrische gegevens resolutie

De metrische gegevens die u hebt geselecteerd in **Diagnostics** bepaalt de resolutie van de metrische gegevens die beschikbaar voor uw account zijn:

* **Cumulatieve** controle biedt metrische gegevens zoals Inkomend/uitgaand verkeer, beschikbaarheid, latentie en succespercentages. Deze metrische gegevens worden geaggregeerd van de blob-, tabel-, bestands- en queue-services.
* **Per API** biedt betere oplossing met metrische gegevens beschikbaar voor afzonderlijke opslagbewerkingen, naast het serviceniveau statistische functies.

## <a name="configure-metrics-alerts"></a>Waarschuwingen voor metrische gegevens configureren

U kunt waarschuwingen om u te waarschuwen wanneer drempels is bereikt voor metrische gegevens voor storage-resources maken.

1. Om te openen de **waarschuwingsregels blade**, schuif omlaag naar de **bewaking** sectie van de **Menu-blade** en selecteer **waarschuwingen (klassiek)**.
2. Selecteer **metrische waarschuwing toevoegen (klassiek)** openen de **een waarschuwingsregel toevoegen** blade
3. Voer een **naam** en **beschrijving** voor uw nieuwe waarschuwingsregel.
4. Selecteer de **Metric** voor die u wilt een waarschuwing, wordt een waarschuwing toevoegen **voorwaarde**, en een **drempelwaarde**. De drempelwaarde-eenheid Typ gewijzigd, afhankelijk van de metrische gegevens die u hebt gekozen. Bijvoorbeeld, "aantal" is het eenheidstype voor *ContainerCount*, terwijl de eenheid voor de *percentnetworkerror aan* meetwaarde is een percentage.
5. Selecteer de **periode**. Metrische gegevens die bereiken of de drempelwaarde overschrijden, binnen de periode van een waarschuwing geactiveerd.
6. (Optioneel) Configureer **e** en **Webhook** meldingen. Zie voor meer informatie over webhooks [een webhook configureren voor een Azure metrische waarschuwing](../../azure-monitor/platform/alerts-webhooks.md). Als u geen e-mailadres of webhook-meldingen configureert, wordt alleen in de Azure-portal waarschuwingen weergegeven.

![Blade 'Een waarschuwingsregel toevoegen' in de Azure-portal](./media/storage-monitor-storage-account/add-alert-rule.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Grafieken met metrische gegevens toevoegen aan het portaldashboard

U kunt Azure Storage grafieken met metrische gegevens voor het gebruik van uw storage-accounts toevoegen aan uw portal-dashboard.

1. Selecteer Klik **dashboard bewerken** tijdens het bekijken van uw dashboard in de [Azure-portal](https://portal.azure.com).
1. In de **Tegelgalerie**, selecteer **zoeken door tegels** > **Type**.
1. Selecteer **Type** > **opslagaccounts**.
1. In **Resources**, selecteer het opslagaccount waarvan metrische gegevens die u wilt toevoegen aan het dashboard.
1. Selecteer **categorieën** > **bewaking**.
1. Slepen en neerzetten het diagram tegel aan uw dashboard voor de metrische gegevens die u wilt weergegeven. Herhaal dit voor alle metrische gegevens u wilt dat op het dashboard weergegeven. In de volgende afbeelding, de grafiek 'Blobs - totaal aantal aanvragen' is gemarkeerd als voorbeeld, maar alle grafieken zijn beschikbaar voor plaatsing op uw dashboard.

   ![Tegelgalerie in Azure portal](./media/storage-monitor-storage-account/storage-customize-dashboard.png)
1. Selecteer **aanpassen voltooid** in de buurt van de bovenkant van het dashboard wanneer u klaar bent met grafieken toe te voegen.

Als u grafieken hebt toegevoegd aan uw dashboard, kunt u verder deze aanpassen zoals beschreven in [grafieken met metrische gegevens aanpassen](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Logboekregistratie configureren

U kunt de opdracht geven voor Azure Storage voor het opslaan van diagnostische logboeken om te lezen, schrijven en verwijderen van aanvragen voor de blob, table en queue-services. Het bewaarbeleid voor gegevens u geldt ook voor deze logboeken.

> [!NOTE]
> Azure Files op dit moment biedt ondersteuning voor Storage Analytics metrics, maar biedt logboekregistratie nog geen ondersteuning.
>

1. In de [Azure-portal](https://portal.azure.com), selecteer **opslagaccounts**, klikt u vervolgens de naam van het opslagaccount om de storage-account-blade te openen.
1. Selecteer **Diagnostics** in de **bewaking** sectie van de menu-blade.

    ![Diagnostische gegevens van menu-item onder controleren in Azure portal.](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)
    
1. Zorg ervoor dat **Status** is ingesteld op **op**, en selecteer de **services** voor die u wilt logboekregistratie inschakelen.

    ![Logboekregistratie in Azure portal configureren.](./media/storage-monitor-storage-account/enable-diagnostics.png)
1. Klik op **Opslaan**.

De logboeken met diagnostische gegevens worden opgeslagen in een blobcontainer met de naam *$logs* in uw opslagaccount. U vindt de logboekgegevens die met behulp van storage explorer, zoals de [Microsoft Opslagverkenner](http://storageexplorer.com), of programmatisch met behulp van de storage-clientbibliotheek of PowerShell.

Zie voor meer informatie over het openen van de container met $logs [vastleggen van Storage inschakelen en toegang krijgen tot logboekgegevens](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Volgende stappen

* Lees meer informatie over [metrische gegevens, logboekregistratie en facturering](../storage-analytics.md) voor Storage Analytics.
* [Azure Storage metrische gegevens en bekijk metrische gegevens inschakelen](../storage-enable-and-view-metrics.md) met behulp van PowerShell en via een programma met C#.
