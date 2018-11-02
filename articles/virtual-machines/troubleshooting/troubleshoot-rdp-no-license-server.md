---
title: De extern bureaublad-licentieserver niet beschikbaar wanneer u verbinding met een Azure-VM maakt | Microsoft Docs
description: Informatie over het oplossen van problemen met RDP is mislukt omdat er geen extern bureaublad-licentieserver beschikbaar | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 550b971602d1736e0ba3981a5b7ca546862ea034
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913850"
---
# <a name="remote-desktop-license-server-isnt-available-when-you-connect-to-an-azure-vm"></a>Extern bureaublad-licentieserver niet beschikbaar wanneer u verbinding met een Azure-VM maakt

Dit artikel helpt het probleem wordt opgelost wanneer u geen verbinding met een Azure-machine (VM maken) omdat er geen extern bureaublad-licentieserver beschikbaar is voor het bieden van een licentie.

## <a name="symptoms"></a>Symptomen

Wanneer u probeert verbinding maken met een virtuele machine (VM), treden de volgende scenario's:

- De VM-schermafbeelding ziet u dat het besturingssysteem volledig geladen en in afwachting van referenties is.
- U ontvangt de volgende foutberichten weergegeven wanneer u probeert een Microsoft Remote Desktop Protocol (RDP)-verbinding te maken:

  - De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar, voor een licentie.

  - Er is geen extern bureaublad-licentieserver is beschikbaar. Extern bureaublad-Services meer werken omdat deze computer voorbij de respijtperiode is en nog niet is verbinding gemaakt met ten minste een geldige Windows Server 2008-licentieserver. Selecteer dit bericht extern bureaublad-sessiehost-Server configureren voor het gebruik van Licentiecontrole openen.

Echter, u kunt verbinding maken met de virtuele machine normaal gesproken met behulp van een beheersessie:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Oorzaak

Dit probleem treedt op als een extern bureaublad-licentieserver niet beschikbaar is voor een licentie voor het starten van een externe sessie. Dit kan worden veroorzaakt door verschillende scenario's, zelfs als een Remote Desktop Session Host-rol is ingesteld op de virtuele machine:

- Er is nooit een extern bureaublad-licentieverlening rol in de omgeving en de respijtperiode 180 dagen ligt boven.
- Een extern bureaublad-licentie is geïnstalleerd in de omgeving, maar deze nooit geactiveerd.
- Een extern bureaublad-licentieserver in de omgeving heeft geen Client Access Licenses (CAL's) is toegevoegd om de verbinding in te stellen.
- Een extern bureaublad-licentie is geïnstalleerd in de omgeving. Er zijn CAL's beschikbaar, maar ze zijn niet juist geconfigureerd.
- Een extern bureaublad-licentie heeft CAL's en deze is geactiveerd. Echter enkele andere problemen op de extern bureaublad-licentieserver te voorkomen dat het leveren van licenties in de omgeving.

## <a name="solution"></a>Oplossing

Dit probleem op te lossen [maakt u een back-up van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md) en volg deze stappen:

1. Verbinding maken met de virtuele machine met een beheersessie:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

    Als u geen verbinding met de virtuele machine maken met behulp van een beheersessie, kunt u de [seriële Console van de virtuele Machine op Azure](serial-console-windows.md) toegang krijgen tot de virtuele machine als volgt:

    1. Toegang tot de seriële Console door te selecteren **ondersteuning en probleemoplossing** > **(Preview) van de seriële console**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

    2. Maak een nieuw kanaal voor een CMD-exemplaar. Voer **CMD** naar het kanaal begint en naam van het kanaal.

    3. Schakel over naar het kanaal dat u de CMD-exemplaar wordt uitgevoerd. In dit geval moet het kanaal 1:

       ```
       ch -si 1
       ```

    4. Selecteer **Enter** opnieuw en voer een geldige gebruikersnaam en wachtwoord, lokaal of domein-ID voor de virtuele machine.

2. Controleer of de virtuele machine een Remote Desktop Session Host-rol is ingeschakeld heeft. Als de functie is ingeschakeld, controleert u of deze correct werkt. Open een verhoogde CMD-exemplaar en volg deze stappen:

    1. Gebruik de volgende opdracht om de status van de Remote Desktop Session Host-rol te controleren:

       ```
        reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
        ```

        Als deze opdracht een waarde van 0 retourneert, betekent dit dat de functie is uitgeschakeld en u naar stap 3 gaat.

    2. Gebruik de volgende opdracht om te controleren van het beleid en zo nodig opnieuw te configureren:

       ```
        reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
       ```

        Als de **LicensingMode** waarde is ingesteld op een andere waarde dan 4, per gebruiker, wordt de waarde ingesteld op 4:

         ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
        ```

       Als de **SpecifiedLicenseServers** waarde bestaat niet of heeft onjuiste licentiegegevens voor server, als volgt wijzigen:

       ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
       ```

    3. Nadat u wijzigingen in het register aanbrengt, start u de virtuele machine opnieuw.

    4. Als u geen CAL's hebt, verwijdert u de Remote Desktop Session Host-rol. Vervolgens wordt het RDP-protocol opnieuw worden ingesteld op normaal. Hierdoor kan alleen twee gelijktijdige RDP-verbindingen met de virtuele machine:

        ```
       dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
        ```

        Als de virtuele machine de rol van extern bureaublad-licentieverlening heeft en deze wordt niet gebruikt, kunt u ook die rol verwijderen:

       ```
        dism /ONLINE /Disable-feature /FeatureName:Licensing
       ```

    5. Zorg ervoor dat de virtuele machine verbinding met de extern bureaublad-licentieserver maken kan. U kunt de verbinding met de poort 135 tussen de virtuele machine en de licentieserver testen: 

       ```
       telnet <FQDN / IP License Server> 135
       ```

3. Als er geen extern bureaublad-licentieserver in de omgeving en u maken wilt, kunt u [installeren van een extern bureaublad-licentieverlening functieservice](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11)). Vervolgens [de extern bureaublad-Services-licentieverlening configureren](https://blogs.technet.microsoft.com/askperf/2013/09/20/rd-licensing-configuration-on-windows-server-2012/).

4. Als een extern bureaublad-licentieserver geconfigureerd en in orde is is, controleert u dat de extern bureaublad-licentieserver is geactiveerd met CAL's.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem is opgelost.
