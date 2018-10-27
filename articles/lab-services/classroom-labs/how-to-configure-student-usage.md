---
title: Configureren van instellingen voor het gebruik in leslokaallabs van Azure Lab-Services | Microsoft Docs
description: Informatie over het configureren van het aantal gebruikers voor de testomgeving, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt.
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
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: a91d5826f52b2beb05f2d9a08f3646bd776b294e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142376"
---
# <a name="configure-usage-settings-and-policies"></a>Instellingen voor het gebruik en het beleid configureren
In dit artikel wordt beschreven hoe u het aantal gebruikers voor de testomgeving configureren, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Geef het aantal gebruikers dat is toegestaan in de testomgeving

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-users"></a>Registratiekoppeling naar gebruikers verzenden

1. Schakel over naar de weergave **Dashboard**. 
2. Selecteer de tegel **Gebruikersregistratie**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. Studenten gebruiken de koppeling om te navigeren naar de pagina waarmee ze met het registreren van het Lab. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 

## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

Selecteer **gebruikers** geregistreerd in het menu links om te zien van de lijst met gebruikers met de testomgeving. 

![Lijst met gebruikers die zijn geregistreerd bij de testomgeving](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Quota instellen per gebruiker

1. Selecteer **gebruikersinstellingen** in het menu links.
2. Selecteer **Quta per gebruiker** tegel. 
3. Selecteer **beperken het aantal uren dat een gebruiker kan een virtuele machine gebruiken**. 
4. Voor **hoeveel uur wilt u geven aan elke gebruiker?**, voer het aantal uren en selecteert u **opslaan**. 

    ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ziet u het aantal uren op de **quotum per gebruiker** tegel nu. 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="manage-user-vms"></a>Gebruiker virtuele machines beheren
Wanneer studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines toegewezen aan studenten op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM kunt doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- De virtuele machine verwijderen. 
- Bekijk het aantal uren dat gebruikers de virtuele machine gebruikt. 


## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)
