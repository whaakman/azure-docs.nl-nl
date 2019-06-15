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
ms.openlocfilehash: 3701c69ff97d840f6cba175df8f02bc55ece498b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123237"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Instellen of opnieuw instellen van wachtwoord voor virtuele machines in leslokaallabs
In dit artikel biedt verschillende manieren om de instelling en resettings wachtwoorden voor toegang tot virtuele machines in leslokaallabs. 

## <a name="lab-owners-teachers"></a>Lab-eigenaars (docenten)
Een lab-eigenaar (docent) kan een wachtwoord instellen voor virtuele machines in de testomgeving op de **referenties instellen** pagina van de wizard lab maken.

![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)

Vervolgens het lab-eigenaar kan het wachtwoord opnieuw instellen (indien nodig) op de **configureren sjabloon** pagina van de wizard lab maken. 

![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

De eigenaar van het testlab kunt het wachtwoord ook opnieuw instellen nadat het lab is gemaakt, klikt u op het dashboard. 

![Menu van wachtwoord opnieuw instellen op de startpagina](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)

Voer vervolgens het nieuwe wachtwoord in de **instellen van wachtwoord** pagina en selecteer **instellen van wachtwoord**. 

![Menu van wachtwoord opnieuw instellen op de startpagina](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Labgebruikers (studenten)
Op het moment van het maken van het lab. Op hetzelfde moment, kan de eigenaar van het lab in- of uitschakelen zodat studenten om in te stellen hun eigen wachtwoorden voor de virtuele machines. Als de eigenaar van het lab (docent) kunt deze optie, wordt de lab-gebruiker (studenten) een optie voor het instellen van het wachtwoord voor de virtuele machine wanneer de student meldt zich aan bij de virtuele machine voor de eerste keer hebt. Op een **Windows VM**, drukt u op **CTRL + ALT + END**, en selecteer **wachtwoord wijzigen**. 

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over andere opties voor het gebruik van studenten u (als de eigenaar van een lab) kunt configureren, Zie het volgende artikel: [Gebruik voor studenten configureren](how-to-configure-student-usage.md).
