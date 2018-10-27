---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 65477f62af80511a73307204c2a6f4b5e0f409d6
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164573"
---
1. Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen. Voer de volgende opdrachten uit als beheerder bij een opdrachtprompt:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Voer de volgende opdracht voor het installeren van de Mobility-Service:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Nu moet de agent worden geregistreerd bij de configuratieserver.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Opdrachtregelargumenten van de Mobility-Service-installatieprogramma

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parameter|Type|Beschrijving|Mogelijke waarden|
|-|-|-|-|
|/ Rol|Verplicht|Hiermee geeft u op of de Mobility-Service (MS) moet worden geïnstalleerd of MasterTarget (MT) moet worden geïnstalleerd.|MS </br> MT|
|/InstallLocation|Optioneel|De locatie waarop de Mobility-Service is geïnstalleerd.|Een map op de computer|
|/Platform|Verplicht|Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. </br> </br>- **VMware**: deze waarde wordt gebruikt als u de Mobility-Service op een virtuele machine die wordt uitgevoerd installeren op *VMware vSphere ESXi-hosts*, *Hyper-V-hosts*, en *fysieke servers*. </br> - **Azure**: deze waarde wordt gebruikt als u een agent op een Azure IaaS-VM installeren. | VMware </br> Azure|
|/ Op de achtergrond|Optioneel|Hiermee geeft u het installatieprogramma uitvoeren in stille modus.| N/A|

>[!TIP]
> De setup-logboeken kunnen u vinden onder % ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.

#### <a name="mobility-service-registration-command-line-arguments"></a>Opdrachtregelargumenten van de Mobility-Service-registratie

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parameter|Type|Beschrijving|Mogelijke waarden|
  |-|-|-|-|
  |/CSEndPoint |Verplicht|IP-adres van de configuratieserver| Een geldig IP-adres|
  |/PassphraseFilePath|Verplicht|Locatie van de wachtwoordzin |Een geldig UNC- of lokaal bestandspad|


>[!TIP]
> De configuratie van Agent-logboeken kunnen u vinden onder % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
