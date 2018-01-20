---
title: Inschakelen van de installatiekopie van een gelicentieerde in uw lab in Azure DevTest Labs | Microsoft Docs
description: Informatie over het inschakelen van de installatiekopie van een gelicentieerde in Azure DevTest Labs met de Azure portal
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: v-craic
ms.openlocfilehash: 022b6340a8d2748624ba292fb4a28a956d28c6f9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Inschakelen van de installatiekopie van een gelicentieerde in uw lab in Azure DevTest Labs

In Azure DevTest Labs is een installatiekopie van een gelicentieerde een met voorwaarden en bepalingen – doorgaans van een derde partij – die moeten worden geaccepteerd voordat de installatiekopie toegankelijk voor gebruikers in de testomgeving is. De volgende secties beschrijven het werken met gelicentieerde afbeeldingen, zodat ze beschikbaar zijn voor gebruik voor het maken van virtuele machines.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Bij het bepalen of een installatiekopie van een gelicentieerde beschikbaar voor gebruikers
De eerste stap bij zodat gebruikers kunnen virtuele machines maken van een installatiekopie van een licentie is om ervoor te zorgen dat de voorwaarden voor de installatiekopie van het gelicentieerde zijn geaccepteerd. De volgende stappen laten zien hoe u kunt de status van de aanbieding van een installatiekopie van een gelicentieerde bekijken en, indien nodig, de voorwaarden accepteren.

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** uit de lijst.

1. Selecteer de gewenste testomgeving uit de lijst van labs.  

1. In het linkerdeelvenster onder **instellingen**, selecteer **configuratie en het beleid**.

1. In het linkerdeelvenster onder **BASISSEN voor virtuele MACHINE**, selecteer **Marketplace-installatiekopieën**. 

    ![Menu-item voor Marketplace-installatiekopieën](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Een lijst met alle beschikbare marketplace-installatiekopieën wordt weergegeven, met inbegrip van de **bieden STATUS** voor elke installatiekopie.

    ![Lijst met marketplace-installatiekopieën weergegeven van de status van de aanbieding voor elke installatiekopie](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Een installatiekopie van een gelicentieerde toont de status van een aanbieding van 
    
    - **Voorwaarden geaccepteerd:** de installatiekopie van het gelicentieerde is beschikbaar voor gebruikers voor het maken van virtuele machines. 
    - **Voorwaarden controleren die nodig zijn:** de installatiekopie van het gelicentieerde is momenteel niet beschikbaar voor gebruikers. De voorwaarden en bepalingen van de licentie moeten worden geaccepteerd voordat lab gebruikers deze gebruiken kunnen om virtuele machines maken. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Een installatiekopie van een gelicentieerde maken beschikbaar voor gebruikers van de testomgeving
Om te zorgen dat de installatiekopie van een licentie is beschikbaar voor gebruikers van de testomgeving, moet eerst de eigenaar van een testlab met beheerdersmachtigingen accepteren de voorwaarden voor de betreffende gelicentieerde afbeelding. Inschakelen van programmatische implementatie voor het abonnement automatisch gekoppeld aan een installatiekopie van een gelicentieerde accepteert de juridische voorwaarden en privacyverklaringen voor de betreffende afbeelding. [Werken met Marketplace-installatiekopieën op Azure Resource Manager](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) biedt aanvullende informatie over programmatische implementatie van installatiekopieën van marketplace.

U kunt programmatische implementatie voor de installatiekopie van een gelicentieerde inschakelen met de volgende stappen:

1. In de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), gaat u naar de lijst met **Marketplace-installatiekopieën**.

1. Identificeer een gelicentieerde installatiekopie voor die u wilt dat gebruikers toegang moet hebben, maar waarvan de bepalingen zijn niet geaccepteerd. Bijvoorbeeld, ziet u mogelijk een gegevens wetenschappelijke virtuele Machine die de status van een wordt **voorwaarden geaccepteerd** of **controleren die nodig zijn de voorwaarden**.

    ![Venster programmatische implementatie configureren](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Gegevens wetenschappelijke VM's zijn installatiekopieën van virtuele Machine van Azure, vooraf is geïnstalleerd en getest met verschillende populaire hulpprogramma's die vaak worden gebruikt voor gegevens analytics, machine learning en AI training geconfigureerd. [Inleiding op Azure gegevens wetenschappelijke virtuele Machine voor Linux en Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) biedt een grote hoeveelheid informatie over DSVMs.
   >
   >

1. In de **bieden STATUS** kolom voor de installatiekopie, selecteer **controleren die nodig zijn de voorwaarden**.

1. Selecteer in het venster programmatische implementatie configureren **inschakelen**.

    ![Venster programmatische implementatie configureren](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > U ziet mogelijk meerdere abonnementen die worden vermeld in het venster programmatische implementatie configureren. Zorg ervoor dat u programmatische implementatie alleen voor het gewenste abonnement wilt inschakelen.
   >
   >


1. Selecteer **Opslaan**. 

    In de lijst met marketplace-installatiekopieën, afbeelding die nu toont **voorwaarden geaccepteerd** en is beschikbaar voor gebruikers om virtuele machines te maken.

> [!NOTE]
> Gebruikers kunnen een aangepaste installatiekopie maken van een installatiekopie van een licentie. Zie [een aangepaste installatiekopie maken van een VHD-bestand](devtest-lab-create-template.md) voor meer informatie.
>
>


## <a name="related-blog-posts"></a>Verwante blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie van een virtuele machine maken](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [Een aangepaste installatiekopie van een VHD-bestand maken](devtest-lab-create-template.md)
- [Een virtuele machine toevoegen aan uw testomgeving](devtest-lab-add-vm.md)