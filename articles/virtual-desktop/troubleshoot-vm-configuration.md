---
title: Het maken van tenant en de host van toepassingen in Windows virtueel bureaublad - Azure
description: Over het oplossen van problemen bij het configureren van een tenant en session host virtuele machine (VM) in een omgeving met virtuele Windows-bureaublad.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: v-chjenk
ms.openlocfilehash: 96a9d8fc7495ea473b0a3250b34251afc5f30c13
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786712"
---
# <a name="tenant-and-host-pool-creation"></a>Tenants en hostpools maken

Gebruik dit artikel als u problemen ondervindt bij het configureren van de virtuele Windows-bureaublad-sessie hosten van virtuele machines (VM's).

## <a name="provide-feedback"></a>Feedback geven

We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden.

## <a name="vms-are-not-joined-to-the-domain"></a>Virtuele machines zijn niet toegevoegd aan het domein

Volg deze instructies als u problemen hebt om virtuele machines toevoegen aan het domein.

- Deelnemen aan de virtuele machine handmatig met behulp van het proces in [een Windows Server-machine toevoegen aan een beheerd domein](https://docs.microsoft.com/azure/active-directory-domain-services/Active-directory-ds-admin-guide-join-windows-vm-portal) of met behulp van de [domain join sjabloon](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
- Ping de domeinnaam uit vanaf de opdrachtregel op virtuele machine.
- Bekijk de lijst met foutberichten voor domein lid worden in [Domain Join-foutberichten oplossen van problemen](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx).

### <a name="error-incorrect-credentials"></a>Fout: Onjuiste referenties

**Oorzaak:** Er is een typefout gemaakt wanneer de referenties zijn ingevoerd in de sjabloon interface corrigeert Azure Resource Manager.

**Fix:** Volg deze instructies voor het corrigeren van de referenties.

1. De virtuele machines handmatig toevoegt aan een domein.
2. Opnieuw implementeren nadat referenties zijn bevestigd. Zie [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).
3. Virtuele machines toevoegen aan een domein met een sjabloon met [lid wordt van een bestaande Windows-VM aan AD-domein](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).

### <a name="error-timeout-waiting-for-user-input"></a>Fout: Time-out tijdens het wachten op invoer van de gebruiker

**Oorzaak:** Het account dat wordt gebruikt voor het voltooien van het domein mogelijk multi-factor authentication (MFA).

**Fix:** Volg deze instructies voor het voltooien van het domein.

1. Tijdelijk MFA voor het account verwijderen.
2. Gebruik een service-account.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Fout: Het account dat wordt gebruikt tijdens het inrichten niet gemachtigd om de bewerking te voltooien

**Oorzaak:** Het gebruikte account beschikt niet over machtigingen voor virtuele machines toevoegen aan het domein vanwege nalevings- en regelgeving.

**Fix:** Volg deze instructies.

1. Gebruik een account dat lid is van de groep Administrators.
2. Verleen de vereiste machtigingen om het account dat wordt gebruikt.

### <a name="error-domain-name-doesnt-resolve"></a>Fout: De domeinnaam van het niet wordt opgelost

**1 oorzaak:** VM's zijn in een resourcegroep die niet is gekoppeld aan het virtuele netwerk (VNET), waar het domein zich bevindt.

**Fix 1:** Maak de VNET-peering tussen het VNET waarin virtuele machines zijn ingericht en de VNET waar de domeincontroller (DC) wordt uitgevoerd. Zie [maken die een virtueel-netwerkpeering - Resource Manager, verschillende abonnementen](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions).

**2 oorzaak:** Wanneer u AadService (AADS), zijn DNS-vermeldingen niet ingesteld.

**Fix 2:** Als u wilt instellen domeinservices, [inschakelen Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Windows-Agent voor virtuele bureaublad en virtuele bureaublad Windows-opstartlaadprogramma worden niet geïnstalleerd

De aanbevolen manier voor het inrichten van virtuele machines met behulp van Azure Resource Manager **maken en inrichten van virtuele Windows-bureaublad toepassingen hosten** sjabloon. De sjabloon installeert automatisch het Windows-Agent voor virtuele bureaublad en Windows virtuele bureaublad Agent Boot Loader.

Volg deze instructies om te bevestigen van dat de onderdelen zijn geïnstalleerd en om te controleren op foutberichten.

1. Controleer of de twee onderdelen worden geïnstalleerd door te controleren **Configuratiescherm** > **programma's** > **programma's en onderdelen**. Als **Windows virtuele bureaublad Agent** en **Windows virtuele bureaublad Agent Boot Loader** zijn niet zichtbaar, ze niet zijn geïnstalleerd op de virtuele machine.
2. Open **Verkenner** en navigeer naar **C:\Windows\Temp\scriptlogs.log**. Als het bestand ontbreekt, betekent dit dat de PowerShell DSC die de twee onderdelen geïnstalleerd niet kunnen worden uitgevoerd op de opgegeven beveiligingscontext is.
3. Als het bestand **C:\Windows\Temp\scriptlogs.log** is aanwezig, opent u het en controleren of er foutberichten.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptlogslog-is-also-missing"></a>Fout: Windows-Agent voor virtuele bureaublad en Windows virtuele bureaublad Agent Boot Loader ontbreken. Er is ook C:\Windows\Temp\scriptlogs.log ontbreekt

**1 oorzaak:** Referenties die u tijdens de invoer voor de Azure Resource Manager-sjabloon zijn onjuist of de machtigingen zijn onvoldoende.

**Fix 1:** De ontbrekende onderdelen handmatig toevoegen aan de virtuele machines met [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

**2 oorzaak:** PowerShell DSC is kunnen starten en uitvoeren, maar is mislukt omdat deze niet kan bij virtuele Windows-bureaublad aanmelden en benodigde informatie te verkrijgen.

**Fix 2:** Controleer of de items in de volgende lijst.

- Zorg ervoor dat het account beschikt niet over MFA.
- Bevestig dat de naam van de tenant juist is en de tenant in de virtuele Windows-bureaublad bestaat.
- Controleer of het account heeft ten minste bijdragerrechten voor extern bureaublad-services.

### <a name="error-authentication-failed-error-in-cwindowstempscriptlogslog"></a>Fout: Verificatie is mislukt, fout in C:\Windows\Temp\scriptlogs.log

**Oorzaak:** PowerShell DSC kon om uit te voeren, maar kan geen verbinding maken met virtuele Windows-bureaublad.

**Fix:** Controleer of de items in de volgende lijst.

- De virtuele machines handmatig registreren met de virtuele Windows-bureaublad-service.
- Controleer of de account die wordt gebruikt om verbinding te maken met virtuele Windows-bureaublad heeft machtigingen voor de tenant om host-pools te maken.
- Controleer of de account beschikt niet over MFA.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Windows virtuele bureaublad Agent is niet registreren bij de virtuele Windows-bureaublad-service

Toen de Windows-Agent voor virtuele bureaublad is geïnstalleerd op sessie VM's hosten (ofwel handmatig of via de Azure Resource Manager-sjabloon en PowerShell DSC), biedt een registratietoken. De volgende sectie bevat informatie over het oplossen van problemen van toepassing op de Windows-Agent voor virtuele bureaublad en het token.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Fout: De status wordt ingediend in de cmdlet Get-RdsSessionHost weergegeven status als niet beschikbaar

![De cmdlet Get-RdsSessionHost wordt de status weergegeven als niet beschikbaar.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Oorzaak:** De agent kan geen tot bijgewerkt naar een nieuwe versie.

**Fix:** Volg deze instructies voor de agent handmatig bijwerken.

1. Download een nieuwe versie van de agent op de sessiehost-VM van de.
2. Taakbeheer starten en stoppen op het tabblad Service de RDAgentBootLoader-service.
3. Voer het installatieprogramma voor de nieuwe versie van de Windows-Agent voor virtuele bureaublad.
4. Wanneer u hierom wordt gevraagd voor het registratietoken, verwijder de vermelding INVALID_TOKEN en druk op next (een nieuw token is niet vereist).
5. Voltooi de installatiewizard.
6. Opent u Taakbeheer en start de service RDAgentBootLoader.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Fout:  Windows-Agent voor virtuele bureaublad registervermelding IsRegistered ziet u een waarde van 0

**Oorzaak:** Registratietoken is verlopen of is gegenereerd met de vervaltijd van 999999.

**Fix:** Volg deze instructies de agent om registerfout te herstellen.

1. Als er al een registratietoken, kunt u deze met Remove-RDSRegistrationInfo verwijderen.
2. Nieuw token genereren met extern bureaublad-services NewRegistrationInfo.
3. Bevestig dat de parameter - ExpriationHours is ingesteld op 72 (max-waarde is 99999).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Fout: Virtueel bureaublad van Windows-agent is niet een heartbeat melden bij het uitvoeren van Get-RdsSessionHost

**1 oorzaak:** RDAgentBootLoader-service is gestopt.

**Fix 1:** Starten van Taakbeheer en, als het tabblad Service een gestopte status voor de RDAgentBootLoader service rapporteert, start u de service.

**2 oorzaak:** Poort 443 kan worden gesloten.

**Fix 2:** Volg deze instructies om poort 443 te openen.

1. Controleer of poort 443 is geopend door te downloaden van het hulpprogramma PSPing uit [Sysinternal extra](https://docs.microsoft.com/sysinternals/downloads/psping).
2. PSPing installeren op de host virtuele machine waarop de agent wordt uitgevoerd.
3. Open de opdrachtprompt als beheerder en geef de onderstaande opdracht uit:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. Controleer of u die informatie PSPing ontvangen van de RDBroker:

    ```
    PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2016 Mark Russinovich
    Sysinternals - www.sysinternals.com
    TCP connect to 13.77.160.237:443:
    5 iterations (warmup 1) ping test:
    Connecting to 13.77.160.237:443 (warmup): from 172.20.17.140:60649: 2.00ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60650: 3.83ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60652: 2.21ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60653: 2.14ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60654: 2.12ms
    TCP connect statistics for 13.77.160.237:443:
    Sent = 4, Received = 4, Lost = 0 (0% loss),
    Minimum = 2.12ms, Maximum = 3.83ms, Average = 2.58ms
    ```

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Oplossen van problemen met de virtuele Windows-bureaublad side-by-side-stack

De side-by-side-stack van virtuele Windows-bureaublad wordt automatisch geïnstalleerd met Windows Server 2019. Gebruik Microsoft Installer (MSI) de side-by-side-stack installeren op Microsoft Windows Server 2016 of Windows Server 2012 R2. Voor Microsoft Windows 10, de side-by-side-stack van virtuele Windows-bureaublad is ingeschakeld met **enablesxstackrs.ps1**.

Er zijn drie manieren de side-by-side-stack wordt geïnstalleerd of ingeschakeld op sessie host groep virtuele machines:

- Met de Azure Resource Manager **maken en inrichten nieuwe virtuele Windows-bureaublad host pool** sjabloon
- Door worden opgenomen en ingeschakeld op de master-image
- Geïnstalleerd of ingeschakeld handmatig op elke virtuele machine (of met extensies/PowerShell)

Als u problemen met de virtuele Windows-bureaublad side-by-side-stack ondervindt, typt u de **qwinsta** opdracht uit vanaf de opdrachtprompt om te bevestigen dat de side-by-side-stack is geïnstalleerd of ingeschakeld.

De uitvoer van **qwinsta** wordt een lijst **rdp-sxs** in de uitvoer als de side-by-side-stack is geïnstalleerd en ingeschakeld.

![Side-by-side-stack geïnstalleerd of ingeschakeld met qwinsta vermeld als rdp-sxs in de uitvoer.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

De hieronder vermelde registervermeldingen onderzoekt en Bevestig dat de waarden overeenkomen. Als er registersleutels ontbreken of waarden komen niet overeen, volg de instructies in [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) over het installeren van de side-by-side-stack.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-oreverseconnectstackfailure"></a>Fout: O_REVERSE_CONNECT_STACK_FAILURE

![O_REVERSE_CONNECT_STACK_FAILURE foutcode.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Oorzaak:** De side-by-side-stack is niet geïnstalleerd op de sessiehost-VM van de.

**Fix:** Volg deze instructies voor de side-by-side-stack installeren op de sessiehost-VM van de.

1. Remote Desktop Protocol (RDP) gebruiken om rechtstreeks in de sessiehost virtuele machine als lokale beheerder.
2. Downloaden en importeren [The Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.
3. Installeer de side-by-side-stack met [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Voor het oplossen van een virtuele Windows-bureaublad side-by-side-stack die niet meer goed werkt

Er zijn omstandigheden die kunnen leiden tot de side-by-side-stack niet goed bekend:

- Volgt niet de juiste volgorde van de stappen uit om in te schakelen de side-by-side-stack
- Auto update to Windows 10 Enhanced Versatile Disc (EVD)
- De rol van Remote Desktop Session Host (RDSH) ontbreekt
- Enablesxsstackrc.ps1 meerdere keren wordt uitgevoerd
- Enablesxsstackrc.ps1 uitgevoerd in een account dat beschikt niet over machtigingen van de lokale beheerder

Aan de hand van de instructies in deze sectie kunnen u de virtuele Windows-bureaublad side-by-side-stack verwijderen. Nadat u de side-by-side-stack verwijdert, gaat u naar "De virtuele machine met de virtuele Windows-bureaubladhost toepassingen registreren" in [een host-pool maken met PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) om de side-by-side-stack opnieuw te installeren.

De virtuele machine gebruikt voor het uitvoeren van herstel moet zich in hetzelfde subnet en domein als de virtuele machine met de niet-functionerende side-by-side-stack.

Volg deze instructies voor het uitvoeren van herstel uit het hetzelfde subnet en het domein:

1. Met standaard Remote Desktop Protocol (RDP) verbinden met de virtuele machine vanaf waar oplossing wordt toegepast.
2. Downloaden van PsExec van https://docs.microsoft.com/sysinternals/downloads/psexec.
3. Pak het gedownloade bestand uit.
4. Start de opdrachtprompt als lokale beheerder.
5. Navigeer naar de map waar PsExec uitgepakt is.
6. Gebruik de volgende opdracht uit vanaf de opdrachtprompt:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname is de naam van de machine van de virtuele machine met de niet-functionerende side-by-side-stack.

7. Accepteer de gebruiksrechtovereenkomst van PsExec door te klikken op Akkoord.

    ![Schermafbeelding van software-licentie-overeenkomst.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Dit dialoogvenster worden alleen de eerste keer die psexec wordt uitgevoerd weergegeven.

8. Nadat de opdrachtpromptsessie geopend op de virtuele machine met de niet-functionerende side-by-side-stack, qwinsta uitvoeren en controleer of er een vermelding die met de rdp-sxs naam beschikbaar is. Als dat niet het geval is, moet u een side-by-side-stack niet aanwezig op de virtuele machine, zodat het probleem is niet aan de side-by-side-stack gebonden.

    ![Opdrachtprompt als Administrator](media/AdministratorCommandPrompt.png)

9. Voer de volgende opdracht uit, een met Microsoft-componenten geïnstalleerd op de virtuele machine met de niet-functionerende side-by-side-stack lijst wordt.

    ```cmd
        wmic product get name
    ```

10. Voer de opdracht hieronder met productnamen uit de bovenstaande stap.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. Verwijderen van alle producten die beginnen met "Extern bureaublad."

12. Nadat alle virtuele bureaublad van Windows-onderdelen zijn verwijderd, volg de instructies voor uw besturingssysteem:

13. Als het besturingssysteem Windows Server, start u de virtuele machine waarop de niet-functionerende side-by-side-stack (ofwel met Azure portal of vanuit het hulpprogramma PsExec) opnieuw.

Als het besturingssysteem Microsoft Windows 10 is, gaat u verder met de onderstaande instructies:

14. Van de virtuele machine met PsExec, open File Explorer en disablesxsstackrc.ps1 kopiëren naar het systeemstation van de virtuele machine met de malfunctioned side-by-side-stack.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname is de naam van de machine van de virtuele machine met de niet-functionerende side-by-side-stack.

15. De aanbevolen procedure: van het hulpprogramma PsExec start PowerShell en navigeer naar de map in de vorige stap en disablesxsstackrc.ps1 uitvoeren. U kunt ook de volgende cmdlets uitvoeren:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Wanneer de cmdlets zijn klaar uitgevoerd, start opnieuw op de virtuele machine met de niet-functionerende side-by-side-stack.

## <a name="remote-licensing-model-is-not-configured"></a>Externe Licensing model is niet geconfigureerd

Als u zich bij meerdere Windows 10 Enterprise-sessie met een Administrator-account aanmelden, ontvangt u mogelijk een melding dat zegt: "extern bureaublad-licentiemodus niet is geconfigureerd, extern bureaublad-Services niet meer zal werken in X dagen. Op de server Connection Broker, Serverbeheer gebruiken om op te geven van de extern bureaublad-licentiemodus." Als u dit bericht ziet, betekent dit dat moet u het handmatig configureren van de modus voor **Per gebruiker**.

Het handmatig configureren van de modus:  

1. Ga naar uw **startmenu** zoekvak typt, en vervolgens zoeken naar en open **gpedit.msc** voor toegang tot de lokale Groepsbeleidsobjecteditor. 
2. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Extern bureaublad-Services** > **extern bureaublad-sessiehost** > **licentieverlening**. 
3. Selecteer **instellen van de extern bureaublad-licentiemodus** en wijzig deze in **Per gebruiker**.

We zoekt momenteel in de time-outproblemen melding en de respijtperiode is en u van plan bent om ze in een toekomstige update. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht over het oplossen van virtuele Windows-bureaublad en de sporen escalatie [overzicht, feedback en ondersteuning voor probleemoplossing](troubleshoot-set-up-overview.md).
- Zie voor het oplossen van problemen tijdens het maken van een tenant en host-pool in een omgeving met virtuele Windows-bureaublad, [Tenant en de host een pool maken](troubleshoot-set-up-issues.md).
- Zie voor het oplossen van problemen tijdens het configureren van een virtuele machine (VM) in virtuele Windows-bureaublad, [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md).
- Zie voor het oplossen van problemen met virtuele Windows-bureaublad-clientverbindingen, [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md).
- Zie voor het oplossen van problemen bij het gebruik van PowerShell met virtuele Windows-bureaublad, [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).