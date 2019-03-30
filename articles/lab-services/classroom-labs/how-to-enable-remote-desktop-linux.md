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
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 28f963ab807cbd679b7efe8b034ab91f2893198e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58659453"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Inschakelen en extern bureaublad gebruiken voor virtuele Linux-machines in een lab in Azure Lab Services
In dit artikel laat zien hoe de volgende taken uitvoeren:

- Extern bureaublad inschakelen voor Linux-VM
- Hoe docent kan verbinden met de virtuele machine via het Remote Desktop Connection (RDP)-sjabloon.
- Hoe studenten verbinding maken met de student VM via RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Extern bureaublad inschakelen voor Linux-VM
Tijdens het maken van lab docenten kunnen inschakelen **verbinding met extern bureaublad** voor **Linux** afbeeldingen. De **verbinding met extern bureaublad inschakelen** optie wordt weergegeven als een Linux-installatiekopie voor de sjabloon is geselecteerd. Wanneer deze optie is ingeschakeld, wordt docenten kunnen verbinden met de sjabloon voor virtuele machine en studenten-VM's via RDP (Remote Desktop). 

![Verbinding met extern bureaublad voor een Linux-installatiekopie inschakelen](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

> [!IMPORTANT] 
> Inschakelen van **verbinding met extern bureaublad** alleen wordt geopend de **RDP** poort op Linux-machines. U, als een docent verbinding maken met de Linux-machine via SSH voor de eerste keer en RDP en GUI-pakketten installeren zodat u kunt verbinding maken met de Linux-machine met behulp van RDP later opnieuw. Vervolgens kunt u **publiceren** de installatiekopie dat studenten RDP in op de student virtuele Linux-machines kunnen. 

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

Wanneer u selecteert de **RDP** optie, het downloadt een RDP-bestand. U opent het verbinding maken met de Linux-machine. 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Docenten die verbinding maken met een student VM met behulp van RDP
Lab-eigenaar (docent) kan verbinding maken met een student virtuele machine bij het overstappen naar de **virtuele Machines** weergeven en selecteren van de **verbinding** pictogram. Voordat u dat docenten moeten **publiceren** de sjablooninstallatiekopie met RDP en GUI-pakketten geïnstalleerd. 

![Verbinding maken met de virtuele machine van de cursist docenten](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

## <a name="students-connecting-to-the-student-vm"></a>Verbinding maken met de virtuele machine van de cursist studenten
Studenten kunnen RDP in hun virtuele Linux-machines na het lab-eigenaar (docent) **publiceert** de sjabloon voor virtuele machine met RDP en GUI-pakketten geïnstalleerd op de computer. Dit zijn de stappen: 

1. Wanneer een student meldt zich aan bij de portal Labs rechtstreeks (`http://labs.azure.com`) of met behulp van een registratiekoppeling (`http://labs.azure.com/register/<registrationCode>`), een tegel voor elk lab de student toegang tot heeft wordt weergegeven. 
2. Selecteer op de tegel **Start** als de virtuele machine is gestopt. 
3. Selecteer **Verbinden**. Deze actie wordt het RDP-bestand naar uw machine gedownload. Opslaan en openen om te verbinden met de Linux-machine via RDP. 

    ![Studenten VM - RDP downloaden](../media/how-to-enable-remote-desktop-linux/student-rdp-download.png)

    U kunt nog steeds verbinding maken met de Linux-VM met behulp van SSH. Selecteer **... (ellips)**  om te zien van de SSH-optie. 
    
    ![VM - studenten SSH](../media/how-to-enable-remote-desktop-linux/student-ssh.png)

    Kopiëren en opslaan van de SSH-verbindingsreeks op de **verbinding maken met uw virtuele machine** in het dialoogvenster. Deze verbindingsreeks vanuit een SSH-terminal gebruiken (zoals [Putty](https://www.putty.org/)) verbinding maken met de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)

