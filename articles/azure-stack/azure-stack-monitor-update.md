---
title: In Azure-Stack met behulp van de bevoegde endpoint-updates controleren | Microsoft Docs
description: "Informatie over het gebruik van het bevoegde eindpunt om te bewaken updatestatus voor Azure-Stack geïntegreerd systemen."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: mabrigg
ms.openlocfilehash: 96eebf340f13f2f5e9e922fee8032d04fce1d130
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Updates controleren in Azure-Stack met behulp van de bevoegde eindpunt

*Van toepassing op: Azure Stack geïntegreerd systemen*

U kunt het bevoegde eindpunt de voortgang van een Azure-Stack update uitvoeren en hervatten van een mislukte update uitvoeren vanaf de laatste geslaagde stap moet u de Stack Azure portal niet meer beschikbaar.  Met de Stack van Azure-portal is de aanbevolen methode voor het beheren van updates in Azure-Stack.

De volgende nieuwe PowerShell-cmdlets voor het updatebeheer van de zijn opgenomen in de update 1710 voor Azure-Stack geïntegreerd systemen.

| Cmdlet  | Beschrijving  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Retourneert de status van de update van de momenteel actieve, voltooid of mislukt. Biedt de statuswaarde op hoog niveau van de updatebewerking en een XML-document met een beschrijving van zowel de huidige stap en de bijbehorende status. |
| `Get-AzureStackUpdateVerboseLog` | Retourneert de uitgebreide logboeken die worden gegenereerd door de update. |
| `Resume-AzureStackUpdate` | Hervatten van een mislukte update op het punt waar deze is mislukt. In bepaalde gevallen, moet u wellicht risicobeperking stappen voltooien voordat u de update hervatten.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Controleer of dat de cmdlets zijn beschikbaar
Omdat de cmdlets nieuw in het updatepakket van 1710 voor Azure-Stack zijn, moet het updateproces 1710 krijgen tot een bepaald moment voordat de bewakingsmogelijkheid beschikbaar is. Normaal gesproken de cmdlets zijn beschikbaar als de status in de beheerdersportal geeft aan dat de update 1710 aan de **opslag Hosts opnieuw** stap. In het bijzonder de cmdlet update plaatsvindt tijdens **stap: uitvoeren van stap 2.6 - PrivilegedEndpoint werken geaccepteerde**.

