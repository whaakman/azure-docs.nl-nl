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
ms.date: 04/20/2018
ms.author: spelluru
ms.openlocfilehash: 17544275f921486529518e37eb171cd0b4f26791
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="manage-classroom-labs-in-azure-lab-services-formerly-azure-devtest-labs"></a>Leslokaal labs in Azure Lab Services (voorheen Azure DevTest Labs) beheren
In dit artikel wordt beschreven hoe maken en configureren van een testomgeving leslokaal, alle leslokaal labs weergeven of verwijderen van een testomgeving leslokaal.

## <a name="create-a-classroom-lab"></a>Een lab leslokaal maken

1. Navigeer naar [Azure Lab Services website](https://labs.azure.com).
2. In de **nieuwe Lab** venster de volgende acties uitvoeren: 
    1. Geef een **naam** voor de testomgeving leslokaal. 
    2. Selecteer de **grootte** van de virtuele machine die u wilt gebruiken in de klas.
    3. Selecteer de **installatiekopie** gebruiken om te maken van de virtuele machine.
    4. Geef de **standaardreferenties** moet worden gebruikt voor aanmelden bij de virtuele machines in de testomgeving.
    7. Selecteer **Opslaan**.

        ![Een lab leslokaal maken](./media/how-to-manage-classroom-labs/new-lab-window.png)
1. U ziet de **startpagina** voor de testomgeving. 
    
    ![Leslokaal lab-startpagina](./media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Gebruiksbeleid configureren

1. Selecteer **gebruiksbeleid**. 
2. In de **gebruiksbeleid**, instellingen, voer de **aantal gebruikers** mogen gebruiken als de testomgeving.
3. Selecteer **Opslaan**. 

    ![Beleid voor het gebruik](./media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Instellen van de sjabloon
Een sjabloon in een testomgeving is van alle gebruikers virtuele machines worden gemaakt. De sjabloon voor virtuele machine zo instellen dat deze is geconfigureerd met exact wat u wilt bieden aan de gebruikers van de testomgeving. U kunt Geef een naam en beschrijving van de sjabloon die de gebruikers van het lab te zien en de zichtbaarheid ingesteld op 'Openbare' exemplaren van de VM-sjabloon beschikbaar maken voor uw lab-gebruikers.  

## <a name="set-template-title-and-description"></a>Set Sjabloontitel en beschrijving
1. In de **sjabloon** sectie **bewerken** (potloodpictogram) voor de sjabloon. 
2. In de **weergave gebruiker** venster Enter een **titel** voor de sjabloon.
3. Voer **beschrijving** voor de sjabloon.
4. Selecteer **Opslaan**.

    ![Leslokaal lab beschrijving](./media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Exemplaren van de sjabloon openbaar maken 
Nadat u de zichtbaarheid van een sjabloon die moet worden ingesteld **openbare**, Azure Lab Services maakt virtuele machines in de testomgeving met behulp van de sjabloon. Het aantal virtuele machines die zijn gemaakt tijdens dit proces zijn hetzelfde als het maximum aantal gebruikers dat is toegestaan in de testomgeving, u in het beleid voor het gebruik van het testlab instellen kunt. Alle virtuele machines hebben dezelfde configuratie als de sjabloon.  

1. Selecteer **zichtbaarheid** in de **sjabloon** sectie. 
2. In de **beschikbaarheid** pagina **openbare**.
    
    > [!IMPORTANT]
    > Wanneer een sjabloon openbaar beschikbaar is, kan de toegang kan niet worden gewijzigd naar privé. 
3. Selecteer **Opslaan**.

    ![Beschikbaarheid](./media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden

1. Selecteer **gebruikersregistratie** tegel.
2. In de **gebruikersregistratie** selecteert u de **kopie** knop. De koppeling is naar het Klembord gekopieerd. Plak deze in een e-editor en een e-mailbericht verzenden naar de student. 

    ![Studenten registratiekoppeling](./media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Alle leslokaal labs weergeven
1. Navigeer naar [Lab Services van Azure-portal](https://labs.azure.com).
2. Controleer of u de labs in het geselecteerde lab-account. 

    ![Alle labs](./media/how-to-manage-classroom-labs/all-labs.png)
3. Gebruik de vervolgkeuzelijst boven aan een andere lab-account selecteren. U ziet labs in het geselecteerde lab-account. 

## <a name="delete-a-classroom-lab"></a>Een lab leslokaal verwijderen
1. Selecteer op de tegel voor de testomgeving drie puntjes (...) in de hoek. 

    ![Selecteer in het lab](./media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecteer **Verwijderen**. 

    ![Knop verwijderen](./media/how-to-manage-classroom-labs/delete-button.png)
3. Op de **verwijderen lab** dialoogvenster, **verwijderen**. 

    ![In het dialoogvenster verwijderen](./media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het instellen van een testomgeving met Azure Lab Services:

- [Een testomgeving leslokaal instellen](how-to-manage-classroom-labs.md)
- [Een aangepaste testomgeving instellen](tutorial-create-custom-lab.md)
