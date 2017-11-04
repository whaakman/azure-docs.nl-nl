---
title: Azure Stack 1710 Update (Build 20171020.1) | Microsoft Docs
description: "Meer informatie over wat er in de update 1710 voor Azure-Stack geïntegreerd systemen, de bekende problemen en waar u de update te downloaden."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: twooley
ms.openlocfilehash: d91a23ae4eb5aee14d3d2fef74467e7f33c458cc
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
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
|Wanneer u een update uitvoert, wordt een foutbericht weergegeven dat vergelijkbaar is met de volgende fout optreden tijdens de stap 'Opslag Hosts opnieuw Opslagknooppunt' van het plan update-actie.<br><br>**{{'name': 'Opnieuw opstarten opslag Hosts', 'beschrijving': 'Opnieuw opstarten opslag Hosts.', 'errorMessage': 'Type 'Restart' van de rol 'BareMetal' heeft een uitzondering: \n\nThe computer HostName-05 wordt overgeslagen. Kunnen niet worden opgehaald van de LastBootUpTime via de WMI-service met het volgende foutbericht weergegeven: de RPC-server is niet beschikbaar. (Uitzondering van HRESULT: 0x800706BA). \nat opnieuw opstarten-Host** | Dit probleem wordt veroorzaakt door een potentiële beschadigd stuurprogramma aanwezig zijn in bepaalde configuraties. | 1. Aanmelden bij de baseboard management controller (BMC) webinterface en opnieuw opstarten van de host die wordt geïdentificeerd in het foutbericht.<br><br>2. De update met behulp van de bevoegde eindpunt hervatten. |
| Wanneer u een update uitvoert, het updateproces wordt weergegeven aan achterstallige en maakt niet uitgevoerd na de stap ' stap: uitvoeren van stap 2.4 - de update installeren ' van het plan update-actie.<br><br>Deze stap wordt gevolgd door een reeks kopieerprocessen van .nupkg bestanden met de bestandsshares van interne infrastructuur. Bijvoorbeeld:<br><br>**1 bestanden kopiëren van content\PerfCollector\VirtualMachines naar \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\PerfCounterConfiguration**  | Het probleem wordt veroorzaakt door logboekbestanden van de schijven op een virtuele machine van de infrastructuur en een probleem in Windows Server Scale-Out File Server (SOFS) die wordt geleverd in een toekomstige update wordt gevuld. | Neem contact op met Microsoft Customer Service and Support (CSS) voor hulp. | 
| Wanneer u een update uitvoert, wordt een vergelijkbaar met de volgende fout optreden tijdens de stap ' stap: uitvoeren van stap 2.13.2 - Update *VM_Name*' van het plan update-actie. (De naam van de virtuele machine kan variëren.)<br><br>**ActionPlanInstanceWarning ece/MachineName: WarningMessage:Task: aanroep van de interface 'LiveUpdate' van de rol Cloud\Fabric\WAS is mislukt:<br>rol 'WAS' Type 'LiveUpdate' heeft een uitzondering veroorzaakt:<br>fout tijdens de opslag de initialisatie: Er is een fout opgetreden tijdens het maken van een API-aanroep met Microsoft Storage-service: {"Message": "is een time-out opgetreden bij het communiceren met Service Fabric. Uitzonderingstype: TimeoutException. Bericht van uitzondering: time-out bewerking. "}**  | Het probleem wordt veroorzaakt door een i/o-time-out in Windows Server die wordt opgelost in een toekomstige update. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, kan een vergelijkbaar met de volgende fout optreden tijdens de stap 'stap 21 start SQL server-VM's."<br><br>**Type 'LiveUpdateRestart' rol 'Informatie' heeft een uitzondering veroorzaakt:<br>VerboseMessage: [VirtualMachines:LiveUpdateRestart] query's uitvoeren voor de computernaam van de VM-Sql01. - 13-10/2017 5:11:50 PM VerboseMessage: [virtuele machines: LiveUpdateRestart] VM is gemarkeerd als HighlyAvailable. -10/13/2017 5:11:50 PM VerboseMessage: [VirtualMachines:LiveUpdateRestart] bij MS. Internal.ServerClusters.ExceptionHelp.Build bij MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline (Boolean forceren) op Microsoft.FailoverClusters.PowerShell.StopClusterResourceCommand.BeginTimedOperation() op Microsoft.FailoverClusters.PowerShell.TimedCmdlet.WrappedProcessRecord() op Microsoft.FailoverClusters.PowerShell.FCCmdlet.ProcessRecord() - 13-10/2017 5:11:50 PM WarningMessage:Task: aanroep van de interface 'LiveUpdateRestart' van de rol ' De Cloud\Fabric\VirtualMachines is mislukt:** | Dit probleem kan optreden als de virtuele machine kan niet opnieuw opstarten is. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, treedt er een foutbericht weergegeven dat vergelijkbaar is met het volgende:<br><br>**2017-10-22T01:37:37.5369944Z rol 'SQL' Type 'Afsluiten' heeft een uitzondering veroorzaakt: Er is een fout opgetreden onderbreken knooppunt 's45r1004 Sql01'.at Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_ App1\EnterpriseCloudEngineServicePkg.code.1.0.597.18\CloudDeployment\Roles\SQL\SQL.psm1: 542at afsluiten, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\ regel EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Classes\SQL\SQL.psm1: 50at regel <ScriptBlock>, <No file>: 18at regel <ScriptBlock>, <No file>: 16 regel** | Dit probleem kan optreden als de virtuele machine kan niet worden geplaatst in een onderbroken status verwijderen uit de rollen. | Neem voor assistentie contact op met Microsoft CSS.
| Wanneer u een update uitvoert, kan een van de volgende fouten optreden:<br><br>**Type 'Valideren' rol 'ADFS' heeft een uitzondering veroorzaakt: validatie voor ADFS/grafiek rol is mislukt met fout: fout bij het controleren van AD FS-test eindpunt *endpoint_URI*: uitzondering 'GetResponse' aanroepen met "0" argument(en) bevatten: "de externe server heeft een fout geretourneerd: (503) Server niet beschikbaar. ' op Invoke-ADFSGraphValidation**<br><br>**Type 'Valideren' rol 'ADFS' heeft een uitzondering veroorzaakt: validatie voor ADFS/grafiek rol is mislukt met fout: fout bij het ophalen van de AD FS-eigenschappen: kan geen verbinding maken met NET.TCP://localhost: 1500/beleid. De verbindingspoging een tijdsspanne van 00:00:02.0498923 geduurd. TCP-foutcode 10061: kan geen verbinding worden gemaakt omdat de doelmachine actief heeft geweigerd 127.0.0.1:1500. op aanroepen ADFSGraphValidation** | Het plan update-actie kan niet Valideer de status van de Active Directory Federation Services (AD FS). | Neem contact op met CSS Microsoft voor assistentie.

## <a name="known-issues-post-installation"></a>Bekende problemen (na de installatie)

Deze sectie bevat na de installatie bekende problemen met 20171020.1 bouwen.

### <a name="portal"></a>Portal

- Het niet mogelijk om resources compute of opslag in de beheerdersportal weer te geven. Dit betekent dat er een fout tijdens de installatie van de update opgetreden en dat de update is niet juist gerapporteerd als geslaagd. Als dit probleem optreedt, neem voor assistentie contact op met Microsoft CSS.
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
- De **verplaatsen** knop wordt uitgeschakeld wanneer u de eigenschappen van een resourcegroep weergeven. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
- U bent geen kunnen machtigingen aan uw abonnement met behulp van de Azure-Stack-portals weergeven. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.
-  Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

   Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.
  
### <a name="backup"></a>Back-up

- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.

### <a name="health-and-monitoring"></a>Status en bewaking

- Als u een exemplaar van de rol infrastructuur opnieuw opstarten, verschijnt er een bericht weergegeven dat aangeeft dat het opnieuw opstarten is mislukt. Echter, het opnieuw opstarten daadwerkelijk is voltooid.

### <a name="services"></a>Services

**Marketplace**
- Wanneer u probeert items toevoegen aan de Stack van Azure marketplace met behulp van de **toevoegen uit Azure** optie, niet alle items mogelijk niet zichtbaar voor downloaden.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Een tenant moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen. 

**SQL/MySQL**
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.

**Compute**
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken.
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
 
**Netwerk**
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Procedures voor veld FRU (replaceable unit)

- Offline-installatiekopieën zijn tijdens de update-uitvoering niet hersteld. Als u vervangen van een scale unit-knooppunt wilt, samen met uw OEM-hardwareleverancier om te controleren of dat het vervangen knooppunt heeft de meest recente patchniveau.

## <a name="download-the-update"></a>De update downloaden

U kunt het 1710 updatepakket downloaden [hier](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van updatebeheer in Azure Stack [beheren van updates in de Azure-Stack overzicht](azure-stack-updates.md).
- Zie voor meer informatie over het toepassen van updates [toepassen van updates in Azure-Stack](azure-stack-apply-updates.md).