---
title: Diagnostische gegevens in Azure Stack
description: Het verzamelen van logboekbestanden voor diagnostische gegevens in Azure-Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 12/15/2017
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: e823aeb4291b3e765b35181c24b41fa58c170cca
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack diagnostische hulpprogramma 's

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*
 
Azure-Stack is een grote verzameling onderdelen samenwerken en interactie met elkaar. Al deze onderdelen genereren hun eigen unieke Logboeken. Hierdoor vaststellen van problemen met een uitdaging, met name voor fouten die afkomstig zijn van meerdere, interactie van onderdelen van de Azure-Stack. 

Onze diagnostische hulpprogramma's zorgen dat het mechanisme voor het verzamelen van logboek is eenvoudig en efficiënt. Verzameling hulpprogramma's voor Meld het volgende diagram laat zien hoe in Azure Stack werk:

![Azure Stack diagnostische hulpprogramma 's](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Trace-Collector
 
De Trace-verzamelaar is standaard ingeschakeld en continu op de achtergrond voor het verzamelen van alle Event Tracing voor Windows (ETW) logboeken van Azure-Stack componentservices worden uitgevoerd. ETW-logboeken worden opgeslagen in een algemene lokale share met een limiet van vijf dagen oud. Zodra deze limiet is bereikt, wordt de oudste bestanden worden verwijderd wanneer er nieuwe worden gemaakt. De standaard maximale grootte van elk bestand is 200 MB. Een controle van de grootte van deze gebeurtenis treedt op elke 2 minuten en als het huidige bestand > = 200 MB, wordt deze opgeslagen en een nieuw bestand is gegenereerd. Er is ook een limiet voor 8 GB op de totale bestandsgrootte gegenereerd per gebeurtenissessie. 

## <a name="log-collection-tool"></a>Hulpprogramma voor log-verzameling
 
De PowerShell-cmdlet **Get-AzureStackLog** kan worden gebruikt voor het verzamelen van Logboeken van alle onderdelen van een Azure-Stack-omgeving. Opgeslagen in het zip-bestanden in een door de gebruiker gedefinieerde locatie. Als het team van de technische ondersteuning van Azure-Stack uw logboeken moet bij het oplossen van een probleem, kunnen ze vragen u dit programma wilt uitvoeren.

> [!CAUTION]
> Deze logboekbestanden mogelijk persoonsgegevens (PII) bevatten. Hiermee rekening houden voordat u alle logboekbestanden openbaar.
 
Hier volgen enkele voorbeeld logboek typen die worden verzameld:
*   **Logboeken van Azure Stack-implementatie**
*   **Windows-gebeurtenislogboeken**
*   **Panther Logboeken**
*   **Cluster-Logboeken**
*   **Diagnostische logboeken van opslag**
*   **ETW-Logboeken**

Deze bestanden worden verzameld en opgeslagen in een share Trace-collector. De **Get-AzureStackLog** PowerShell-cmdlet kan vervolgens worden gebruikt voor het verzamelen van deze indien nodig.
 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Voor het uitvoeren van Get-AzureStackLog op een systeem Azure Stack Development Kit (ASDK)
1. Meld u aan als **AzureStack\CloudAdmin** op de host.
2. Open een PowerShell-venster als beheerder.
3. Voer de **Get-AzureStackLog** PowerShell-cmdlet.

**Voorbeelden:**

  Alle logboeken voor alle rollen verzamelen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  Logboeken van de rollen van virtuele machines en BareMetal verzamelen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  Logboeken van virtuele machines en BareMetal rollen, met datum filteren voor logboekbestanden voor de afgelopen 8 uur verzamelen:
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Logboeken van virtuele machines en BareMetal rollen, met datum filteren voor logboekbestanden voor de periode tussen de 8 uur geleden en 2 uur geleden verzamelen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="to-run-get-azurestacklog-on-an-azure-stack-integrated-system"></a>Voor het uitvoeren van Get-AzureStackLog op een Azure-Stack geïntegreerd systeem

Het hulpprogramma log verzameling op een geïntegreerd systeem uitgevoerd, moet u beschikken over naar de bevoorrechte eindpunt (PEP). Hier volgt een voorbeeldscript dat u kunt uitvoeren met behulp van de PEP voor het verzamelen van Logboeken op een geïntegreerde systeem:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDRESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Wanneer u de logboeken van de PEP verzameld, geeft u de **OutputPath** parameter moet een locatie op de machine Hardware Lifecycle Host (HLH). Zorg er ook voor dat de locatie is versleuteld.
- De parameters **OutputSharePath** en **OutputShareCredential** zijn optioneel en worden gebruikt wanneer u logboeken naar een externe gedeelde map uploadt. Deze parameters gebruiken *bovendien* naar **OutputPath**. Als **OutputPath** niet is opgegeven, het hulpprogramma log verzameling maakt gebruik van het systeemstation van de VM PEP voor opslag. Hierdoor kan het script is mislukt, omdat de schijfruimte beperkt is.
- Zoals weergegeven in het vorige voorbeeld de **FromDate** en **ToDate** parameters kunnen worden gebruikt voor het verzamelen van Logboeken voor een bepaalde periode. Dit kan erg handig scenario's zoals het verzamelen van Logboeken nadat een updatepakket zijn toegepast op een geïntegreerde systeem.

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>De parameter-overwegingen voor zowel ASDK en geïntegreerde systemen

- Als de **FromDate** en **ToDate** parameters niet zijn opgegeven, logboeken worden standaard verzameld voor de afgelopen vier uur.
- U kunt de **TimeOutInMinutes** -parameter voor de time-out voor logboekverzameling ingesteld. Dit is standaard ingesteld op 150 (2,5 uur).

- Op dit moment kunt u de **FilterByRole** parameter filter logboekgegevens verzameld door de volgende rollen:

   |   |   |   |
   | - | - | - |
   | ACSMigrationService     | ACSMonitoringService   | ACSSettingsService |
   | ACS                     | ACSFabric              | ACSFrontEnd        |
   | ACSTableMaster          | ACSTableServer         | ACSWac             |
   | ADFS                    | ASAppGateway           | BareMetal          |
   | BRP                     | CA                     | CPI                |
   | CRP                     | DeploymentMachine      | DHCP               |
   | Domein                  | ECE                    | ECESeedRing        | 
   | FabricRing              | FabricRingServices     | FRP                |
   | Gateway                 | HealthMonitoring       | HRP                |   
   | IBC                     | InfraServiceController | KeyVaultAdminResourceProvider|
   | KeyVaultControlPlane    | KeyVaultDataPlane      | NC                 |   
   | NonPrivilegedAppGateway | NRP                    | SeedRing           |
   | SeedRingServices        | SLB                    | SQL                |   
   | SRP                     | Storage                | StorageController  |
   | URP                     | UsageBridge            | VirtualMachines    |  
   | WAS                     | WASPUBLIC              | WDS                |


### <a name="bkmk_gui"></a>Verzamelen van logboeken met behulp van een grafische gebruikersinterface
In plaats van de vereiste parameters voor de cmdlet Get-AzureStackLog voor het ophalen van Azure-Stack-logboeken bieden, kunt u gebruikmaken van de beschikbare open-source Azure Stack-hulpprogramma's zich in de belangrijkste Azure Stack extra GitHub extra opslagplaats op http://aka.ms/AzureStackTools.

De **ERCS_AzureStackLogs.ps1** PowerShell-script wordt opgeslagen in de GitHub-opslagplaats voor hulpprogramma's en regelmatig wordt bijgewerkt. Om ervoor te zorgen dat u de laatst beschikbare versie hebt, moet u deze rechtstreeks vanuit http://aka.ms/ERCS downloaden. Het script hebt gestart vanuit een administratief PowerShell-sessie, verbinding maakt met het bevoorrechte eindpunt en Get-AzureStackLog uitvoert met de opgegeven parameters. Als er geen parameters zijn opgegeven, wordt het script wordt standaard ingesteld op te vragen voor parameters via een grafische gebruikersinterface.

U kunt bekijken voor meer informatie over het ERCS_AzureStackLogs.ps1 PowerShell-script, [een korte video](https://www.youtube.com/watch?v=Utt7pLsXEBc) of weergeven van het script [Leesmij-bestand](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md) zich in de Azure-Stack extra GitHub-opslagplaats. 

### <a name="additional-considerations"></a>Aanvullende overwegingen

* De opdracht wordt uitgevoerd op basis van welke rollen van Logboeken van de verzamelen enige tijd. Factoren omvatten ook de tijd die is opgegeven voor het logboek verzamelen en het aantal knooppunten in de Azure-Stack-omgeving.
* Nadat het logboekverzameling is voltooid, controleert u de nieuwe map gemaakt de **OutputPath** parameter die is opgegeven in de opdracht.
* Elke functie heeft haar Logboeken in afzonderlijke zip-bestanden. Afhankelijk van de grootte van de logboeken die worden verzameld, kan een rol de logboeken splitsen in meerdere zip-bestanden hebben. Gebruik een hulpprogramma dat kunt pak bulksgewijs (zoals 7zip) voor een functie als u dat alle logboekbestanden naar één map wilt zijn uitgepakt. Selecteer de gecomprimeerde bestanden voor de rol en selecteer **extraheren hier**. Hiermee wordt de logboekbestanden voor die rol in een enkele samengevoegde map uitgepakt.
* Een bestand met de naam **Get-AzureStackLog_Output.log** ook in de map waarin de gecomprimeerde logboekbestanden gemaakt. Dit bestand is een logboek van de uitvoer van de opdracht, die kan worden gebruikt voor het oplossen van problemen tijdens de logboekgegevens verzameld.
* Voor het onderzoeken van een specifieke fout mogelijk van meer dan één component logboeken nodig.
    -   Systeem- en gebeurtenislogboeken voor alle virtuele machines van infrastructuur worden bijgehouden in de *virtuele machines* rol.
    -   Systeem- en gebeurtenislogboeken op alle hosts worden bijgehouden in de *BareMetal* rol.
    -   Failover-Cluster en Hyper-V logboeken worden bijgehouden in de *opslag* rol.
    -   ACS-logboeken worden bijgehouden in de *opslag* en *ACS* rollen.

> [!NOTE]
> Grootte en leeftijd limieten gelden voor de logboeken die worden verzameld omdat het is essentieel om ervoor te zorgen efficiënt gebruik van uw opslagruimte om te controleren of dat deze flooded met Logboeken niet ophalen. Bij het oplossen van een probleem moet u echter soms logboeken die niet meer vanwege deze limieten bestaat mogelijk. Het is dus **ten zeerste aangeraden** offloaden van uw logboeken naar een externe opslagruimte (een opslagaccount in Azure, een opslagapparaat extra on-premises enzovoort) in elke 8 tot 12 uur en bewaar deze er voor 1-3 maanden, afhankelijk van uw vereisten. Controleer ook op dat deze locatie voor de opslag is versleuteld.

## <a name="next-steps"></a>Volgende stappen
[Microsoft Azure-Stack probleemoplossing](azure-stack-troubleshooting.md)

