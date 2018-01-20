---
title: Archiveren van Azure bewakingsgegevens | Microsoft Docs
description: Het logboek en meetwaarde op die gegevens archiveren geproduceerd in Azure met een opslagaccount.
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: a3ab4713861d4d9681ad2ac5f084255fc29462ce
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="archive-azure-monitoring-data"></a>Archief Azure bewakingsgegevens

Verschillende lagen van uw Azure-omgeving produceren logboek en metrische gegevens die kunnen worden gearchiveerd naar een Azure Storage-Account. U kunt dit doen om een geschiedenis van het bewaken van gegevens gedurende een bepaalde periode in een goedkope, niet-doorzoekbare archief nadat die gegevens de bewaarperiode in Log Analytics of Azure-Monitor is verstreken behouden. Deze zelfstudie stappen aan de hand van de configuratie van uw Azure-omgeving voor het archiveren van gegevens naar een opslagaccount.

> [!div class="checklist"]
> * Een opslagaccount voor het opslaan van bewakingsgegevens maken
> * Abonnement logboeken te routeren 
> * Route bron van gegevens 
> * Route voor virtuele machine (Gast OS) van gegevens 
> * De bewakingsgegevens weergeven in het 
> * Uw resources opschonen 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Een opslagaccount maken

U moet eerst een opslagaccount waarvoor de bewakingsgegevens worden gearchiveerd instellen. Hiervoor [Volg de stappen die hier](../storage/common/storage-create-storage-account.md).

## <a name="route-subscription-logs-to-the-storage-account"></a>Route abonnement logboeken naar de storage-account

U bent nu klaar om te beginnen met het instellen van uw Azure-omgeving voor het routeren van bewakingsgegevens naar een opslagaccount. We eerst configureren abonnementsniveau gegevens (in de Azure Activity Log) om te worden doorgestuurd naar het opslagaccount. De [ **Azure Activity Log** ](monitoring-overview-activity-logs.md) biedt een geschiedenis van gebeurtenissen in de Azure-abonnement. U kunt het bladeren in de Azure portal om te bepalen *die* gemaakt, bijgewerkt of verwijderd *wat* bronnen en *wanneer* ze dit hebben gedaan.

1. Klik op de **Monitor** knop gevonden op de linkernavigatiebalk-lijst, klikt u vervolgens op **activiteitenlogboek**.

   ![Uplogboekgedeelte van de activiteit](media/monitor-tutorial-archive-monitoring-data/activity-log-home.png)

2. Klik in de sectie activiteitenlogboek die wordt weergegeven op de **exporteren** knop.

3. In de **Export activiteitenlogboek** sectie die wordt weergegeven, schakel het selectievakje voor **exporteren naar een opslagaccount** en klik op **Selecteer een opslagaccount.**

   ![Geëxporteerde activiteitenlogboeken](media/monitor-tutorial-archive-monitoring-data/activity-log-export.png)

4. In de sectie die wordt weergegeven, gebruikt de **opslagaccount** vervolgkeuzelijst om de naam van het opslagaccount dat u hebt gemaakt in de voorgaande te selecteren **een opslagaccount maken** stap en klik vervolgens op **OK**.

   ![Kies een opslagaccount](media/monitor-tutorial-archive-monitoring-data/activity-log-storage.png)

5. Stel de **bewaartermijn (dagen)** schuifregelaar tot en met 30. Met deze schuifregelaar Hiermee stelt u een aantal dagen wilt bewaren van de bewakingsgegevens in het opslagaccount. Azure Monitor wordt automatisch verwijderd voor gegevens die ouder zijn dan het aantal dagen opgegeven. Een bewaartermijn van nul dagen worden de gegevens voor onbepaalde tijd opgeslagen.

6. Klik op **opslaan** en sluit u deze sectie.

Het bewaken van gegevens uit uw abonnement is nu die binnenkomen in het opslagaccount.

## <a name="route-resource-data-to-the-storage-account"></a>Gegevens van de bron routeren naar het storage-account

Nu we niveau van de resource-gegevens (metrische gegevens voor resources en diagnostische logboeken) worden doorgestuurd naar het opslagaccount door in te stellen configureren **diagnostische broninstellingen**.

1. Klik op de **Monitor** knop gevonden op de linkernavigatiebalk-lijst, klikt u vervolgens op **diagnostische instellingen**. Hier kunt u een lijst van alle resources in uw abonnement bewakingsgegevens via Azure Monitor produceren bekijken. Als u geen resources in deze lijst, kunt u [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) voordat u dat u een resource die u kunt een diagnostische instelling configureren hebt op.

