---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ccc2b574ea054a1b0ecf32a1e59691050fb66fcf
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227184"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Labels van een virtuele Machine via sjablonen
Eerst laten we kijken tagging via sjablonen. [Deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) plaatst u tags op de volgende resources: Compute (virtuele Machine), opslagruimte (Opslagaccount) en netwerk (openbare IP-adres, Virtueelnetwerk en netwerk-Interface). Deze sjabloon is voor een Windows-VM, maar kan worden aangepast voor virtuele Linux-machines.

Klik op de **implementeren in Azure** knop van de [koppeling naar sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Dit, gaat u naar de [Azure-portal](https://portal.azure.com/) waarin u deze sjabloon kunt implementeren.

![Eenvoudige implementatie met Tags](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Deze sjabloon bevat de volgende codes: *afdeling*, *toepassing*, en *gemaakt door*. U kunt toevoegen/bewerken deze tags rechtstreeks in de sjabloon als u andere tagnaam.

![Azure-codes in een sjabloon](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Zoals u ziet, worden de labels worden gedefinieerd als sleutel/waarde-paren, gescheiden door een dubbele punt (:). De labels moeten worden gedefinieerd in deze indeling:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Sla het sjabloonbestand op als u klaar bent met de codes van uw keuze te bewerken.

Vervolgens in de **Parameters bewerken** sectie, kunt u de waarden invullen voor uw tags.

![Tags te bewerken in Azure portal](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klik op **maken** om deze sjabloon met de waarden in de code te implementeren.

## <a name="tagging-through-the-portal"></a>Via de Portal-tagging
Na het maken van uw resources met labels, kunt u weergeven, toevoegen en verwijderen van tags in de portal.

Selecteer het pictogram voor tags om de labels weer te geven:

![Pictogram van de labels in Azure portal](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Toevoegen van een nieuwe tag via de portal door te definiëren van uw eigen sleutel/waarde-paar en sla deze op.

![Nieuwe Tag toevoegen in Azure portal](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Uw nieuwe tag wordt nu weergegeven in de lijst met tags voor uw resource.

![Nieuwe Tag die zijn opgeslagen in Azure portal](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

