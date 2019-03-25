---
title: Instellen van een share van de gebruiker profiel voor een pool van de host Windows Virtual Desktop Preview - Azure
description: Over het instellen van een container FSLogix profiel voor een groep van de host Windows Virtual Desktop Preview.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: c9c2ca2cc27c5fa757b8ff6846e0a6a8f7087875
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403711"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Een gebruikersprofielshare instellen voor een hostpool

De service Windows Virtual Desktop Preview biedt FSLogix profiel containers als de gebruiker aanbevolen profiel-oplossing. We raden het gebruik van de gebruiker profiel schijf (UDP)-oplossing en deze in toekomstige versies van virtuele Windows-bureaublad wordt afgeschaft.

In deze sectie wordt uitgelegd hoe u voor het instellen van een share FSLogix profiel container voor een groep host.

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Een nieuwe virtuele machine die als een bestandsshare fungeren zal maken

Bij het maken van de virtuele machine, moet u deze plaatsen op de hetzelfde virtuele netwerk als de host groep virtuele machines of op een virtueel netwerk dat verbonden met het hosten van toepassingen van virtuele machines is. U kunt een virtuele machine maken op verschillende manieren:

- [Een virtuele machine maken van de installatiekopie van een Azure-galerie](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Een virtuele machine van een beheerde installatiekopie maken](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Een virtuele machine uit een niet-beheerde installatiekopie maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Na het maken van de virtuele machine, voeg deze toe aan het domein door het volgende doen:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Op de virtuele machine, start u **Configuratiescherm** en selecteer **System**.
3. Selecteer **computernaam**, selecteer **instellingen wijzigen**, en selecteer vervolgens **wijzigen...**
4. Selecteer **domein** en voer de Active Directory-domein op het virtuele netwerk.
5. Verifiëren met een domeinaccount met bevoegdheden voor domeindeelname computers.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>De virtuele machine om te fungeren als een bestandsshare voor gebruikersprofielen voorbereiden

Hier volgen de algemene instructies over het voorbereiden van een virtuele machine om te fungeren als een bestandsshare voor gebruikersprofielen:

1. Deelnemen aan de sessie host virtuele machines naar een [Active Directory-beveiligingsgroep](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Deze beveiligingsgroep wordt gebruikt voor verificatie van de sessie hosts virtuele machines op de virtuele machine van een bestand delen dat u zojuist hebt gemaakt.
2. [Verbinding maken met de virtuele machine van file share](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. Maak een map op het bestandsshare virtuele machine op de **C-station** die wordt gebruikt als de profiel-share.
4. Met de rechtermuisknop op de nieuwe map, selecteert u **eigenschappen**, selecteer **delen**en selecteer vervolgens **Geavanceerd delen...** .
5. Selecteer **deel deze map**, selecteer **machtigingen...** en selecteer vervolgens **toevoegen...** .
6. Zoeken naar de beveiligingsgroep waaraan u de sessie hosten van virtuele machines toegevoegd, moet u ervoor zorgen dat de groep heeft **volledig beheer**.
7. Na het toevoegen van de beveiligingsgroep, met de rechtermuisknop op de map, selecteert u **eigenschappen**, selecteer **delen**, kopieert u de **netwerkpad** voor later gebruik.

Zie de volgende aanbevolen procedures van machtigingen voor [FSLogix documentatie](https://support.fslogix.com/index.php/forum-main/faqs/84-best-practices#120).

## <a name="configure-the-fslogix-profile-container"></a>De container FSLogix-profiel configureren

Voor het configureren van de virtuele machines met de software FSLogix, doe het volgende op elke machine die is geregistreerd bij de host van toepassingen:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Start een internetbrowser en navigeer naar de volgende [koppeling](https://go.microsoft.com/fwlink/?linkid=2084562) voor het downloaden van de agent FSLogix. Als onderdeel van de openbare preview van virtuele Windows-bureaublad krijgt u een licentiesleutel om de software FSLogix te activeren. De sleutel is het LicenseKey.txt-bestand dat is opgenomen in het FSLogix agent ZIP-bestand.
3. Installeer de agent FSLogix.
4. Navigeer naar **Program Files** > **FSLogix** > **Apps** om te bevestigen van de agent is geïnstalleerd.
5. Voer vanuit het startmenu **RegEdit** als beheerder. Navigeer naar **Computer\\HKEY_LOCAL_MACHINE\\software\\FSLogix\\profielen**
6. Maak de volgende waarden:

| Name                | Type               | Gegevenswaarde /                        |
|---------------------|--------------------|-----------------------------------|
| Ingeschakeld             | DWORD              | 1                                 |
| VHDLocations        | Waarde met meerdere tekenreeksen | "Netwerkpad voor de bestandsshare" |
| VolumeType          | String             | VHDX                              |
| SizeInMBs           | DWORD              | 'geheel getal voor de grootte van het profiel'     |
| IsDynamic           | DWORD              | 1                                 |
| LockedRetryCount    | DWORD              | 1                                 |
| LockedRetryInterval | DWORD              | 0                                 |
