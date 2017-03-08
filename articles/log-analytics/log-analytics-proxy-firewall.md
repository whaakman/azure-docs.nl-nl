---
title: Proxy- en firewallinstellingen configureren in Azure Log Analytics | Microsoft Docs
description: Configureer de proxy- en firewallinstellingen als uw agents of OMS-services gebruik moeten maken van bepaalde poorten.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b55ebd80-efd4-4220-971b-c18aea1b1ab2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/10/2017
ms.author: banders;magoedte
translationtype: Human Translation
ms.sourcegitcommit: 6a527fa303f1e2bd06ac662e545d6b6a1d299fb4
ms.openlocfilehash: cd06dfd498540970dc8ed29650f4d9e3ca57939b
ms.lasthandoff: 02/10/2017


---
# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Proxy- en firewallinstellingen configureren in Log Analytics
De acties die nodig zijn om de proxy- en firewallinstellingen voor Log Analytics te configureren, zijn afhankelijk van het type agents dat u gebruikt. Raadpleeg de volgende gedeelten om na te gaan wat er geldt voor het type agent dat u gebruikt.

## <a name="settings-for-the-oms-gateway"></a>Instellingen voor de OMS-gateway

Als uw agents geen toegang tot internet hebben, kunnen ze in plaats daarvan via uw eigen netwerkresources gegevens naar de OMS-gateway verzenden. De gateway verzamelt de ontvangen gegevens en stuurt deze namens de agents naar de OMS-service.

Configureer agents die met de OMS-gateway communiceren, met een volledig gekwalificeerde domeinnaam en een aangepast poortnummer.

De OMS-gateway heeft internettoegang nodig. Gebruik voor de OMS-gateway dezelfde proxyserver- of firewallinstellingen die u toepast voor het type agents dat u heeft. Zie voor meer informatie over de OMS-gateway [Connect computers and devices to OMS using the OMS Gateway](log-analytics-oms-gateway.md) (Computers en apparaten verbinden met OMS via de OMS-gateway).

## <a name="configure-settings-with-the-microsoft-monitoring-agent"></a>Instellingen configureren met Microsoft Monitoring Agent
Microsoft Monitoring Agent moet toegang hebben tot het poortnummer van uw domeinen en de URL’s om verbinding te kunnen maken met en om te kunnen registreren bij de OMS-service. Als u een proxyserver gebruikt voor communicatie tussen de agent en de OMS-service, moet u controleren of de juiste resources toegankelijk zijn. Als u een firewall gebruikt om toegang tot internet te beperken, moet u uw firewall zodanig configureren dat toegang tot OMS wordt toegestaan. In de volgende tabellen staat welke poorten er nodig zijn voor OMS.

| **Agentresource** | **Poorten** | **HTTPS-controle overslaan** |
| --- | --- | --- |
| \*.ods.opinsights.azure.com |443 |Ja |
| \*.oms.opinsights.azure.com |443 |Ja |
| \*.blob.core.windows.net |443 |Ja |
| \*.azure-automation.net |443 |Ja |
| ods.systemcenteradvisor.com |443 | |

Volg de volgende procedure om de proxyinstellingen voor Microsoft Monitoring Agent te configureren via het Configuratiescherm. U moet de procedure voor elke server uitvoeren. Als er veel servers zijn die u moet configureren, is het wellicht eenvoudiger om een script te gebruiken om dit proces te automatiseren. Zie de volgende procedure [Proxyinstellingen met een script configureren voor de Microsoft Monitoring Agent](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script) als u dit wilt doen.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Proxyinstellingen via het Configuratiescherm configureren voor de Microsoft Monitoring Agent
1. Open het **Configuratiescherm**.
2. Open **Microsoft Monitoring Agent**.
3. Klik op het tabblad **Proxyinstellingen**.<br>  
   ![tabblad proxyinstellingen](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)
4. Selecteer **Een proxyserver gebruiken** en voer de URL en het poortnummer in (als er een poortnummer is vereist), zoals hieronder wordt weergegeven. Als er voor uw proxyserver verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in om de proxyserver te openen.

