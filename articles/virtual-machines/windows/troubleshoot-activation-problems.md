---
title: Problemen met Windows virtuele machine activering in Azure | Microsoft Docs
description: Biedt de stappen voor probleemoplossing voor het oplossen van problemen met Windows virtuele machine activation in Azure
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: f6095d98ada2974bce03ec8f5527367837daafd3
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Windows Azure virtuele machine activation problemen oplossen

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u problemen hebt bij het activeren van Windows Azure virtuele machine (VM) die van een aangepaste installatiekopie wordt gemaakt, kunt u de informatie in dit document op te lossen. 

## <a name="symptom"></a>Symptoom

Wanneer u probeert te activeren van een Windows Azure VM, u een foutbericht ontvangt bericht lijkt op het volgende voorbeeld:

**Fout: 0xC004F074 die de LicensingService Software gerapporteerd dat de computer niet kan worden geactiveerd. Er is geen ManagementService KMS (Key) kan worden bereikt. Raadpleeg het logboek voor toepassingsgebeurtenissen voor meer informatie.**

## <a name="cause"></a>Oorzaak
In het algemeen optreden activeringsproblemen Azure VM als de virtuele machine van Windows is niet geconfigureerd met behulp van de desbetreffende Installatiecode voor KMS-client of de virtuele machine van Windows een probleem met de Azure-KMS-service (kms.core.windows.net, poort 1668 is). 

## <a name="solution"></a>Oplossing

>[!NOTE]
>Als u een site-naar-site-VPN en geforceerde tunneling, Zie [gebruik Azure aangepaste routes om in te schakelen van KMS-activering met geforceerde tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>Als u ExpressRoute gebruikt en u hebt een standaardroute gepubliceerd, Zie [Azure VM mislukken om te activeren via ExpressRoute](https://blogs.technet.microsoft.com/jpaztech/2016/05/16/azure-vm-may-fail-to-activate-over-expressroute/).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>Stap 1 de desbetreffende Installatiecode voor KMS-client configureren (voor Windows Server 2016 en Windows Server 2012 R2)

Voor de virtuele machine die wordt gemaakt van een aangepaste installatiekopie van Windows Server 2016 of Windows Server 2012 R2, moet u de desbetreffende Installatiecode voor KMS-client configureren voor de virtuele machine.

Deze stap is niet van toepassing op Windows 2012 of Windows 2008 R2. De functie activering automatisering van virtuele Machine (AVMA), waardoor wordt alleen ondersteund door Windows Server 2016 en Windows Server 2012 R2 wordt gebruikt.

1. Voer **slmgr.vbs/dlv** bij een opdrachtprompt met verhoogde bevoegdheden. Controleer de waarde van de beschrijving in de uitvoer en Ga na of deze is gemaakt vanuit retail (RETAIL kanaal) of (VOLUME_KMSCLIENT) volume license-media:
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Als **slmgr.vbs/dlv** detailhandel, voer de volgende opdrachten om in te stellen toont de [Installatiecode voor KMS-client](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) voor de versie van Windows Server wordt gebruikt en gedwongen proberen opnieuw te activeren: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Bijvoorbeeld: voor Windows Server 2016 Datacenter, moet u de volgende opdracht uitvoeren:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>Stap 2 de connectiviteit tussen de virtuele machine en Azure KMS-service controleren

1. Downloaden en uitpakken van de [psping om](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) hulpprogramma naar een lokale map in de virtuele machine die niet worden geactiveerd. 

2. Ga naar Start, zoeken op Windows PowerShell, met de rechtermuisknop op Windows PowerShell en selecteer als administrator uitvoeren.

3. Zorg ervoor dat de virtuele machine is geconfigureerd voor gebruik van de juiste Azure KMS-server. Voer hiertoe de volgende opdracht:
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    De opdracht moet worden geretourneerd: naam van de Key Management Service-machine ingesteld op kms.core.windows.net:1688 is.

4. Controleer of met behulp van psping om dat er een verbinding met de KMS-server. Ga naar de map waar u het downloaden van de Pstools.zip uitgepakt en voer het volgende:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  Zorg ervoor dat u ziet in de uitvoer van de laatste seconde coderegel: verzonden = 4, ontvangen = 4, verloren = 0 (0% verlies).

  Als verloren is groter dan 0 (nul), is de virtuele machine heeft geen verbinding met de KMS-server. In deze situatie is als de virtuele machine zich in een virtueel netwerk en heeft een aangepaste DNS-server opgegeven, moet u ervoor zorgen dat DNS-server kunnen omzetten kms.core.windows.net. Of wijzig de DNS-server in een kms.core.windows.net is opgelost.

  U ziet dat als u alle DNS-servers uit een virtueel netwerk verwijderen, virtuele machines van Azure interne DNS-service gebruiken. Deze service kunt kms.core.windows.net oplossen.
  
Controleer ook of de Gast-firewall is niet geconfigureerd op een wijze waarbij activation pogingen blokkeren.

5. Nadat u geslaagde verbinding met kms.core.windows.net verifieert, voer de volgende opdracht op die Windows PowerShell-prompt met verhoogde bevoegdheden. Met deze opdracht wordt activering geprobeerd meerdere keren.

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Een geslaagde activering wordt informatie de volgende strekking weergegeven:

**Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) activeren... Product is geactiveerd.**

## <a name="faq"></a>Veelgestelde vragen 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Ik heb de Windows Server 2016 gemaakt uit Azure Marketplace. Heb ik nodig voor het configureren van de KMS-code voor het activeren van de Windows Server 2016? 
 
Nee. De afbeelding in Azure Marketplace heeft de juiste KMS clientinstallatiecode al is geconfigureerd. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Windows-activering werkt hetzelfde ongeacht als de virtuele machine van Azure hybride gebruik voordeel (HUB) of niet gebruikmaakt? 
 
Ja. 
 
### <a name="what-happens-if-windows-activation-period-expires"></a>Wat gebeurt er als Windows-activering is verlopen? 
 
Wanneer de respijtperiode is verlopen en Windows is nog niet geactiveerd, wordt Windows Server 2008 R2 en latere versies van Windows extra meldingen over het activeren van weergeven. De bureaubladachtergrond blijft zwart en Windows Update wordt geïnstalleerd, beveiliging en alleen essentiële updates, maar niet optionele updates. Zie de sectie meldingen aan de onderkant van de [licentieverlening voorwaarden](http://technet.microsoft.com/en-us/library/ff793403.aspx) pagina.   

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