U kunt ook bepalen of de cmdlets zijn beschikbaar via een programma door de opdrachtenlijst van de bevoegde eindpunt opvragen. Voer hiervoor de volgende opdrachten vanaf de host van de levenscyclus van hardware of vanaf een bevoorrechte toegang werkstation. Controleer ook of dat het eindpunt van de bevoegdheden heeft een vertrouwde host. Zie voor meer informatie stap 1 van [toegang tot de bevoegde eindpunt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Maken van een PowerShell-sessie op een van de ERCS virtuele machines in uw Azure-Stack-omgeving (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*-ERCS03). Vervang *voorvoegsel* met de virtuele machine voorvoegseltekenreeks die specifiek is voor uw omgeving.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Wanneer u wordt gevraagd om referenties, gebruiken de &lt; *Azure Stack domein*&gt;\cloudadmin-account of een account dat lid is van de groep CloudAdmins. Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domeinbeheerdersaccount voor het account CloudAdmin.

2. De volledige lijst met opdrachten die beschikbaar in de beschermde eindpunt zijn ophalen. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Vaststellen of de bevoegde eindpunt is bijgewerkt.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Lijst met de opdrachten die specifiek zijn voor de module Microsoft.AzureStack.UpdateManagement.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Bijvoorbeeld:
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Get-AzureStackUpdateVerboseLog                     0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>De update-cmdlets

> [!NOTE]
> De volgende opdrachten uitvoeren vanaf de host van de levenscyclus van hardware of vanaf een bevoorrechte toegang werkstation. Controleer ook of dat het eindpunt van de bevoegdheden heeft een vertrouwde host. Zie voor meer informatie stap 1 van [toegang tot de bevoegde eindpunt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Verbinding maken met de bevoorrechte eindpunt en sessievariabele toewijzen

Voer de volgende opdrachten een PowerShell-sessie maken op een van de ERCS virtuele machines in uw Azure-Stack-omgeving (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*-ERCS03), en een sessievariabele toewijzen.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Wanneer u wordt gevraagd om referenties, gebruiken de &lt; *Azure Stack domein*&gt;\cloudadmin-account of een account dat lid is van de groep CloudAdmins. Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domeinbeheerdersaccount voor het account CloudAdmin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Op hoog niveau van de status van de huidige update uitvoeren ophalen 

Als u een statuswaarde op hoog niveau van de huidige update uitvoeren, voer de volgende opdrachten: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Mogelijke waarden:

- Actief
- Voltooid
- Mislukt 
- Geannuleerd

U kunt deze opdrachten herhaaldelijk voor de meest actuele status uitvoeren. U moet niet opnieuw een verbinding opnieuw te controleren.

### <a name="get-the-full-update-run-status-with-details"></a>De volledige update uitvoeringsstatus met details ophalen 

U kunt de volledige update-uitvoering samenvatting als een XML-tekenreeks. U kunt de tekenreeks naar een bestand voor onderzoek, schrijven of converteren naar een XML-document en PowerShell gebruiken om te parseren. De volgende opdracht parseert het XML-bestand om een hiërarchische lijst van de stappen die momenteel worden uitgevoerd.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

In het volgende voorbeeld heeft de site op het hoogste stap (Cloud-Update) een onderliggende wilt bijwerken en opnieuw opstarten van de opslag-hosts. Laat het plan opslag Hosts opnieuw bijwerken van de Blob Storage-service op een van de hosts.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']") 

    FullStepIndex : 2
    Index         : 2
    Name          : Cloud Update
    Description   : Perform cloud update.
    StartTimeUtc  : 2017-10-13T12:50:39.9020351Z
    Status        : InProgress
    Task          : Task
    
    FullStepIndex  : 2.9
    Index          : 9
    Name           : Restart Storage Hosts
    Description    : Restart Storage Hosts.
    EceErrorAction : Stop
    StartTimeUtc   : 2017-10-13T15:44:06.7431447Z
    Status         : InProgress
    Task           : Task
    
    FullStepIndex : 2.9.2
    Index         : 2
    Name          : PreUpdate ACS Blob Service
    Description   : Check function level, update deployment artifacts, configure Blob service settings
    StartTimeUtc  : 2017-10-13T15:44:26.0708525Z
    Status        : InProgress
    Task          : Task
```

### <a name="get-the-verbose-progress-log"></a>Het logboek uitgebreide voortgang ophalen

U kunt het logboek schrijven naar een bestand voor onderzoek. Dit kunt u een update-fout onderzoeken.

```powershell
$log = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateVerboseLog }

$log > ".\UpdateVerboseLog.txt" 
```

### <a name="actively-view-the-verbose-logging"></a>De uitgebreide logboekregistratie actief weergeven

Naar actief weergave voor het uitgebreide logboek tijdens het bijwerken uitvoeren en Ga naar de meest recente vermeldingen de volgende opdrachten uitvoeren om in te voeren van de sessie in de interactieve modus en om het logboek weer te geven:

```powershell
Enter-PSSession -Session $pepSession 

Get-AzureStackUpdateVerboseLog -Wait 
```
Het logboek elke 60 seconden bijgewerkt en nieuwe inhoud (indien beschikbaar) is geschreven naar de console. 

Tijdens langlopende achtergrondprocessen, kan de uitvoer van de console niet worden geschreven naar de console gedurende een bepaalde periode. Als u wilt annuleren van de uitvoer van de interactieve, drukt u op Ctrl + C. 

### <a name="resume-a-failed-update-operation"></a>Een mislukte update hervatten

Als de update is mislukt, kunt u de update-uitvoering waar deze was gebleven hervatten.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Problemen oplossen

Het bevoegde eindpunt is beschikbaar op alle ERCS virtuele machines in de Azure-Stack-omgeving. Omdat de verbinding niet naar een maximaal beschikbare eindpunt gemaakt is, treden incidentele onderbrekingen, waarschuwingen of foutberichten op. Deze berichten kunnen erop wijzen dat de sessie is beëindigd of dat er is een fout opgetreden bij het communiceren met de Service ECE. Dit is verwacht gedrag. U kunt een nieuwe privileged endpoint-sessie maken op een van de andere ERCS virtuele machines of probeer het opnieuw over enkele minuten. 

## <a name="next-steps"></a>Volgende stappen

- [Het beheren van updates in Azure-Stack](azure-stack-updates.md) 