2. Klik op een resource in de lijst en klik vervolgens op **diagnostische gegevens inschakelen**.
   
   ![Diagnostische gegevens inschakelen](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-turn-on.png)

   Als al een instelling geconfigureerd is, in plaats daarvan ziet u de bestaande instellingen en een knop **diagnostische instelling toevoegen**. Op deze knop klikt.

   De diagnostische instelling van een bron is een definitie van *wat* bewakingsgegevens van een bepaalde bron moet worden gestuurd en *waar* bewakingsgegevens moet gaan.

3. Geef de instelling in de sectie een **naam** en schakel het selectievakje voor **archiveren naar een opslagaccount**.

   ![Diagnostische instellingen sectie](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-home.png)

4. Klik op de **configureren** knop onder **archiveren naar een opslagaccount** en selecteer het opslagaccount dat u in de voorgaande sectie hebt gemaakt. Klik op **OK**.

   ![Diagnostische instellingen voor storage-account](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-storage.png)

5. Schakel de selectievakjes onder **logboek** en **metriek**. Afhankelijk van het resourcetype mogelijk alleen hebt u een van deze opties. Deze selectievakjes bepalen welke categorieën van logboek- en meetwaarde op die gegevens beschikbaar voor resourcetype worden verzonden naar de bestemming die u hebt geselecteerd, in dit geval een opslagaccount.

   ![Diagnostische instellingen voor categorieën](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-categories.png)
   
6. Stel de **bewaartermijn (dagen)** schuifregelaar tot en met 30. Met deze schuifregelaar Hiermee stelt u een aantal dagen wilt bewaren van de bewakingsgegevens in het opslagaccount. Azure Monitor wordt automatisch verwijderd voor gegevens die ouder zijn dan het aantal dagen opgegeven. Een bewaartermijn van nul dagen worden de gegevens voor onbepaalde tijd opgeslagen.

7. Klik op **Opslaan**.

Het bewaken van gegevens van de resource is nu die binnenkomen in het opslagaccount.

## <a name="route-virtual-machine-guest-os-data-to-the-storage-account"></a>Gegevens van de virtuele machine (Gast OS) routeren naar het storage-account

1. Als u nog geen een virtuele machine in uw abonnement [maken van een virtuele machine](../virtual-machines/windows/quick-create-portal.md).

2. Klik in de lijst links navigatie in de portal op **virtuele Machines**.

3. Klik in de lijst met virtuele machines die wordt weergegeven, op de virtuele machine die u hebt gemaakt.

4. Klik in de volgende sectie op **diagnostische instellingen** op de linkernavigatiebalk. Deze sectie kunt u voor het instellen van de controle out-of-box-extensie van de Azure-Monitor op uw virtuele machine en de gegevens routeren naar een opslagaccount wordt geproduceerd door Windows of Linux.

   ![Navigeer naar de diagnostische instellingen](media/monitor-tutorial-archive-monitoring-data/guest-navigation.png)

5. Klik op **gastniveau bewaking inschakelen** in de sectie die wordt weergegeven.

   ![Schakel de diagnostische instellingen](media/monitor-tutorial-archive-monitoring-data/guest-enable.png)

6. Wanneer de diagnostische instelling correct is opgeslagen, de **overzicht** tabblad bevat een overzicht van de gegevens worden verzameld en waar deze wordt opgeslagen. Klik op de **prestatiemeteritems** sectie om te controleren van de set met Windows performance prestatiemeteritems worden verzameld.

   ![Instellingen van de prestatiemeteritems](media/monitor-tutorial-archive-monitoring-data/guest-perf-counters.png)
   
7. Klik op de **logboeken** tabblad en schakel de selectievakjes in voor **informatie** niveau toepassing aanmeldt en wordt vastgelegd.

   ![Instellingen voor logboeken](media/monitor-tutorial-archive-monitoring-data/guest-logs.png)

8. Klik op de **Agent** tabblad en klikt u onder **opslagaccount** Klik op de naam van het opslagaccount wordt weergegeven.

   ![Bijwerken van opslagaccount](media/monitor-tutorial-archive-monitoring-data/guest-storage-home.png)

9. Kies in de sectie het opslagaccount dat u hebt gemaakt in de voorgaande **een opslagaccount maken** stap.

10. Klik op **Opslaan**.

Het bewaken van gegevens van uw virtuele machines is nu die binnenkomen in het opslagaccount.

## <a name="view-the-monitoring-data-in-the-storage-account"></a>De bewakingsgegevens weergeven in de storage-account

Als u de voorgaande stappen hebt gevolgd, is gegevens begonnen met het tot uw storage-account.

