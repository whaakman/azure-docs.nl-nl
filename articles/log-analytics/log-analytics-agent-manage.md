---
title: Het beheren van Azure Log Analytics Agent | Microsoft Docs
description: In dit artikel beschrijft de verschillende beheertaken die u normaal gesproken wordt uitvoert tijdens de levenscyclus van de Microsoft Monitoring Agent (MMA) op een machine is geïmplementeerd.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: magoedte
ms.openlocfilehash: 5ff4f79a607143683b37726f1c02a6057dc6b9b0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Beheer en onderhoud van de agent Log Analytics voor Windows en Linux

Na de eerste implementatie van de Windows- of Linux-agent voor logboekanalyse moet u de agent opnieuw te configureren, of verwijderen van de computer als de fase buiten gebruik stellen in de levenscyclus is bereikt.  U kunt deze taken routineonderhoud eenvoudig beheren handmatig of door middel van automatisering, waardoor zowel operationele problemen als dalen.

## <a name="adding-or-removing-a-workspace"></a>Het toevoegen of verwijderen van een werkruimte 

### <a name="windows-agent"></a>Windows-agent

#### <a name="update-settings-from-control-panel"></a>Update-instellingen in het Configuratiescherm

1. Meld u aan op de computer met een account dat beheerdersrechten heeft.
2. Open het **Configuratiescherm**.
3. Selecteer **Microsoft Monitoring Agent** en klik vervolgens op de **Azure logboekanalyse (OMS)** tabblad.
4. Als een werkruimte verwijderen, selecteert u deze en klik vervolgens op **verwijderen**. Herhaal deze stap voor alle andere werkruimte die u wilt dat de agent aan.
5. Als u een werkruimte toevoegt, klikt u op **toevoegen** en klik op de **Log Analytics-werkruimte toevoegen** in het dialoogvenster, plak de werkruimte-ID en Werkruimtesleutel (primaire sleutel). Als de computer een werkruimte voor logboekanalyse in Azure Government cloud rapporteren moet, selecteert u Azure US Government in de vervolgkeuzelijst van de Azure-Cloud. 
6. Klik op **OK** om uw wijzigingen op te slaan.

#### <a name="remove-a-workspace-using-powershell"></a>Verwijderen van een werkruimte met behulp van PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Toevoegen van een werkruimte in Azure commerciële met behulp van PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>Toevoegen van een werkruimte in Azure voor US Government met behulp van PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Als u eerder hebt gebruikt de opdrachtregel of script te installeren of configureren van de agent `EnableAzureOperationalInsights` is vervangen door `AddCloudWorkspace` en `RemoveCloudWorkspace`.
>

### <a name="linux-agent"></a>Linux-agent
De volgende stappen laten zien hoe de Linux-agent opnieuw te configureren als u wilt registreren met een andere werkruimte of wilt verwijderen van een werkruimte van de configuratie ervan.  

1.  Voer de volgende opdracht om te controleren of dat deze is geregistreerd met een werkruimte.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l` 

    Er moet een status vergelijkbaar met het volgende voorbeeld - geretourneerd 

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

    Het is belangrijk dat de status ook ziet u de agent wordt uitgevoerd, anders de volgende stappen voor het configureren van de agent niet worden voltooid.  

2. Als deze al is geregistreerd met een werkruimte, verwijdert u de geregistreerde werkruimte door de volgende opdracht uit te voeren.  Anders als dit niet is geregistreerd, gaat u verder met de volgende stap.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -X`  
    
3. Als u wilt registreren bij een andere werkruimte, voert u de opdracht `/opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <shared key> [-d <top level domain>]` 
4. Duurde om te controleren of uw wijzigingen invloed, voert u de opdracht.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l` 

    Er moet een status vergelijkbaar met het volgende voorbeeld - geretourneerd 

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

De agent-service hoeft niet opnieuw worden opgestart om de wijzigingen pas van kracht.

## <a name="update-proxy-settings"></a>Proxy-instellingen bijwerken 
De agent om te communiceren met de service via een proxyserver configureren of [OMS Gateway](log-analytics-oms-gateway.md) na de implementatie gebruikt u een van de volgende methoden om deze taak te voltooien.

### <a name="windows-agent"></a>Windows-agent

#### <a name="update-settings-using-control-panel"></a>Update-instellingen via het Configuratiescherm

1. Meld u aan op de computer met een account dat beheerdersrechten heeft.
2. Open het **Configuratiescherm**.
3. Selecteer **Microsoft Monitoring Agent** en klik vervolgens op de **Proxy-instellingen** tabblad.
4. Klik op **een proxyserver gebruiken** en geef de URL en het poortnummer van de proxyserver of gateway. Als uw proxy of Gateway OMS verificatie vereist, typ de gebruikersnaam en wachtwoord om te verifiëren en klik vervolgens op **OK**. 

#### <a name="update-settings-using-powershell"></a>Update-instellingen met behulp van PowerShell 

Kopieer de volgende PowerShell-code, bijwerken met de informatie die specifiek zijn voor uw omgeving en sla het bestand met een PS1-bestandsextensie.  Voer het script op elke computer die rechtstreeks verbinding met de Log Analytics-service maakt.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

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
```  

