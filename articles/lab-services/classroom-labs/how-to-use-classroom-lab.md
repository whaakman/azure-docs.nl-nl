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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 85da6989740446c980bcb9f2c89a6c31afe488fb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651325"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Toegang tot een leslokaallab in Azure Lab Services
In dit artikel wordt beschreven hoe u toegang krijgt tot een leslokaallab, hoe u verbinding maakt met de virtuele machine in het lab en hoe u de virtuele machine kunt stoppen. 

## <a name="view-all-the-classroom-labs"></a>Alle leslokaallabs weergeven

1. Navigeer naar de **registratie-URL** die u hebt ontvangen van de docent. 
2. Meld u aan bij de service met uw schoolaccount om de registratie te voltooien. 
3. Controleer nadat u zich hebt geregistreerd of u de virtuele machines ziet voor de labs waartoe u toegang hebt. 

    ![Alle labs weergeven](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Verbinding maken met de virtuele machine in een leslokaallab

1. Selecteer **Verbinding maken** op de tegel van de virtuele machine van het lab dat u wilt openen.

    ![Alle labs weergeven](../media/how-to-use-classroom-lab/connect-button.png)
2. Het duurt een paar minuten voordat de virtuele machine klaar is.

    ![De virtuele machine gereedmaken](../media/how-to-use-classroom-lab/getting-virtual-machine-ready.png)
3. Sla het RDP-bestand op de harde schijf op en open het. 
    
    ![Sla het RDP-bestand op.](../media/how-to-use-classroom-lab/save-rdp-file.png)
4. Gebruik de **gebruikersnaam** en het **wachtwoord** dat u van uw docent hebt gekregen om zich aan te melden bij de computer. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>De virtuele machine in een leslokaallab stoppen

Selecteer **Stoppen** op de tegel van het leslokaallab. Zodra de virtuele machine is gestopt, wordt de knop **Start** op de tegel geactiveerd. 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)

 