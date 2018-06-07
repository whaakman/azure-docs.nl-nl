---
title: Leslokaal labs in Azure Lab Services beheren | Microsoft Docs
description: Meer informatie over het maken en configureren van een testomgeving leslokaal, weergeven van alle de leslokaal labs gedeeld de registratie te koppelen met een lab-gebruiker of verwijderen van een testomgeving.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 705209becff7c8ad20e7d09f056aa1ae1577b7ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654972"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Leslokaal labs in Azure Lab Services beheren 
In dit artikel wordt beschreven hoe maken en configureren van een testomgeving leslokaal, alle leslokaal labs weergeven of verwijderen van een testomgeving leslokaal.

## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com).
2. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** op voor het leslokaallab. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in het leslokaal.
    3. Kies de **afbeelding**  die u wilt gebruiken om de virtuele machine te maken.
    4. Geef de **standaardreferenties** moet worden gebruikt voor aanmelden bij de virtuele machines in de testomgeving.
    7. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. U ziet de **startpagina** voor het lab. 
    
    ![Startpagina leslokaallab](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **Gebruiksbeleid**. 
2. Voer in het **Gebruiksbeleid**, Instellingen, het **aantal gebruikers** in dat het lab mag gebruiken.
3. Selecteer **Opslaan**. 

    ![Gebruiksbeleid](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>De sjabloon instellen
Een sjabloon in een lab is een basiskopie van de virtuele machine waarvan de virtuele machines van alle gebruikers worden gemaakt. Stel de sjabloon voor de virtuele machine zo in dat deze is geconfigureerd met precies wat u de labgebruikers wilt bieden. U kunt een naam en een beschrijving opgeven voor de sjabloon die de gebruikers van het lab kunnen zien. Stel de zichtbaarheid van de sjabloon in op openbaar om instanties van de sjabloon-VM voor de gebruikers van uw lab beschikbaar te maken.  

### <a name="set-template-title-and-description"></a>Set Sjabloontitel en beschrijving
1. Selecteer in de sectie **Sjabloon** de optie **Bewerken** (potloodpictogram) voor de sjabloon. 
2. Voer in het venster **Gebruikersweergave** een **titel** in voor de sjabloon.
3. Voer een **Beschrijving** in voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Beschrijving van leslokaallab](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Exemplaren van de sjabloon openbaar maken 
Nadat u de zichtbaarheid van een sjabloon hebt ingesteld op **Openbaar**, maakt Azure Lab Services virtuele machines in het lab met behulp van de sjabloon. Het aantal virtuele machines dat in dit proces wordt gemaakt, is hetzelfde als het maximum aantal gebruikers dat in het lab is toegestaan. Dit kunt u instellen in het gebruiksbeleid van het lab. Alle virtuele machines hebben dezelfde configuratie als de sjabloon.  

1. Selecteer **Zichtbaarheid** in de sectie **Sjabloon**. 
2. Selecteer op de pagina **Beschikbaarheid** de optie **Openbaar**.
    
    > [!IMPORTANT]
    > Wanneer een sjabloon openbaar beschikbaar is, kan de toegang kan niet worden gewijzigd naar privé. 
3. Selecteer **Opslaan**.

    ![Beschikbaarheid](../media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Selecteer de tegel **Gebruikersregistratie**.
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Alle leslokaal labs weergeven
1. Navigeer naar [Lab Services van Azure-portal](https://labs.azure.com).
2. Controleer of u de labs in het geselecteerde lab-account. 

    ![Alle labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Gebruik de vervolgkeuzelijst boven aan een andere lab-account selecteren. U ziet labs in het geselecteerde lab-account. 

## <a name="delete-a-classroom-lab"></a>Een lab leslokaal verwijderen
1. Selecteer op de tegel voor de testomgeving drie puntjes (...) in de hoek. 

    ![Het lab selecteren](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecteer **Verwijderen**. 

    ![Knop verwijderen](../media/how-to-manage-classroom-labs/delete-button.png)
3. Op de **verwijderen lab** dialoogvenster, **verwijderen**. 

    ![In het dialoogvenster verwijderen](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](how-to-manage-classroom-labs.md)
- [Een testomgeving instellen](../tutorial-create-custom-lab.md)
