---
title: Azure Log Analytics-agent configureren voor hybride Linux-computer | Microsoft Docs
description: Informatie over het implementeren van de Log Analytics-agent voor Linux, die wordt uitgevoerd op computers buiten Azure, en het inschakelen van gegevensverzameling met Log Analytics.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 06/14/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 535b96a2bb86f1996e6ff0aba19339b55a18ee50
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515866"
---
# <a name="configure-log-analytics-agent-for-linux-computers-in-a-hybrid-environment"></a>Log Analytics-agent configureren voor Linux-computers in een hybride omgeving
[Azure Log Analytics](../platform/agent-windows.md) kan gegevens rechtstreeks vanuit uw fysieke of virtuele Linux-computer in uw datacentrum of andere cloudomgeving verzamelen en onderbrengen in één opslagplaats voor uitvoerige analyse en correlatie. In deze Quick Start wordt beschreven hoe u met een paar eenvoudige stappen gegevens van uw Linux-computer configureert en verzamelt.  Zie het volgende onderwerp, [Gegevens over Azure Virtual Machines verzamelen](quick-collect-azurevm.md), voor informatie over virtuele Linux-machines in Azure.  

Voordat u doorgaat, moet u eerst het overzichts artikel [](../platform/log-analytics-agent.md) van de log Analytics agent raadplegen om te begrijpen welke configuraties worden ondersteund, welke [Linux-besturings systemen](../platform/log-analytics-agent.md#supported-linux-operating-systems) en [netwerk firewall configuratie](../platform/log-analytics-agent.md#network-firewall-requirements)worden ondersteund. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Selecteer in de Azure-portal de optie **Alle services**. Voer **log Analytics**in de lijst met resources in. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.

    ![Azure Portal](media/quick-collect-linux-computer/azure-portal-01.png) 

2. Selecteer **maken**en selecteer vervolgens opties voor de volgende items:

   * Geef een naam op voor de nieuwe **Log Analytics-werkruimte**, bijvoorbeeld *StandaardLAWerkruimte*. OMS-werkruimten worden nu aangeduid als Log Analytics-werkruimten.   
   * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
   * Selecteer bij **Resourcegroep** een bestaande resourcegroep die een of meer virtuele Azure-machines bevat.  
   * Selecteer de **Locatie** waarop uw virtuele machines zijn geïmplementeerd.  Bekijk [in welke regio's Log Analytics beschikbaar is](https://azure.microsoft.com/regions/services/) voor aanvullende informatie.  
   * Als u een werkruimte maakt in een nieuw abonnement dat na 2 april 2018 is gemaakt, gebruikt deze automatisch de prijscategorie *Per GB*. De optie voor het selecteren van een prijscategorie is dan niet beschikbaar.  Als u een werkruimte maakt voor een bestaand abonnement dat is gemaakt vóór 2 april, of voor een abonnement dat is gekoppeld aan een bestaande EA-inschrijving, kunt u hier uw gewenste prijscategorie selecteren.  Zie [Prijzen voor Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) voor aanvullende informatie over de afzonderlijke lagen.

        ![Log Analytics resource-Blade maken](media/quick-collect-linux-computer/create-loganalytics-workspace-02.png)<br>  

3. Nadat u de vereiste gegevens hebt opgegeven in het deel venster **log Analytics werkruimte** , selecteert u **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="obtain-workspace-id-and-key"></a>Werkruimte-ID en -sleutel ophalen
Voordat u de Log Analytics-agent voor Linux installeert, hebt u eerst de werkruimte-id en -sleutel voor de Log Analytics-werkruimte nodig.  Het script voor de agentwrapper heeft deze informatie nodig om de agent correct te configureren en ervoor te zorgen dat deze kan communiceren met Log Analytics.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Selecteer in de Azure Portal **alle services** in de linkerbovenhoek. Voer **log Analytics**in de lijst met resources in. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten de *StandaardLAWerkruimte* die u eerder hebt gemaakt.
3. Selecteer **Geavanceerde instellingen**.

    ![Geavanceerde instellingen van Log Analytics](media/quick-collect-linux-computer/log-analytics-advanced-settings-01.png) 
 
4. Selecteer **Verbonden bronnen** en selecteer vervolgens **Linux-servers**.   
5. De waarde rechts van **Werkruimte-ID** en **Primaire sleutel**. Kopieer en plak beide in uw favoriete editor.   

## <a name="install-the-agent-for-linux"></a>De agent voor Linux installeren
Met de volgende stappen wordt de installatie van de agent voor Log Analytics in Azure en de Azure Government-cloud geconfigureerd.  

>[!NOTE]
>De Log Analytics-agent voor Linux kan niet worden geconfigureerd om aan meer dan één Log Analytics-werkruimte te rapporteren.  

Als uw Linux-computer via een proxyserver moet communiceren met Log Analytics, kan de proxyconfiguratie op de opdrachtregel worden opgegeven door `-p [protocol://][user:password@]proxyhost[:port]` op te nemen.  De eigenschap *proxyhost* accepteert een volledig gekwalificeerde domeinnaam of het IP-adres van de proxyserver. 

Bijvoorbeeld: `https://user01:password@proxy01.contoso.com:30443`

1. Als u de Linux-computer wilt configureren om verbinding te maken met Log Analytics, voert u de volgende opdracht uit om de eerder gekopieerde werkruimte-ID en primaire sleutel op te geven. Met de volgende opdracht wordt de agent gedownload, de bijbehorende controlesom gevalideerd en de agent geïnstalleerd. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    De volgende opdracht bevat de proxyparameter `-p` en een voorbeeld van de syntaxis.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Als u de Linux-computer wilt configureren om verbinding te maken met Log Analytics in de Azure Government-cloud, voert u de volgende opdracht uit met de eerder gekopieerde werkruimte-ID en primaire sleutel. Met de volgende opdracht wordt de agent gedownload, de bijbehorende controlesom gevalideerd en de agent geïnstalleerd. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    De volgende opdracht bevat de proxyparameter `-p` en een voorbeeld van de syntaxis.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Start de agent opnieuw door de volgende opdracht uit te voeren: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Gebeurtenis- en prestatiegegevens verzamelen
Log Analytics kan gebeurtenissen uit de Linux Syslog en prestatiemeteritems verzamelen die u opgeeft voor langetermijnanalyses en -rapportages en kan actie ondernemen wanneer een bepaalde voorwaarde wordt gedetecteerd.  Volg deze stappen om eerst het verzamelen van gebeurtenissen uit de Linux Syslog en diverse algemene prestatiemeters te configureren.  

1. Selecteer **Syslog**.  
2. U kunt een gebeurtenislogboek toevoegen door de naam van het logboek te typen. Voer **syslog** in en selecteer vervolgens het plus **+** teken.  
3. Schakel in de tabel de ernstcategorieën **Info**, **Kennisgeving** en **Fouten opsporen** uit. 
4. Selecteer boven aan de pagina **Opslaan** om de configuratie op te slaan.
5. Selecteer **Linux-prestatiegegevens** om het verzamelen van prestatiemeteritems op een Linux-computer in te schakelen. 
6. Wanneer u Linux-prestatiemeteritems voor een nieuwe Log Analytics-werkruimte voor het eerst configureert, krijgt u de optie om snel verschillende algemene prestatiemeteritems te maken. Deze worden weer gegeven met een selectie vakje naast elk. 

    ![Standaard Windows-prestatiemeteritems geselecteerd](media/quick-collect-linux-computer/linux-perfcounters-default.png)
    
    Selecteer **de geselecteerde prestatie meter items toevoegen**. Ze worden toegevoegd en vooraf ingesteld met een sample-interval van tien seconden.

7. Selecteer boven aan de pagina **Opslaan** om de configuratie op te slaan.

## <a name="view-data-collected"></a>Verzamelde gegevens weergeven
Nu u gegevensverzameling hebt ingeschakeld, geven we een voorbeeld van een eenvoudige zoekopdracht in logboeken om enkele gegevens afkomstig van de doelcomputer weer te geven.  

1. Ga in het Azure Portal naar Log Analytics en selecteer de werk ruimte die u eerder hebt gemaakt.
2. Selecteer de tegel **zoeken** in Logboeken en klik in het deel venster zoeken in Logboeken op `Perf` het veld query en vervolgens op ENTER of selecteer de zoek knop rechts van het query veld.

    ![Voorbeeld van zoeken in logboeken in Log Analytics](media/quick-collect-linux-computer/log-analytics-portal-queryexample.png)

    Zo geeft de query in de volgende afbeelding 735 prestatierecords als resultaat.

    ![Zoekresultaat uit Log Analytics-logboeken](media/quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, kunt u de agent verwijderen van de Linux-computer en de Log Analytics-werkruimte verwijderen.  

Voer de volgende opdracht op de Linux-computer uit om de agent te verwijderen. Met het argument *--purge* worden de agent en de configuratie ervan volledig verwijderd.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

Als u de werk ruimte wilt verwijderen, selecteert u de Log Analytics werk ruimte die u eerder hebt gemaakt en selecteert u op de pagina resource de optie **verwijderen**.

![Log Analytics-resource verwijderen](media/quick-collect-linux-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Volgende stappen
Nu u bezig bent met het verzamelen van operationele gegevens en prestatiegegevens van uw on-premises Windows-computer, kunt u beginnen met het verkennen, analyseren en het ondernemen van actie op gegevens die u *gratis* verzamelt.  

Voor informatie over het weergeven en analyseren van de gegevens gaat u verder met de zelfstudie.   

> [!div class="nextstepaction"]
> [Gegevens weergeven of analyseren in Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md)
