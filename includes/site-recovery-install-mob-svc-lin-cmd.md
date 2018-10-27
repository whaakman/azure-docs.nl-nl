---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 2b92aba8b9a8d8f46ae2aeac3a7bfe60a4755f9b
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164640"
---
1. Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld: map/Tmp) op de server die u wilt beveiligen. Voer de volgende opdrachten in een terminal:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Voer de volgende opdracht voor het installeren van de Mobility-Service:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Nadat de installatie is voltooid, moet de Mobility-Service zijn geregistreerd met de configuratieserver. Voer de volgende opdracht om de Mobility-Service met de configuratieserver registreren:

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Mobility Service installer-opdrachtregel

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parameter|Type|Beschrijving|Mogelijke waarden|
|-|-|-|-|
|-r |Verplicht|Hiermee geeft u op of de Mobility-Service (MS) moet worden geïnstalleerd of MasterTarget (MT) moet worden geïnstalleerd.|MS </br> MT|
|-d |Optioneel|De locatie waarop de Mobility-Service is geïnstalleerd.|/usr/local/ASR|
|-v|Verplicht|Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. </br> </br>- **VMware**: deze waarde wordt gebruikt als u de Mobility-Service op een virtuele machine die wordt uitgevoerd installeren op *VMware vSphere ESXi-hosts*, *Hyper-V-hosts*, en *fysieke servers*. </br> - **Azure**: deze waarde wordt gebruikt als u een agent op een Azure IaaS-VM installeren.| VMware </br> Azure|
|-q|Optioneel|Hiermee geeft u het installatieprogramma uitvoeren in stille modus.| N/A|


#### <a name="mobility-service-configuration-command-line"></a>Mobility-Service configureren vanaf de opdrachtregel

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Type|Beschrijving|Mogelijke waarden|
|-|-|-|-|
|-i |Verplicht|IP-adres van de configuratieserver|Een geldig IP-adres|
|-P |Verplicht|Volledig pad voor het bestand waarin de woordgroep voor de verbinding wordt opgeslagen|Een geldige map|
