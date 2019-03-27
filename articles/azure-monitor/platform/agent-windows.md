---
title: Windows-computers verbinden met Azure Log Analytics | Microsoft Docs
description: In dit artikel wordt beschreven hoe u verbinding maken met Windows-computers die worden gehost in andere clouds of on-premises naar Log Analytics met de Microsoft Monitoring Agent (MMA).
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
ms.date: 03/12/2019
ms.author: magoedte
ms.openlocfilehash: c7031e54c354392379fee83dbf2a777ba726c5e7
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480050"
---
# <a name="connect-windows-computers-to-azure-monitor"></a>Windows-computers verbinden met Azure Monitor

Als u wilt bewaken en beheren van virtuele machines of fysieke computers in uw lokale datacenter of andere cloudomgeving met Azure Monitor, moet u de Log Analytics-agent (ook aangeduid als de Microsoft Monitoring Agent (MMA)) implementeren en deze configureren rapporteren aan een of meer Log Analytics-werkruimten. De agent biedt ook ondersteuning voor de functie Hybrid Runbook Worker voor Azure Automation.  

De agent wordt op een bewaakte Windows-computer wordt vermeld als de service Microsoft Monitoring Agent. De service Microsoft Monitoring Agent verzamelt gebeurtenissen van logboekbestanden en Windows-gebeurtenislogboek, prestatiegegevens en andere telemetrie. Zelfs als de agent kan niet communiceren met Azure Monitor rapporteert, wordt de agent blijft om uit te voeren en de verzamelde gegevens op de schijf van de bewaakte computer in de wachtrij geplaatst. Wanneer de verbinding wordt hersteld, verzendt de Microsoft Monitoring Agent-service de verzamelde gegevens naar de service.

De agent kan worden geïnstalleerd met behulp van een van de volgende methoden. De meeste installaties gebruik een combinatie van deze methoden voor het installeren van verschillende sets computers, waar nodig.  Later in dit artikel vindt u informatie over het gebruik van elke methode.

* Handmatige installatie. Setup handmatig op de computer met de wizard setup vanaf de opdrachtregel wordt uitgevoerd of geïmplementeerd met behulp van een bestaand hulpprogramma voor softwaredistributie.
* Azure Automation Desired State Configuration (DSC). Met behulp van DSC in Azure Automation met een script voor Windows-computers die al zijn geïmplementeerd in uw omgeving.  
* PowerShell-script.
* Resource Manager-sjabloon voor virtuele machines waarop Windows wordt uitgevoerd on-premises beschikken in Azure Stack.  

