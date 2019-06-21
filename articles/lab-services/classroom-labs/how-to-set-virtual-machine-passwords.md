---
title: Instellen van wachtwoorden voor virtuele machines in Azure Lab-Services | Microsoft Docs
description: Informatie over het instellen en wachtwoorden opnieuw instellen voor virtuele machines (VM's) in leslokaallabs van Azure Lab-Services.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: c1564fadef35a20d0d87db8439ae1cc3dc923318
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144110"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Instellen of opnieuw instellen van wachtwoord voor virtuele machines in leslokaallabs
In dit artikel biedt verschillende manieren om de instelling en resettings wachtwoorden voor toegang tot virtuele machines in leslokaallabs. 

## <a name="lab-owners-teachers"></a>Lab-eigenaars (docenten)
Een lab-eigenaar (docent) kunt instellen/opnieuw instellen van het wachtwoord voor virtuele machines op het moment van het maken van de testomgeving (wizard lab maken) of na het maken van de testomgeving (op het dashboard). 

### <a name="set-password-at-the-time-of-lab-creation"></a>Instellen van het wachtwoord op het moment van lab maken
Een lab-eigenaar (docent) kan een wachtwoord instellen voor virtuele machines in de testomgeving op de **referenties instellen** pagina van de wizard lab maken.

![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)

Door het in-/ uitschakelen de **hetzelfde wachtwoord gebruiken voor alle virtuele machines** optie op deze pagina een docent hetzelfde wachtwoord gebruiken voor alle virtuele machines in een testomgeving of toestaan dat studenten instellen van wachtwoorden voor hun VM's kunt kiezen. Deze instelling is standaard ingeschakeld voor alle Windows en Linux-besturingssysteem-installatiekopieën, met uitzondering van Ubuntu. Wanneer deze instelling is uitgeschakeld, wordt studenten gevraagd een wachtwoord instellen wanneer ze verbinding maken met de virtuele machine voor de eerste keer proberen. 

De eigenaar van het lab kan dit wachtwoord opnieuw instellen (indien nodig) op de **configureren sjabloon** pagina van de wizard lab maken. 

![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

De eigenaar van het testlab kunt het wachtwoord ook opnieuw instellen nadat het lab is gemaakt, klikt u op het dashboard. 

### <a name="reset-password-on-the-dashboard"></a>Wachtwoord opnieuw instellen op het dashboard

1. Selecteer het overloopmenu (verticale drie punten) op de tegel lab, en selecteer **wachtwoord opnieuw instellen**. 

    ![Menu van wachtwoord opnieuw instellen op de startpagina](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. Op de **instellen van wachtwoord** in het dialoogvenster, voer een wachtwoord in en selecteer **instellen van wachtwoord**.
    
    ![Het dialoogvenster wachtwoord instellen](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Labgebruikers (studenten)
Op het moment van het maken van de testomgeving, de eigenaar van het testlab kunt inschakelen of uitschakelen de **hetzelfde wachtwoord gebruiken voor alle virtuele machines**. Als deze optie is ingeschakeld, studenten wachtwoord niet opnieuw instellen. Alle virtuele machines in de labs wordt hetzelfde wachtwoord dat ingesteld door de docent hebben. 

Als deze optie is uitgeschakeld, moeten gebruikers een wachtwoord instellen wanneer de virtuele machine voor de eerste keer verbinding probeert te maken. Wanneer de gebruikers (studenten) selecteert de **Connect** knop op de tegel lab op de **mijn virtuele machines** pagina, de gebruiker ziet het volgende dialoogvenster het wachtwoord worden ingesteld voor de virtuele machine: 

![Wachtwoord opnieuw instellen voor de student](../media/how-to-set-virtual-machine-passwords/student-set-password.png)

Studenten ook kunt het wachtwoord instellen door te klikken op het overloopmenu (**verticale drie punten**) op de tegel lab en selecteren **wachtwoord opnieuw instellen**. 

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over andere opties voor het gebruik van studenten u (als de eigenaar van een lab) kunt configureren, Zie het volgende artikel: [Gebruik voor studenten configureren](how-to-configure-student-usage.md).
