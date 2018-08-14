---
title: Gebruik van een lab bijhouden in Azure Lab Services | Microsoft Docs
description: In deze zelfstudie houdt u, als maker/eigenaar van een lab, het gebruik van uw lab bij.
services: lab-services
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
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 710d157dcf4c6d060e59bcfbb69455e2ddc91bdd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450127"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Zelfstudie: Gebruik van een lab bijhouden in Azure Lab Services
In deze zelfstudie leert u hoe een maker of eigenaar van een lab het gebruik van een lab kan bijhouden.

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Gebruikers weergeven die zijn geregistreerd bij uw lab
> * Het gebruik van virtuele machines in het lab weergeven
> * Virtuele machines van studenten beheren 


## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts.
3. Op de pagina **Mijn labs** selecteert u het lab waarvoor u het gebruik wilt bijhouden. 
4. Selecteer het tabblad **Gebruikers**. U ziet studenten die zijn geregistreerd bij uw lab. Selecteer **Registratiekoppeling**, kopieer de koppeling en stuur deze naar elke nieuwe student die nog niet is geregistreerd bij uw lab. 

    ![Geregistreerde gebruikers](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Het gebruik van virtuele machines in het lab weergeven 

1. Selecteer **Virtuele machines** in het menu aan de linkerkant. 
2. Controleer of u de status van virtuele machines kunt zien en het aantal uur dat de virtuele machines actief zijn geweest. De tijd die u besteedt op de virtuele machine van een student wordt niet meegeteld in de gebruikstijd die in de laatste kolom wordt weergegeven. 

    ![VM-gebruik](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Virtuele machines van studenten beheren 
Als u de muisaanwijzer boven een rij in de lijst van virtuele machine houdt, ziet u besturingselementen waarmee u de volgende taken kunt uitvoeren: 

- Verbinding maken met een VM
- Een VM starten
- Een VM stoppen
- Een VM verwijderen

![Besturingselementen van virtuele machines](../media/tutorial-track-usage/vm-controls.png) 



## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u gebruikers vindt die zijn geregistreerd bij het lab, hoe u het gebruik van virtuele machines in het lab bijhoudt en hoe u virtuele machines in het lab beheert.

Voor meer informatie over leslokaallabs raadpleegt u de onderwerpen onder [Handleidingen](how-to-manage-lab-accounts.md).
