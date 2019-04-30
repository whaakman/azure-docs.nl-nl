---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103977"
---
## <a name="use-the-azure-portal"></a>Azure Portal gebruiken
1. Selecteer de virtuele machine die u wilt implementeren, en selecteer vervolgens de *opnieuw implementeren* knop in de *instellingen* blade. Mogelijk moet u scrollen om te controleren de **ondersteuning en probleemoplossing** sectie met de knop 'Implementeren' zoals in het volgende voorbeeld:

    ![Azure VM-beheerblade](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Om te bevestigen dat de bewerking, selecteer de *opnieuw implementeren* knop:

    ![Opnieuw implementeren van een VM-beheerblade](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. De **Status** van de virtuele machine wordt gewijzigd in *bijwerken* als de virtuele machine wordt voorbereid om te implementeren, zoals wordt weergegeven in het volgende voorbeeld:

    ![Virtuele machine bijwerken](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. De **Status** wordt gewijzigd in *vanaf* als de virtuele machine wordt opgestart op een nieuwe Azure-host, zoals wordt weergegeven in het volgende voorbeeld:

    ![VM gestart](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Nadat de virtuele machine klaar is met het opstartproces, de **Status** retourneert vervolgens naar *met*, die wijzen op de virtuele machine is opnieuw is gedistribueerd:

    ![VM uitgevoerd](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

<!-- Update_Description: update meta properties -->