>[!NOTE]
>Azure Security Center (ASC) is afhankelijk van de Microsoft Monitoring Agent (ook wel de Log Analytics-Windows-agent genoemd) en installeert en configureert om te rapporteren aan een Log Analytics-werkruimte als onderdeel van de implementatie. ASC bevat een automatische inrichting optie dat automatische installatie van de Log Analytics-Windows-agent op alle virtuele machines in uw abonnement en configureert deze om te rapporteren aan een specifieke werkruimte. Zie voor meer informatie over deze optie [automatische inrichting van Log Analytics-agent inschakelen](../../security-center/security-center-enable-data-collection.md#enable-automatic-provisioning-of-microsoft-monitoring-agent-).
>

Als u meer wilt weten over de ondersteunde configuratie, kunt u de informatie over [ondersteunde Windows-besturingssystemen](log-analytics-agent.md#supported-windows-operating-systems) en de [ netwerkconfiguratie voor de firewall](log-analytics-agent.md#network-firewall-requirements) raadplegen.

## <a name="obtain-workspace-id-and-key"></a>Werkruimte-ID en -sleutel ophalen
Voordat u de Log Analytics-agent voor Windows installeert, moet u de werkruimte-ID en sleutel voor uw Log Analytics-werkruimte.  Deze informatie is vereist tijdens de installatie van elke installatiemethode correct configureren van de agent en ervoor zorgen dat deze kan communiceren met Azure Monitor in Azure commerciële en US Government-cloud.  

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. In de lijst met Log Analytics-werkruimten, selecteer de werkruimte die u van plan bent over het configureren van de agent om te rapporteren aan.
3. Selecteer **Geavanceerde instellingen**.<br><br> ![Geavanceerde instellingen van Log Analytics](media/agent-windows/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **Verbonden bronnen** en selecteer vervolgens **Windows-servers**.   
5. Kopiëren en plakken in uw favoriete editor de **werkruimte-ID** en **primaire sleutel**.    
   
## <a name="configure-agent-to-use-tls-12"></a>Voor het gebruik van TLS 1.2-Agent configureren
Het configureren van gebruik van de [TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) protocol voor communicatie tussen de Windows-agent en de Log Analytics-service, u kunt de volgende stappen om in te schakelen voordat de agent is geïnstalleerd op de virtuele machine of later.   

1. Ga naar de volgende registersubsleutel: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**
2. Maak een subsleutel onder **protocollen** voor TLS 1.2 **HKLM\System\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2**
3. Maak een **Client** subsleutels onder de subsleutel TLS 1.2-protocol versie u eerder hebt gemaakt. Bijvoorbeeld, **HKLM\System\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**.
4. Maken van de volgende DWORD-waarden onder **HKLM\System\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**:

    * **Ingeschakeld** [waarde = 1]
    * **DisabledByDefault** [waarde = 0]  

Configureren van .NET Framework 4.6 of later cryptografie, als door het standaard ter ondersteuning van veilige is uitgeschakeld. De [sterke cryptografie](https://docs.microsoft.com/dotnet/framework/network-programming/tls#schusestrongcrypto) maakt gebruik van veiligere netwerkprotocollen, zoals TLS 1.2 en protocollen blokkeert die niet beveiligd zijn. 

1. Ga naar de volgende registersubsleutel: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319**.  
2. Maken van de DWORD-waarde **schusestrongcrypto toe** onder deze subsleutel met de waarde **1**.  
3. Ga naar de volgende registersubsleutel: **HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\.NETFramework\v4.0.30319**.  
4. Maken van de DWORD-waarde **schusestrongcrypto toe** onder deze subsleutel met de waarde **1**. 
5. Het systeem om de instellingen van kracht opnieuw opstarten. 

## <a name="install-the-agent-using-setup-wizard"></a>De agent installeren met de wizard setup
De volgende stappen uit installeren en configureren van de Log Analytics-agent in Azure en Azure Government-cloud met behulp van de wizard setup voor de agent op uw computer. Als u wilt meer informatie over het configureren van de agent ook een rapport met een beheergroep van System Center Operations Manager, Zie [Operations Manager-agent met de Wizard Setup van Agent implementeren](https://docs.microsoft.com/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard).

1. In uw Log Analytics-werkruimte van de **Windows Servers** pagina die u eerder hebt, selecteert u de juiste navigeren **Windows-Agent downloaden** versie te downloaden, afhankelijk van de processorarchitectuur van het Windows-besturingssysteem.   
2. Voer Setup uit om de agent op de computer te installeren.
2. Klik op de pagina **Welkom** op **Volgende**.
3. Lees de licentie op de pagina **Licentievoorwaarden** en klik op **Akkoord**.
4. Op de pagina **Doelmap** wijzigt u desgewenst de standaardinstallatiemap en klikt u op **Volgende**.
5. Op de pagina **Installatieopties voor agent** kiest u ervoor de agent verbinding te laten maken met Azure Log Analytics en klikt u op **Volgende**.   
6. Ga op de pagina **Azure Log Analytics** als volgt te werk:
   1. Plak de **Werkruimte-id** en **Werkruimtesleutel (primaire sleutel)** die u eerder hebt gekopieerd.  Als u de computer wilt laten rapporteren bij een Log Analytics-werkruimte in de Azure Government-cloud, selecteert u **Azure US Government** in de vervolgkeuzelijst **Azure Cloud**.  
   2. Als de computer met de Log Analytics-service moet communiceren via een proxyserver, klikt u op **Geavanceerd** en geeft u de URL en het poortnummer van de proxyserver op.  Als er voor uw proxyserver verificatie is vereist, voert u de gebruikersnaam en het wachtwoord in voor verificatie met de proxyserver. Klik vervolgens op **Volgende**.  
7. Klik op **Volgende** als u de vereiste configuratie-instellingen hebt voltooid.<br><br> ![Werkruimte-id en primaire sleutel plakken](media/agent-windows/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Controleer op de pagina **Gereed om te installeren** uw keuzes en klik op **Installeren**.
9. Klik op de pagina **Configuratie voltooid** op **Voltooien**.

Als u klaar bent wordt de **MMA** in het **Configuratiescherm** weergegeven. Als u wilt controleren of deze rapporteert aan Log Analytics, Bekijk [of agents verbonden zijn met Log Analytics](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>De agent installeren met de opdrachtregel
Het gedownloade bestand voor de agent is een op zichzelf staand installatiepakket.  Het installatieprogramma voor de agent en de ondersteunende bestanden zijn opgenomen in het pakket en moeten worden geëxtraheerd om te kunnen correct installeren via de opdrachtregel die wordt weergegeven in de volgende voorbeelden.    

>[!NOTE]
>Als u een agent bijwerkt wilt, moet u de Log Analytics scripting API gebruiken. Zie het onderwerp [beheren en onderhouden van de Log Analytics-agent voor Windows en Linux](agent-manage.md) voor meer informatie.

De volgende tabel ziet u de specifieke parameters die worden ondersteund door het installatieprogramma voor de agent, inclusief wanneer geïmplementeerd met behulp van Automation DSC.

|MMA-specifieke opties                   |Opmerkingen         |
|---------------------------------------|--------------|
| NOAPM=1                               | Een optionele parameter. Installeert de agent zonder .NET Application Performance Monitoring.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = de agent om te rapporteren aan een werkruimte configureren                |
|OPINSIGHTS_WORKSPACE_ID                | Werkruimte-ID (guid) voor de werkruimte toe te voegen                    |
|OPINSIGHTS_WORKSPACE_KEY               | De sleutel van de werkruimte is in eerste instantie wordt geverifieerd met de werkruimte |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Geef de cloudomgeving waar de werkruimte zich bevindt <br> 0 = commerciële azure-cloud (standaard) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | URI voor de proxy gebruiken |
|OPINSIGHTS_PROXY_USERNAME               | Gebruikersnaam voor toegang tot een geverifieerde proxyserver |
|OPINSIGHTS_PROXY_PASSWORD               | Wachtwoord voor toegang tot een geverifieerde proxyserver |

1. De bestanden van de installatie van agent ophalen uit een opdrachtprompt met verhoogde bevoegdheid uitvoeren `MMASetup-<platform>.exe /c` en wordt u gevraagd voor het pad voor het uitpakken van bestanden.  U kunt ook het pad opgeven doordat argumenten worden doorgegeven `MMASetup-<platform>.exe /c /t:<Full Path>`.  
2. Als u op de achtergrond de agent installeren en configureren zodat deze rapporteert aan een werkruimte in de commerciële Azure-cloud, vanuit de map wilt u de setup-bestanden naar het type uitgepakt: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID=<your workspace ID> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

   of voor het configureren van de agent om te rapporteren aan Azure US Government-cloud, typ: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace ID> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installeer de agent met behulp van DSC in Azure Automation

U kunt het volgende voorbeeldscript voor het installeren van de agent met behulp van Azure Automation DSC.   Als u een Automation-account hebt, raadpleegt u [aan de slag met Azure Automation](/azure/automation/) voor informatie over vereisten en stappen voor het maken van een Automation-account vereist voordat u Automation DSC.  Als u niet bekend met Automation DSC bent, raadpleegt u [aan de slag met Automation DSC](../../automation/automation-dsc-getting-started.md).

Het volgende voorbeeld installeert de 64-bits agent, geïdentificeerd door de `URI` waarde. U kunt ook de 32-bits versie gebruiken door de URI-waarde te vervangen. De URI's voor beide versies zijn:

- Windows 64-bits agent- https://go.microsoft.com/fwlink/?LinkId=828603
- Agent voor Windows 32-bits- https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>In dit voorbeeld procedure en script biedt geen ondersteuning voor het bijwerken van de agent al is geïmplementeerd op een Windows-computer.

De 32-bits en 64-bits versies van de agentpakket hebben verschillende productcodes en nieuwe versies uitgebracht ook een unieke waarde bevatten.  De productcode is een GUID die de principal-id van een toepassing of het product en wordt vertegenwoordigd door het Windows-installatieprogramma **ProductCode** eigenschap.  De `ProductId` waarde in de **MMAgent.ps1** script moet overeenkomen met de productcode uit het 32-bits of 64-bits agent installer-pakket.

Als u wilt de productcode rechtstreeks vanaf het agent-installatiepakket ophalen, kunt u Orca.exe uit de [Windows SDK-onderdelen voor Windows Installer-ontwikkelaars](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) dat wil zeggen een onderdeel van de Windows Software Development Kit of met behulp van De volgende PowerShell een [voorbeeldscript](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) geschreven door een Microsoft Valuable Professional (MVP).  Voor beide benaderingen, moet u eerst om op te halen de **MOMagent.msi** -bestand van het installatiepakket MMASetup.  Dit wordt weergegeven in de eerste stap in de sectie eerder [de agent installeren met de opdrachtregel](#install-the-agent-using-the-command-line).  

1. Importeren van de DSC xPSDesiredStateConfiguration Module op basis van [ https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration ](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure Automation.  
2.  Variabele assets voor Azure Automation maken *OPSINSIGHTS_WS_ID* en *OPSINSIGHTS_WS_KEY*. Stel *OPSINSIGHTS_WS_ID* naar uw Log Analytics-werkruimte-ID en stel *OPSINSIGHTS_WS_KEY* op de primaire sleutel van uw werkruimte.
3.  Kopieer het script en sla deze op als MMAgent.ps1.

    ```powershell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration
        Import-DscResource -ModuleName PSDesiredStateConfiguration

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
                Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. Update de `ProductId` waarde in het script met de productcode opgehaald uit de meest recente versie van de agent installeren met behulp van de methoden die eerder aanbevolen pakket. 
5. [Importeer het script van de configuratie van MMAgent.ps1](../../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) in uw Automation-account. 
5. [Toewijzen van een Windows-computer of een knooppunt](../../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) aan de configuratie. Het knooppunt controleert de configuratie ervan binnen 15 minuten en de agent wordt doorgestuurd naar het knooppunt.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Controleer de agent verbinding met Log Analytics

Zodra de installatie van de agent is voltooid, is controle is verbonden en rapportage kan worden geconfigureerd op twee manieren.  

Bij de computer in **Configuratiescherm**, zoek het item **Microsoft Monitoring Agent**.  Selecteer deze en klik op de **Azure Log Analytics** tabblad en de agent moet worden weergegeven een bericht weergegeven: **De Microsoft Monitoring Agent verbonden is met de Microsoft Operations Management Suite-service.**<br><br> ![Verbindingsstatus van MMA met Log Analytics](media/agent-windows/log-analytics-mma-laworkspace-status.png)

U kunt ook een eenvoudige logboekquery uitvoeren in Azure portal.  

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Azure Monitor**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Azure Monitor**.  
2. Selecteer **logboeken** in het menu. 
2. In het deelvenster Logboeken in het querytype-veld:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

U ziet in de lijst met zoekresultaten geretourneerd heartbeat-records voor de computer waarmee wordt aangegeven dat is verbonden en te rapporteren aan de service.   

## <a name="next-steps"></a>Volgende stappen

Beoordeling [beheren en onderhouden van de Log Analytics-agent voor Windows en Linux](agent-manage.md) voor meer informatie over het beheren van de agent tijdens de levenscyclus van de implementatie op uw virtuele machines.  
