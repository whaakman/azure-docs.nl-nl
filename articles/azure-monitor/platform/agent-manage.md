---
title: Beheren van de Azure Log Analytics-Agent | Microsoft Docs
description: Dit artikel beschrijft de verschillende beheertaken die u doorgaans tijdens de levenscyclus van de Microsoft Monitoring Agent (MMA) geïmplementeerd op een virtuele machine wordt uitgevoerd.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: magoedte
ms.openlocfilehash: 19530aa676e681f9a6ec50d2cacf77711dcb0110
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730289"
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Beheer en onderhoud van de Log Analytics-agent voor Windows en Linux

Na de initiële implementatie van de Log Analytics Windows of Linux-agent in Azure Monitor moet u mogelijk opnieuw configureren van de agent, bijwerken of verwijderen van de computer als de fase buiten gebruik stellen in de levenscyclus is bereikt. U kunt deze taken dagelijks onderhoud gemakkelijk beheren handmatig of via automatisering, waardoor zowel operationele fout en kosten.

## <a name="upgrading-agent"></a>Agent upgraden

De Log Analytics-agent voor Windows en Linux kan worden bijgewerkt naar de nieuwste versie handmatig of automatisch afhankelijk van de, implementatiescenario en de virtuele machine wordt uitgevoerd in de omgeving. De volgende methoden kunnen worden gebruikt om bij te werken van de agent.

| Omgeving | Installatiemethode | Methode voor het bijwerken |
|--------|----------|-------------|
| Azure VM | Log Analytics-agent VM-extensie voor Windows/Linux | Agent wordt automatisch bijgewerkt door standaard, tenzij u uw Azure Resource Manager-sjabloon als u wilt uitschrijven via de eigenschap geconfigureerd *autoUpgradeMinorVersion* naar **false**. |
| Aangepaste Azure-VM-installatiekopieën | Handmatige installatie van Log Analytics-agent voor Windows/Linux | Virtuele machines bijwerken naar de nieuwste versie van de agent moet worden uitgevoerd vanaf de opdrachtregel met de Windows installer-pakket- of Linux zelfuitpakkend en installeerbare shell-script bundel.|
| Non-Azure VMs | Handmatige installatie van Log Analytics-agent voor Windows/Linux | Virtuele machines bijwerken naar de nieuwste versie van de agent moet worden uitgevoerd vanaf de opdrachtregel met de Windows installer-pakket- of Linux zelfuitpakkend en installeerbare shell-script bundel. |

### <a name="upgrade-windows-agent"></a>Windows-agent bijwerken 

Voor het bijwerken van de agent op een Windows-VM naar de meest recente versie niet geïnstalleerd met behulp van de Log Analytics VM-extensie, u uitgevoerd vanaf de opdrachtprompt, script of andere oplossing voor automatisering, of met behulp van de MMASetup -\<platform\>.msi Setup De wizard.  

U kunt de meest recente versie van de Windows-agent downloaden van uw Log Analytics-werkruimte door de volgende stappen uit te voeren.

1. Meld u aan bij Azure Portal.

2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics-werkruimten**.

3. In de lijst met Log Analytics-werkruimten, selecteer de werkruimte.

4. Selecteer in uw Log Analytics-werkruimte **geavanceerde instellingen**en selecteer vervolgens **verbonden bronnen**, en tot slot **Windows Servers**.

5. Uit de **Windows Servers** pagina, selecteert u de juiste **Windows-Agent downloaden** versie te downloaden, afhankelijk van de processorarchitectuur van het Windows-besturingssysteem.

