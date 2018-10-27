---
title: Beheren van een sjabloon van een leslokaallab in Azure Lab Services | Microsoft Docs
description: Informatie over het maken en beheren van een sjabloon classroom lab in Azure Lab-Services.
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
ms.date: 10/25/2018
ms.author: spelluru
ms.openlocfilehash: 3ecbef3b3063ceb413b852f8000b44a85d28d08e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142387"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Maken en beheren van een sjabloon leslokaal in Azure Lab Services
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Vervolgens publiceert u de sjabloon voor exemplaren van de VM-sjabloon beschikbaar maken voor uw labgebruikers. Wanneer u een sjabloon publiceert, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon.

In dit artikel wordt beschreven hoe u maken en beheren van een sjabloon voor virtuele machine in een leslokaallab van Azure Lab-Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Een sjabloon tijdens het maken van een leslokaallab publiceren
U kunt eerst instellen en publiceren van een sjabloon tijdens het maken van een leslokaallab.

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal gebruikers** op dat in het lab is toegestaan. 
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) die in het lab worden gemaakt. 
    2. Selecteer de **regio** waarin de virtuele machines moeten worden gemaakt. 
    3. Selecteer de **VM-installatiekopie** die moet worden gebruikt voor het maken van virtuele machines in het lab. 
    4. Selecteer **Volgende**.

        ![VM-specificaties opgeven](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
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
8. In deze zelfstudie zijn de volgende stappen optioneel: 
    1. De sjabloon-VM starten door **Starten** te selecteren.
    2. De sjabloon-VM verbinden door **Verbinding maken** te selecteren. 
    3. Software installeren en configureren op uw sjabloon-VM. 
    4. **Stop** de virtuele machine.  
    5. Een **Beschrijving** voor de sjabloon invoeren

        ![Volgende op de pagina Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecteer **Volgende** op de sjabloonpagina. 
10. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Als u de sjabloon onmiddellijk wilt publiceren, schakelt u het selectievakje in voor *Ik begrijp dat ik de sjabloon niet kan wijzigen nadat deze is gepubliceerd. Dit proces kan slechts één keer worden uitgevoerd en kan tot een uur duren*. Selecteer vervolgens **publiceren**.  

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, [publiceren van de sjabloon](#publish-the-template) sectie de [leslokaallabs beheren ](how-to-manage-classroom-labs.md) artikel.

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="set-up-a-template-after-creating-a-lab"></a>Een sjabloon na het maken van een lab instellen 
U kunt ook een sjabloon instellen nadat u het lab hebt gemaakt.   

### <a name="set-template-title-and-description"></a>Set-Sjabloontitel en beschrijving
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>De sjabloon-VM instellen
 Maak verbinding met de sjabloon-VM en installeer de vereiste software erop voordat u deze beschikbaar maakt voor uw studenten. 

1. Wacht totdat de sjabloon-VM gereed is. Zodra de VM gereed is, wordt de knop **Starten** beschikbaar. Selecteer **Starten** om de VM te starten.

    ![De sjabloon-VM starten](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Selecteer **Verbinding maken** en volg de aanwijzingen om de verbinding met de VM tot stand te brengen. 

    ![Verbinding maken met de sjabloon-VM](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Installeer alle software die de studenten nodig hebben om het lab te doorlopen (bijvoorbeeld Visual Studio, Azure Storage Explorer, enzovoort). 
2. Verbreek de verbinding met de sjabloon-VM (sluit de Extern bureaublad-sessie). 
3. **Stop** de sjabloon-VM door **Stoppen** te selecteren. 

    ![De sjabloon-VM stoppen](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>De sjabloon publiceren 
Als u niet de sjabloon publiceert tijdens het maken van de testomgeving, kunt u deze later publiceren. Voordat u publiceert, kunt u verbinding maakt met de VM-sjabloon en bijwerken met de software. Wanneer u een sjabloon publiceert, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon. 

1. Selecteer **Publiceren** in de sectie **Sjabloon**. 

    ![De sjabloon-VM publiceren](../media/tutorial-setup-classroom-lab/public-access.png)
1. Op de **publiceren van de sjabloon** dialoogvenster controleert u het bericht en selecteer **publiceren**. Dit proces kan enige tijd duren, afhankelijk van het aantal virtuele machines dat wordt gemaakt. Het aantal VM’s dat wordt gemaakt, is gelijk aan het aantal gebruikers dat is toegestaan in het lab.
    
    > [!IMPORTANT]
    > Zodra een sjabloon is gepubliceerd, kan dit niet meer ongedaan worden gemaakt. U kunt de sjabloon echter opnieuw publiceren. 
4. Ga naar de pagina **Virtuele machines** en controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. 

    ![Virtuele machines](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Wacht totdat de virtuele machines zijn gemaakt. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="republish-the-template"></a>De sjabloon publiceren 
Nadat u een sjabloon hebt gepubliceerd, kunt u wel verbinding maken met de VM-sjabloon, bijwerken en vervolgens opnieuw te publiceren. Wanneer u een VM-sjabloon opnieuw publiceert, alle virtuele machines verbinding verbroken gebruiker en ze worden opnieuw gemaakt op basis van de bijgewerkte sjabloon. 

1. Selecteer op de dashboardpagina van uw praktijksessie **opnieuw publiceren** in de sectie van de sjabloon. 

    ![Knop publiceren](../media/how-to-create-manage-template/republish-button.png)
2. Op de **opnieuw publiceren van de sjabloon** dialoogvenster, bekijk de tekst en selecteert u **opnieuw publiceren** om door te gaan. Selecteer anders **annuleren**. 

    ![Het bericht sjabloon publiceren](../media/how-to-create-manage-template/republish-template-message.png)
3. U ziet de status van de opnieuw publiceren op de tegel in de **sjabloon** sectie.

    ![Status van het opnieuw te publiceren](../media/how-to-create-manage-template/republish-status-publishing.png)
4. Nadat de sjabloon is gepubliceerd, de status is ingesteld op **gepubliceerd**. 

    ![Opnieuw publiceren geslaagd](../media/how-to-create-manage-template/republish-success.png)

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een lab instellen](../tutorial-create-custom-lab.md)