1. Voor bepaalde gegevenstypen, bijvoorbeeld het activiteitenlogboek, moet er een activiteit die wordt een gebeurtenis gegenereerd in het opslagaccount. Volg voor het genereren van activiteit in het activiteitenlogboek, [deze instructies](./monitor-quick-audit-notify-action-in-subscription.md). Mogelijk moet u wachten tot vijf minuten duren voordat de gebeurtenis wordt weergegeven in het opslagaccount.

2. Navigeer in de portal naar de **Opslagaccounts** sectie door vinden op de linkernavigatiebalk.

3. Identificeren van het opslagaccount dat u in de voorgaande sectie hebt gemaakt en klikt u op.

4. Klik op **Blobs**, vervolgens op de container met het label **insights operationele logboeken** en ten slotte op de container met het label **name = standaard**. Dit is de container met het activiteitenlogboek. Het bewaken van gegevens is onderverdeeld in containers op bron-ID (alleen de abonnement-ID voor het logboek) en klik vervolgens op datum en tijd. De volledige notatie voor deze BLOB's is:

   Insights-operationele-logboeken/name = standaard/resourceId = / ABONNEMENTEN / {abonnements-ID} / y = {4-cijferige numerieke year} / m = {jaartallen met twee numerieke month} / d = {jaartallen met twee numerieke dag} /h = {jaartallen met twee 24-uurs klok hour}/m=00/PT1H.json

5. Ga naar het bestand PT1H.json door te klikken in de containers voor de resource-ID, datum en tijd. Klik op het bestand PT1H.json en op **downloaden**. Elke blob PT1H.json bevat een JSON-blob van gebeurtenissen die hebben plaatsgevonden binnen het uur die is opgegeven in de blob-URL (bijvoorbeeld, h = 12). Tijdens het huidige uur gebeurtenissen naar het bestand PT1H.json toegevoegd wanneer deze zich voordoen. De minuut waarde (m = 00) is altijd 00, omdat de logboekgebeurtenissen worden onderverdeeld in afzonderlijke blobs per uur.

   U kunt nu de JSON-gebeurtenis die is opgeslagen in de storage-account weergeven. Voor diagnostische logboeken bron is de notatie voor de blobs:

   Insights - logs-{categorie logboeknaam} / resourceId = / {resource ID} / y = {4-cijferige numerieke year} / m = {jaartallen met twee numerieke month} / d = {jaartallen met twee numerieke dag} /h = {jaartallen met twee 24-uurs klok hour}/m=00/PT1H.json

6. Gastbesturingssysteem bewakingsgegevens worden opgeslagen in tabellen. Ga terug naar de oorspronkelijke storage-account en klik op **tabellen**. Er zijn tabellen voor metrische gegevens, prestatiemeteritems en gebeurtenislogboeken.

U hebt nu ingesteld het bewaken van gegevens naar een opslagaccount worden gearchiveerd.

## <a name="clean-up-resources"></a>Resources opschonen

1. Ga terug naar de **activiteitenlogboek exporteren** sectie van de voorgaande **abonnement logboeken doorsturen naar het opslagaccount** stap en klik op **opnieuw**.

2. Navigeer naar de **diagnostische instellingen** sectie, klikt u op de resource waarop u een diagnostische instelling in de voorgaande gemaakt **brongegevens doorsturen naar het opslagaccount** stap en vervolgens de instelling vinden u gemaakte, klikt u op de **bewerkt u de instelling** en klik op **verwijderen**.

3. Navigeer naar de **diagnostische instellingen** sectie op de virtuele machine die u hebt geconfigureerd in de voorgaande **doorsturen van gegevens van de virtuele machine (Gast OS) naar het opslagaccount** stap en klikt u onder de  **Agent** tabblad Klik **verwijderen** (onder de **agent verwijderen van Azure Diagnostics** sectie).

4. Navigeer naar het opslagaccount dat u hebt gemaakt in de voorgaande **een opslagaccount maken** stap en klik op **opslagaccount verwijderen**. Typ de naam van het storage-account en klik vervolgens op **verwijderen**.

5. Als u een virtuele machine of de logische App voor de voorgaande stappen hebt gemaakt, moet u die ook verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u gegevens van uw Azure-omgeving (abonnement, resource en gastbesturingssysteem) naar een opslagaccount worden gearchiveerd bewaking hebt ingesteld. 


> [!div class="checklist"]
> * Een opslagaccount voor het opslaan van bewakingsgegevens maken
> * Abonnement logboeken te routeren 
> * Route bron van gegevens 
> * Route voor virtuele machine (Gast OS) van gegevens 
> * De bewakingsgegevens weergeven in het 
> * Uw resources opschonen 

Om meer uit uw gegevens en afgeleid van extra inzichten, moet u ook uw gegevens verzenden in logboekanalyse.

> [!div class="nextstepaction"]
> [Aan de slag met Log Analytics](../log-analytics/log-analytics-get-started.md)
