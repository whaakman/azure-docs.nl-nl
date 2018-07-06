---
title: Gegevens verzamelen van on-premises Linux-computers met Azure Log Analytics | Microsoft Docs
description: Lees meer over het implementeren van de Log Analytics-agent voor Linux en over hoe u het verzamelen van gegevens uit dat besturingssysteem met Log Analytics kunt inschakelen.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/02/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: aca8ee7305f919068733dfa3cb636a0f9bad7b05
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127166"
---
# <a name="collect-data-from-linux-computer-hosted-in-your-environment"></a>Gegevens verzamelen van een Linux-computer die in uw omgeving wordt gehost
[Azure Log Analytics](log-analytics-overview.md) kan gegevens rechtstreeks vanuit uw fysieke of virtuele Linux-computer en andere resources in uw omgeving verzamelen en onderbrengen in één opslagplaats voor uitvoerige analyse en correlatie.  In deze Quick Start wordt beschreven hoe u met een paar eenvoudige stappen gegevens van uw Linux-computer configureert en verzamelt.  Zie het volgende onderwerp, [Gegevens over Azure Virtual Machines verzamelen](log-analytics-quick-collect-azurevm.md), voor informatie over virtuele Linux-machines in Azure.  

Als u meer wilt weten over de ondersteunde configuratie, kunt u de informatie over [ondersteunde Linux-besturingssystemen](log-analytics-concept-hybrid.md#supported-linux-operating-systems) en de [ netwerkconfiguratie voor de firewall](log-analytics-concept-hybrid.md#network-firewall-requirements) raadplegen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klik op **Maken** en geef uw keuze aan voor de volgende items:

  * Geef een naam op voor de nieuwe **OMS-werkruimte**, bijvoorbeeld *StandaardLAWerkruimte*. 
  * Selecteer een **abonnement** om te koppelen door een selectie in de vervolgkeuzelijst te maken als de geselecteerde standaardwaarde niet juist is.
  * Selecteer bij **Resourcegroep** een bestaande resourcegroep die een of meer virtuele Azure-machines bevat.  
  * Selecteer de **Locatie** waarop uw virtuele machines zijn geïmplementeerd.  Bekijk [in welke regio's Log Analytics beschikbaar is](https://azure.microsoft.com/regions/services/) voor aanvullende informatie.  
  * Als u een werkruimte maakt in een nieuw abonnement dat na 2 april 2018 is gemaakt, gebruikt deze automatisch de prijscategorie *Per GB*. De optie voor het selecteren van een prijscategorie is dan niet beschikbaar.  Als u een werkruimte maakt voor een bestaand abonnement dat is gemaakt vóór 2 april, of voor een abonnement dat is gekoppeld aan een bestaande EA-inschrijving, kunt u hier uw gewenste prijscategorie selecteren.  Zie [Prijzen voor Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) voor aanvullende informatie over de afzonderlijke lagen.

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)<br>  

3. Nadat u de vereiste gegevens hebt opgegeven in het deelvenster **OMS-werkruimte**, klikt u op **OK**.  

Terwijl de gegevens worden geverifieerd en de werkruimte wordt gemaakt, kunt u de voortgang bijhouden onder **Meldingen** in het menu. 

## <a name="obtain-workspace-id-and-key"></a>Werkruimte-ID en -sleutel ophalen
Voordat u de OMS-agent voor Linux installeert, hebt u eerst de werkruimte-ID en -sleutel voor uw Log Analytics-werkruimte nodig.  Het script voor de agentwrapper heeft deze informatie nodig om de agent correct te configureren en ervoor te zorgen dat deze kan communiceren met Log Analytics.  

1. Klik in Azure Portal in de linkerbovenhoek op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten de *StandaardLAWerkruimte* die u eerder hebt gemaakt.
3. Selecteer **Geavanceerde instellingen**.<br><br> ![Geavanceerde instellingen van Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **Verbonden bronnen** en selecteer vervolgens **Linux-servers**.   
5. De waarde rechts van **Werkruimte-ID** en **Primaire sleutel**. Kopieer en plak beide in uw favoriete editor.   

## <a name="install-the-agent-for-linux"></a>De agent voor Linux installeren
Met de volgende stappen wordt de installatie van de agent voor Log Analytics in Azure en de Azure Government-cloud geconfigureerd.  

>[!NOTE]
>De OMS-agent voor Linux kan niet worden geconfigureerd om aan meer dan één werkruimte voor Log Analytics te rapporteren.  

Als uw Linux-computer via een proxyserver moet communiceren met Log Analytics, kan de proxyconfiguratie op de opdrachtregel worden opgegeven door `-p [protocol://][user:password@]proxyhost[:port]` op te nemen.  De eigenschap *proxyhost* accepteert een volledig gekwalificeerde domeinnaam of het IP-adres van de proxyserver. 

Bijvoorbeeld: `https://user01:password@proxy01.contoso.com:30443`

1. Als u de Linux-computer wilt configureren om verbinding te maken met Log Analytics, voert u de volgende opdracht uit om de eerder gekopieerde werkruimte-ID en primaire sleutel op te geven.  Met de volgende opdracht wordt de agent gedownload, de bijbehorende controlesom gevalideerd en de agent geïnstalleerd. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    De volgende opdracht bevat de proxyparameter `-p` en een voorbeeld van de syntaxis.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Als u de Linux-computer wilt configureren om verbinding te maken met Log Analytics in de Azure Government-cloud, voert u de volgende opdracht uit met de eerder gekopieerde werkruimte-ID en primaire sleutel.  Met de volgende opdracht wordt de agent gedownload, de bijbehorende controlesom gevalideerd en de agent geïnstalleerd. 

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
2. U kunt een gebeurtenislogboek toevoegen door de naam van het logboek te typen.  Typ **Syslog** en klik op het plusteken **+**.  
3. Schakel in de tabel de ernstcategorieën **Info**, **Kennisgeving** en **Fouten opsporen** uit. 
4. Klik op **Opslaan** bovenaan de pagina om de configuratie op te slaan.
5. Selecteer **Linux-prestatiegegevens** om het verzamelen van prestatiemeteritems op een Linux-computer in te schakelen. 
6. Wanneer u Linux-prestatiemeteritems voor een nieuwe Log Analytics-werkruimte voor het eerst configureert, krijgt u de optie om snel verschillende algemene prestatiemeteritems te maken. Ze worden weergegeven met een selectievakje ernaast.<br><br> ![Standaard Windows-prestatiemeteritems geselecteerd](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png)<br> Klik op **De geselecteerde prestatiemeteritems toevoegen**.  Ze worden toegevoegd en vooraf ingesteld met een sample-interval van tien seconden.  
7. Klik op **Opslaan** bovenaan de pagina om de configuratie op te slaan.

## <a name="view-data-collected"></a>Verzamelde gegevens weergeven
Nu u gegevensverzameling hebt ingeschakeld, geven we een voorbeeld van een eenvoudige zoekopdracht in logboeken om enkele gegevens afkomstig van de doelcomputer weer te geven.  

1. Navigeer in Azure Portal naar Log Analytics en selecteer de werkruimte die u eerder hebt gemaakt.
2. Klik op de tegel **Zoeken in logboeken**. In het deelvenster Zoeken in logboeken typt u `Perf` in het queryveld en drukt u op Enter of klikt u op de zoekknop rechts van het queryveld.<br><br> ![Voorbeeld van zoeken in logboeken in Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Zo geeft de query in de volgende afbeelding 735 prestatierecords als resultaat.<br><br> ![Resultaat van zoeken in logboeken in Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, kunt u de agent verwijderen van de Linux-computer en de Log Analytics-werkruimte verwijderen.  

Voer de volgende opdracht op de Linux-computer uit om de agent te verwijderen.  Met het argument *--purge* worden de agent en de configuratie ervan volledig verwijderd.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

U verwijdert de werkruimte door de eerder gemaakte Log Analytics-werkruimte te selecteren en op de resourcepagina op **Verwijderen** te klikken.<br><br> ![Log Analytics-resource verwijderen](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Volgende stappen
Nu u bezig bent met het verzamelen van operationele gegevens en prestatiegegevens van uw on-premises Windows-computer, kunt u beginnen met het verkennen, analyseren en het ondernemen van actie op gegevens die u *gratis* verzamelt.  

Voor informatie over het weergeven en analyseren van de gegevens gaat u verder met de zelfstudie.   

> [!div class="nextstepaction"]
> [Gegevens weergeven of analyseren in Log Analytics](log-analytics-tutorial-viewdata.md)