Gebruik de volgende procedure om een PowerShell-script te maken dat u kunt uitvoeren om de proxyinstellingen in te stellen voor elke agent die rechtstreeks verbinding maakt met de servers.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Proxyinstellingen met een script configureren voor de Microsoft Monitoring Agent
Kopieer het volgende voorbeeld, werk het bij met informatie over uw omgeving, sla het op met een PS1-bestandsnaamextensie en voer het script daarna uit op elke computer die rechtstreeks wordt verbonden met de OMS-service.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)


## <a name="configure-settings-with-operations-manager"></a>Instellingen configureren met Operations Manager
Een Operations Manager-beheergroep moet toegang hebben tot de poortnummers van uw domeinen en URL’s om verbinding te kunnen maken met en om te kunnen registreren bij de OMS-service. Als u een proxyserver gebruikt voor communicatie tussen de Operations Manager-beheerserver en de OMS-service, moet u controleren of de juiste resources toegankelijk zijn. Als u een firewall gebruikt om toegang tot internet te beperken, moet u uw firewall zodanig configureren dat toegang tot OMS wordt toegestaan. Zelfs als een Operations Manager-beheerserver zich niet achter een proxyserver bevindt, bevinden de agents zich daar mogelijk wel. In dat geval moet de proxyserver op dezelfde manier als de agents worden geconfigureerd, anders kunt u Security and Log Management-oplossingsgegevens (SAM) niet inschakelen en doorsturen naar de OMS-webservice.

Als u de Operations Manager-agents wilt laten communiceren met de OMS-service, moet uw Operations Manager-infrastructuur (inclusief agents) de juiste proxyinstellingen en -versie hebben. De proxyinstellingen voor agents worden opgegeven in de Operations Manager-console. U moet een van de volgende versies gebruiken:

* Operations Manager 2012 SP1 Update Rollup 7 of hoger
* Operations Manager 2012 R2 Update Rollup 3 of hoger

In de volgende tabellen worden de poorten vermeld die betrekking hebben op deze taken.

> [!NOTE]
> In enkele van de volgende resources wordt melding gemaakt van Advisor en Operationeel inzicht; beide zijn oudere versies van OMS. De resources in de lijst worden in de toekomst echter gewijzigd.
>
>

Hier volgt een lijst met agentresources en -poorten:<br>

| **Agentresource** | **Poorten** |
| --- | --- |
| \*.ods.opinsights.azure.com |443 |
| \*.oms.opinsights.azure.com |443 |
| \*.blob.core.windows.net/\* |443 |
| ods.systemcenteradvisor.com |443 |

<br>
Hier volgt een lijst met resources en poorten voor beheerservers:<br>

| **Resource voor beheerserver** | **Poorten** | **HTTPS-controle overslaan** |
| --- | --- | --- |
| service.systemcenteradvisor.com |443 | |
| \*.service.opinsights.azure.com |443 | |
| \*.blob.core.windows.net |443 |Ja |
| data.systemcenteradvisor.com |443 | |
| ods.systemcenteradvisor.com |443 | |
| \*.ods.opinsights.azure.com |443 |Ja |
| \*.azure-automation.net |443 |Ja |

<br>
Hier volgt een lijst met OMS- en Operations Manager-consoleresources en -poorten.<br>

| **OMS- en Operations Manager-consoleresources** | **Poorten** |
| --- | --- |
| service.systemcenteradvisor.com |443 |
| \*.service.opinsights.azure.com |443 |
| \*.live.com |Poort 80 en 443 |
| \*.microsoft.com |Poort 80 en 443 |
| \*.microsoftonline.com |Poort 80 en 443 |
| \*.mms.microsoft.com |Poort 80 en 443 |
| login.windows.net |Poort 80 en 443 |

<br>

