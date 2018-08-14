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
ms.date: 07/30/2018
ms.author: spelluru
ms.openlocfilehash: dadc90e6a39b9e9689bab0249e6496fdea6f6205
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450212"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Zelfstudie: toegang tot een leslokaallab in Azure Lab Services
In deze zelfstudie maakt u als student verbinding met een virtuele machine (VM) in een leslokaallab. 

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Registratiekoppeling gebruiken 
> * Verbinding maken met de virtuele machine

## <a name="use-the-registration-link"></a>De registratiekoppeling gebruiken

1. Navigeer naar de **registratie-URL** die u hebt ontvangen van de docent. 
2. Meld u aan bij de service met uw schoolaccount om de registratie te voltooien. 
3. Controleer nadat u zich hebt geregistreerd of u de virtuele machines ziet voor het lab waartoe u toegang hebt. 
2. Wacht tot de virtuele machine gereed is en **start** dan de virtuele machine.

    ![De virtuele machine starten](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Selecteer **Verbinding maken** op de tegel van de virtuele machine van het lab dat u wilt openen. 

    ![Verbinding maken met de virtuele machine](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Sla het RDP-bestand op de harde schijf op en open het. 
3. Gebruik de **gebruikersnaam** en het **wachtwoord** dat u van uw docent hebt gekregen om zich aan te melden bij de computer. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab geopend met behulp van de registratiekoppeling die u van uw docent krijgt.

Als eigenaar van een lab wilt u zien wie er is geregistreerd bij uw lab en wilt u het gebruik van virtuele machines bijhouden. Ga naar de volgende zelfstudie als u wilt weten hoe u dat doet:

> [!div class="nextstepaction"]
> [Gebruik van een lab bijhouden](tutorial-track-usage.md) 