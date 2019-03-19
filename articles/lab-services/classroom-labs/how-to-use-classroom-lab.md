---
title: Toegang tot een leslokaallab in Azure Lab Services | Microsoft Docs
description: In deze zelfstudie hebt u toegang tot virtuele machines in een leslokaallab dat is ingesteld door een docent.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 02/07/2019
ms.author: spelluru
ms.openlocfilehash: 7c4a73c86245067a07dc253041c43ae6c17f7172
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096976"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Toegang tot een leslokaallab in Azure Lab Services
In dit artikel wordt beschreven hoe u toegang krijgt tot een leslokaallab, hoe u verbinding maakt met de virtuele machine in het lab en hoe u de virtuele machine kunt stoppen. 

## <a name="register-to-a-lab"></a>Registreren voor een lab
1. Navigeer naar de **registratie-URL** die u hebt ontvangen van de docent. 
2. Meld u aan bij de service met uw schoolaccount om de registratie te voltooien. 
3. Controleer nadat u zich hebt geregistreerd of u de virtuele machines ziet voor het lab waartoe u toegang hebt. 
2. Wacht tot de virtuele machine gereed is en **start** dan de virtuele machine. Dit proces duurt enige tijd.  

    ![De virtuele machine starten](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)


## <a name="view-all-the-classroom-labs"></a>Alle leslokaallabs weergeven
Als u zich voor de labs registreert, kunt u alle leslokaallabs weergeven door de volgende stappen uit te voeren: 

1. Navigeer naar [https://labs.azure.com](https://labs.azure.com). 
2. Meld u aan bij de service met het gebruikersaccount waarmee u zich hebt geregistreerd voor het lab. 
3. Controleer of u alle labs ziet waarvoor u toegangsrechten hebt. 

    ![Alle labs weergeven](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Verbinding maken met de virtuele machine in een leslokaallab

1. Selecteer **Start** op de tegel om de virtuele machine te starten als deze nog niet is gestart. 
2. Selecteer **Verbinding maken** op de tegel van de virtuele machine van het lab dat u wilt openen. 
3. Voer een van de volgende stappen uit: 
   1. Voor **Windows** opslaan voor virtuele machines, de **RDP** bestand naar de harde schijf. Open het RDP-bestand verbinding maken met de virtuele machine. Gebruik de **gebruikersnaam** en **wachtwoord** u ophalen uit uw docenten/professor aan te melden bij de computer. 
   3. Voor **Linux** virtuele machines, kopiëren en opslaan van de SSH-verbindingsreeks op de **verbinding maken met uw virtuele machine** in het dialoogvenster. Deze verbindingsreeks vanuit een SSH-terminal gebruiken (zoals [Putty](https://www.putty.org/)) verbinding maken met de virtuele machine.
      ## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>De virtuele machine in een leslokaallab stoppen

Selecteer **Stoppen** op de tegel om de virtuele machine te stoppen. Zodra de virtuele machine is gestopt, wordt de knop **Start** op de tegel geactiveerd. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
 