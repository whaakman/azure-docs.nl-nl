---
title: Gegevens verzamelen van lokale Linux-computers met Azure Log Analytics | Microsoft Docs
description: Informatie over het implementeren van de agent voor logboekanalyse voor Linux en verzamelen van gegevens van dat besturingssysteem met logboekanalyse inschakelen.
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
ms.date: 10/13/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: d22fe6456c3bd886f8f8863d362c0084fbe03da3
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Verzamel gegevens van Linux-computers die worden gehost in uw omgeving
[Azure Log Analytics](log-analytics-overview.md) kunt verzamelen van gegevens rechtstreeks vanuit uw fysieke of virtuele Linux-computers en andere bronnen in uw omgeving op één opslagplaats voor gedetailleerde analyse- en correlatie.  Deze snelstartgids wordt beschreven hoe u configureert en verzamelen gegevens van uw Linux-computer met een paar eenvoudige stappen.  Zie het volgende onderwerp voor Azure Linux VM's [verzamelen van gegevens over Azure Virtual Machines](log-analytics-quick-collect-azurevm.md).  
 
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

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Na het opgeven van de vereiste informatie op de **OMS-werkruimte** deelvenster, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="obtain-workspace-id-and-key"></a>Werkruimte-ID en sleutel ophalen
Voordat u de OMS-agent voor Linux installeert, moet u de werkruimte-ID en de sleutel voor uw werkruimte voor logboekanalyse.  Deze informatie is vereist door het script agent wrapper correct configureren van de agent en ervoor zorgen dat deze kan communiceren met logboekanalyse.  

1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten *DefaultLAWorkspace* eerder hebt gemaakt.
3. Selecteer **geavanceerde instellingen**.<br><br> ![Log Analytics geavanceerde instellingen](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **verbonden bronnen**, en selecteer vervolgens **Linux-Servers**.   
5. De waarde rechts van **werkruimte-ID** en **primaire sleutel**. Kopieer en plak beide in uw favoriete editor.   

## <a name="install-the-agent-for-linux"></a>Installeer de agent voor Linux
De volgende stappen configureren van de installatie van de agent voor logboekanalyse in Azure en Azure Government cloud.  

1. Voer de volgende opdracht, waarbij de werkruimte-ID en de primaire sleutel eerder hebt gekopieerd voor het configureren van de Linux-computer verbinding maken met logboekanalyse.  Met deze opdracht downloadt de agent, valideert de controlesom en installeert deze. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Voer de volgende opdracht, waarbij de werkruimte-ID en de primaire sleutel eerder hebt gekopieerd voor het configureren van de Linux-computer verbinding maken met logboekanalyse in Azure Government cloud.  Met deze opdracht downloadt de agent, valideert de controlesom en installeert deze. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Om te communiceren met een proxy-agent configureren

De volgende stappen uitvoeren als uw Linux-computers nodig om te communiceren via een proxyserver met logboekanalyse.  De configuratiewaarde proxy heeft de volgende syntaxis `[protocol://][user:password@]proxyhost[:port]`.

1. Bewerk het bestand `/etc/opt/microsoft/omsagent/proxy.conf` door het uitvoeren van de volgende opdrachten en de waarden voor uw specifieke instellingen wijzigen.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Start de agent opnieuw door de volgende opdracht uit te voeren: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Gebeurtenis-en prestatiegegevens verzamelen
Log Analytics kunt verzamelen van gebeurtenissen van de Linux Syslog en prestatiemeteritems die u opgeeft voor langere termijn analyse en rapportage en actie ondernemen wanneer een bepaalde voorwaarde wordt gedetecteerd.  Volg deze stappen voor het verzamelen van gebeurtenissen van de Linux Syslog en algemene prestatiemeters voor verschillende beginnen met configureren.  

1. Selecteer **Syslog**.  
2. U kunt een gebeurtenislogboek toevoegen door te typen in de naam van het logboek.  Type **Syslog** en klik vervolgens op het plusteken  **+** .  
3. In de tabel, schakelt u de ernst **Info**, **kennisgeving** en **Debug**. 
4. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.
5. Selecteer **Linux prestatiegegevens** voor het verzamelen van prestatiemeteritems op een Windows-computer. 
6. Wanneer u Linux-prestatiemeteritems voor een nieuwe werkruimte voor logboekanalyse voor het eerst configureert, krijgt u de optie snel maken verschillende algemene prestatiemeteritems. Ze worden weergegeven met een selectievakje in naast elke.<br><br> ![Standaard Windows-prestatiemeteritems geselecteerd](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Klik op **de geselecteerde prestatiemeteritems toevoegen**.  Ze zijn toegevoegd en voorinstelling met een tweede tien verzameling controle-interval.  
7. Klik op **opslaan** boven aan de pagina om de configuratie op te slaan.

## <a name="view-data-collected"></a>Gegevens weergeven die worden verzameld
Nu dat u gegevensverzameling hebt ingeschakeld, kunt een voorbeeld van een eenvoudige logboek zoekopdracht om te zien van sommige gegevens van de doelcomputer wordt uitgevoerd.  

1. Navigeer naar Log Analytics en selecteer de werkruimte eerder hebt gemaakt in de Azure portal.
2. Klik op de **logboek zoeken** tegel en klik in het deelvenster logboek zoeken in het veld querytype `Perf` en vervolgens drukt Voer of klik op de zoekknop aan de rechterkant van het queryveld.<br><br> ![Meld u logboekanalyse zoeken query voorbeeld](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Bijvoorbeeld: de query in de volgende afbeelding 735 prestaties records geretourneerd.<br><br> ![Log Analytics zoekresultaat melden](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, kunt u de agent verwijderen uit de Linux-computer en de werkruimte voor logboekanalyse verwijderen.  

Als u wilt verwijderen van de agent, moet u de volgende stappen uitvoeren.

1. Download de Linux-agent [universal script](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) op de computer.
2. Uitvoeren van de bundel .sh bestand w de *--opschonen* argument op de computer waarop de agent en de configuratie volledig verwijderd.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Selecteer de werkruimte voor logboekanalyse u eerder en klik vervolgens op pagina resource gemaakt voor het verwijderen van de werkruimte **verwijderen**.<br><br> ![Log Analytics-bron verwijderen](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u operationeel verzamelt en prestatiegegevens van uw on-premises Linux-computer, u eenvoudig beginnen kunt verkennen, analyseren en nemen actie op gegevens die u verzamelt voor *gratis*.  

Voor informatie over het weergeven en analyseren van de gegevens gaat u verder met de zelfstudie.   

> [!div class="nextstepaction"]
> [Weergeven of gegevens analyseren in Log Analytics](log-analytics-tutorial-viewdata.md)
