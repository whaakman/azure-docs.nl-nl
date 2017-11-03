---
title: Verzamelen van gegevens over Azure Virtual Machines | Microsoft Docs
description: Informatie over het inschakelen van de VM-extensie van de OMS-Agent en inschakelen van verzamelen van gegevens van uw Azure VM's met logboekanalyse.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 2dec744b512a86a30cec1f334e265572fa7acc3e
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-about-azure-virtual-machines"></a>Verzamelt gegevens over Azure Virtual Machines
[Azure Log Analytics](log-analytics-overview.md) kunt verzamelen van gegevens rechtstreeks vanuit uw virtuele Azure-machines en andere bronnen in uw omgeving op één opslagplaats voor gedetailleerde analyse- en correlatie.  Deze snelstartgids wordt beschreven hoe u configureert en verzamelen van gegevens van uw Azure Linux- of Windows-VM's met een paar eenvoudige stappen.  
 
Deze snelstartgids wordt ervan uitgegaan dat u hebt een bestaande virtuele machine van Azure. Als u niet kunt u [maken van een virtuele machine van Windows](../virtual-machines/windows/quick-create-portal.md) of [maken van een Linux-VM](../virtual-machines/linux/quick-create-cli.md) onze snelstartgidsen VM te volgen.

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.<br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br>  
2. Klik op **maken**, en selecteer vervolgens de opties voor de volgende items:

  * Geef een naam op voor de nieuwe **OMS-werkruimte**, zoals *DefaultLAWorkspace*. 
  * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
  * Voor **resourcegroep**, selecteer een bestaande resourcegroep die een of meer virtuele machines in Azure bevat.  
  * Selecteer de **locatie** op uw virtuele machines worden geïmplementeerd.  Zie voor aanvullende informatie die [Log Analytics is beschikbaar in regio's](https://azure.microsoft.com/regions/services/).
  * U kunt kiezen uit drie verschillende **Prijscategorieën** in Log Analytics, maar voor deze snelstartgids die u gaat selecteren de **gratis** laag.  Zie voor meer informatie over de specifieke lagen, [Log Analytics prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Na het opgeven van de vereiste informatie op de **OMS-werkruimte** deelvenster, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="enable-the-log-analytics-vm-extension"></a>De Log Analytics VM-extensie inschakelen
Voor Windows en Linux virtuele machines al geïmplementeerd in Azure, kunt u de Log Analytics-agent installeren met de Log Analytics VM-extensie.  Met de extensie, vereenvoudigt u het installatieproces en configureert automatisch de agent voor het verzenden van gegevens naar de werkruimte voor logboekanalyse die u opgeeft. De agent is ook automatisch bijgewerkt, hebt u de nieuwste functies en oplossingen.

De banner merkt u aan de bovenkant van uw pagina logboekanalyse resource in de portal uitnodiging om bij te werken.  De upgrade is niet nodig voor het doel van deze snelstartgids.<br>

![Log Analytics upgrade wordt aangekondigd in de Azure portal](media/log-analytics-quick-collect-azurevm/log-analytics-portal-upgradebanner.png).    
1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten *DefaultLAWorkspace* eerder hebt gemaakt.
3. Klik op het menu links onder werkruimte gegevensbronnen **virtuele machines**.  
4. In de lijst met **virtuele machines**, selecteer een virtuele machine die u wilt dat de agent te installeren op. U ziet dat de **OMS verbindingsstatus** voor de virtuele machine geeft aan dat deze **niet verbonden**.
5. Selecteer in de details voor de virtuele machine, **Connect**. De agent wordt automatisch geïnstalleerd en geconfigureerd voor uw werkruimte voor logboekanalyse. Dit proces duurt een paar minuten gedurende welke de **Status** is **verbinding maakt met**.
6. Na het installeren en de agent verbinden met de **OMS verbindingsstatus** wordt bijgewerkt met **deze werkruimte**.

## <a name="collect-event-and-performance-data"></a>Gebeurtenis-en prestatiegegevens verzamelen
Log Analytics kunt verzamelen van gebeurtenissen van de gebeurtenislogboeken van Windows of Linux Syslog en prestatiemeteritems die u opgeeft voor langere termijn analyse en rapportage en actie ondernemen wanneer een bepaalde voorwaarde wordt gedetecteerd.  Volg deze stappen voor het verzamelen van gebeurtenissen van het logboek voor Windows en Linux Syslog en algemene prestatiemeters voor verschillende beginnen met configureren.  

### <a name="data-collection-from-windows-vm"></a>Het verzamelen van de virtuele machine van Windows
1. Selecteer **geavanceerde instellingen**.<br> ![Log Analytics geavanceerde instellingen](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br> 
3. Selecteer **gegevens**, en selecteer vervolgens **Windows-gebeurtenislogboeken**.  
4. U kunt een gebeurtenislogboek toevoegen door te typen in de naam van het logboek.  Type **System** en klik vervolgens op het plusteken  **+** .  
5. Schakel in de tabel de ernst **fout** en **waarschuwing**.   
6. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.
7. Selecteer **Windows-prestatiegegevens** voor het verzamelen van prestatiemeteritems op een Windows-computer. 
8. Wanneer u Windows-prestatiemeteritems voor een nieuwe werkruimte voor logboekanalyse voor het eerst configureert, krijgt u de optie snel maken verschillende algemene prestatiemeteritems. Ze worden weergegeven met een selectievakje in naast elke.<br> ![Standaard Windows-prestatiemeteritems geselecteerd](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Klik op **de geselecteerde prestatiemeteritems toevoegen**.  Ze zijn toegevoegd en voorinstelling met een tweede tien verzameling controle-interval.  
9. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.

### <a name="data-collection-from-linux-vm"></a>Verzamelen van gegevens via Linux-VM

1. Selecteer **Syslog**.  
2. U kunt een gebeurtenislogboek toevoegen door te typen in de naam van het logboek.  Type **Syslog** en klik vervolgens op het plusteken  **+** .  
3. In de tabel, schakelt u de ernst **Info**, **kennisgeving** en **Debug**. 
4. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.
5. Selecteer **Linux prestatiegegevens** voor het verzamelen van prestatiemeteritems op een Windows-computer. 
6. Wanneer u Linux-prestatiemeteritems voor een nieuwe werkruimte voor logboekanalyse voor het eerst configureert, krijgt u de optie snel maken verschillende algemene prestatiemeteritems. Ze worden weergegeven met een selectievakje in naast elke.<br> ![Standaard Windows-prestatiemeteritems geselecteerd](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br> Klik op **de geselecteerde prestatiemeteritems toevoegen**.  Ze zijn toegevoegd en voorinstelling met een tweede tien verzameling controle-interval.  
7. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.

## <a name="view-data-collected"></a>Gegevens weergeven die worden verzameld
Nu dat u gegevensverzameling hebt ingeschakeld, kunt een voorbeeld van een eenvoudige logboek zoekopdracht om te zien van sommige gegevens van het doel-virtuele machines worden uitgevoerd.  

1. Navigeer naar Log Analytics en selecteer de werkruimte eerder hebt gemaakt in de Azure portal.
2. Klik op de **logboek zoeken** tegel en klik in het deelvenster logboek zoeken in het veld querytype `Type=Perf` en vervolgens drukt Voer of klik op de zoekknop aan de rechterkant van het queryveld.<br> ![Meld u logboekanalyse zoeken query voorbeeld](./media/log-analytics-quick-collect-azurevm/log-analytics-portal-queryexample.png)<br> Bijvoorbeeld: de query in de volgende afbeelding 78,000 prestaties records geretourneerd.  De resultaten worden aanzienlijk minder.<br> ![Log Analytics zoekresultaat melden](media/log-analytics-quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, verwijdert u de werkruimte voor logboekanalyse. Om dit te doen, selecteert u de werkruimte voor logboekanalyse u eerder en klik vervolgens op pagina resource gemaakt **verwijderen**.<br> ![Log Analytics-bron verwijderen](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u operationeel verzamelt en prestatiegegevens van uw Windows- of Linux virtuele machines, u gemakkelijk kunt begint verkennen, analyseren en nemen actie op gegevens die u verzamelt voor *gratis*.  

Voor informatie over het weergeven en analyseren van de gegevens gaat u verder met de zelfstudie.   

> [!div class="nextstepaction"]
> [Weergeven of gegevens analyseren in Log Analytics](log-analytics-tutorial-viewdata.md)
