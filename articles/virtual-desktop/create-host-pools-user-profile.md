---
title: Instellen van een share van de gebruiker profiel voor een pool van de host Windows Virtual Desktop Preview - Azure
description: Over het instellen van een container FSLogix profiel voor een groep van de host Windows Virtual Desktop Preview.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: f6516e37107a16d80c4d9eb9514782bdbcc44184
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925218"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Een gebruikersprofielshare instellen voor een hostpool

De service Windows Virtual Desktop Preview biedt FSLogix profiel containers als de gebruiker aanbevolen profiel-oplossing. Wordt niet aanbevolen met behulp van de gebruiker profiel schijf (UDP)-oplossing die is afgeschaft in toekomstige versies van virtuele Windows-bureaublad.

In deze sectie wordt uitgelegd hoe u voor het instellen van een share FSLogix profiel container voor een groep host. Raadpleeg voor algemene documentatie met betrekking tot FSLogix de [FSLogix site](https://docs.fslogix.com/).

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

1. De Windows virtuele bureaublad Active Directory: gebruikers toevoegen aan een [Active Directory-beveiligingsgroep](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Deze beveiligingsgroep wordt gebruikt voor verificatie van de virtuele Windows-bureaublad-gebruikers aan de virtuele machine van een bestand delen dat u zojuist hebt gemaakt.
2. [Verbinding maken met de virtuele machine van file share](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. Maak een map op het bestandsshare virtuele machine op de **C-station** die wordt gebruikt als de profiel-share.
4. Met de rechtermuisknop op de nieuwe map, selecteert u **eigenschappen**, selecteer **delen**en selecteer vervolgens **Geavanceerd delen...** .
5. Selecteer **deel deze map**, selecteer **machtigingen...** en selecteer vervolgens **toevoegen...** .
6. Zoeken naar de beveiligingsgroep waaraan u de virtuele Windows-bureaublad-gebruikers toegevoegd, moet u ervoor zorgen dat de groep heeft **volledig beheer**.
7. Na het toevoegen van de beveiligingsgroep, met de rechtermuisknop op de map, selecteert u **eigenschappen**, selecteer **delen**, kopieert u de **netwerkpad** voor later gebruik.

Zie voor meer informatie over machtigingen voor de [FSLogix documentatie](https://docs.fslogix.com/display/20170529/Requirements%2B-%2BProfile%2BContainers).

## <a name="configure-the-fslogix-profile-container"></a>De container FSLogix-profiel configureren

Voor het configureren van de virtuele machines met de software FSLogix, doe het volgende op elke machine die is geregistreerd bij de host van toepassingen:

1. [Verbinding maken met de virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) met de referenties die u hebt opgegeven bij het maken van de virtuele machine.
2. Start een internetbrowser en navigeer naar [deze koppeling](https://go.microsoft.com/fwlink/?linkid=2084562) voor het downloaden van de agent FSLogix. Als onderdeel van de openbare preview van virtuele Windows-bureaublad krijgt u een licentiesleutel om de software FSLogix te activeren. De sleutel is het LicenseKey.txt-bestand dat is opgenomen in het FSLogix agent ZIP-bestand.
3. Navigeer naar een \\ \\Win32\\Release of \\ \\X64\\Release in het ZIP-bestand en voer **FSLogixAppsSetup** de FSLogix-agent te installeren.
4. Navigeer naar **Program Files** > **FSLogix** > **Apps** om te bevestigen van de agent is geïnstalleerd.
5. Voer vanuit het startmenu **RegEdit** als beheerder. Navigeer naar **Computer\\HKEY_LOCAL_MACHINE\\software\\FSLogix**.
6. Een sleutel met de naam **profielen**.
7. De volgende waarden voor de profielen voor toets maken:

| Name                | Type               | Gegevenswaarde /                        |
|---------------------|--------------------|-----------------------------------|
| Enabled             | DWORD              | 1                                 |
| VHDLocations        | Waarde met meerdere tekenreeksen | "Netwerkpad voor de bestandsshare"     |

>[!IMPORTANT]
>Als u wilt beveiligen uw virtuele Windows-bureaublad-omgeving in Azure, wordt aangeraden dat u binnenkomende poort 3389 op uw VM's niet openen. Virtuele Windows-bureaublad zijn vereist om een open binnenkomende poort 3389 voor gebruikers toegang krijgen tot virtuele machines van de host-pool. Als u poort 3389 voor het oplossen van problemen opent moet, raden wij aan u [just-in-time-VM-toegang](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).