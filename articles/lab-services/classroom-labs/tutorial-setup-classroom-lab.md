---
title: Opzetten van een leslokaallab met Azure Lab Services | Microsoft Docs
description: In deze zelfstudie stelt u een lab in voor gebruik in een leslokaal.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: a96ba4aec7c23c040921a647cc4986aaf53fb30c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651326"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Zelfstudie: Een leslokaallab instellen 
In deze zelfstudie stelt u een leslokaallab in met een set virtuele machines die worden gebruikt door studenten in het leslokaal.  

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een leslokaallab maken
> * Het leslokaallab configureren
> * Registratiekoppeling naar studenten verzenden

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com).
2. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** op voor het leslokaallab. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in het leslokaal.
    3. Kies de **afbeelding**  die u wilt gebruiken om de virtuele machine te maken.
    4. Geef de **standaardgegevens** op voor het aanmelden bij de virtuele machines in het lab. 
    7. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. U ziet de **startpagina** voor het lab. 
    
    ![Startpagina leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)

## <a name="set-up-the-template"></a>De sjabloon instellen 
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Stel de zichtbaarheid van de sjabloon in op openbaar om instanties van de sjabloon-VM voor de gebruikers van uw lab beschikbaar te maken. 

### <a name="set-title-and-description"></a>Titel en beschrijving instellen
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/tutorial-setup-classroom-lab/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Exemplaren van de sjabloon openbaar maken
Nadat u de zichtbaarheid van een sjabloon hebt ingesteld op **Openbaar**, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon. 

1. Selecteer **Zichtbaarheid** in de sectie **Sjabloon**. 
2. Selecteer op de pagina **Beschikbaarheid** de optie **Openbaar**.
    
    > [!IMPORTANT]
    > Wanneer een sjabloon openbaar beschikbaar is, kan de toegang kan niet worden gewijzigd naar privé. 
3. Selecteer **Opslaan**.

    ![Beschikbaarheid](../media/tutorial-setup-classroom-lab/public-access.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Selecteer de tegel **Gebruikersregistratie**.
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab gemaakt en het lab geconfigureerd. Ga voor meer informatie over hoe een student toegang kan krijgen tot een virtuele machine in het lab met behulp van de registratiekoppeling naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Verbinding maken met een virtuele machine in het leslokaallab](tutorial-connect-virtual-machine-classroom-lab.md)

