---
title: Updates in Azure Stack met behulp van het eindpunt van de bevoegde controleren | Microsoft Docs
description: Informatie over het gebruik van het eindpunt van de bevoegdheden voor het bewaken van de updatestatus voor geïntegreerde Azure Stack-systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: 09d3d996e4c939d6691162d66f303536a3f2038d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239261"
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Updates controleren in Azure Stack met behulp van de bevoegde eindpunt

*Van toepassing op: Azure Stack-geïntegreerde systemen*

U kunt de [bevoegde eindpunt](azure-stack-privileged-endpoint.md) voor het bewaken van de voortgang van een Azure Stack update-uitvoering en hervatten van een mislukte update uitvoeren vanaf de laatste geslaagde stap moet de Azure Stack-portal niet beschikbaar.  Met behulp van de Azure Stack-portal is de aanbevolen methode voor het beheren van updates in Azure Stack.

De volgende nieuwe PowerShell-cmdlets voor het updatebeheer van de zijn opgenomen in de update 1710 voor geïntegreerde Azure Stack-systemen.

| Cmdlet  | Beschrijving  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Retourneert de status van de update momenteel wordt uitgevoerd, voltooid of mislukt. Biedt de statuswaarde op hoog niveau van de update-uitvoering en een XML-document met een beschrijving van zowel de huidige stap en de bijbehorende status. |
| `Resume-AzureStackUpdate` | Hervat een mislukte update op het punt waar deze is mislukt. Mogelijk moet u stappen voltooien voordat u de update hervatten in bepaalde scenario's.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Controleer of dat de cmdlets zijn beschikbaar
Omdat de cmdlets nieuw in het updatepakket van 1710 voor Azure Stack zijn, moet het updateproces 1710 om te gaan naar een bepaald moment voordat de detectie beschikbaar is. Normaal gesproken de cmdlets zijn beschikbaar als de status in de beheerdersportal geeft aan dat de update 1710 aan de **opslag Hosts opnieuw** stap. Met name de cmdlet update plaatsvindt tijdens **stap: Uitvoeren van stap 2.6 - de Update PrivilegedEndpoint whitelist**.

U kunt ook bepalen of de cmdlets zijn beschikbaar via een programma door het opvragen van de lijst met opdrachten van de bevoegde eindpunt. U doet dit door de volgende opdrachten worden uitgevoerd vanaf de host van de levenscyclus van hardware of op een werkstation met bevoegde toegang. Controleer ook of dat het eindpunt van de bevoegde is een vertrouwde host. Voor meer informatie raadpleegt u stap 1 van [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Maken van een PowerShell-sessie op een van de ERCS virtuele machines in uw Azure Stack-omgeving (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*-ERCS03). Vervang *voorvoegsel* met de tekenreeks voor het voorvoegsel van virtuele machine die specifiek is voor uw omgeving.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Wanneer u hierom wordt gevraagd om referenties op te geven, gebruikt u de &lt; *Azure Stack-domein*&gt;\cloudadmin, of een account dat lid is van de groep CloudAdmins. Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domein administrator-account voor het account CloudAdmin.

2. De volledige lijst met opdrachten die beschikbaar in het privileged eindpunt zijn ophalen. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Vaststellen of het eindpunt van de bevoegde is bijgewerkt.

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
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>De update management-cmdlets gebruiken

> [!NOTE]
> De volgende opdrachten uitvoeren vanaf de host van de levenscyclus van hardware of op een werkstation met bevoegde toegang. Controleer ook of dat het eindpunt van de bevoegde is een vertrouwde host. Voor meer informatie raadpleegt u stap 1 van [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Verbinding maken met het eindpunt van de bevoegde en toewijzen van de variabele van sessie

Voer de volgende opdrachten om te maken van een PowerShell-sessie op een van de ERCS virtuele machines in uw Azure Stack-omgeving (*voorvoegsel*-ERCS01, *voorvoegsel*-ERCS02, of *voorvoegsel*-ERCS03), en om toe te wijzen een sessievariabele.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Wanneer u hierom wordt gevraagd om referenties op te geven, gebruikt u de &lt; *Azure Stack-domein*&gt;\cloudadmin, of een account dat lid is van de groep CloudAdmins. Voer het wachtwoord dat is opgegeven tijdens de installatie voor de AzureStackAdmin domein administrator-account voor het account CloudAdmin.

### <a name="get-high-level-status-of-the-current-update-run"></a>De statuswaarde op hoog niveau van de huidige update-uitvoering ophalen 

Als u een statuswaarde op hoog niveau van de huidige update-uitvoering, voer de volgende opdrachten: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Mogelijke waarden:

- In uitvoering
- Voltooid
- Mislukt 
- Geannuleerd

U kunt deze opdrachten herhaaldelijk om te zien van de meest recente status uitvoeren. U hebt geen opnieuw verbinding opnieuw te controleren.

### <a name="get-the-full-update-run-status-with-details"></a>De volledige update uitvoeringsstatus met details 

U kunt de volledige update-uitvoering samenvatting als een XML-tekenreeks. U kunt de tekenreeks naar een bestand voor onderzoek, schrijven of converteren naar een XML-document en PowerShell gebruiken om te parseren. De volgende opdracht parseert het XML-bestand als u een hiërarchische lijst van de stappen die momenteel wordt uitgevoerd.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

In het volgende voorbeeld heeft de op het hoogste niveau stap (Cloud-Update) een plan voor onderliggende bijwerken en opnieuw starten van de opslag-hosts. U ziet dat het plan opnieuw opslag Hosts van de Blob Storage-service op een van de hosts bijwerken.

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

### <a name="resume-a-failed-update-operation"></a>Een mislukte update hervatten

Als de update is mislukt, kunt u de update-uitvoering waar deze afgebroken hervatten.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Problemen oplossen

Het eindpunt van de bevoegde is beschikbaar op alle ERCS virtuele machines in de Azure Stack-omgeving. Omdat de verbinding niet naar een maximaal beschikbare eindpunt is gemaakt, kunt u incidentele onderbrekingen, waarschuwingen of foutberichten ondervinden. Deze berichten kunnen erop wijzen dat de sessie is beëindigd of dat er is een fout opgetreden bij het communiceren met de Service EEG. Dit gedrag is verwacht. U kunt de bewerking over een paar minuten opnieuw proberen of een nieuwe privileged endpoint-sessie op een van de andere ERCS virtuele machines maken. 

## <a name="next-steps"></a>Volgende stappen

- [Updates beheren in Azure Stack](azure-stack-updates.md) 


