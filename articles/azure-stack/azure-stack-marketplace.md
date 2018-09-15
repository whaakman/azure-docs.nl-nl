---
title: Een aangepast marketplace-item publiceren in Azure Stack (cloud-operator) | Microsoft Docs
description: Als Azure Stack-operators, informatie over het publiceren van een aangepast marketplace-item in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 12310c088777d65bef211747806f942433857e40
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632345"
---
# <a name="the-azure-stack-marketplace-overview"></a>Het overzicht van Azure Stack Marketplace

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De Marketplace is een verzameling van services, toepassingen en bronnen die is afgestemd op Azure Stack. Resources omvatten netwerken, virtuele machines en opslag. Gebruikers op ieder nieuwe resources maken en implementeren van nieuwe toepassingen. Het zien als een winkelwagen catalogus waar gebruikers kunnen bladeren en kies de items die ze willen gebruiken. Voor het gebruik van een Marketplace-item, moeten gebruikers zich abonneren op een aanbieding die ze toegang tot het item verleent.

Als Azure Stack-operators, beslist u welke items om toe te voegen (publiceren) op de Marketplace. U kunt items zoals databases, App Services, enzovoort publiceren. Publicatie zorgt ervoor dat ze zichtbaar voor alle gebruikers. U kunt aangepaste items die u maakt. U kunt ook publiceren items uit een groeiend [lijst met items voor de Azure Marketplace](azure-stack-marketplace-azure-items.md). Wanneer u een item in de Marketplace publiceert, kunnen gebruikers ze bekijken binnen vijf minuten.

> [!Caution]  
> Alle galerie-item artefacten bekend als afbeeldingen en json-bestanden zijn toegankelijk zonder verificatie na zodat ze beschikbaar zijn in de Azure Stack marketplace. Zie voor meer overwegingen bij het publiceren van aangepaste marketplace-items [maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md).

Als u de Marketplace, in de beheerconsole schakelt **+ een resource maken**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Items voor de Marketplace
Een Azure Stack Marketplace-item is een service, toepassing of resource die uw gebruikers kunnen downloaden en gebruiken. Alle Azure Stack Marketplace-items zijn zichtbaar voor alle gebruikers, met inbegrip van beheeritems zoals plannen en aanbiedingen. Deze items nodig geen voor een abonnement om te bekijken, maar zijn niet functioneel voor gebruikers.

Elke Marketplace-item heeft:

* Een Azure Resource Manager-sjabloon voor de inrichting van resources
* Metagegevens, zoals tekenreeksen, pictogrammen en andere marketingmateriaal
* Opmaak van gegevens om weer te geven van het item in de portal

Elk gepubliceerd naar de Marketplace-item maakt gebruik van de Azure Gallery-pakket (.azpkg)-indeling. Toevoegen-implementatie of runtime resources (zoals code, zip-bestanden met software of installatiekopieën van virtuele machines) met Azure Stack afzonderlijk, niet als onderdeel van de Marketplace-item. 

Met versie 1803 of hoger, Azure Stack afbeeldingen worden geconverteerd naar sparse bestanden wanneer ze worden gedownload vanuit Azure of wanneer het uploaden van aangepaste installatiekopieën. Dit proces voegt tijd bij het toevoegen van een afbeelding, maar bespaart ruimte en versnelt de implementatie van deze installatiekopieën. Conversie is alleen van toepassing op nieuwe afbeeldingen.  Bestaande installatiekopieën worden niet gewijzigd. 

## <a name="next-steps"></a>Volgende stappen
[Marketplace-items downloaden](azure-stack-download-azure-marketplace-item.md)  
[Een Marketplace-item maken en publiceren](azure-stack-create-and-publish-marketplace-item.md)

