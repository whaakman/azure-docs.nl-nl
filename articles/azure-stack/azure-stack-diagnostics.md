---
title: Diagnostische gegevens in Azure Stack
description: Over het verzamelen van logboekbestanden voor diagnostische gegevens in Azure Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 08/22/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: a36609ae63351070bb28469d9ccf1f3deb7bc6ff
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42616946"
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack diagnostische hulpprogramma 's

Azure Stack is een grote verzameling van onderdelen die samenwerken en interactie met elkaar. Al deze onderdelen genereren hun eigen unieke Logboeken. Hierdoor worden vaststellen van problemen met een uitdaging, met name voor fouten die afkomstig zijn van meerdere, communiceren onderdelen van Azure Stack. 

Onze diagnostische hulpprogramma's zorgen dat het mechanisme voor het verzamelen van logboek is eenvoudig en efficiënt. Het volgende diagram laat zien Meld hoe verzameling hulpprogramma's werken met Azure Stack:

![Azure Stack diagnostische hulpprogramma 's](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Trace-Collector
 
De Trace-Collector is standaard ingeschakeld en achtereen wordt uitgevoerd op de achtergrond voor het verzamelen van alle logboeken van Event Tracing voor Windows (ETW) van Azure Stack componentservices. ETW-logboeken worden opgeslagen in een algemene lokale share met een limiet van vijf dagen oud. Zodra deze limiet is bereikt, worden de oudste bestanden verwijderd nadat er nieuwe labels zijn gemaakt. De standaard maximale grootte van elk bestand is 200 MB. Een controle grootte wordt elke twee minuten, en als het huidige bestand > = 200 MB, wordt deze opgeslagen en een nieuw bestand wordt gegenereerd. Er is ook een limiet van 8 GB op de totale bestandsgrootte gegenereerd per event-sessie. 

## <a name="log-collection-tool"></a>Hulpprogramma voor het verzamelen van Logboeken
 
De PowerShell-cmdlet **Get-AzureStackLog** kan worden gebruikt voor het verzamelen van Logboeken van alle onderdelen in een Azure Stack-omgeving. Deze slaat ze op in het zip-bestanden in een door de gebruiker gedefinieerde locatie. Als de Azure Stack-team voor technische ondersteuning nodig heeft voor uw logboeken op een probleem op te lossen, kunnen ze vragen u dit hulpprogramma uit te voeren.

> [!CAUTION]
> Deze logboekbestanden kunnen persoonsgegevens (PII) bevatten. Voordat u alle logboekbestanden openbaar plaatst, moet u hiermee rekening gehouden.
 
Hier volgen enkele typen in een voorbeeld van de logboeken die worden verzameld:
*   **Logboeken van Azure Stack-implementatie**
*   **Windows-gebeurtenislogboeken**
*   **Panther Logboeken**
*   **Clusterlogboeken**
*   **Opslag, diagnostische logboeken**
*   **ETW-Logboeken**

Deze bestanden worden verzameld en opgeslagen in een share Trace-collector. De **Get-AzureStackLog** PowerShell-cmdlet kan vervolgens worden gebruikt voor het verzamelen van deze indien nodig.

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Als u wilt uitvoeren van Get-AzureStackLog in Azure Stack-geïntegreerde systemen 
Het hulpprogramma log verzameling op een geïntegreerd systeem uitgevoerd, moet u beschikken over naar de bevoegde eindpunt (PEP). Hier volgt een voorbeeld-script dat u kunt uitvoeren met behulp van de PEP voor het verzamelen van Logboeken op een geïntegreerd systeem:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- De parameters **OutputSharePath** en **OutputShareCredential** worden gebruikt om Logboeken te uploaden naar een externe gedeelde map.
- Zoals wordt weergegeven in het vorige voorbeeld de **FromDate** en **ToDate** parameters kunnen worden gebruikt voor het verzamelen van Logboeken voor een bepaalde periode. Dit kan handig voor scenario's zoals het verzamelen van Logboeken na een updatepakket toepast op een geïntegreerd systeem.


 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Get-AzureStackLog uitvoeren op een Azure Stack Development Kit (ASDK)-systeem
1. Meld u als **AzureStack\CloudAdmin** op de host.
2. Open een PowerShell-venster als beheerder.
3. Voer de **Get-AzureStackLog** PowerShell-cmdlet.

**Voorbeelden:**

  Verzamelen van alle logboeken voor alle rollen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen, met datumfiltering voor de logboekbestanden voor de afgelopen 8 uur:
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen, met datumfiltering voor de logboekbestanden voor de periode tussen de 8 uur geleden en 2 uur geleden:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Parameter-overwegingen voor zowel ASDK en geïntegreerde systemen

- Als de **FromDate** en **ToDate** parameters niet zijn opgegeven, wordt standaard logboeken voor de afgelopen vier uur worden verzameld.
- Gebruik de **FilterByNode** parameter Logboeken filteren op computernaam. Bijvoorbeeld: ```Get-AzureStackLog -OutputPath <path> -FilterByNode azs-xrp01```
- Gebruik de **FilterByLogType** parameter voor het filteren van Logboeken op type. U kunt kiezen om te filteren op bestand, delen of WindowsEvent. Bijvoorbeeld: ```Get-AzureStackLog -OutputPath <path> -FilterByLogType File```
- U kunt de **TimeOutInMinutes** parameter voor de time-out voor de logboekverzameling instellen. Dit is standaard ingesteld op 150 (2,5 uur).
- In versie 1805 en hoger, is de dump bestand logboekverzameling standaard uitgeschakeld. Als u wilt inschakelen, gebruikt u de **IncludeDumpFile** parameter overschakelen. 
- Op dit moment kunt u de **FilterByRole** parameter voor de logboekverzameling filter door de volgende rollen:

 |   |   |   |    |
 | - | - | - | -  |   
 |ACS|Compute|InfraServiceController|QueryServiceCoordinator|
 |ACSBlob|KPI|Infrastructuur|QueryServiceWorker|
 |ACSDownloadService|CRP|KeyVaultAdminResourceProvider|SeedRing|
 |ACSFabric|DatacenterIntegration|KeyVaultControlPlane|SeedRingServices|
 |ACSFrontEnd|DeploymentMachine|KeyVaultDataPlane|SLB|
 |ACSMetrics|DiskRP|KeyVaultInternalControlPlane|SlbVips|
 |ACSMigrationService|Domein|KeyVaultInternalDataPlane|SQL|
 |ACSMonitoringService|EEG|KeyVaultNamingService|SRP|
 |ACSSettingsService|EventAdminRP|MDM|Storage|
 |ACSTableMaster|EventRP|MetricsAdminRP|StorageAccounts|
 |ACSTableServer|ExternalDNS|MetricsRP|StorageController|
 |ACSWac|Fabric|MetricsServer|Tenant|
 |ADFS|FabricRing|MetricsStoreService|TraceCollector|
 |ApplicationController|FabricRingServices|MonAdminRP|URP|
 |ASAppGateway|FirstTierAggregationService|MonitoringAgent|Gebruik|
 |AzureBridge|FRP|MonRP|UsageBridge|
 |AzureMonitor|Gallery|NC|virtuele machines|
 |AzureStackBitlocker|Gateway|Netwerk|IS|
 |BareMetal|HealthMonitoring|NonPrivilegedAppGateway|WASBootstrap|
 |BRP|HintingServiceV2|NRP|WASPUBLIC|
 |CA|HRP|OboService|WindowsDefender|
 |CacheService|IBC|OEM|     |
 |Cloud|id-provider|OnboardRP|     |   
 |Cluster|IDN 's|PXE|     |
 |   |   |   |    |


### <a name="bkmk_gui"></a>Logboeken verzamelen met een grafische gebruikersinterface
In plaats van de vereiste parameters voor de cmdlet Get-AzureStackLog voor het ophalen van Azure Stack-logboeken bieden, kunt u ook gebruikmaken van de beschikbare open-source Azure Stack-hulpprogramma's zich in de Azure Stack-hulpprogramma's GitHub extra hoofdopslagplaats op http://aka.ms/AzureStackTools.

De **ERCS_AzureStackLogs.ps1** PowerShell-script wordt opgeslagen in de GitHub-opslagplaats voor hulpprogramma's en regelmatig wordt bijgewerkt. Om ervoor te zorgen dat u de meest recente beschikbare versie hebt, moet u deze rechtstreeks vanuit downloaden http://aka.ms/ERCS. Gestart vanuit een PowerShell-sessie met beheerdersrechten, het script maakt verbinding met het eindpunt van de bevoegdheden en Get-AzureStackLog uitgevoerd met de opgegeven parameters. Als er geen parameters zijn opgegeven, standaard het script dat wordt gevraagd om parameters via een grafische gebruikersinterface.

Voor meer informatie over het ERCS_AzureStackLogs.ps1 PowerShell-script, kunt u bekijken [een korte video](https://www.youtube.com/watch?v=Utt7pLsXEBc) of weergeven van het script [Leesmij-bestand](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md) zich in de GitHub-opslagplaats van de Azure Stack-hulpprogramma's. 

### <a name="additional-considerations"></a>Aanvullende overwegingen

* De opdracht duurt enige tijd worden uitgevoerd op basis van welke rollen zijn de logboeken verzamelen. Factoren omvatten ook de tijdsduur die is opgegeven voor het verzamelen en het aantal knooppunten in de Azure Stack-omgeving.
* Als zich verzameling wordt uitgevoerd, controleert u de nieuwe map gemaakt in de **OutputSharePath** parameter die is opgegeven in de opdracht.
* Elke rol heeft de logboeken in afzonderlijke zip-bestanden. Afhankelijk van de grootte van de logboeken die worden verzameld, kan een rol de logboeken splitsen meerdere zip-bestanden hebben. Voor een rol gebruiken als u wilt dat alle logboekbestanden uitgepakt één map, een hulpprogramma waarmee u in bulk (zoals 7zip uitpakken kunt). Selecteer alle ZIP-bestanden voor de rol en selecteer **hier extraheren**. Hiermee wordt de logboekbestanden voor die rol in een enkele, samengevoegde map uitgepakt.
* Een bestand met de naam **Get-AzureStackLog_Output.log** ook wordt gemaakt in de map waarin de gecomprimeerde logboekbestanden. Dit bestand is een logboek van de uitvoer van de opdracht, die kan worden gebruikt voor het oplossen van problemen tijdens logboekverzameling. Soms bevat het logboekbestand `PS>TerminatingError` vermeldingen die kunnen worden genegeerd, tenzij de verwachte logboekbestanden ontbreken na het aanmelden van de verzameling wordt uitgevoerd.
* Voor het onderzoeken van een specifieke fout mogelijk van meer dan één onderdeel Logboeken nodig.
    -   Systeem- en gebeurtenislogboeken voor alle infrastructuur-VM's worden verzameld de *virtuele machines* rol.
    -   Systeem- en gebeurtenislogboeken voor alle hosts worden verzameld de *BareMetal* rol.
    -   Logboeken voor Failover-Cluster en Hyper-V worden verzameld de *opslag* rol.
    -   ACS-logboeken worden verzameld de *opslag* en *ACS* rollen.

> [!NOTE]
> Limieten voor grootte en leeftijd worden afgedwongen voor de logboeken die worden verzameld, is het essentieel om ervoor te zorgen efficiënte benutting van uw opslagruimte om te controleren of dat deze overspoeld met Logboeken niet ophalen. Bij het oplossen van een probleem moet u echter soms logboeken die niet meer vanwege deze limieten bestaat mogelijk. Het is dus **ten zeerste aangeraden** dat u uw logboeken naar een externe opslagruimte (een opslagaccount in Azure, een extra on-premises-opslagapparaat enz.)-offload elke 8 tot 12 uur en houd ze daar voor 1-3 maanden, afhankelijk van uw vereisten. Zorg er ook voor dat deze locatie voor de opslag is versleuteld.

## <a name="next-steps"></a>Volgende stappen
[Het oplossen van Microsoft Azure Stack](azure-stack-troubleshooting.md)

