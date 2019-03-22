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
ms.openlocfilehash: 3ec3abffc7962051f4cfc02d5369581ca193d70e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57775574"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Zelfstudie: Toegang tot een leslokaallab in Azure Lab Services
In deze zelfstudie maakt u als student verbinding met een virtuele machine (VM) in een leslokaallab. 

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Registratiekoppeling gebruiken 
> * Verbinding maken met de virtuele machine

## <a name="use-the-registration-link"></a>De registratiekoppeling gebruiken

1. Navigeer naar de **registratie-URL** die u hebt ontvangen van de docent. U hoeft de registratie-URL niet meer te gebruiken nadat u de registratie hebt voltooid. In plaats daarvan gebruikt u deze URL: [https://labs.azure.com](https://labs.azure.com). 
1. Meld u aan bij de service met uw schoolaccount om de registratie te voltooien. 
2. Controleer nadat u zich hebt geregistreerd of u de virtuele machines ziet voor het lab waartoe u toegang hebt. 
3. Wacht tot de virtuele machine gereed is en **start** dan de virtuele machine. Dit proces duurt enige tijd.  

    ![De virtuele machine starten](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Selecteer **Verbinding maken** op de tegel van de virtuele machine van het lab dat u wilt openen. 

    ![Verbinding maken met de virtuele machine](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Voer een van de volgende stappen uit: 
    1. Voor **Windows** opslaan voor virtuele machines, de **RDP** bestand naar de harde schijf. Open het RDP-bestand verbinding maken met de virtuele machine. Gebruik de **gebruikersnaam** en **wachtwoord** u ophalen uit uw docenten/professor aan te melden bij de computer. 
    3. Voor **Linux** virtuele machines, kopiëren en opslaan van de SSH-verbindingsreeks op de **verbinding maken met uw virtuele machine** in het dialoogvenster. Deze verbindingsreeks vanuit een SSH-terminal gebruiken (zoals [Putty](https://www.putty.org/)) verbinding maken met de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab geopend met behulp van de registratiekoppeling die u van uw docent krijgt.

Als eigenaar van een lab wilt u zien wie zich heeft geregistreerd bij uw lab en wilt u het gebruik van virtuele machines bijhouden. Ga verder met de volgende zelfstudie voor meer informatie over het bijhouden van het gebruik van het lab:

> [!div class="nextstepaction"]
> [Gebruik van een lab bijhouden](tutorial-track-usage.md) 