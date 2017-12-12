---
title: Azure Stack 1710 Update (Build 20171020.1) | Microsoft Docs
description: "Meer informatie over wat er in de update 1710 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mabrigg
ms.openlocfilehash: 7945b802e8c1482d9c6379a26618df23535daacf
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure Stack 1710 Update (Build 20171020.1)

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel worden de verbeteringen beschreven en wordt in dit updatepakket bekende problemen voor deze release en downloaden van de update worden opgelost. Bekende problemen zijn onderverdeeld in bekende problemen rechtstreeks verband houden met het updateproces en bekende problemen met de build (na de installatie).

> [!IMPORTANT]
> Dit updatepakket is alleen voor Azure-Stack geïntegreerd systemen. Dit updatepakket zijn niet van toepassing Azure Stack Development Kit.

## <a name="improvements-and-fixes"></a>Verbeteringen en oplossingen

Deze update bevat de volgende verbeteringen van de kwaliteit en oplossingen voor Azure-Stack.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Verbeteringen in Windows Server 2016 en oplossingen

- Updates voor WindowsServer 2016: 10 oktober 2017: KB4041691 (OS-Build 14393.1770. Zie [https://support.microsoft.com/help/4041691](https://support.microsoft.com/help/4041691) voor meer informatie.

### <a name="additional-quality-improvements-and-fixes"></a>Verbetering van de aanvullende kwaliteit en oplossingen

- Bevoorrechte eindpunt PowerShell-cmdlets voor het oplossen en het bijwerken van de server Network Time Protocol (NAP) wordt toegevoegd.
- Ondersteuning toegevoegd voor het bijwerken van de bevoegde eindpunt net genoeg Administration (JEA) eindpunt modules en cmdlet goedgekeurde IP-adressen. 
- Vaste lokale taal fouten in de beschermde eindpunt.
- De mogelijkheid om te draaien gatewayreferenties toegevoegd.
- Het lokale beheerdersaccount CBLocalAdmin verwijderd. 
- De inhoud van de heartbeat-waarschuwing sjabloon vaste aanbrengen of heartbeat waarschuwingen werk correct nadat deze is bijgewerkt.
- De Fabric-resourceprovider omgaan met time-outs tijdens FRU bewerkingen vast. 
- De mogelijkheid voor cloud-ontwikkelaars voor Azure Resource Manager-API-profielen gebruiken op Azure-Stack is toegevoegd.
- De Windows Update-service op de implementatie van virtuele machine (DVM) uitgeschakeld. 
- De stroom van het knooppunt aan/uit-acties verwijderd van de gebruikersinterface.
- Verschillende andere prestaties en stabiliteit oplossingen. 
 
## <a name="known-issues-with-the-update-process"></a>Bekende problemen met het updateproces kan controleren

Deze sectie vindt bekende problemen die tijdens de installatie van de update 1710 optreden kunnen.

> [!IMPORTANT]
> Als de update is mislukt, wanneer u later wilt hervatten van de update moet u de `Resume-AzureStackUpdate` cmdlet van het bevoegde eindpunt. De update niet hervatten met behulp van de beheerdersportal. (Dit is een bekend probleem in deze release.) Zie voor meer informatie [in Azure-Stack met behulp van de bevoegde endpoint-updates controleren](azure-stack-monitor-update.md).

| Symptoom  | Oorzaak  | Oplossing |
|---------|---------|---------|
|Wanneer u een update uitvoert, een vergelijkbaar met de volgende fout<br>optreden tijdens de stap 'Opslag Hosts opnieuw Opslagknooppunt'<br> van de planning van de update-actie.<br><br>**{'name': 'Opnieuw opstarten opslag Hosts', 'beschrijving': ' opnieuw opstarten<br> opslag Hosts. errorMessage "," ":" Typ 'Restart' van de rol<br> 'BareMetal' Deze gebeurtenis treedt op een uitzondering: \n\n de computer<br> HostName-05 wordt overgeslagen. Kunnen niet worden opgehaald van de LastBootUpTime<br> via de WMI-service met het volgende foutbericht weergegeven:<br> de RPC-server is niet beschikbaar.<br> (Uitzondering van HRESULT: 0x800706BA). \nat opnieuw opstarten-Host** | Dit probleem wordt veroorzaakt door een potentiële beschadigd stuurprogramma aanwezig zijn in bepaalde configuraties. | 1. Aanmelden bij de baseboard management controller (BMC) webinterface en opnieuw opstarten van de host die wordt geïdentificeerd in het foutbericht.<br><br>2. De update met behulp van de bevoegde eindpunt hervatten. |
| Wanneer u een update uitvoert, lijkt het updateproces achterstallige<br> en niet uitgevoerd na de stap ' stap: uitvoeren van stap 2.4 - installatie<br> bijwerken' van het plan update-actie.<br><br>Deze stap wordt gevolgd door een reeks kopieerprocessen van .nupkg<br> bestanden met de bestandsshares van interne infrastructuur. Bijvoorbeeld:<br><br>**1 bestanden kopiëren vanaf content\PerfCollector\VirtualMachines naar <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**<br><br>Of het bericht wordt weergegeven:<br><br>**WarningMessage:Task: Aanroep van de interface 'LiveUpdate'<br> van rol Cloud\Fabric\VirtualMachines is mislukt:<br> 'LiveUpdate' rol informatie gegeven Type een<br> uitzondering: Er is onvoldoende ruimte op de schijf .**  | Het probleem wordt veroorzaakt door logboekbestanden van de schijven op een virtuele machine van de infrastructuur en een probleem in Windows Server Scale-Out File Server (SOFS) die wordt geleverd in een toekomstige update wordt gevuld. | Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp. | 
| Wanneer u een update uitvoert, een vergelijkbaar met de volgende fout<br> optreden tijdens de stap ' stap: uitvoeren van stap 2.13.2 - Update<br> *VM_Name*'van het plan van de update in te grijpen. (De virtuele machine<br> naam kan veranderen.)<br><br>**ActionPlanInstanceWarning ece/MachineName:<br> WarningMessage:Task: aanroep van de interface 'LiveUpdate' van<br> rol Cloud\Fabric\WAS is mislukt: Typ 'LiveUpdate' van de rol<br> 'Is' heeft een uitzondering veroorzaakt: fout tijdens het opslag<br> initialisatie: Er is een fout opgetreden tijdens het maken van een API<br> aanroepen met Microsoft Storage-service: {"Message": "een time-out<br> is opgetreden tijdens het communiceren met Service Fabric.<br> Uitzonderingstype: TimeoutException.<br> Bericht van uitzondering: time-out bewerking. "}**  | Het probleem wordt veroorzaakt door een i/o-time-out in Windows Server die wordt opgelost in een toekomstige update. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, een vergelijkbaar met de volgende fout<br> treedt op tijdens stap 'stap 21 start SQL server-VM's."<br><br>**Typ 'LiveUpdateRestart' van de rol informatie deze gebeurtenis treedt op een<br> uitzondering: VerboseMessage: [VirtualMachines:LiveUpdateRestart]<br> opvragen van de computernaam van de VM-Sql01. - 13-10/2017 5:11:50 PM VerboseMessage: [virtuele machines: LiveUpdateRestart]<br> VM is gemarkeerd als HighlyAvailable. - 13-10/2017 5:11:50 PM<br> VerboseMessage: [VirtualMachines:LiveUpdateRestart] op<br>MS. Internal.ServerClusters.ExceptionHelp.Build op<br>MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(Boolean forceren) op Microsoft.FailoverClusters.PowerShell.<br> StopClusterResourceCommand.BeginTimedOperation() op <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>ProcessRecord() op Microsoft.FailoverClusters.PowerShell.<br> FCCmdlet.ProcessRecord() - 13-10/2017 5:11:50 PM waarschuwing<br>bericht: taak: aanroep van de interface 'LiveUpdateRestart' van<br> rol Cloud\Fabric\VirtualMachines is mislukt:** | Dit probleem kan optreden als de virtuele machine kan niet opnieuw opstarten is. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, treedt er een foutbericht weergegeven dat vergelijkbaar is met het volgende:<br><br>**2017-10-22T01:37:37.5369944Z rol 'SQL' Type 'Afsluiten'<br> heeft een uitzondering veroorzaakt: Er is een fout opgetreden knooppunt<br> 's45r1004 Sql01'.at Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\ Applications\ EnterpriseCloud <br>EngineApplicationType &#95; App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 op<br> afsluiten, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br> Toepassingen \EnterpriseCloudEngineApplicationType &#95; App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br>Deployment\Classes\SQL\SQL.psm1: regel 50 op &#60; ScriptBlock &#62;<br> &#60; Er is geen bestand >: regel 18 op &#60; ScriptBlock &#62; &#60; Er is geen bestand &#62;: 16 regel** | Dit probleem kan optreden als de virtuele machine kan niet worden geplaatst in een onderbroken status verwijderen uit de rollen. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, kan een van de volgende fouten optreden:<br><br>**Type 'Valideren' rol 'ADFS' heeft een uitzondering veroorzaakt: validatie<br> voor ADFS/Graph-rol is mislukt met fout: fout bij het controleren van AD FS<br> endpoint-test *endpoint_URI*: uitzondering aanroepen<br> ' GetResponse"met"0"argument(en) bevatten:" de externe server<br> heeft een fout geretourneerd: (503) Server niet beschikbaar. ' bij Invoke -<br>ADFSGraphValidation**<br><br>**Type 'Valideren' rol 'ADFS' heeft een uitzondering veroorzaakt: validatie<br> voor ADFS/Graph-rol is mislukt met fout: fout bij het ophalen<br> ADFS-eigenschappen: kan geen verbinding maken met <br>NET.TCP://localhost: 1500/beleid. De verbindingspoging geduurd<br> voor een periode van 00:00:02.0498923. TCP-foutcode<br> 10061: kan geen verbinding worden gemaakt omdat het doel<br> machine actief heeft geweigerd 127.0.0.1:1500.<br> op Invoke-ADFSGraphValidation** | Het plan update-actie kan niet Valideer de status van de Active Directory Federation Services (AD FS). | Neem voor assistentie contact op met Microsoft CSS.

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Deze sectie bevat na de installatie bekende problemen met 20171020.1 bouwen.

### <a name="portal"></a>Portal

- Het niet mogelijk om resources compute of opslag in de beheerdersportal weer te geven. Dit betekent dat er een fout tijdens de installatie van de update opgetreden en dat de update is niet juist gerapporteerd als geslaagd. Als dit probleem optreedt, neem voor assistentie contact op met Microsoft CSS.
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
- Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

   Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement met behulp van de Azure-Stack-portals weergeven. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.
  
### <a name="backup"></a>Back-up

- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.

### <a name="health-and-monitoring"></a>Status en bewaking

- Als u een exemplaar van de rol infrastructuur opnieuw opstarten, verschijnt er een bericht weergegeven dat aangeeft dat het opnieuw opstarten is mislukt. Echter, het opnieuw opstarten daadwerkelijk is voltooid.

### <a name="marketplace"></a>Marketplace
- Wanneer u probeert items toevoegen aan de Stack van Azure marketplace met behulp van de **toevoegen uit Azure** optie, niet alle items mogelijk niet zichtbaar voor downloaden.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.

### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken.
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.
 
### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.
 
### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Procedures voor veld FRU (replaceable unit)

- Offline-installatiekopieën zijn tijdens de update-uitvoering niet hersteld. Als u vervangen van een scale unit-knooppunt wilt, samen met uw OEM-hardwareleverancier om te controleren of dat het vervangen knooppunt heeft de meest recente patchniveau.

## <a name="download-the-update"></a>De update downloaden

U kunt het 1710 updatepakket downloaden [hier](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van updatebeheer in Azure Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).