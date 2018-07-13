---
title: Oplossen van problemen met activering van Windows-virtuele machine in Azure | Microsoft Docs
description: Biedt de stappen voor problemen oplossen voor het oplossen van problemen met de activering van de Windows virtuele machine in Azure
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 3dc6466083a3dd6882933b8acdd7d64e4a8acd04
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006985"
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Problemen met Windows Azure virtuele machine-activering

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Als u problemen ondervindt bij het activeren van Windows Azure virtuele machine (VM) die is gemaakt op basis van een aangepaste installatiekopie, kunt u de informatie in dit document om het probleem te verhelpen. 

## <a name="understanding-azure-kms-endpoints-for-windows-product-activation-of-azure-virtual-machines"></a>Wat is Azure KMS-eindpunten voor Windows-productactivering van Azure Virtual Machines?
Azure maakt gebruik van verschillende eindpunten voor KMS-activering, afhankelijk van de cloud-regio waar de virtuele machine zich bevindt. Wanneer u deze handleiding voor probleemoplossing, gebruikt u de juiste KMS-eindpunt dat van toepassing voor uw regio is.

* Openbare cloud van Azure-regio's: kms.core.windows.net:1688
* Azure China nationale cloud-regio's: kms.core.chinacloudapi.cn:1688
* Azure Duitsland nationale cloud-regio's: kms.core.cloudapi.de:1688
* Azure VS (overheid) nationale cloud-regio's: kms.core.usgovcloudapi.net:1688

## <a name="symptom"></a>Symptoom

Als u probeert te activeren van een Windows Azure VM, krijgt u een fout bericht lijkt op het volgende voorbeeld:

**Fout: 0xC004F074 die de Software-LicensingService gerapporteerd dat de computer niet kan worden geactiveerd. Er is geen sleutel managementservice zijn (KMS) kan worden bereikt. Raadpleeg het toepassingsgebeurtenislogboek voor meer informatie.**

## <a name="cause"></a>Oorzaak
Over het algemeen optreden activeringsproblemen virtuele Azure-machine als de Windows-VM niet is geconfigureerd met behulp van de desbetreffende Installatiecode voor KMS-client of de Windows-VM een verbindingsprobleem met de Azure-KMS-service (kms.core.windows.net, poort 1668 heeft). 

## <a name="solution"></a>Oplossing

>[!NOTE]
>Als u een site-naar-site-VPN en geforceerde tunneling, Zie [aangepaste routes gebruiken Azure om in te schakelen van KMS-activering met geforceerde tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>Als u ExpressRoute gebruikt en u hebt een standaard-route die zijn gepubliceerd, Zie [virtuele Azure-machine kan niet worden geactiveerd via ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>Stap 1 de desbetreffende Installatiecode voor KMS-client configureren (voor Windows Server 2016 en Windows Server 2012 R2)

Voor de virtuele machine die is gemaakt op basis van een aangepaste installatiekopie van Windows Server 2016 of Windows Server 2012 R2, moet u de desbetreffende Installatiecode voor KMS-client configureren voor de virtuele machine.

Deze stap is niet van toepassing op Windows 2012 of Windows 2008 R2. De functie activering automatisering van virtuele Machine (AVMA), die wordt alleen ondersteund door Windows Server 2016 en Windows Server 2012 R2 wordt gebruikt.

1. Voer **slmgr.vbs/dlv** bij een opdrachtprompt met verhoogde bevoegdheid. Controleer de waarde van de beschrijving in de uitvoer en vervolgens kunt u bepalen of deze is gemaakt van retail (detailhandel) of (VOLUME_KMSCLIENT) volume license-media:
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Als **slmgr.vbs/dlv** toont detailhandel, voer de volgende opdrachten om in te stellen de [Installatiecode voor KMS-client](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) voor de versie van Windows Server wordt gebruikt en geforceerd probeer opnieuw te activeren: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Bijvoorbeeld, voor Windows Server 2016 Datacenter, moet u de volgende opdracht uitvoeren:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>Stap 2 de connectiviteit tussen de virtuele machine en Azure KMS-service controleren

1. Downloaden en uitpakken van de [PSping](http:/technet.microsoft.com/sysinternals/jj729731.aspx) hulpprogramma naar een lokale map op de virtuele machine die niet worden geactiveerd. 

2. Naar begin gaan, zoeken op Windows PowerShell, met de rechtermuisknop op Windows PowerShell en selecteer vervolgens als administrator uitvoeren.

3. Zorg ervoor dat de virtuele machine is geconfigureerd voor het gebruik van de juiste Azure KMS-server. U doet dit door de volgende opdracht uitvoeren:
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    De opdracht moet worden geretourneerd: naam van de Key Management Service-machine ingesteld op kms.core.windows.net:1688 is.

4. Controleer of met behulp van Psping dat u verbinding met de KMS-server hebben. Ga naar de map waar u het downloaden van het Pstools.zip uitgepakt en voer het volgende:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  Zorg ervoor dat u ziet in de tweede laatste regel van de uitvoer: verzonden = 4, ontvangen = 4, verloren = 0 (0% verlies).

  Als verloren is groter dan 0 (nul), is de virtuele machine heeft geen verbinding met de KMS-server. In dit geval is als de virtuele machine zich in een virtueel netwerk en heeft een aangepaste DNS-server opgegeven, moet u ervoor zorgen dat DNS-server kunnen omzetten van kms.core.windows.net. Of de DNS-server wijzigt in een kms.core.windows.net is opgelost.

  U ziet dat als u alle DNS-servers van een virtueel netwerk verwijderen, VM's gebruiken de interne DNS-service van Azure. Deze service kunt kms.core.windows.net oplossen.
  
Controleer ook of de Gast-firewall niet is geconfigureerd op een manier die activering wordt blokkeren.

5. Nadat u geslaagde verbinding met kms.core.windows.net verifieert, voer de volgende opdracht bij die met verhoogde bevoegdheid Windows PowerShell-prompt. Met deze opdracht wordt activering geprobeerd meerdere keren.

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Een geslaagde activering retourneert informatie die lijkt op het volgende:

**Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) activeren... Product is geactiveerd.**

## <a name="faq"></a>Veelgestelde vragen 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Ik heb de Windows Server 2016 op Azure Marketplace hebt gemaakt. Heb ik nodig voor het configureren van KMS-sleutel voor het activeren van de Windows Server 2016? 
 
Nee. De installatiekopie in Azure Marketplace is de juiste KMS-clientinstallatiecode al geconfigureerd. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Windows-activering werkt hetzelfde ongeacht als de virtuele machine van Azure Hybrid Use Benefit (HUB) of niet gebruikmaakt? 
 
Ja. 
 
### <a name="what-happens-if-windows-activation-period-expires"></a>Wat gebeurt er als Windows is verstreken? 
 
Wanneer de respijtperiode is verlopen en Windows is nog niet geactiveerd, wordt Windows Server 2008 R2 en latere versies van Windows aanvullende meldingen over het activeren van weergeven. Achtergrond van het bureaublad blijft zwart en Windows Update wordt geïnstalleerd, beveiliging en alleen essentiële updates, maar niet optionele updates. Zie de sectie meldingen aan de onderkant van de [licentieverlening voorwaarden](http://technet.microsoft.com/library/ff793403.aspx) pagina.   

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
