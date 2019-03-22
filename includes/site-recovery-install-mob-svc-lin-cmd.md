---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: aa2e6d80620f0a4cf5063919a6de53e4de20f706
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58077007"
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

|Parameter|Type|Description|Mogelijke waarden|
|-|-|-|-|
|-r |Verplicht|Hiermee geeft u op of de Mobility-Service (MS) moet worden geïnstalleerd of MasterTarget (MT) moet worden geïnstalleerd.|MS </br> MT|
|-d |Optioneel|De locatie waarop de Mobility-Service is geïnstalleerd.|/usr/local/ASR|
|-v|Verplicht|Hiermee geeft u het platform waarop de Mobility-Service is geïnstalleerd. </br> </br>- **VMware**: Deze waarde wordt gebruikt als u de Mobility-Service op een virtuele machine die wordt uitgevoerd installeren op *VMware vSphere ESXi-hosts*, *Hyper-V-hosts*, en *fysieke servers*. </br> - **Azure**: Gebruik deze waarde als u een agent op een Azure IaaS-VM installeren.| VMware </br> Azure|
|-q|Optioneel|Hiermee geeft u het installatieprogramma uitvoeren in stille modus.| N/A|


#### <a name="mobility-service-configuration-command-line"></a>Mobility-Service configureren vanaf de opdrachtregel

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Type|Description|Mogelijke waarden|
|-|-|-|-|
|-i |Verplicht|IP-adres van de configuratieserver|Een geldig IP-adres|
|-P |Verplicht|Volledig pad voor het bestand waarin de woordgroep voor de verbinding wordt opgeslagen|Een geldige map|