### <a name="linux-agent"></a>Linux-agent
De volgende stappen uitvoeren als uw Linux-computers moeten communiceren via een proxy of Gateway met logboekanalyse OMS.  De waarde voor de proxyconfiguratie heeft de volgende syntaxis `[protocol://][user:password@]proxyhost[:port]`.  De eigenschap *proxyhost* accepteert een volledig gekwalificeerde domeinnaam of het IP-adres van de proxyserver.

1. Bewerk het bestand `/etc/opt/microsoft/omsagent/proxy.conf` door de volgende opdrachten uit te voeren en de waarden aan te passen aan uw specifieke instellingen.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Start de agent opnieuw door de volgende opdracht uit te voeren: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="uninstall-agent"></a>Agent verwijderen
Gebruik een van de volgende procedures om het verwijderen van de Windows- of Linux-agent met de wizard setup of vanaf de opdrachtregel.

### <a name="windows-agent"></a>Windows-agent

#### <a name="uninstall-from-control-panel"></a>Verwijderen van het Configuratiescherm
1. Meld u aan op de computer met een account dat beheerdersrechten heeft.  
2. In **Configuratiescherm**, klikt u op **programma's en onderdelen**.
3. In **programma's en onderdelen**, klikt u op **Microsoft Monitoring Agent**, klikt u op **verwijderen**, en klik vervolgens op **Ja**.

>[!NOTE]
>De Wizard Setup van Agent kan ook worden uitgevoerd door te dubbelklikken op **MMASetup -<platform>.exe**, die beschikbaar is voor het downloaden van een werkruimte in de Azure-portal.

#### <a name="uninstall-from-the-command-line"></a>Verwijderen vanaf de opdrachtregel
Het gedownloade bestand voor de agent is een zelfstandig installatiepakket dat is gemaakt met IExpress.  Het installatieprogramma voor de agent en de ondersteunende bestanden zijn opgenomen in het pakket en moet worden opgehaald wordt geplaatst om correct te verwijderen via de opdrachtregel die wordt weergegeven in het volgende voorbeeld. 

1. Meld u aan op de computer met een account dat beheerdersrechten heeft.  
2. De installatiebestanden van agent ophalen uit een verhoogde opdrachtprompt uitgevoerd `extract MMASetup-<platform>.exe` en krijgt u voor het pad naar de bestanden moeten worden uitgepakt.  U kunt ook het pad opgeven door de argumenten `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`.  Zie voor meer informatie over het opdrachtregelprogramma switches wordt ondersteund door IExpress [opdrachtregelopties voor IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) en werk vervolgens in het voorbeeld aan uw behoeften.
3. Typ bij de opdrachtprompt `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.  

### <a name="linux-agent"></a>Linux-agent
Voer de volgende opdracht op de Linux-computer uit om de agent te verwijderen.  Met het argument *--purge* worden de agent en de configuratie ervan volledig verwijderd.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Om te rapporteren aan een beheergroep van Operations Manager-agent configureren

### <a name="windows-agent"></a>Windows-agent
Voer de volgende stappen uit voor het configureren van de OMS-Agent voor Windows om te rapporteren aan een beheergroep van System Center Operations Manager. 

1. Meld u aan op de computer met een account dat beheerdersrechten heeft.
2. Open het **Configuratiescherm**. 
3. Klik op **Microsoft Monitoring Agent** en klik vervolgens op de **Operations Manager** tabblad.
4. Als uw Operations Manager-servers integratie met Active Directory hebt, klikt u op **automatisch bijwerken van beheergroeptoewijzingen van AD DS**.
5. Klik op **toevoegen** openen de **toevoegen van een beheergroep** in het dialoogvenster.
6. In **beheergroepsnaam** veld, typ de naam van uw beheergroep.
7. In de **primaire beheerserver** veld, typt u de naam van de primaire beheerserver.
8. In de **beheerserverpoort** veld, typt u het TCP-poortnummer.
9. Onder **Agentactieacount**, kiest u het lokale systeemaccount of een lokale account.
10. Klik op **OK** sluiten de **toevoegen van een beheergroep** in het dialoogvenster en klik vervolgens op **OK** sluiten de **eigenschappen van Microsoft Monitoring Agent** in het dialoogvenster.

### <a name="linux-agent"></a>Linux-agent
De volgende stappen uitvoeren om te configureren van de OMS-Agent voor Linux om te rapporteren aan een beheergroep van System Center Operations Manager. 

1. Bewerk het bestand `/etc/opt/omi/conf/omiserver.conf`
2. Zorg ervoor dat de regels die beginnen met `httpsport=` definieert de poort 1270. Zoals: `httpsport=1270`
3. Start de OMI-server: `sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Volgende stappen

Bekijk [probleemoplossing van de Linux-agent](log-analytics-agent-linux-support.md) als u problemen ondervindt tijdens de installatie of het beheren van de agent.  