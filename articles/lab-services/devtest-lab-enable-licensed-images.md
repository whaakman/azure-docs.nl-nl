---
title: Inschakelen van een gelicentieerde installatiekopie in uw testomgeving in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het inschakelen van een gelicentieerde installatiekopie in Azure DevTest Labs met behulp van de Azure portal
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 42f3378196b4e3e1dfe1234e801e87875f9c61fc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247650"
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Een gelicentieerde installatiekopie in uw testomgeving in Azure DevTest Labs inschakelen

In Azure DevTest Labs is een gelicentieerde installatiekopie een met de voorwaarden en bepalingen – meestal van een derde partij – die moet worden geaccepteerd voordat de installatiekopie toegankelijk voor gebruikers in het lab is. De volgende secties wordt beschreven hoe u werken met installatiekopieën met licentie, zodat ze beschikbaar zijn voor het gebruiken voor het maken van virtuele machines.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Bepalen of een gelicentieerde installatiekopie beschikbaar voor gebruikers is
De eerste stap bij zodat gebruikers een voor het maken van virtuele machines van een gelicentieerde installatiekopie is om ervoor te zorgen dat de algemene voorwaarden zijn geaccepteerd voor de gelicentieerde installatiekopie. De volgende stappen laten zien hoe u kunt de status van de aanbieding van een gelicentieerde installatiekopie bekijken en, indien nodig, de voorwaarden accepteren.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.  

1. In het linkerdeelvenster onder **instellingen**, selecteer **configuratie en het beleid**.

1. In het linkerdeelvenster onder **VM-BASISSEN**, selecteer **Marketplace-installatiekopieën**. 

    ![Marketplace-installatiekopieën menu-item](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Een lijst met alle beschikbare marketplace-installatiekopieën wordt weergegeven, met inbegrip van de **STATUS bieden** voor elke afbeelding.

    ![Lijst van marketplace-installatiekopieën met status van de aanbieding voor elke afbeelding](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Een gelicentieerde installatiekopie bevat de status van een aanbieding van 
    
    - **Voorwaarden geaccepteerd:** de gelicentieerde installatiekopie is beschikbaar voor gebruikers om te maken van virtuele machines. 
    - **Beoordeling is vereist de voorwaarden:** de gelicentieerde installatiekopie is momenteel niet beschikbaar voor gebruikers. De voorwaarden en bepalingen van de licentie moeten worden geaccepteerd voordat labgebruikers deze gebruiken kunnen om virtuele machines maken. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Een gelicentieerde installatiekopie maken beschikbaar voor labgebruikers
Als u wilt controleren of dat een gelicentieerde installatiekopie is beschikbaar voor labgebruikers, moet eerst de eigenaar van een testlab met beheerdersmachtigingen accepteren de voorwaarden en bepalingen van die installatiekopie een licentie. Programmatische implementatie voor het abonnement die automatisch zijn gekoppeld aan een gelicentieerde installatiekopie inschakelen, accepteert de juridische voorwaarden en privacyverklaringen voor die installatiekopie. [Werken met de Marketplace-installatiekopieën op Azure Resource Manager](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) bevat aanvullende informatie over programmatische implementatie van marketplace-installatiekopieën.

U kunt programmatische implementatie voor een gelicentieerde installatiekopie inschakelen door de volgende stappen:

1. In de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040), gaat u naar de lijst met **Marketplace-installatiekopieën**.

1. Identificeren van een gelicentieerde installatiekopie waarvoor u wilt dat gebruikers toegang moet hebben, maar die niet zijn geaccepteerd. Bijvoorbeeld, ziet u een Data Science Virtual Machine waarin de status van een van beide **voorwaarden geaccepteerd** of **beoordeling is vereist de voorwaarden**.

    ![Venster programmatische implementatie configureren](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Data Science-VM's zijn installatiekopieën voor virtuele Machine van Azure, vooraf zijn geïnstalleerd, geconfigureerd en getest met diverse populaire programma's die veel worden gebruikt voor gegevensanalyse, machine learning en AI-training. [Inleiding op Azure Data Science Virtual Machine voor Linux en Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) biedt een grote hoeveelheid informatie over Dsvm.
   >
   >

1. In de **bieden STATUS** kolom voor de installatiekopie, selecteer **beoordeling is vereist de voorwaarden**.

1. Selecteer in het venster programmatische implementatie configureren **inschakelen**.

    ![Venster programmatische implementatie configureren](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > U ziet mogelijk meerdere abonnementen die worden vermeld in het venster programmatische implementatie configureren. Zorg ervoor dat u programmatische implementatie alleen voor het gewenste abonnement wilt inschakelen.
   >
   >


1. Selecteer **Opslaan**. 

    In de lijst van marketplace-installatiekopieën, afbeelding die nu toont **voorwaarden geaccepteerd** en is beschikbaar voor gebruikers om virtuele machines te maken.

> [!NOTE]
> Gebruikers kunnen een aangepaste installatiekopie maken van een gelicentieerde installatiekopie. Zie [een aangepaste installatiekopie maken van een VHD-bestand](devtest-lab-create-template.md) voor meer informatie.
>
>


## <a name="related-blog-posts"></a>Gerelateerde blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie van een virtuele machine maken](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [Een aangepaste installatiekopie van een VHD-bestand maken](devtest-lab-create-template.md)
- [Een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md)