Voer de volgende procedures uit om uw Operations Manager-beheergroep te registreren bij de OMS-service. Als er communicatieproblemen zijn tussen de beheergroep en de OMS-service, gebruikt u de validatieprocedures om problemen met gegevensoverdracht naar de OMS-service op te lossen.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Uitzonderingen aanvragen voor de OMS-service-eindpunten
1. Gebruik de informatie uit de eerste tabel om te controleren of de resources die nodig zijn voor de Operations Manager-beheerserver, toegankelijk zijn via de firewalls die u gebruikt.
2. Gebruik de informatie uit de tweede tabel om te controleren of de resources die nodig zijn voor de Operations-console in Operations Manager en OMS, toegankelijk zijn via de firewalls die u gebruikt.
3. Als u een proxyserver gebruikt met Internet Explorer, controleert u of de server is geconfigureerd en goed werkt. Voor de controle opent u een beveiligde webverbinding (HTTPS), bijvoorbeeld [https://bing.com](https://bing.com). Als de beveiligde webverbinding niet in een browser werkt, werkt deze waarschijnlijk ook niet in de Operations Manager-beheerconsole met de webservices in de cloud.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>De proxyserver configureren in de Operations Manager-console
1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Vouw **Operationeel inzicht** uit en selecteer **Verbinding met Operationeel inzicht**.<br>  
   ![Verbinding met Operations Manager OMS](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Klik in de weergave OMS-verbinding op **Proxyserver configureren**.<br>  
   ![Proxyserver configureren om Operations Manager met OMS te verbinden](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Selecteer in het gedeelte Proxyserver van de instellingenwizard van Operationeel inzicht **Een proxyserver gebruiken voor toegang tot de Operationeel inzicht-webservice** en voer de URL in met het poortnummer, bijvoorbeeld **http://myproxy:80**.<br>  
   ![OMS-proxyadres van Operations Manager](./media/log-analytics-proxy-firewall/proxy-om03.png)

### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Referenties opgeven als er verificatie is vereist voor de proxyserver
 De referenties en instellingen van de proxyserver moeten worden doorgegeven aan beheerde computers die op hun beurt rapporteren aan OMS. Deze servers moeten zich in de *Microsoft System Center Advisor Monitoring Server-groep* bevinden. De referenties worden versleuteld in het register van elke server in de groep.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer onder **RunAs-configuratie** de optie **Profielen**.
3. Open het profiel **System Center Advisor RunAs-profielproxy**.<br>  
   ![afbeelding van het System Center Advisor RunAs-proxyprofiel](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Klik in de wizard RunAs-profiel op **Toevoegen** om een RunAs-account te gebruiken. U kunt een nieuw RunAs-account maken of een bestaand account gebruiken. Dit account moet over voldoende rechten beschikken voor het doorgeven van de proxyserver.<br>   
   ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Als u het account voor beheer wilt instellen, kiest u **Een geselecteerde klasse, groep of object** om het vak Object zoeken te openen.<br>  
   ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Zoek en selecteer de **Microsoft System Center Advisor Monitoring Server-groep**.<br>  
   ![afbeelding van het vak Object zoeken](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klik op **OK** om het vak Een RunAs-account toevoegen te sluiten.<br>  
   ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Voltooi de wizard en sla de wijzigingen op.<br>  
   ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)

### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Controleren of OMS Management Packs worden gedownload
Als u oplossingen hebt toegevoegd aan OMS, kunt u ze in de Operations Manager-console als Management Packs bekijken via **Beheer**. Zoek naar *System Center Advisor* om ze snel te vinden.<br>  
   ![gedownloade managementpacks](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png)  <br>  
U kunt ook op OMS Management Packs controleren door de volgende Windows PowerShell-opdracht uit te voeren in de Operations Manager-beheerserver:

   ```  
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
   ```  

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Controleren of Operations Manager gegevens verzendt naar de OMS-service
1. Open in de Operations Manager-beheerserver de prestatiemeter (perfmon.exe) en selecteer **Prestatiemeter**.
2. Klik op **Toevoegen** en selecteer **Health Service-beheergroepen**.
3. Voeg alle items toe die beginnen met **HTTP**.<br>  
   ![items toevoegen](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Als uw Operations Manager-configuratie goed is, ziet u bij de Health Service Management-items activiteit voor gebeurtenissen en andere gegevensitems. Dit gebeurt op basis van de Management Packs die u in OMS hebt toegevoegd en het geconfigureerde beleid voor de verzameling van logboeken.<br>  
   ![Prestatiemeter die aangeeft dat er activiteit is](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)

## <a name="next-steps"></a>Volgende stappen
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
* Vertrouwd raken met [zoekopdrachten naar logboeken](log-analytics-log-searches.md) om gedetailleerde informatie te bekijken die is verzameld door oplossingen.

