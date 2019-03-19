---
title: Multi-VM-omgevingen in Azure Lab Services inschakelen | Microsoft Docs
description: Informatie over het maken van een omgeving met meerdere virtuele machines in een sjabloon voor virtuele machine in een leslokaallab van Azure Lab-Services.
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
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 6faf32232c42f863bff52fdfb3c0714aee8e9b88
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190558"
---
# <a name="create-an-environment-with-multiple-vms-inside-a-template-vm-of-a-classroom-lab"></a>Een omgeving met meerdere VM's binnen een virtuele machine van een leslokaallab-sjabloon maken
Azure Lab-Services kunt op dit moment u een sjabloon voor virtuele machine in een testomgeving instellen en een enkele kopie beschikbaar maken voor elk van de gebruiker. Maar als u een docent die een IT-klasse voor het instellen van firewalls of servers bent, moet u mogelijk voor elk van uw studenten met een omgeving waarin meerdere virtuele machines met elkaar via een netwerk kunnen communiceren.

Geneste virtualisatie kunt u een omgeving met meerdere VM's in een lab in de sjabloon voor virtuele machine maken. Publiceren van de sjabloon om elke gebruiker in de testomgeving met een virtuele machine met meerdere virtuele machines binnen het instellen te bieden.

## <a name="what-is-nested-virtualization"></a>Wat is de geneste virtualisatie?
Geneste virtualisatie kunt u virtuele machines binnen een virtuele machine maken. Geneste virtualisatie via Hyper-V wordt uitgevoerd, en is alleen beschikbaar op Windows-VM's.

Zie voor meer informatie over geneste virtualisatie, de volgende artikelen:

- [Geneste virtualisatie in Azure](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Het inschakelen van geneste virtualisatie in een Azure-VM](../../virtual-machines/windows/nested-virtualization.md)

## <a name="use-nested-virtualization-in-azure-lab-services"></a>Gebruik van geneste virtualisatie in Azure Lab Services
De belangrijke stappen zijn:

1. Maak een **grote** grootte **Windows** machine van de sjabloon voor de testomgeving. 
2. Verbinding maken met het en [geneste virtualisatie inschakelen](../../virtual-machines/windows/nested-virtualization.md).


De volgende procedure kunt u de gedetailleerde stappen: 

1. Een lab-account maken als u er nog geen hebt. Zie voor instructies [zelfstudie: Instellen van een lab-account met Azure Lab Services](tutorial-setup-lab-account.md).
2. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
3. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
4. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal virtuele machines** in het lab op. U kunt het aantal virtuele machines vergroten of verkleinen na het maken van de testomgeving of in een bestaand lab. Zie voor meer informatie [Aantal virtuele machines in een testomgeving bijwerken](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer **grote** voor de grootte van virtuele machines (VM's) in het lab worden gemaakt. Op dit moment alleen de grote omvang biedt ondersteuning voor geneste virtualisatie.
    2. Kies een installatiekopie van virtuele machine die is een **Windows image**. Geneste virtualization is alleen beschikbaar op Windows-machines. 
    3. Selecteer **Volgende**.

        ![VM-specificaties opgeven](../media/how-to-enable-multi-vm-environment/large-windows-vm.png)    
5. Geef op de pagina **Referenties instellen** de standaardreferenties voor alle virtuele machines in het lab op. 
    1. Geef de **naam van de gebruiker** op voor alle virtuele machines in het lab.
    2. Geef het **wachtwoord** voor de gebruiker op. 

        > [!IMPORTANT]
        > Noteer de gebruikersnaam en het wachtwoord. Deze worden niet opnieuw weergegeven.
    3. Selecteer **Maken**. 

        ![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Op de pagina **Sjabloon configureren** wordt de status van het maken van het lab weergegeven. Het maken van de sjabloon in het lab duurt maximaal 20 minuten. 

    ![Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nadat de configuratie van de sjabloon is voltooid, wordt de volgende pagina weergegeven: 

    ![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Op de **configureren sjabloon** weergeeft, schakelt **Connect** verbinding maken met de sjabloon voor virtuele machine voor het configureren van geneste virtualisatie. U kunt ook later configureren nadat u de stappen in deze wizard hebt voltooid. 
9. In de sjabloon voor virtuele machine, geneste virtualisatie instellen en configureren van een virtueel netwerk met meerdere virtuele machines. Zie voor gedetailleerde stapsgewijze instructies, [het inschakelen van geneste virtualisatie in een Azure-VM](../../virtual-machines/windows/nested-virtualization.md). Hier volgt een snel overzicht van de stappen uit: 
    1. De Hyper-V-functie in de sjabloon voor virtuele machine inschakelen.
    2. Instellen van een intern virtueel netwerk met een internetverbinding voor de geneste virtuele machines
    3. Virtuele machines maakt via de Hyper-V-beheer
    4. Een IP-adres toewijzen aan de virtuele machines
10. Selecteer **Volgende** op de sjabloonpagina. 
11. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Als u de sjabloon onmiddellijk wilt publiceren, selecteert u **Publiceren**.  

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie het gedeelte [De sjabloon publiceren](how-to-create-manage-template.md#publish-the-template-vm) in het artikel [Leslokaallabs beheren](how-to-manage-classroom-labs.md) voor meer informatie over het configureren en publiceren nadat de wizard is voltooid.

        ![Sjabloon publiceren](../media/how-to-enable-multi-vm-environment/publish-template-page.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/how-to-enable-multi-vm-environment/dashboard.png)


## <a name="next-steps"></a>Volgende stappen

Elke gebruiker wordt nu één virtuele machine met een omgeving met meerdere VM's binnen deze opgehaald. Als u wilt leren hoe u gebruikers toevoegen aan het lab en registratiekoppeling naar hen verzenden, Zie het volgende artikel: [Gebruikers toevoegen aan het lab](tutorial-setup-classroom-lab.md#add-users-to-the-lab).