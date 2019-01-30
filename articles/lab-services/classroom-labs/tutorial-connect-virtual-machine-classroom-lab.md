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
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 1328835714086dcec71b0e9dd4d1916794f557a6
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390171"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Zelfstudie: Toegang tot een leslokaallab in Azure Lab Services
In deze zelfstudie maakt u als student verbinding met een virtuele machine (VM) in een leslokaallab. 

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Registratiekoppeling gebruiken 
> * Verbinding maken met de virtuele machine

## <a name="use-the-registration-link"></a>De registratiekoppeling gebruiken

1. Navigeer naar de **registratie-URL** die u hebt ontvangen van de docent. 
2. Meld u aan bij de service met uw schoolaccount om de registratie te voltooien. 
3. Controleer nadat u zich hebt geregistreerd of u de virtuele machines ziet voor het lab waartoe u toegang hebt. 
2. Wacht tot de virtuele machine gereed is en **start** dan de virtuele machine. Dit proces duurt enige tijd.  

    ![De virtuele machine starten](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Selecteer **Verbinding maken** op de tegel van de virtuele machine van het lab dat u wilt openen. 

    ![Verbinding maken met de virtuele machine](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Sla het RDP-bestand op de harde schijf op en open het (ervan uitgaande dat het een Windows-VM is)
3. Gebruik de **gebruikersnaam** en het **wachtwoord** dat u van uw docent hebt gekregen om zich aan te melden bij de computer. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab geopend met behulp van de registratiekoppeling die u van uw docent krijgt.

Als eigenaar van een lab wilt u zien wie zich heeft geregistreerd bij uw lab en wilt u het gebruik van virtuele machines bijhouden. Ga verder met de volgende zelfstudie voor meer informatie over het bijhouden van het gebruik van het lab:

> [!div class="nextstepaction"]
> [Gebruik van een lab bijhouden](tutorial-track-usage.md) 