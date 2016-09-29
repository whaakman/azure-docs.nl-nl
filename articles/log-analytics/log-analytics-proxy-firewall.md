<properties
    pageTitle="Proxy- en firewallinstellingen configureren in Log Analytics | Microsoft Azure"
    description="Configureer de proxy- en firewallinstellingen als uw agents of OMS-services gebruik moeten maken van bepaalde poorten."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>


# Proxy- en firewallinstellingen configureren in Log Analytics

De acties die nodig zijn voor het configureren van de proxy- en firewallinstellingen voor Log Analytics in OMS, verschillen wanneer u Operations Manager en de bijbehorende agents gebruikt in plaats van Microsoft Monitoring Agents die rechtstreeks verbinding maken met servers. Raadpleeg de volgende gedeelten om na te gaan wat er geldt voor het type agent dat u gebruikt.

## De proxy- en firewallinstellingen configureren met Microsoft Monitoring Agent

Microsoft Monitoring Agent moet toegang hebben tot het poortnummer van uw domeinen en de URL’s om verbinding te kunnen maken met en om te kunnen registreren bij de OMS-service. Als u een proxyserver gebruikt voor communicatie tussen de agent en de OMS-service, moet u controleren of de juiste resources toegankelijk zijn. Als u een firewall gebruikt om toegang tot internet te beperken, moet u uw firewall zodanig configureren dat toegang tot OMS wordt toegestaan. In de volgende tabellen staat welke poorten er nodig zijn voor OMS.

|**Agentresource**|**Poorten**|**HTTPS-controle overslaan**|
|--------------|-----|--------------|
|\*.ods.opinsights.azure.com|443|Ja|
|\*.oms.opinsights.azure.com|443|Ja|
|\*.blob.core.windows.net|443|Ja|
|ods.systemcenteradvisor.com|443| |

Volg de volgende procedure om de proxyinstellingen voor Microsoft Monitoring Agent te configureren via het Configuratiescherm. U moet de procedure voor elke server uitvoeren. Als er veel servers zijn die u moet configureren, is het wellicht eenvoudiger om een script te gebruiken om dit proces te automatiseren. Zie de volgende procedure [Proxyinstellingen met een script configureren voor de Microsoft Monitoring Agent](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script) als u dit wilt doen.

### Proxyinstellingen via het Configuratiescherm configureren voor de Microsoft Monitoring Agent

1. Open het **Configuratiescherm**.

2. Open **Microsoft Monitoring Agent**.

