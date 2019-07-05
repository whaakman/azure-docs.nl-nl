---
title: Voorbereiden en aanpassen van een master VHD-installatiekopie - Azure
description: Het voorbereiden, aanpassen en een virtuele Windows-bureaublad preview master-installatiekopie uploaden naar Azure.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/03/2019
ms.author: helohr
ms.openlocfilehash: 186086a94470faaf19fa8a3c07939b9856ae2ec9
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466824"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Een VHD-hoofdinstallatiekopie voorbereiden en aanpassen

In dit artikel wordt uitgelegd hoe u de installatiekopie van een master virtuele harde schijf (VHD) voorbereiden voor het uploaden naar Azure, waaronder het maken van virtuele machines (VM's) en software installeren op deze. Deze instructies zijn voor een virtuele Windows-bureaublad Preview-specifieke configuratie die kan worden gebruikt met de bestaande processen van uw organisatie.

## <a name="create-a-vm"></a>Een virtuele machine maken

Meerdere Windows 10 Enterprise-sessie is beschikbaar in de galerie met installatiekopieën van Azure. Er zijn twee opties voor het aanpassen van deze installatiekopie.

De eerste optie is voor het inrichten van een virtuele machine (VM) in Azure door de instructies in [een virtuele machine maken vanaf een beheerde installatiekopie](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed), en gaat u verder met [Software voorbereidings- en installatie](set-up-customize-master-image.md#software-preparation-and-installation).

De tweede optie is voor het maken van de installatiekopie lokaal downloaden van de installatiekopie, inrichten van een Hyper-V-VM en aan te passen aan uw behoeften, die we in de volgende sectie behandelen.

### <a name="local-image-creation"></a>Het maken van lokale installatiekopieën

Nadat u de installatiekopie hebt gedownload naar een lokale locatie, opent u **Hyper-V-beheer** aan een virtuele machine maken met de VHD die u hebt gekopieerd. De volgende instructies zijn een eenvoudige versie, maar u vindt meer gedetailleerde instructies in [maken van een virtuele machine in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v).

Een virtuele machine maken met de gekopieerde VHD:

1. Open de **Wizard Nieuwe virtuele Machine**.

2. Selecteer op de pagina generatie opgeven **generatie 1**.

    ![Een schermafbeelding van de pagina generatie opgeven. De optie "Generation 1" is geselecteerd.](media/a41174fd41302a181e46385e1e701975.png)

3. Onder Type controlepunt uitschakelen controlepunten Hiervoor schakelt het selectievakje in.

    ![Een schermafbeelding van het Type controlepunt-gedeelte van de pagina controlepunten.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

U kunt ook de volgende cmdlet uitvoeren in PowerShell om uit te schakelen van controlepunten.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Vaste schijf

Als u een virtuele machine vanaf een bestaande VHD maken, maakt het een dynamische schijf standaard. Het kan worden gewijzigd naar een vaste schijf door het selecteren van **schijf bewerken...**  zoals wordt weergegeven in de volgende afbeelding. Zie voor meer instructies, [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

![Een schermafbeelding van de schijf bewerken wordt gebruikt.](media/35772414b5a0f81f06f54065561d1414.png)

U kunt ook de volgende PowerShell-cmdlet als u wilt wijzigen van de schijf naar een vaste schijf uitvoeren.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>Software voorbereidings- en installatie

In deze sectie bevat informatie over hoe u voorbereidt en FSLogix, Windows Defender en andere veelgebruikte toepassingen installeren. 

Als u Office 365 ProPlus en OneDrive op uw virtuele machine installeert, Zie [Office installeren op een master VHD-installatiekopie](install-office-on-wvd-master-image.md). Volg de koppeling in de volgende stappen van dit artikel gaat u terug naar dit artikel en het model VHD-proces te voltooien.

Als uw gebruikers nodig hebben voor toegang tot bepaalde LOB-toepassingen, raden wij dat u deze installeren na het voltooien van deze sectie instructies.

### <a name="disable-automatic-updates"></a>Automatische Updates uitschakelen

Automatische Updates via lokaal groepsbeleid uitschakelen:

1. Open **Editor voor lokaal groepsbeleid\\Beheersjablonen\\Windows-onderdelen\\Windows Update**.
2. Met de rechtermuisknop op **automatische updates configureren** en stel deze in op **uitgeschakelde**.

U kunt ook de volgende opdracht uitvoeren op de opdrachtregel om Automatische Updates uitschakelen.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>Indeling Start opgeven voor Windows 10-computers (optioneel)

Voer deze opdracht uit om op te geven van een indeling Start voor Windows 10-pc's.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="set-up-user-profile-container-fslogix"></a>Instellen van de gebruiker profiel container (FSLogix)

Als u wilt de container FSLogix opnemen als onderdeel van de installatiekopie, volg de instructies in [instellen van een share van de gebruiker profiel voor een groep host](create-host-pools-user-profile.md#configure-the-fslogix-profile-container). U kunt de functionaliteit van de container FSLogix met testen [in deze Quick Start](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start).

### <a name="configure-windows-defender"></a>Windows Defender configureren

Als Windows Defender is geconfigureerd in de virtuele machine, zorg er dan voor dat deze heeft geconfigureerd voor niet-scan de volledige inhoud van de VHD en VHDX-bestanden tijdens de bijlage.

Deze configuratie alleen verwijderd VHD en VHDX-bestanden worden gescand tijdens de bijlage, maar heeft geen invloed op realtime scannen.

Zie voor meer instructies voor het configureren van Windows Defender in Windows Server, [configureren van Windows Defender Antivirus uitsluitingen voor Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus).

Zie voor meer informatie over het configureren van bepaalde bestanden uitsluiten van scannen met Windows Defender, [configureren en valideren van uitsluitingen op basis van de extensie en mapnaam bestandslocatie](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus).

### <a name="configure-session-timeout-policies"></a>Configureren van beleid voor time-out van sessie

Externe sessie-beleid kunnen worden afgedwongen op het niveau van Groepsbeleid, omdat alle virtuele machines in een groep host deel van dezelfde beveiligingsgroep uitmaken.

Externe sessie-beleid configureren:

1. Navigeer naar **Beheersjablonen** > **Windows-onderdelen** > **extern bureaublad-Services**  >  **Extern bureaublad-sessiehost** > **sessie tijdslimieten**.
2. Selecteer in het deelvenster aan de rechterkant de **tijdslimiet voor actieve maar niet-actieve sessies van extern bureaublad-Services** beleid.
3. Nadat het modale venster wordt weergegeven, wijzigt u de beleidsoptie van **niet geconfigureerd** naar **ingeschakeld** aan het beleid te activeren.
4. In de vervolgkeuzelijst onder de optie voor de hoeveelheid tijd te instellen **4 uur**.

U kunt ook externe sessie-beleid handmatig configureren door het uitvoeren van de volgende opdrachten:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 10800000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 7200000 /f
```

### <a name="set-up-time-zone-redirection"></a>Instellen van de tijdzone-omleiding

Tijdzone-omleiding kan worden afgedwongen op het niveau van Groepsbeleid, omdat alle virtuele machines in een groep host deel van dezelfde beveiligingsgroep uitmaken.

Om te leiden tijdzones:

1. Open op de Active Directory-server, de **Group Policy Management Console**.
2. Vouw uw domein en groepsbeleidsobjecten.
3. Met de rechtermuisknop op de **Group Policy Object** dat u hebt gemaakt voor de instellingen voor Groepsbeleid en selecteer **bewerken**.
4. In de **Editor voor Groepsbeleidsbeheer**, gaat u naar **Computerconfiguratie** > **beleid** > **Administrative Sjablonen** > **Windows-onderdelen** > **extern bureaublad-Services** > **extern bureaublad-sessiehost**   >  **Apparaat en Resource-omleiding**.
5. Schakel de **tijdzoneomleiding toestaan** instelling.

U kunt ook deze opdracht uitvoeren op de master-image tijdzones omleiden:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Opslaginzicht uitschakelen

Voor virtuele Windows-bureaublad-sessiehost die gebruikmaken van Windows 10 Enterprise of Windows 10 Enterprise meerdere sessies, raden wij Opslaginzicht uitschakelen. U kunt Opslaginzicht uitschakelen in het menu instellingen onder **opslag**, zoals weergegeven in de volgende schermafbeelding:

![Een schermafbeelding van het Storage-menu onder instellingen. De optie 'Opslaginzicht' is uitgeschakeld.](media/storagesense.png)

U kunt ook de instelling met het register wijzigen door het uitvoeren van de volgende opdracht uit:

```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Ondersteuning voor extra talen opnemen

Dit artikel wordt niet beschreven hoe het configureren van taal en regionale ondersteuning. Raadpleeg voor meer informatie de volgende artikelen:

- [Talen toevoegen aan Windows-installatiekopieën](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows)
- [Onderdelen op verzoek](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)
- [Taal en regio onderdelen op verzoek (DOM)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod)

### <a name="other-applications-and-registry-configuration"></a>Andere toepassingen en de Register-configuratie

In deze sectie bevat informatie over toepassings- en configuratie van besturingssysteem. Alle configuratie in deze sectie vindt plaats via registervermeldingen die kunnen worden uitgevoerd door de opdrachtregel en regedit hulpprogramma's.

>[!NOTE]
>Aanbevolen procedures kunt u implementeren in de configuratie met groepsbeleidsobjecten (GPO's) of invoer van register. De beheerder kan een van beide opties op basis van de behoeften van hun organisatie kiezen.

Voor feedback hub verzameling van telemetriegegevens op meerdere Windows 10 Enterprise-sessie, kunt u deze opdracht uitvoeren:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v AllowTelemetry /t REG_DWORD /d 3 /f
```

Voer de volgende opdracht om op te lossen Watson crashes:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

Voer de volgende opdrachten in de Register-editor om op te lossen 5 k resolutie ondersteuning. U kunt de opdrachten moet uitvoeren voordat u de side-by-side-stack kunt inschakelen.

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxYResolution /t REG_DWORD /d 2880 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxYResolution /t REG_DWORD /d 2880 /f
```

## <a name="prepare-the-image-for-upload-to-azure"></a>De installatiekopie van het voorbereiden voor upload naar Azure

Nadat u hebt de configuratie voltooid en alle toepassingen geïnstalleerd, volgt u de instructies in [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) voorbereiden van de installatiekopie.

Na het voorbereiden van de afbeelding voor het uploaden, zorg ervoor dat de virtuele machine blijft in de status uitgeschakeld of toewijzing ongedaan gemaakt.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Master-image te uploaden naar een opslagaccount in Azure

In deze sectie is alleen van toepassing wanneer de master-image lokaal is gemaakt.

De volgende instructies wordt uitgelegd hoe u een master-image te uploaden naar een Azure storage-account. Als u nog een Azure storage-account hebt, volgt u de instructies in [in dit artikel](https://code.visualstudio.com/tutorials/static-website/create-storage) een te maken.

1. Converteren naar vaste de VM-installatiekopie (VHD) als u dat nog niet gedaan hebt. Als u niet de installatiekopie van het converteren naar vaste, kunt u de installatiekopie kan niet met succes maken.

2. Upload de VHD naar een blobcontainer in uw storage-account. U kunt snel uploaden met de [Opslagverkenner hulpprogramma](https://azure.microsoft.com/features/storage-explorer/). Zie voor meer informatie over het hulpprogramma Storage Explorer, [in dit artikel](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

    ![Een schermafbeelding van Microsoft Azure Storage Explorer van het hulpprogramma zoekvenster. Het selectievakje '.vhd- of vhdx-bestanden uploaden als pagina-blobs (aanbevolen)' is ingeschakeld.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. Ga vervolgens naar de Azure-portal in uw browser en zoek naar 'Installatiekopieën'. Uw zoekopdracht moet leiden u de **afbeelding maken** pagina, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Een schermafbeelding van de pagina van de installatiekopie maken van de Azure-portal, gevuld met voorbeelden van waarden voor de installatiekopie.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Als u de installatiekopie hebt gemaakt, ziet u een melding zoals in de volgende schermafbeelding:

    ![Een schermafbeelding van de melding 'installatiekopie is gemaakt'.](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een installatiekopie hebt, kunt u maken of bijwerken van de host van toepassingen. Zie de volgende artikelen voor meer informatie over het maken en bijwerken van de host van toepassingen:

- [Een host-pool maken met een Azure Resource Manager-sjabloon](create-host-pools-arm-template.md)
- [Zelfstudie: Een host-pool maken met Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Een host-pool maken met PowerShell](create-host-pools-powershell.md)
- [Instellen van een share van de gebruiker profiel voor een groep host](create-host-pools-user-profile.md)
- [Configureren van de taakverdelingsmethode voor een virtuele Windows-bureaublad](configure-host-pool-load-balancing.md)