>[!NOTE]
>Tijdens de upgrade van de Log Analytics-agent voor Windows, biedt het geen ondersteuning configureren of opnieuw configureren van een werkruimte om te rapporteren aan. Voor het configureren van de agent, moet u Volg een van de ondersteunde methoden die worden vermeld onder [toevoegen of verwijderen van een werkruimte](#adding-or-removing-a-workspace).
>

#### <a name="to-upgrade-using-the-setup-wizard"></a>Om bij te werken met de Wizard Setup

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. Voer **MMASetup -\<platform\>.exe** om de installatiewizard te starten.

3. Klik op de eerste pagina van de Wizard Setup **volgende**.

4. In de **Setup van Microsoft Monitoring Agent** in het dialoogvenster, klikt u op **ik ga akkoord** om de gebruiksrechtovereenkomst te accepteren.

5. In de **Setup van Microsoft Monitoring Agent** in het dialoogvenster, klikt u op **Upgrade**. Op de statuspagina wordt de voortgang van de upgrade weergegeven.

6. Wanneer de **Microsoft Monitoring Agent-configuratie is voltooid.** pagina wordt weergegeven, klikt u op **voltooien**.

#### <a name="to-upgrade-from-the-command-line"></a>Upgrade uitvoeren vanaf de opdrachtregel

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. De bestanden van de installatie van agent ophalen uit een opdrachtprompt met verhoogde bevoegdheid uitvoeren `MMASetup-<platform>.exe /c` en wordt u gevraagd voor het pad voor het uitpakken van bestanden. U kunt ook het pad opgeven doordat argumenten worden doorgegeven `MMASetup-<platform>.exe /c /t:<Full Path>`.

3. Voer de volgende opdracht, waarbij D:\ de locatie is van het upgradelogboekbestand.

    ```dos
    setup.exe /qn /l*v D:\logs\AgentUpgrade.log AcceptEndUserLicenseAgreement=1
    ```

### <a name="upgrade-linux-agent"></a>Linux-agent bijwerken 

Een upgrade uitvoeren voor eerdere versies (> 1.0.0-47) wordt ondersteund. Uitvoeren van de installatie met de `--upgrade` opdracht alle onderdelen van de agent wordt bijgewerkt naar de nieuwste versie.

Voer de volgende opdracht de agent bij te werken.

`sudo sh ./omsagent-*.universal.x64.sh --upgrade`

## <a name="adding-or-removing-a-workspace"></a>Toevoegen of verwijderen van een werkruimte

### <a name="windows-agent"></a>Windows-agent

#### <a name="update-settings-from-control-panel"></a>Update-instellingen van het Configuratiescherm

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. Open het **Configuratiescherm**.

3. Selecteer **Microsoft Monitoring Agent** en klik vervolgens op de **Azure Log Analytics** tabblad.

4. Als u een werkruimte is verwijderd, selecteert u deze en klik vervolgens op **verwijderen**. Herhaal deze stap voor elke andere werkruimte die u wilt dat de agent stoppen met rapporteren aan.

5. Als u een werkruimte toevoegt, klikt u op **toevoegen** en klik op de **een Log Analytics-werkruimte toevoegen** in het dialoogvenster, plak de werkruimte-ID en Werkruimtesleutel (primaire sleutel). Als de computer laten bij een Log Analytics-werkruimte in Azure Government-cloud rapporteren wilt, selecteert u Azure US Government uit de vervolgkeuzelijst Azure-Cloud.

6. Klik op **OK** om uw wijzigingen op te slaan.

#### <a name="remove-a-workspace-using-powershell"></a>Verwijderen van een werkruimte met behulp van PowerShell

```powershell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Een werkruimte toevoegen in Azure commerciële met behulp van PowerShell

```powershell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>Een werkruimte toevoegen in Azure voor de Amerikaanse overheid met behulp van PowerShell

```powershell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Als u eerder hebt gebruikt de opdrachtregel of script te installeren of configureren van de agent `EnableAzureOperationalInsights` is vervangen door `AddCloudWorkspace` en `RemoveCloudWorkspace`.
>

### <a name="linux-agent"></a>Linux-agent
De volgende stappen laten zien hoe u de Linux-agent opnieuw te configureren als u besluit te registreren bij een andere werkruimte of een werkruimte verwijderen uit de configuratie ervan.

1. Als u wilt controleren of dat deze aan een werkruimte is geregistreerd, moet u de volgende opdracht uitvoeren:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Er moet een status die vergelijkbaar is met het volgende voorbeeld retourneren:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

    Het is belangrijk dat de status ook ziet u de agent wordt uitgevoerd, anders de volgende stappen voor het configureren van de agent wordt niet voltooid.

2. Als deze is al geregistreerd bij een werkruimte, moet u de geregistreerde werkruimte verwijderen door de volgende opdracht uit. Anders als deze niet is geregistreerd, gaat u verder met de volgende stap.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -X`

3. Als u wilt registreren met een andere werkruimte, moet u de volgende opdracht uitvoeren:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <shared key> [-d <top level domain>]`
    
4. Als u wilt controleren of dat uw wijzigingen hebben invloed op heeft, moet u de volgende opdracht uitvoeren:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Er moet een status die vergelijkbaar is met het volgende voorbeeld retourneren:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

De agent-service hoeft niet opnieuw worden gestart om de wijzigingen worden doorgevoerd.

## <a name="update-proxy-settings"></a>Proxy-instellingen bijwerken
Het configureren van de agent kan communiceren met de service via een proxyserver of [Log Analytics gateway](gateway.md) na de implementatie, gebruikt u een van de volgende methoden om deze taak te voltooien.

### <a name="windows-agent"></a>Windows-agent

#### <a name="update-settings-using-control-panel"></a>Via het Configuratiescherm-instellingen bijwerken

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. Open het **Configuratiescherm**.

3. Selecteer **Microsoft Monitoring Agent** en klik vervolgens op de **Proxy-instellingen** tabblad.

4. Klik op **Een proxyserver gebruiken** en geef de URL en het poortnummer van de proxyserver of gateway op. Als er voor uw proxyserver of Log Analytics-gateway verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in voor verificatie en klikt u op **OK**.

#### <a name="update-settings-using-powershell"></a>Update-instellingen met behulp van PowerShell

Kopieer de volgende PowerShell-voorbeeldcode, bijwerken met informatie die specifiek zijn voor uw omgeving en opslaan met een PS1-bestandsnaamextensie. Voer het script uit op elke computer die rechtstreeks verbinding met de Log Analytics-werkruimte in Azure Monitor maakt.

```powershell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object. We need to determine if we
#have the right update rollup with the API we need. If not, no need to run the rest of the script.
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
Voer de volgende stappen uit als uw Linux-computers nodig hebt om te communiceren via een proxyserver of Log Analytics-gateway. De waarde voor de proxyconfiguratie heeft de volgende syntaxis `[protocol://][user:password@]proxyhost[:port]`. De eigenschap *proxyhost* accepteert een volledig gekwalificeerde domeinnaam of het IP-adres van de proxyserver.

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
Gebruik een van de volgende procedures voor het verwijderen van de Windows- of Linux-agent met de opdrachtregel of setup-wizard.

### <a name="windows-agent"></a>Windows-agent

#### <a name="uninstall-from-control-panel"></a>Verwijderen van het Configuratiescherm
1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. In **Configuratiescherm**, klikt u op **programma's en onderdelen**.

3. In **programma's en onderdelen**, klikt u op **Microsoft Monitoring Agent**, klikt u op **verwijderen**, en klik vervolgens op **Ja**.

>[!NOTE]
>De Wizard Setup van Agent kan ook worden uitgevoerd door te dubbelklikken op **MMASetup -\<platform\>.exe**, deze is beschikbaar voor downloaden van een werkruimte in de Azure-portal.

#### <a name="uninstall-from-the-command-line"></a>Verwijderen vanaf de opdrachtregel
Het gedownloade bestand voor de agent is een op zichzelf staand installatiepakket met IExpress gemaakt. Het installatieprogramma voor de agent en de ondersteunende bestanden zijn opgenomen in het pakket en moet worden opgehaald wordt geplaatst om correct verwijderen via de opdrachtregel die wordt weergegeven in het volgende voorbeeld.

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. De bestanden van de installatie van agent ophalen uit een opdrachtprompt met verhoogde bevoegdheid uitvoeren `extract MMASetup-<platform>.exe` en wordt u gevraagd voor het pad voor het uitpakken van bestanden. U kunt ook het pad opgeven doordat argumenten worden doorgegeven `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`. Zie voor meer informatie over de opdrachtregelopties die wordt ondersteund door IExpress [opdrachtregelopties voor IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) en werk vervolgens in het voorbeeld aan uw behoeften.

3. Typ bij de opdrachtprompt `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.

### <a name="linux-agent"></a>Linux-agent
Voer de volgende opdracht op de Linux-computer uit om de agent te verwijderen. Met het argument *--purge* worden de agent en de configuratie ervan volledig verwijderd.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Om te rapporteren aan een beheergroep van Operations Manager-agent configureren

### <a name="windows-agent"></a>Windows-agent
Voer de volgende stappen uit voor het configureren van de Log Analytics-agent voor Windows om te rapporteren aan een beheergroep van System Center Operations Manager.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Meld u aan bij de computer met een account dat beheerdersrechten heeft.

2. Open het **Configuratiescherm**.

3. Klik op **Microsoft Monitoring Agent** en klik vervolgens op de **Operations Manager** tabblad.

4. Als uw Operations Manager-beheerservers integratie met Active Directory hebt, klikt u op **automatisch bijwerken van beheergroeptoewijzingen van AD DS**.

5. Klik op **toevoegen** openen de **toevoegen van een beheergroep** in het dialoogvenster.

6. In **beheergroepsnaam** veld, typt u de naam van uw beheergroep.

7. In de **primaire beheerserver** veld, typt u de naam van de computer van de primaire beheerserver.

8. In de **beheerserverpoort** veld, typt u het TCP-poortnummer.

9. Onder **actie-Account van Agent**, kiest u het lokale systeemaccount of een lokale domeinaccount.

10. Klik op **OK** sluiten de **toevoegen van een beheergroep** in het dialoogvenster en klik vervolgens op **OK** sluiten de **eigenschappen van Microsoft Monitoring Agent** in het dialoogvenster.

### <a name="linux-agent"></a>Linux-agent
Voer de volgende stappen uit voor het configureren van de Log Analytics-agent voor Linux om te rapporteren aan een beheergroep van System Center Operations Manager.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Bewerk het bestand `/etc/opt/omi/conf/omiserver.conf`

2. Zorg ervoor dat de regel die begint met `httpsport=` definieert de poort 1270. Zoals: `httpsport=1270`

3. De OMI-server opnieuw opstarten: `sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Volgende stappen

Beoordeling [het oplossen van de Linux-agent](agent-linux-troubleshoot.md) als u problemen ondervindt tijdens het installeren of beheren van de agent.
