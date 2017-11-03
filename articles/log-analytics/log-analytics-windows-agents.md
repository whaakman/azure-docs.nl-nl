---
title: Windows-computers verbinding met Azure Log Analytics | Microsoft Docs
description: In dit artikel bevat de stappen om de Windows-computers in uw on-premises infrastructuur met behulp van een aangepaste versie van de Microsoft Monitoring Agent (MMA) met de Log Analytics-service.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5f04f3b9135167c0f339c58323ebd931b260109
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Windows-computers verbinding met de Log Analytics-service in Azure

In dit artikel bevat de stappen om Windows-computers in uw on-premises infrastructuur met behulp van een aangepaste versie van de Microsoft Monitoring Agent (MMA) met OMS werkruimten. U moet installeren en verbinding maken met agents voor alle computers die u wilt vrijgeven zodat ze gegevens verzenden naar het Log Analytics-service en te werken van die gegevens. Elke agent kan aan meerdere werkruimten rapporteren.

U kunt met behulp van de installatie vanaf de opdrachtregel, agents installeren of met Desired State Configuration (DSC) in Azure Automation.  

>[!NOTE]
Voor virtuele machines in Azure wordt uitgevoerd, kunt u de installatie vereenvoudigen met behulp van de [extensie van virtuele machine](log-analytics-azure-vm-extension.md).

Op computers met een internetverbinding, de agent de verbinding met het Internet gebruikt om gegevens te verzenden aan OMS. Voor computers die u geen verbinding met Internet hebt, kunt u een proxy of de [OMS Gateway](log-analytics-oms-gateway.md).

Uw Windows-computers verbinden met OMS is eenvoudig met behulp van de drie eenvoudige stappen:

1. Het installatiebestand van de agent downloaden vanuit de OMS-portal
2. De agent installeert met welke methode die u kiest
3. De agent configureren of het toevoegen van extra werkruimten, indien nodig

Het volgende diagram toont de relatie tussen uw Windows-computers en OMS nadat u hebt geïnstalleerd en geconfigureerd agents.

