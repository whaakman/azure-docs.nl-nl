---
title: Verzamel gegevens van de lokale Windows-computers met Azure Log Analytics | Microsoft Docs
description: Informatie over het implementeren van de agent Log Analytics voor Windows wordt uitgevoerd op computers buiten Azure en schakel het verzamelen van gegevens met logboekanalyse.
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
ms.date: 10/16/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 6da36184baee921c828b037e1337df2d14c79462
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-windows-computers-hosted-in-your-environment"></a>Verzamel gegevens van Windows-computers die worden gehost in uw omgeving
[Azure Log Analytics](log-analytics-overview.md) kunt verzamelen van gegevens rechtstreeks vanuit uw fysieke of virtuele Windows-computers en andere bronnen in uw omgeving op één opslagplaats voor gedetailleerde analyse- en correlatie.  Deze snelstartgids wordt beschreven hoe u configureert en verzamelen gegevens van uw Windows-computer met een paar eenvoudige stappen.  Zie het volgende onderwerp voor de virtuele Windows Azure-machines, [verzamelen van gegevens over Azure Virtual Machines](log-analytics-quick-collect-azurevm.md).  
 
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klik op **maken**, en selecteer vervolgens de opties voor de volgende items:

  * Geef een naam op voor de nieuwe **OMS-werkruimte**, zoals *DefaultLAWorkspace*. 
  * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
  * Voor **resourcegroep**, selecteer een bestaande resourcegroep die een of meer virtuele machines in Azure bevat.  
  * Selecteer de **locatie** op uw virtuele machines worden geïmplementeerd.  Zie voor aanvullende informatie die [Log Analytics is beschikbaar in regio's](https://azure.microsoft.com/regions/services/).
  * U kunt kiezen uit drie verschillende **Prijscategorieën** in Log Analytics, maar voor deze snelstartgids die u gaat selecteren de **gratis** laag.  Zie voor meer informatie over de specifieke lagen, [Log Analytics prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Na het opgeven van de vereiste informatie op de **OMS-werkruimte** deelvenster, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="obtain-workspace-id-and-key"></a>Werkruimte-ID en sleutel ophalen
Voordat u de Microsoft Monitoring Agent voor Windows installeert, moet u de werkruimte-ID en de sleutel voor uw werkruimte voor logboekanalyse.  Deze informatie is vereist door de wizard setup correct configureren van de agent en ervoor zorgen dat deze kan communiceren met logboekanalyse.  

1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten *DefaultLAWorkspace* eerder hebt gemaakt.
3. Selecteer **geavanceerde instellingen**.<br><br> ![Log Analytics geavanceerde instellingen](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **verbonden bronnen**, en selecteer vervolgens **Windows-Servers**.   
5. De waarde rechts van **werkruimte-ID** en **primaire sleutel**. Kopieer en plak beide in uw favoriete editor.   

## <a name="install-the-agent-for-windows"></a>Installeer de agent voor Windows
De volgende stappen installeren en configureren van de agent voor logboekanalyse in Azure en Azure Government cloud met behulp van de setup van Microsoft Monitoring Agent op uw computer.  

1. Op de **Windows-Servers** pagina, selecteer de relevante **Windows-Agent downloaden** versie te downloaden, afhankelijk van de processorarchitectuur van de Windows-besturingssysteem.
2. Voer de installatie voor de installatie van de agent op uw computer.
2. Op de **Welkom** pagina, klikt u op **volgende**.
3. Op de **licentievoorwaarden** pagina, lees de licentievoorwaarden en klik vervolgens op **ik ga akkoord**.
4. Op de **doelmap** pagina, wijzigen of de standaardinstallatiemap houden en klik vervolgens op **volgende**.
5. Op de **installatieopties voor Agent** pagina, kiest u de agent verbinden met naar Azure logboekanalyse (OMS) en klik vervolgens op **volgende**.   
6. Op de **Azure Log Analytics** pagina, voert u het volgende:
   1. Plak de **werkruimte-ID** en **Werkruimtesleutel (primaire sleutel)** die u eerder hebt gekopieerd.  Als de computer een werkruimte voor logboekanalyse in Azure Government cloud rapporteren moet, selecteert u **Azure US Government** van de **Azure-Cloud** vervolgkeuzelijst.  
   2. Als de computer communiceren met de Log Analytics-service via een proxyserver moet, klikt u op **Geavanceerd** en geef de URL en het poortnummer van de proxyserver.  Als de proxyserver verificatie vereist, typ de gebruikersnaam en wachtwoord voor verificatie met de proxyserver en klik vervolgens op **volgende**.  
7. Klik op **volgende** als u klaar bent met het leveren van de vereiste configuratie-instellingen.<br><br> ![Werkruimte-ID en Primary Key plakken](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Op de **gereed voor installatie** pagina, Controleer uw selecties en klik vervolgens op **installeren**.
9. Op de **configuratie voltooid** pagina, klikt u op **voltooien**.

Na voltooiing wordt de **Microsoft Monitoring Agent** wordt weergegeven in **Configuratiescherm**. U kunt uw configuratie bekijken en controleren of de agent is verbonden met logboekanalyse. Wanneer verbonden, op de **Azure logboekanalyse (OMS)** tabblad, de agent geeft een bericht weergegeven: **de Microsoft Monitoring Agent verbonden is met de Microsoft Operations Management Suite-service.**<br><br> ![De verbindingsstatus MMA met Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Gebeurtenis-en prestatiegegevens verzamelen
Log Analytics kunt verzamelen van gebeurtenissen van de Windows-gebeurtenislogboek en prestatiemeteritems die u opgeeft voor langere termijn analyse en rapportage en actie ondernemen wanneer een bepaalde voorwaarde wordt gedetecteerd.  Volg deze stappen voor het verzamelen van gebeurtenissen uit de Windows-gebeurtenislogboek en algemene prestatiemeters voor verschillende beginnen met configureren.  

1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
2. Selecteer **geavanceerde instellingen**.<br><br> ![Log Analytics geavanceerde instellingen](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br> 
3. Selecteer **gegevens**, en selecteer vervolgens **Windows-gebeurtenislogboeken**.  
4. U kunt een gebeurtenislogboek toevoegen door te typen in de naam van het logboek.  Type **System** en klik vervolgens op het plusteken  **+** .  
5. Schakel in de tabel de ernst **fout** en **waarschuwing**.   
6. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.
7. Selecteer **Windows-prestatiegegevens** voor het verzamelen van prestatiemeteritems op een Windows-computer. 
8. Wanneer u Windows-prestatiemeteritems voor een nieuwe werkruimte voor logboekanalyse voor het eerst configureert, krijgt u de optie snel maken verschillende algemene prestatiemeteritems. Ze worden weergegeven met een selectievakje in naast elke.<br> ![Standaard Windows-prestatiemeteritems geselecteerd](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Klik op **de geselecteerde prestatiemeteritems toevoegen**.  Ze zijn toegevoegd en voorinstelling met een tweede tien verzameling controle-interval.  
9. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.

## <a name="view-data-collected"></a>Gegevens weergeven die worden verzameld
Nu dat u gegevensverzameling hebt ingeschakeld, kunt een voorbeeld van een eenvoudige logboek zoekopdracht om te zien van sommige gegevens van de doelcomputer wordt uitgevoerd.  

1. Klik in de Azure-portal onder de geselecteerde werkruimte op het **logboek zoeken** tegel.  
2. Klik in het deelvenster logboek zoeken in het veld querytype `Perf` en vervolgens drukt Voer of klik op de zoekknop aan de rechterkant van de queryveld.<br><br> ![Meld u logboekanalyse zoeken query voorbeeld](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Bijvoorbeeld: de query in de volgende afbeelding 735 prestaties records geretourneerd.<br><br> ![Log Analytics zoekresultaat melden](media/log-analytics-quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, kunt u de agent verwijderen uit de Windows-computer en de werkruimte voor logboekanalyse verwijderen.  

Als u wilt verwijderen van de agent, moet u de volgende stappen uitvoeren.

1. Open het **Configuratiescherm**.
2. Open **programma's en onderdelen**.
3. In **programma's en onderdelen**, selecteer **Microsoft Monitoring Agent** en klik op **verwijderen**.

Selecteer de werkruimte voor logboekanalyse u eerder en klik vervolgens op pagina resource gemaakt voor het verwijderen van de werkruimte **verwijderen**.<br><br> ![Log Analytics-bron verwijderen](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u operationeel verzamelt en prestatiegegevens van uw on-premises Linux-computer, u eenvoudig beginnen kunt verkennen, analyseren en nemen actie op gegevens die u verzamelt voor *gratis*.  

Voor informatie over het weergeven en analyseren van de gegevens gaat u verder met de zelfstudie.   

> [!div class="nextstepaction"]
> [Weergeven of gegevens analyseren in Log Analytics](log-analytics-tutorial-viewdata.md)
