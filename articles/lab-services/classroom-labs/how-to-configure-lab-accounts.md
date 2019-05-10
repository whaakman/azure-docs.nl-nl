---
title: Lab-accounts configureren in Azure Lab Services | Microsoft Docs
description: Informatie over het configureren van een lab-account nadat deze gemaakt.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: ba469c038f04a31a57e798b97b5120bec573feae
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414040"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Lab-accounts configureren in Azure Lab Services 
Een lab-account is in Azure Lab-Services, een container voor beheerde labtypen zoals leslokaallabs. Een beheerder stelt u een lab-account met Azure Lab-Services en biedt toegang tot een lab-eigenaren die labs in het account maken kunnen. In dit artikel wordt beschreven hoe u een lab-account maken, alle lab-accounts weergeven of verwijderen van een lab-account.

## <a name="connect-with-a-peer-virtual-network"></a>Verbinding maken met een virtueel netwerk van peer
Als u wilt verbinding maken met een virtueel netwerk als een peer-netwerk met het virtuele netwerk van de testomgeving, de volgende stappen uit:

1. Op de **Lab-Account** weergeeft, schakelt **Labs configuratie** in het menu links.

    ![Configuratiepagina voor Labs](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. Voor **virtuele peernetwerk**, selecteer **ingeschakeld** of **uitgeschakelde**. De standaardwaarde is **uitgeschakelde**. Om in te schakelen op het virtuele peernetwerk, voer de volgende stappen uit: 
    1. Selecteer **ingeschakeld**.
    2. Selecteer de **VNet** uit de vervolgkeuzelijst. 
3. Selecteer **Opslaan** op de werkbalk. 

Labs gemaakt in dit account zijn verbonden met het geselecteerde virtuele netwerk. Ze hebben toegang tot de bronnen in de geselecteerde virtuele netwerk. Zie voor meer informatie, [van uw testlab-netwerk verbinden met een virtueel netwerk van de peer in Azure Lab Services](how-to-connect-peer-virtual-network.md).

Wanneer u een virtueel netwerk voor de **virtuele peernetwerk** veld, de **labmaker toestaan om te kiezen lab locatie** optie is uitgeschakeld. Dit is omdat labs in het lab-account moeten zich in dezelfde regio bevinden als het lab-account waarmee ze verbinding kunnen maken met resources in het virtuele peernetwerk. 

## <a name="allow-lab-creator-to-pick-location-for-the-lab"></a>Labmaker om op te halen van de locatie voor de testomgeving toestaan
Labmaker labs maken in een andere locatie dan de locatie van het lab-account door deze stappen te volgen, kunt u toestaan: 

1. Op de **Lab-Account** weergeeft, schakelt **Labs configuratie** in het menu links.
2. Voor de **labmaker toestaan om te kiezen lab locatie**, selecteer **ingeschakeld** als u wilt dat de labmaker kunnen een locatie voor de testomgeving te selecteren. Als deze uitgeschakeld, worden de labs automatisch gemaakt op dezelfde locatie waarin de lab-account bestaat. 
    
    Dit veld wordt uitgeschakeld wanneer u een virtueel netwerk voor de **virtuele peernetwerk** veld. Dit is omdat labs in het lab-account moeten zich in dezelfde regio als het lab-account voor toegang tot bronnen in het virtuele peernetwerk. 
1. Selecteer **Opslaan** op de werkbalk. 

    ![Lab-locatie instellen](../media/how-to-manage-lab-accounts/labs-configuration-page-lab-location.png)


## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Geef een adresbereik voor virtuele machines in het lab
De volgende procedure bevat stappen om op te geven van een adresbereik voor virtuele machines in het lab. Als u bijwerkt naar het bereik dat u eerder hebt opgegeven, wordt de gewijzigde adresbereik geldt alleen voor virtuele machines die zijn gemaakt nadat de wijziging is doorgevoerd. 

Hier zijn enkele beperkingen bij het opgeven van het adresbereik dat u rekening moet houden. 

- Het voorvoegsel moet kleiner zijn dan of gelijk zijn aan 23. 
- Als een virtueel netwerk is gekoppeld aan het lab-account, kan niet het opgegeven adresbereik-adresbereik van gekoppelde virtuele netwerk overlapt.

1. Op de **Lab-Account** weergeeft, schakelt **Labs configuratie** in het menu links.
2. Voor de **adresbereik** veld, geeft u het adresbereik voor virtuele machines die worden gemaakt in het lab. Het adresbereik moet zich in de notatie klasseloze routing tussen domeinen (CIDR) (voorbeeld: 10.20.0.0/23). Virtuele machines in de testomgeving wordt in dit adresbereik worden gemaakt.
3. Selecteer **Opslaan** op de werkbalk. 

    ![Adresbereik configureren](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Een gebruiker toevoegen aan de rol Labmaker
Om een leslokaallab in een labaccount in te kunnen stellen, moet de gebruiker lid zijn van de rol **Labmaker** in het labaccount. Het account dat u hebt gebruikt voor het maken van het lab-account wordt automatisch toegevoegd aan deze rol. Als u van plan bent een leslokaallab te maken met hetzelfde gebruikersaccount, kunt u deze stap overslaan. Als u een ander gebruikersaccount wilt gebruiken om een leslokaallab te maken, voert u de volgende stappen uit: 

Als u machtigingen wilt toekennen aan docenten om labs te maken voor hun lessen, voegt u hen toe aan de rol **Labmaker**:

1. Selecteer op de pagina **Lab-account** de optie **Toegangsbeheer (IAM)** en klik op **+ Roltoewijzing toevoegen** in de werkbalk. 

    ![Access Control -> Knop Roltoewijzing toevoegen](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Selecteer op de pagina **Roltoewijzing toevoegen** de optie **Labmaker** als **Rol**. Selecteer de gebruiker die u wilt toevoegen aan de rol Labmaker en selecteer **Opslaan**. 

    ![Labmaker toevoegen](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>Microsoft Azure Marketplace-installatiekopieën opgeven die beschikbaar zijn voor labmakers
Als eigenaar van een labaccount kunt u de Marketplace-installatiekopieën opgeven die labmakers kunnen gebruiken in het labaccount. 

1. Selecteer **Marketplace-installatiekopieën** in het menu aan de linkerkant. Standaard ziet u de volledige lijst met installatiekopieën (zowel ingeschakelde als uitgeschakelde). U kunt de lijst filteren om alleen ingeschakelde/uitgeschakelde installatiekopieën te bekijken door de optie **Alleen ingeschakeld**/**Alleen uitgeschakeld** in de vervolgkeuzelijst bovenaan te selecteren. 
    
    ![Pagina Microsoft Azure Marketplace-installatiekopieën](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    De Marketplace-installatiekopieën die worden weergegeven in de lijst, zijn de enige die voldoen aan de volgende voorwaarden:
        
    - Hiermee wordt een enkele VM gemaakt.
    - Maakt gebruik van Azure Resource Manager om VM’s in te richten
    - Hiervoor hoeft u geen extra licentieabonnement aan te schaffen
2. Als u een Microsoft Azure Marketplace-installatiekopie die is ingeschakeld wilt **uitschakelen**, voert u een van de volgende acties uit: 
    1. Selecteer **... (beletselteken)**  in de laatste kolom en selecteer **Installatiekopie uitschakelen**. 

        ![Een installatiekopie uitschakelen](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Selecteer een of meer installatiekopieën in de lijst door de selectievakjes bij de namen van de installatiekopieën in de lijst te selecteren en **Geselecteerde installatiekopieën uitschakelen** te selecteren. 

        ![Meerdere installatiekopieën uitschakelen](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Als u een Microsoft Azure Marketplace-installatiekopie wilt **inschakelen**, voert u een van de volgende acties uit: 
    1. Selecteer **... (beletselteken)**  in de laatste kolom en selecteer **Installatiekopie inschakelen**. 
    2. Selecteer een of meer installatiekopieën in de lijst door de selectievakjes bij de namen van de installatiekopieën in de lijst te selecteren en **Geselecteerde installatiekopieën inschakelen** te selecteren. 




## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Het gebruik van een lab configureren en beheren als labeigenaar](how-to-configure-student-usage.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)