![OMS-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Als de beleidsregels van uw IT-beveiliging niet toestaan computers in uw netwerk verbinding maken met Internet dat, kunt u uw computers verbinding maken met de OMS-Gateway configureren. Zie voor meer informatie en stapsgewijze instructies voor het configureren van uw servers om te communiceren via een OMS-Gateway met de OMS-service [computers koppelen aan OMS met behulp van de Gateway OMS](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Systeemvereisten en de vereiste configuratie
Voordat u installeert of agents te implementeren, controleert u de volgende informatie om te controleren of u aan de vereisten voldoet.

- U kunt de OMS MMA alleen installeren op computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger.
- U moet een Azure-abonnement.  Zie voor meer informatie [aan de slag met logboekanalyse](log-analytics-get-started.md).
- Elke Windows-computer moet verbinding maken met Internet via HTTPS of met de OMS-Gateway. Deze verbinding kan worden direct, via een proxy of via de OMS-Gateway.
- U kunt de OMS MMA installeren op zelfstandige computers, servers en virtuele machines. Als u wilt dat Azure gehoste virtuele machines te verbinden met OMS, Zie [verbinding maken met Azure virtuele machines met logboekanalyse](log-analytics-azure-vm-extension.md).
- De agent moet TCP-poort 443 gebruikt voor verschillende resources.

### <a name="network"></a>Netwerk

Voor Windows-agents verbinding maken met en registreren met de OMS-service, moeten ze toegang tot netwerkbronnen, inclusief de poortnummers en domein-URL's hebben.

- Voor proxyservers moet u ervoor zorgen dat de juiste resources voor de proxyserver zijn geconfigureerd in de instellingen voor de agent.
- Voor firewalls die toegang tot Internet beperken, moeten u of uw netwerken engineers uw firewall om toegang te verlenen aan OMS configureren. De agent-instellingen hoeven niet te worden aangepast.

De volgende tabel bevat de resources die nodig zijn voor communicatie.

>[!NOTE]
>Sommige van de volgende bronnen vermeld operationeel inzicht, waarop een vorige naam op voor logboekanalyse is.

| Agentresource | Poorten | HTTPS-controle overslaan |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |



## <a name="download-the-agent-setup-file-from-oms"></a>Download het installatiebestand van de agent van OMS
1. In de [OMS-portal](https://www.mms.microsoft.com)op de **overzicht** pagina, klikt u op de **instellingen** tegel.  Klik op de **verbonden bronnen** boven op het tabblad.  
    ![Tabblad verbonden bronnen](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klik op **Windows-Servers** en klik vervolgens op **Windows-Agent downloaden** van toepassing op uw computer processortype voor het downloaden van het setup-bestand.
3. Aan de rechterkant van **werkruimte-ID**, klikt u op het pictogram kopiëren en plakken van de ID in Kladblok.
4. Aan de rechterkant van **primaire sleutel**, klikt u op het pictogram kopiëren en plakken van de sleutel in Kladblok.     

## <a name="install-the-agent-using-setup"></a>Installeer de agent via setup
1. Voer de installatie voor de installatie van de agent op een computer die u wilt beheren.
2. Klik op de welkomstpagina op **volgende**.
3. Lees de licentievoorwaarden op de pagina licentievoorwaarden en klik vervolgens op **ik ga akkoord**.
4. Op de pagina doelmap wijzigen of de standaardinstallatiemap houden en klik vervolgens op **volgende**.
5. Op de pagina installatieopties voor Agent kunt u de agent verbinden met naar Azure logboekanalyse (OMS), Operations Manager, of u kunt de keuzes leeg laten als u wilt dat de agent later configureren. Klik op **Volgende**.   
    - Als u wilt verbinding maken naar Azure logboekanalyse (OMS), plak de **werkruimte-ID** en **Werkruimtesleutel (primaire sleutel)** die u in de vorige procedure hebt gekopieerd in Kladblok en klik vervolgens op **volgende**.  
        ![Werkruimte-ID en Primary Key plakken](./media/log-analytics-windows-agents/connect-workspace.png)
    - Als u verbinden met Operations Manager wilt, typt u de **Beheergroepsnaam**, **beheerserver** naam, en **Beheerserverpoort**, en klik vervolgens op **volgende**. Het lokale systeemaccount of een lokale account kiezen op de pagina Agentactie-Account en klik vervolgens op **volgende**.  
        ![beheergroepconfiguratie](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![actie-account van agent](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Controleer uw selecties op de pagina Ready to Install en klik vervolgens op **installeren**.
7. Pagina van de configuratie is voltooid, klikt u op **voltooien**.
8. Na voltooiing wordt de **Microsoft Monitoring Agent** wordt weergegeven in **Configuratiescherm**. U kunt de configuratie er bekijken en controleren of de agent aan Operational Insights (OMS) is verbonden. Wanneer verbonden met OMS, de agent wordt weergegeven voor een bericht weergegeven: **de Microsoft Monitoring Agent verbonden is met de Microsoft Operations Management Suite-service.**

## <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Volg de volgende procedure om de proxyinstellingen voor Microsoft Monitoring Agent te configureren via het Configuratiescherm. U moet deze procedure gebruiken voor elke server. Als er veel servers zijn die u moet configureren, is het wellicht eenvoudiger om een script te gebruiken om dit proces te automatiseren. Zie de volgende procedure [Proxyinstellingen met een script configureren voor de Microsoft Monitoring Agent](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script) als u dit wilt doen.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Proxyinstellingen via het Configuratiescherm configureren voor de Microsoft Monitoring Agent
1. Open het **Configuratiescherm**.
2. Open **Microsoft Monitoring Agent**.
3. Klik op het tabblad **Proxyinstellingen**.  
    ![tabblad proxyinstellingen](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Selecteer **Een proxyserver gebruiken** en voer de URL en het poortnummer in (als er een poortnummer is vereist), zoals hieronder wordt weergegeven. Als er voor uw proxyserver verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in om de proxyserver te openen.


### <a name="verify-agent-connectivity-to-oms"></a>Controleer of de agent-connectiviteit met OMS

U kunt eenvoudig controleren of uw agents communiceert met de volgende procedure OMS:

1.  Open het Configuratiescherm op de computer met de Windows-agent.
2.  Open Microsoft Monitoring Agent.
3.  Klik op het tabblad Azure logboekanalyse (OMS).
4.  U ziet in de kolom Status van de agent verbonden is met de Operations Management Suite-service.

![Agent verbonden](./media/log-analytics-windows-agents/mma-connected.png)


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



## <a name="install-the-agent-using-the-command-line"></a>Installeer de agent via de opdrachtregel
- Wijzigen en gebruik vervolgens het volgende voorbeeld voor het installeren van de agent via de opdrachtregel. Het voorbeeld wordt een volledig stille installatie uitgevoerd.

    >[!NOTE]
    Als u een agent bijwerken wilt, moet u de logboekanalyse scripting API gebruiken. Zie de volgende sectie een agent bijwerken.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

De agent IExpress gebruikt als de zelfstandig uitpakken met behulp van de `/c` opdracht. Ziet u de opdrachtregelopties op [opdrachtregelopties voor IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) en werk vervolgens in het voorbeeld aan uw behoeften.

|MMA-specifieke opties                   |Opmerkingen         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = de agent voor het rapporteren van een werkruimte configureren                |
|OPINSIGHTS_WORKSPACE_ID                | Werkruimte-Id (guid) voor de werkruimte toevoegen                    |
|OPINSIGHTS_WORKSPACE_KEY               | Werkruimtesleutel in eerste instantie wordt geverifieerd met de werkruimte |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Geef de cloudomgeving waar de werkruimte zich bevindt <br> 0 = azure commerciële cloudservice (standaard) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | URI voor de proxy te gebruiken |
|OPINSIGHTS_PROXY_USERNAME               | Gebruikersnaam voor toegang tot een geverifieerde proxyserver |
|OPINSIGHTS_PROXY_PASSWORD               | Wachtwoord voor toegang tot een geverifieerde proxyserver |

>[!NOTE]
Installeer de agent met geen werkruimte geconfigureerd om te voorkomen kunt u door op de opdrachtregel lengtelimiet van IExpress, en een script vervolgens gebruiken voor het instellen van de configuratie voor de werkruimte.

>[!NOTE]
Als u krijgt een `Command line option syntax error.` bij gebruik van de `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, kunt u de volgende tijdelijke oplossing:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Een werkruimte met een script toevoegen
Toevoegen van een werkruimte voor logboekanalyse agent scripting API gebruiken met het volgende voorbeeld:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

U kunt een werkruimte toevoegen in Azure voor US Government met het volgende voorbeeldscript:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Als u eerder hebt gebruikt de opdrachtregel of script te installeren of configureren van de agent `EnableAzureOperationalInsights` is vervangen door `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>De agent installeert met DSC in Azure Automation

Het volgende scriptvoorbeeld kunt u de agent installeert met DSC in Azure Automation. In het voorbeeld installeert de 64-bits agent, geïdentificeerd door de `URI` waarde. U kunt ook de 32-bits versie door de URI-waarde te vervangen. De URI voor beide versies zijn:

- Windows 64-bits agent - https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bits agent - https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
In dit voorbeeld procedure en het script wordt een bestaand agent niet bijgewerkt.

1. Importeer de DSC xPSDesiredStateConfiguration Module op basis van [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure Automation.  
2.  Maken van Azure Automation-variabele assets voor *OPSINSIGHTS_WS_ID* en *OPSINSIGHTS_WS_KEY*. Stel *OPSINSIGHTS_WS_ID* tot uw logboekanalyse OMS-werkruimte-ID en een set *OPSINSIGHTS_WS_KEY* op de primaire sleutel van uw werkruimte.
3.  Gebruik het volgende script en sla deze op als MMAgent.ps1
4.  Wijzigen en vervolgens het volgende voorbeeld met de agent installeert met DSC in Azure Automation. MMAgent.ps1 in Azure Automation importeren met behulp van de Azure Automation-interface of de cmdlet.
5.  Een knooppunt toewijzen aan de configuratie. Het knooppunt controleert de configuratie en de MMA wordt doorgeschoven, is naar het knooppunt binnen 15 minuten.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a>De meest recente product-id-waarde niet verkrijgen

De `ProductId value` in de MMAgent.ps1 script is uniek voor elke agentversie. Wanneer een bijgewerkte versie van elke agent wordt gepubliceerd, verandert de product-id-waarde. Dus als de product-id in de toekomst wordt gewijzigd, u de versie van de beveiligingsagent met een eenvoudige script vinden kunt. Nadat u de meest recente versie van de beveiligingsagent geïnstalleerd op een testserver hebt, kunt u het volgende script het geïnstalleerde product-id-waarde op te halen. De laatste waarde van de product-id kunt u de waarde in het script MMAgent.ps1 bijwerken.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Een agent handmatig configureren of extra werkruimten toevoegen
Als u agents hebt geïnstalleerd, maar is niet geconfigureerd of als u wilt dat de agent voor het rapporteren van meerdere werkruimten, kunt u de volgende informatie op een agent inschakelen of configureer het opnieuw. Nadat u de agent hebt geconfigureerd, worden geregistreerd bij de agent-service en krijgen de vereiste configuratiegegevens en management packs die oplossingsgegevens bevatten.

1. Nadat u Microsoft Monitoring Agent hebt geïnstalleerd, opent u **Configuratiescherm**.
2. Open **Microsoft Monitoring Agent** en klik vervolgens op de **Azure logboekanalyse (OMS)** tabblad.   
3. Klik op **toevoegen** openen de **Log Analytics-werkruimte toevoegen** vak.
4. Plak de **werkruimte-ID** en **Werkruimtesleutel (primaire sleutel)** die u hebt gekopieerd in Kladblok in een eerdere procedure voor de werkruimte die u wilt toevoegen en klik op **OK**.  
    ![Configureer operationeel inzicht](./media/log-analytics-windows-agents/add-workspace.png)

Nadat de gegevens worden verzameld van computers die worden bewaakt door de agent, wordt het aantal computers worden bewaakt door OMS weergegeven in de OMS-portal op de **verbonden bronnen** tabblad **instellingen** als **Servers verbonden**.


## <a name="to-disable-an-agent"></a>Een agent uitschakelen
1. Open na de installatie van de agent **Configuratiescherm**.
2. Microsoft Monitoring Agent te openen en klik op de **Azure logboekanalyse (OMS)** tabblad.
3. Selecteer een werkruimte en klik vervolgens op **verwijderen**. Herhaal deze stap voor alle andere werkruimten.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Configureer desgewenst agents om te rapporteren aan een beheergroep van Operations Manager

Als u Operations Manager in uw IT-infrastructuur, kunt u de agent MMA ook gebruiken als een Operations Manager-agent.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>MMA agents om te rapporteren aan een Operations Manager-beheergroep configureren
1.  Open op de computer waarop de agent is geïnstalleerd, **Configuratiescherm**.  
2.  Open **Microsoft Monitoring Agent** en klik vervolgens op de **Operations Manager** tabblad.  
    ![Microsoft Monitoring Agent Operations Manager-tabblad](./media/log-analytics-windows-agents/om-mg01.png)
3.  Als uw Operations Manager-servers integratie met Active Directory hebt, klikt u op **automatisch bijwerken van beheergroeptoewijzingen van AD DS**.
4.  Klik op **toevoegen** openen de **toevoegen van een beheergroep** in het dialoogvenster.  
    ![Microsoft Monitoring Agent een beheergroep toevoegen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  In **beheergroepsnaam** typt u de naam van uw beheergroep.
6.  In de **primaire beheerserver** typt u de computernaam van de primaire beheerserver.
7.  In de **beheerserverpoort** typt u het TCP-poortnummer.
8.  Onder **Agentactieacount**, kiest u het lokale systeemaccount of een lokale account.
9.  Klik op **OK** sluiten de **toevoegen van een beheergroep** in het dialoogvenster en klik vervolgens op **OK** sluiten de **eigenschappen van Microsoft Monitoring Agent** in het dialoogvenster.


## <a name="next-steps"></a>Volgende stappen

- [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