3. Klik op het tabblad **Proxyinstellingen**.<br>  
  ![tabblad proxyinstellingen](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Selecteer **Een proxyserver gebruiken** en voer de URL en het poortnummer in (als er een poortnummer is vereist), zoals hieronder wordt weergegeven. Als er voor uw proxyserver verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in om de proxyserver te openen.

Gebruik de volgende procedure om een PowerShell-script te maken dat u kunt uitvoeren om de proxyinstellingen in te stellen voor elke agent die rechtstreeks verbinding maakt met de servers.

### Proxyinstellingen met een script configureren voor de Microsoft Monitoring Agent

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
        

## Proxy- en firewallinstellingen configureren met Operations Manager

Een Operations Manager-beheergroep moet toegang hebben tot de poortnummers van uw domeinen en URL’s om verbinding te kunnen maken met en om te kunnen registreren bij de OMS-service. Als u een proxyserver gebruikt voor communicatie tussen de Operations Manager-beheerserver en de OMS-service, moet u controleren of de juiste resources toegankelijk zijn. Als u een firewall gebruikt om toegang tot internet te beperken, moet u uw firewall zodanig configureren dat toegang tot OMS wordt toegestaan. Zelfs als een Operations Manager-beheerserver zich niet achter een proxyserver bevindt, bevinden de agents zich daar mogelijk wel. In dat geval moet de proxyserver op dezelfde manier als de agents worden geconfigureerd, anders kunt u Security and Log Management-oplossingsgegevens (SAM) niet inschakelen en doorsturen naar de OMS-webservice.

Als u de Operations Manager-agents wilt laten communiceren met de OMS-service, moet uw Operations Manager-infrastructuur (inclusief agents) de juiste proxyinstellingen en -versie hebben. De proxyinstellingen voor agents worden opgegeven in de Operations Manager-console. U moet een van de volgende versies gebruiken:

- Operations Manager 2012 SP1 Update Rollup 7 of hoger
- Operations Manager 2012 R2 Update Rollup 3 of hoger


In de volgende tabellen worden de poorten vermeld die betrekking hebben op deze taken.

>[AZURE.NOTE] In enkele van de volgende resources wordt melding gemaakt van Advisor en Operationeel inzicht; beide zijn oudere versies van OMS. De resources in de lijst worden in de toekomst echter gewijzigd.

Hier volgt een lijst met agentresources en -poorten:<br>

|**Agentresource**|**Poorten**|
|--------------|-----|
|\*.ods.opinsights.azure.com|443|
|\*.oms.opinsights.azure.com|443|
|\*.blob.core.windows.net/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Hier volgt een lijst met resources en poorten voor beheerservers:<br>

|**Resource voor beheerserver**|**Poorten**|**HTTPS-controle overslaan**|
|--------------|-----|--------------|
|service.systemcenteradvisor.com|443| |
|\*.service.opinsights.azure.com|443| |
|\*.blob.core.windows.net|443|Ja| 
|data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*.ods.opinsights.azure.com|443|Ja| 
<br>
Hier volgt een lijst met OMS- en Operations Manager-consoleresources en -poorten.<br>

|**OMS- en Operations Manager-consoleresources**|**Poorten**|
|----|----|
|service.systemcenteradvisor.com|443|
|\*.service.opinsights.azure.com|443|
|\*.live.com|Poort 80 en 443|
|\*.microsoft.com|Poort 80 en 443|
|\*.microsoftonline.com|Poort 80 en 443|
|\*.mms.microsoft.com|Poort 80 en 443|
|login.windows.net|Poort 80 en 443|
<br>

Voer de volgende procedures uit om uw Operations Manager-beheergroep te registreren bij de OMS-service. Als er communicatieproblemen zijn tussen de beheergroep en de OMS-service, gebruikt u de validatieprocedures om problemen met gegevensoverdracht naar de OMS-service op te lossen.

### Uitzonderingen aanvragen voor de OMS-service-eindpunten

1. Gebruik de informatie uit de eerste tabel om te controleren of de resources die nodig zijn voor de Operations Manager-beheerserver, toegankelijk zijn via de firewalls die u gebruikt.
2. Gebruik de informatie uit de tweede tabel om te controleren of de resources die nodig zijn voor de Operations-console in Operations Manager en OMS, toegankelijk zijn via de firewalls die u gebruikt.
3. Als u een proxyserver gebruikt met Internet Explorer, controleert u of de server is geconfigureerd en goed werkt. Voor de controle opent u een beveiligde webverbinding (HTTPS), bijvoorbeeld [https://bing.com](https://bing.com). Als de beveiligde webverbinding niet in een browser werkt, werkt deze waarschijnlijk ook niet in de Operations Manager-beheerconsole met de webservices in de cloud.

### De proxyserver configureren in de Operations Manager-console

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.

2. Vouw **Operationeel inzicht** uit en selecteer **Verbinding met Operationeel inzicht**.<br>  
    ![Verbinding met Operations Manager OMS](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Klik in de weergave OMS-verbinding op **Proxyserver configureren**.<br>  
    ![Proxyserver configureren om Operations Manager met OMS te verbinden](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Selecteer in het gedeelte Proxyserver van de instellingenwizard van Operationeel inzicht **Een proxyserver gebruiken voor toegang tot de Operationeel inzicht-webservice** en voer de URL in met het poortnummer, bijvoorbeeld **http://myproxy:80**.<br>  
    ![OMS-proxyadres van Operations Manager](./media/log-analytics-proxy-firewall/proxy-om03.png)


### Referenties opgeven als er verificatie is vereist voor de proxyserver
 De referenties en instellingen van de proxyserver moeten worden doorgegeven aan beheerde computers die op hun beurt rapporteren aan OMS. Deze servers moeten zich in de *Microsoft System Center Advisor Monitoring Server-groep* bevinden. De referenties worden versleuteld in het register van elke server in de groep.

1. Open de Operations Manager-console en selecteer de werkruimte **Beheer**.
2. Selecteer onder **RunAs-configuratie** de optie **Profielen**.
3. Open het profiel **System Center Advisor RunAs-profielproxy**.  
    ![afbeelding van het System Center Advisor RunAs-proxyprofiel](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Klik in de wizard RunAs-profiel op **Toevoegen** om een RunAs-account te gebruiken. U kunt een nieuw RunAs-account maken of een bestaand account gebruiken. Dit account moet over voldoende rechten beschikken voor het doorgeven van de proxyserver.  
    ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Als u het account voor beheer wilt instellen, kiest u **Een geselecteerde klasse, groep of object** om het vak Object zoeken te openen.  
    ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Zoek en selecteer de **Microsoft System Center Advisor Monitoring Server-groep**.  
    ![afbeelding van het vak Object zoeken](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klik op **OK** om het vak Een RunAs-account toevoegen te sluiten.  
    ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Voltooi de wizard en sla de wijzigingen op.  
    ![afbeelding van de wizard RunAs-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### Controleren of OMS Management Packs worden gedownload

Als u oplossingen hebt toegevoegd aan OMS, kunt u ze in de Operations Manager-console als Management Packs bekijken via **Beheer**. Zoek naar *System Center Advisor* om ze snel te vinden.  
    ![Management Packs die zijn gedownload](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) U kunt ook controleren op OMS Management Packs via de volgende Windows PowerShell-opdracht in de Operations Manager-beheerserver:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### Controleren of Operations Manager gegevens verzendt naar de OMS-service

1. Open in de Operations Manager-beheerserver de prestatiemeter (perfmon.exe) en selecteer **Prestatiemeter**.
2. Klik op **Toevoegen** en selecteer **Health Service-beheergroepen**.
3. Voeg alle items toe die beginnen met **HTTP**.  
    ![items toevoegen](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Als uw Operations Manager-configuratie goed is, ziet u bij de Health Service Management-items activiteit voor gebeurtenissen en andere gegevensitems. Dit gebeurt op basis van de Management Packs die u in OMS hebt toegevoegd en het geconfigureerde beleid voor de verzameling van logboeken.  
    ![Prestatiemeter die aangeeft dat er activiteit is](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## Volgende stappen

- [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
- Vertrouwd raken met [zoekopdrachten naar logboeken](log-analytics-log-searches.md) om gedetailleerde informatie te bekijken die is verzameld door oplossingen.



<!--HONumber=Sep16_HO3-->


