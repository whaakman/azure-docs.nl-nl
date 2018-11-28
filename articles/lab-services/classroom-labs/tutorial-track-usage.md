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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 49d5761e3e37e1265938d1f1b27324de667a13ca
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707092"
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
2. Controleer of u de status van virtuele machines kunt zien en het aantal uur dat de virtuele machines actief zijn geweest. De tijd die u besteedt op de VM van een student, wordt niet meegeteld in de gebruikstijd die wordt weergegeven in de laatste kolom. 

    ![VM-gebruik](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Virtuele machines van studenten beheren 
Als u met de muisaanwijzer een rij aanwijst in de lijst met virtuele machines, ziet u besturingselementen waarmee u de volgende taken kunt uitvoeren (zoals weergegeven in de afbeelding in de vorige sectie): 

- Verbinding maken met een VM
- Een VM starten
- Een VM stoppen
- Een VM verwijderen



## <a name="next-steps"></a>Volgende stappen
Zie de artikelen onder [Instructiegidsen](how-to-manage-lab-accounts.md) voor meer informatie over leslokaallabs.
