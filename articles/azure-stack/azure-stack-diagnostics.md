---
title: Diagnostische gegevens in Azure Stack
description: Over het verzamelen van logboekbestanden voor diagnostische gegevens in Azure Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: ef8a01228ff31afa78f469b2ad80b864fc9d3497
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283449"
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



### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Get-AzureStackLog uitvoeren op een Azure Stack Development Kit (ASDK)-systeem
Volg deze stappen om uit te voeren van Get-AzureStackLog op een hostcomputer ASDK.

1. Meld u als **AzureStack\CloudAdmin** op de hostcomputer ASDK.
2. Open een nieuwe PowerShell-venster als beheerder.
3. Voer de **Get-AzureStackLog** PowerShell-cmdlet.

**Voorbeelden:**

  Verzamelen van alle logboeken voor alle rollen:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen, met datumfiltering voor de logboekbestanden voor de afgelopen 8 uur:
    
  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Logboeken verzamelen van virtuele machines en BareMetal rollen, met datumfiltering voor de logboekbestanden voor de periode tussen de 8 uur geleden en 2 uur geleden:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Parameter-overwegingen voor zowel ASDK en geïntegreerde systemen

- De parameters **OutputSharePath** en **OutputShareCredential** worden gebruikt voor het opslaan van Logboeken in een gebruiker die is opgegeven locatie.

- De **FromDate** en **ToDate** parameters kunnen worden gebruikt voor het verzamelen van Logboeken voor een bepaalde periode. Als deze parameters niet zijn opgegeven, worden logboeken verzameld voor de afgelopen vier uur standaard.

- Gebruik de **FilterByNode** parameter Logboeken filteren op computernaam. Bijvoorbeeld:

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByNode azs-xrp01
    ```
- Gebruik de **FilterByLogType** parameter voor het filteren van Logboeken op type. U kunt kiezen om te filteren op bestand, delen of WindowsEvent. Bijvoorbeeld:

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByLogType File
    ```
- U kunt de **TimeOutInMinutes** parameter voor de time-out voor de logboekverzameling instellen. Dit is standaard ingesteld op 150 (2,5 uur).
- Logboekverzameling dump-bestand is standaard uitgeschakeld. Als u wilt inschakelen, gebruikt u de **IncludeDumpFile** parameter overschakelen. 
- Op dit moment kunt u de **FilterByRole** parameter voor de logboekverzameling filter door de volgende rollen:

 |   |   |   |    |
 | - | - | - | -  |   
 |ACS                   |CacheService                   |IBC                            |OEM|
 |ACSDownloadService    |Compute                        |InfraServiceController         |OnboardRP|
 |ACSFabric             |KPI                            |KeyVaultAdminResourceProvider  |PXE|
 |ACSFrontEnd           |CRP                            |KeyVaultControlPlane           |QueryServiceCoordinator|
 |ACSMetrics            |DeploymentMachine              |KeyVaultDataPlane              |QueryServiceWorker|
 |ACSMigrationService   |DiskRP                         |KeyVaultInternalControlPlane   |SeedRing|
 |ACSMonitoringService  |Domain                         |KeyVaultInternalDataPlane      |SeedRingServices|
 |ACSSettingsService    |EEG                            |KeyVaultNamingService          |SLB|
 |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL|
 |ACSTableServer        |EventRP                        |MetricsAdminRP                 |SRP   |
 |ACSWac                |ExternalDNS                    |MetricsRP                      |Storage|
 |ADFS                  |FabricRing                     |MetricsServer                  |StorageController   |
 |ApplicationController |FabricRingServices             |MetricsStoreService            |URP   |
 |ASAppGateway          |FirstTierAggregationService    |MonAdminRP                     |UsageBridge|
 |AzureBridge           |FRP                            |MonRP                          |virtuele machines   |
 |AzureMonitor          |Gateway                        |NC                             |IS|
 |BareMetal             |HealthMonitoring               |NonPrivilegedAppGateway        |WASPUBLIC|
 |BRP                   |HintingServiceV2               |NRP                            |   |
 |CA                    |HRP                            |OboService                     |   |
 |   |   |   |    |

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

