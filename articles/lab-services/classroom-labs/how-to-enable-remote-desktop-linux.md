---
title: Extern bureaublad inschakelen voor Linux in Azure Lab Services | Microsoft Docs
description: Leer hoe u extern bureaublad inschakelen voor virtuele Linux-machines in een lab in Azure Lab-Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 389d467bd9672743d4a086e8a1c505fb0366dba7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237143"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Inschakelen en extern bureaublad gebruiken voor virtuele Linux-machines in een lab in Azure Lab Services
In dit artikel laat zien hoe de volgende taken uitvoeren:

- Extern bureaublad inschakelen voor Linux-VM
- Hoe docent kan verbinden met de virtuele machine via het Remote Desktop Connection (RDP)-sjabloon.
- Hoe studenten verbinding maken met de student VM via RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Extern bureaublad inschakelen voor Linux-VM
Tijdens het maken van lab docenten kunnen inschakelen **verbinding met extern bureaublad** voor **Linux** afbeeldingen. De **verbinding met extern bureaublad inschakelen** optie wordt weergegeven als een Linux-installatiekopie voor de sjabloon is geselecteerd. Wanneer deze optie is ingeschakeld, wordt docenten kunnen verbinden met de sjabloon voor virtuele machine en studenten-VM's via RDP (Remote Desktop). 

![Verbinding met extern bureaublad voor een Linux-installatiekopie inschakelen](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

Op de **verbinding met extern bureaublad inschakelen** berichtvenster, selecteer **doorgaan met extern bureaublad**. 

![Verbinding met extern bureaublad voor een Linux-installatiekopie inschakelen](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Inschakelen van **verbinding met extern bureaublad** alleen wordt geopend de **RDP** poort op Linux-machines. U, als een docent verbinding maken met de Linux-machine via SSH voor de eerste keer en RDP en GUI-pakketten installeren zodat u kunt verbinding maken met de Linux-machine met behulp van RDP later opnieuw. Vervolgens kunt u **publiceren** de installatiekopie dat studenten RDP in op de student virtuele Linux-machines kunnen. 

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De verbinding met extern bureaublad wordt momenteel ondersteund voor de volgende besturingssystemen:

- openSUSE Leap 42,3
- Op basis van centOS 7.5
- Debian 9 'Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Docenten die verbinding maken met de VM-sjabloon met behulp van RDP
Docenten moeten verbinding maken met de sjabloon voor virtuele machine via SSH eerst en RDP en GUI-pakketten installeren op deze. De docenten kunnen vervolgens verbinding maken met de virtuele Linux-machines met behulp van RDP gebruik maken van de volgende stappen: 

U ziet de **extern bureaublad** optie verbinding maken met de VM-sjabloon op het moment van het maken van het lab. 

![Verbinding maken met sjabloon via RDP op het moment van maken](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

U ziet de **extern bureaublad** optie op de startpagina van de testomgeving nadat het lab is gemaakt en de sjabloon voor virtuele machine wordt gestart. Start de VM-sjabloon als deze niet al is gestart. 

![Verbinding maken met sjabloon via RDP nadat het lab is gemaakt](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Zie voor meer informatie over verbinding maken met de virtuele machine met SSH of RDP [verbinding maken met SSH of RDP]((#connect-using-ssh-or-rdp). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Docenten die verbinding maken met een student VM met behulp van RDP
Een docent verbinding kan maken met een student virtuele machine bij het overstappen naar de **virtuele Machines** weergeven en selecteren van de **verbinding** pictogram. Voordat u dat docenten moeten **publiceren** de sjablooninstallatiekopie met RDP en GUI-pakketten geïnstalleerd. 

![Verbinding maken met de virtuele machine van de cursist docenten](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Zie voor meer informatie over verbinding maken met de virtuele machine met SSH of RDP [verbinding maken met SSH of RDP]((#connect-using-ssh-or-rdp). 

## <a name="students-connecting-to-the-student-vm"></a>Verbinding maken met de virtuele machine van de cursist studenten
Studenten kunnen RDP in hun virtuele Linux-machines na het lab-eigenaar (docent) **publiceert** de sjabloon voor virtuele machine met RDP en GUI-pakketten geïnstalleerd op de computer. Dit zijn de stappen: 

1. Wanneer een student meldt zich aan bij de portal Labs rechtstreeks (`https://labs.azure.com`) of met behulp van een registratiekoppeling (`https://labs.azure.com/register/<registrationCode>`), een tegel voor elk lab de student toegang tot heeft wordt weergegeven. 
2. Selecteer op de tegel **Start** als de virtuele machine is gestopt. 
3. Selecteer **Verbinden**. Ziet u twee opties voor het verbinding maken met de virtuele machine: **SSH** en **extern bureaublad**.

    ![VM - opties voor studenten](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>Verbinding maken met SSH of RDP
Als u selecteert de **SSH** optie, ziet u het volgende **verbinding maken met uw virtuele machine** in het dialoogvenster:  

![SSH-verbindingsreeks](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Selecteer de **kopie** knop naast het tekstvak in op naar het Klembord kopiëren. Sla de SSH-verbindingsreeks. Deze verbindingsreeks vanuit een SSH-terminal gebruiken (zoals [Putty](https://www.putty.org/)) verbinding maken met de virtuele machine.

Als u selecteert de **RDP** optie, een RDP-bestand wordt gedownload naar uw machine. Opslaan en open het verbinding maken met de machine. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

