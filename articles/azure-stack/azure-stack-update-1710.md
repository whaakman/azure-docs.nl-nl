---
title: Azure Stack 1710 Update (Build 20171020.1) | Microsoft Docs
description: Meer informatie over wat er in de update 1710 voor Azure Stack geïntegreerde systemen, de bekende problemen en het downloaden van de update.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: cf870551a3dbd9b5ea0ef6f886dc6451e43b2c25
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377187"
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure Stack 1710 Update (Build 20171020.1)

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel worden de verbeteringen en correcties in deze updatepakket, bekende problemen voor deze release en waar u de update te downloaden. Bekende problemen zijn onderverdeeld in bekende problemen direct met betrekking tot het updateproces en bekende problemen met de build (na de installatie).

> [!IMPORTANT]
> Dit pakket is alleen voor geïntegreerde Azure Stack-systemen. Dit pakket niet van toepassing op Azure Stack Development Kit.

## <a name="improvements-and-fixes"></a>Verbeteringen en oplossingen

Deze update bevat de volgende verbeteringen van de kwaliteit en oplossingen voor Azure Stack.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 verbeteringen en oplossingen

- Updates voor WindowsServer 2016: 10 oktober 2017: KB4041691 (Build van besturingssysteem 14393.1770. Zie [ https://support.microsoft.com/help/4041691 ](https://support.microsoft.com/help/4041691) voor meer informatie.

### <a name="additional-quality-improvements-and-fixes"></a>Aanvullende kwaliteit verbeteringen en oplossingen

- Toegevoegd bevoegde eindpunt PowerShell-cmdlets om u te helpen bij het oplossen en het netwerk tijd Protocol (NTP)-server bijwerken.
- Er is ondersteuning toegevoegd voor het bijwerken van de bevoegde eindpunt alleen Enough Administration (JEA) eindpunt modules en cmdlet-whitelist. 
- Fouten voor lokale talen, het eindpunt van de bevoegde vast.
- De mogelijkheid om te roteren van de gateway is toegevoegd.
- De CBLocalAdmin lokale administrator-account verwijderd. 
- De inhoud van de heartbeat alert sjabloon vast om te maken of heartbeat waarschuwingen werk correct na een update.
- De Fabric-resourceprovider om op te lossen time-outs tijdens de bewerkingen voor FRU opgelost. 
- De mogelijkheid voor cloudontwikkelaars naar Azure Resource Manager API profielen gebruiken in Azure Stack toegevoegd.
- De Windows Update-service op de implementatie van virtuele machine (DVM) uitgeschakeld. 
- De kracht van het knooppunt aan/uit-acties verwijderd uit de gebruikersinterface.
- Verschillende andere prestaties en stabiliteit oplossingen. 
 
## <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces

In deze sectie bevat bekende problemen die tijdens de installatie van de update 1710 optreden kunnen.

> [!IMPORTANT]
> Als de update is mislukt, als u later opnieuw probeert te hervatten van de update moet u de `Resume-AzureStackUpdate` cmdlet van het eindpunt van de bevoegdheden. De update niet hervatten met behulp van de beheerdersportal. (Dit is een bekend probleem in deze release.) Zie voor meer informatie, [-updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren](azure-stack-monitor-update.md).

| Symptoom  | Oorzaak  | Oplossing |
|---------|---------|---------|
|Wanneer u een update, een fout die vergelijkbaar is met het volgende uitvoeren<br>zich kunnen voordoen tijdens de stap 'Storage Hosts opnieuw Opslagknooppunt'<br> van de planning van de update-actie.<br><br>**{'naam': 'Opnieuw opstarten opslag Hosts', 'description': ' opnieuw opstarten<br> opslag Hosts. "," foutbericht ":" Typ 'Restart' van de rol<br> 'BareMetal' Deze gebeurtenis treedt een uitzondering: \n\n de computer<br> hostnaam-05 is overgeslagen. Kunnen niet worden opgehaald van de LastBootUpTime<br> via de WMI-service met de volgende strekking weergegeven:<br> de RPC-server is niet beschikbaar.<br> (Uitzondering van HRESULT: 0x800706BA). \nat opnieuw opstarten-Host** | Dit probleem wordt veroorzaakt door een mogelijke stuurprogramma aanwezig zijn in sommige configuraties. | 1. Aanmelden bij de webinterface van baseboard management controller (BMC) en start opnieuw op de host die wordt geïdentificeerd in het foutbericht.<br><br>2. De update met behulp van het eindpunt van de bevoegde hervatten. |
| Als u een update uitvoert, het updateproces wordt weergegeven op i/o<br> en niet wordt uitgevoerd na de stap ' stap: stap 2.4 - installatie wordt uitgevoerd<br> bijwerken' van het plan van de actie update.<br><br>Deze stap wordt gevolgd door een reeks kopieerprocessen van .nupkg<br> bestanden naar de interne infrastructuur-bestandsshares. Bijvoorbeeld:<br><br>**1-bestanden kopiëren vanaf content\PerfCollector\VirtualMachines naar <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**<br><br>Of het bericht wordt weergegeven:<br><br>**WarningMessage:Task: Aanroep van de interface 'LiveUpdate'<br> van rol Cloud\Fabric\VirtualMachines is mislukt:<br> Type 'LiveUpdate' rol 'Informatie' Deze gebeurtenis treedt op een<br> uitzondering: Er is onvoldoende ruimte op de schijf .**  | Het probleem wordt veroorzaakt door logboekbestanden van het invullen van de schijven op een virtuele machine van de infrastructuur en een probleem in Windows Server Scale-Out bestandsserver (SOFS) die worden geleverd in een toekomstige update. | Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp. | 
| Wanneer u een update, een fout die vergelijkbaar is met het volgende uitvoeren<br> zich kunnen voordoen tijdens de stap ' stap: stap 2.13.2 - Update wordt uitgevoerd<br> *VM_Name*'van de update actieplan bevat. (De virtuele machine<br> naam kan variëren.)<br><br>**ActionPlanInstanceWarning EEG/MachineName:<br> WarningMessage:Task: aanroep van de interface 'LiveUpdate' van<br> rol Cloud\Fabric\WAS is mislukt: Type rol 'LiveUpdate'<br> 'Is' heeft een uitzondering veroorzaakt: fout tijdens het opslag<br> initialisatie: Er is een fout opgetreden tijdens het maken van een API<br> aanroepen naar Microsoft Storage-service: {"Message": "Er is een time-out<br> is opgetreden tijdens het communiceren met Service Fabric.<br> Uitzonderingstype: TimeoutException.<br> Bericht van uitzondering: bewerking is een time-out. "}**  | Het probleem wordt veroorzaakt door een i/o-out in Windows Server die wordt opgelost in een toekomstige update. | Neem voor hulp contact op met Microsoft CSS.
| Wanneer u een update, een fout die vergelijkbaar is met het volgende uitvoeren<br> treedt op tijdens de stap 'stap 21 SQL server virtuele machines opnieuw opstarten."<br><br>**Typ 'LiveUpdateRestart' van de rol informatie gegeven een<br> uitzondering: VerboseMessage: [VirtualMachines:LiveUpdateRestart]<br> query's uitvoeren voor VM-MachineName-Sql01. - 10/13/2017 17:11:50 uur VerboseMessage: [virtuele machines: LiveUpdateRestart]<br> VM is gemarkeerd als HighlyAvailable.: 10/13/2017 17:11:50 uur<br> VerboseMessage: [VirtualMachines:LiveUpdateRestart] op<br>MS. Internal.ServerClusters.ExceptionHelp.Build op<br>MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(Booleaanse forceren) op Microsoft.FailoverClusters.PowerShell.<br> StopClusterResourceCommand.BeginTimedOperation() op <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>ProcessRecord() op Microsoft.FailoverClusters.PowerShell.<br> FCCmdlet.ProcessRecord() - 10/13/2017 17:11:50 uur waarschuwing<br>bericht: taak: aanroep van de interface 'LiveUpdateRestart' van<br> rol Cloud\Fabric\VirtualMachines is mislukt:** | Dit probleem kan optreden als de virtuele machine kan niet opnieuw opstarten is. | Neem voor hulp contact op met Microsoft CSS.
| Als u een update uitvoert, kan een vergelijkbaar met de volgende fout optreden:<br><br>**2017-10-22T01:37:37.5369944Z Type 'Afsluiten' van de rol 'SQL'<br> heeft een uitzondering gemeld: Er is een fout opgetreden knooppunt<br> 's45r1004-Sql01' clientcommunicatie Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\ Applications\ EnterpriseCloud <br>EngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 op<br> Afsluiten, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br>toepassingen \EnterpriseCloudEngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br> Deployment\Classes\SQL\SQL.psm1: 50 op regel < ScriptBlock&#62;,<br> <No file>: 18 op regel < ScriptBlock&#62;, < geen bestand&#62;: 16 regel** | Dit probleem kan optreden als de virtuele machine kan niet worden geplaatst in een onderbroken status verwijderen uit de rollen. | Neem voor hulp contact op met Microsoft CSS.
| Als u een update uitvoert, kan een van de volgende fouten optreden:<br><br>**Type 'Verifiëren' van de rol 'ADFS' heeft een uitzondering gemeld: validatie<br> voor AD FS/Graph-rol is mislukt met fout: fout bij het controleren van AD FS<br> probe eindpunt *endpoint_URI*: uitzondering aanroepen<br> " GetResponse"met"0"argument(en) vereist: ' de externe server<br> heeft een fout geretourneerd: (503) Server niet beschikbaar." bij Invoke -<br>ADFSGraphValidation**<br><br>**Type 'Verifiëren' van de rol 'ADFS' heeft een uitzondering gemeld: validatie<br> voor AD FS/Graph-rol is mislukt met fout: fout bij het ophalen<br> ADFS-eigenschappen: kan geen verbinding maken met <br>NET.TCP://localhost: 1500/beleid. De verbindingspoging heeft geduurd<br> voor een tijdsduur van 00:00:02.0498923. TCP-foutcode<br> 10061: Er is geen verbinding kan worden gemaakt omdat het doel<br> machine actief is geweigerd door deze 127.0.0.1:1500.<br> op Invoke-ADFSGraphValidation** | De update-actieplan kan Active Directory Federation Services (AD FS)-status niet valideren. | Neem voor hulp contact op met Microsoft CSS.

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

In deze sectie bevat na de installatie bekende problemen met 20171020.1 bouwen.

### <a name="portal"></a>Portal

- Het niet mogelijk om resources voor berekenen of opslaan in de beheerdersportal weer te geven. Hiermee wordt aangegeven dat er een fout tijdens de installatie van de update opgetreden en dat de update is niet correct gerapporteerd als geslaagd. Als dit probleem optreedt, neem voor hulp contact op met Microsoft CSS.
- Mogelijk ziet u een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, selecteert u het tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep, de **verplaatsen** knop is uitgeschakeld. Dit gedrag is verwacht. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
- Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij aan de bovenkant van de lijst. U moet nog steeds mogelijk om te selecteren van een item, zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Hebt u meerdere gebruikersabonnementen, is de vervolgkeuzelijst resource groep mag niet leeg zijn. 

   Als tijdelijke oplossing voor de laatste twee problemen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt PowerShell gebruiken in plaats daarvan.
- Verwijderen van zwevende resources leidt van gebruiker-abonnementen. Als tijdelijke oplossing, eerst Gebruikersbronnen of de hele resourcegroep verwijderen en verwijder vervolgens gebruikersabonnementen.
- U bent geen machtigingen om uw abonnement met behulp van de Azure Stack-portals te bekijken. Als tijdelijke oplossing, kunt u machtigingen controleren met behulp van PowerShell.
- De **servicestatus** blade niet wordt geladen. Wanneer u de Service Health-blade opent in de beheerder of de gebruiker-portal, Azure Stack, een fout wordt weergegeven en informatie niet kan worden geladen. Dit is normaal. Hoewel u kunt selecteren en opent u de Health-Service, wordt deze functie is nog niet beschikbaar maar geïmplementeerd in een toekomstige versie van Azure Stack.
 

### <a name="backup"></a>Backup

- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.

### <a name="health-and-monitoring"></a>Status en bewaking

- Als u een infrastructuur rolinstantie opnieuw opstarten, ontvangt u mogelijk een bericht weergegeven dat aangeeft dat het opnieuw opstarten is mislukt. Echter, opnieuw opstarten van het daadwerkelijk is voltooid.

### <a name="marketplace"></a>Marketplace
- Als u probeert te items toevoegen aan de Azure Stack marketplace met behulp van de **toevoegen vanuit Azure** optie, niet alle items mogelijk niet zichtbaar is voor downloaden.
- Gebruikers de volledige marketplace zonder abonnement kunnen zoeken en met beheerdersrechten objecten, zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet functioneel voor gebruikers.

### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat het maken van de virtuele machine mislukt.
- U kunt een beschikbaarheidsset voor virtuele machine alleen met een foutdomein van een, en een updatedomein van een configureren.
- Er is geen marketplace-ervaring te maken van virtuele-machineschaalsets. U kunt een schaalset met behulp van een sjabloon maken.
- Instellingen voor vergroten/verkleinen voor schaalsets voor virtuele machines zijn niet beschikbaar in de portal. Als tijdelijke oplossing, kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege de verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.
 
### <a name="networking"></a>Netwerken
- U kunt een load balancer kan maken met een openbaar IP-adres met behulp van de portal. Als tijdelijke oplossing, kunt u PowerShell gebruiken om te maken van de load balancer.
- Wanneer u een network load balancer maakt, moet u een regel address translation (NAT) maken. Als u dit niet doet, ontvangt u een fout opgetreden tijdens het toevoegen van een NAT-regel nadat de load balancer is gemaakt.
- U kunt een openbaar IP-adres van een virtuele machine (VM) kan niet loskoppelen, nadat de virtuele machine is gemaakt en die zijn gekoppeld aan dat IP-adres. Ontkoppeling wordt weergegeven om te werken, maar het eerder toegewezen openbare IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u het IP-adres aan een nieuwe virtuele machine opnieuw toewijzen (vaak aangeduid als een *wisselen van VIP*). Alle toekomstige probeert verbinding maken via dit resultaat van de IP-adres in een verbinding naar de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe certificaat. U moet de nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Het kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan resulteren in een niet-overeenkomende staat.
 
### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Veld FRU (replaceable unit) procedures

- Offline-installatiekopieën worden tijdens de update is uitgevoerd, niet gevuld. Als u vervangen door een scale unit-knooppunt wilt, samen met de leverancier van de OEM-hardware om te controleren of dat het vervangen knooppunt heeft de meest recente patchniveau.

## <a name="download-the-update"></a>De update downloaden

U kunt het downloaden van de update 1710 [hier](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van updatebeheer in Azure Stack, [-updates beheren in Azure Stack-overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates [toepassen van updates in Azure Stack](azure-stack-apply-updates.md).
