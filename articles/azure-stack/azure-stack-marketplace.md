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
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 6c8a2cd746f1090e802f90a49f9a1f7469e98f5f
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121513"
---
# <a name="azure-stack-marketplace-overview"></a>Overzicht van Azure Stack Marketplace

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De Azure Stack Marketplace is een verzameling van services, toepassingen en bronnen die is afgestemd op Azure Stack. Resources omvatten netwerken, virtuele machines en opslag. Gebruik de Marketplace nieuwe resources maken en implementeren van nieuwe toepassingen. of bladeren en kies de items die u wilt gebruiken. Voor het gebruik van een Marketplace-item, moeten gebruikers zich abonneren op een aanbieding die ze toegang tot het item verleent.

Als Azure Stack-operators, beslist u welke items om toe te voegen (publiceren) op de Marketplace. U kunt items zoals databases, App-Services, publiceren en enzovoort. Publicatie zorgt ervoor dat ze zichtbaar voor alle gebruikers. Kunt u aangepaste items die u maakt, of kunt u items uit een groeiend [lijst met items voor de Azure Marketplace](azure-stack-marketplace-azure-items.md). Wanneer u een item in de Marketplace publiceert, kunnen gebruikers ze bekijken binnen vijf minuten.

> [!CAUTION]  
> Alle galerie-item-artefacten, zoals afbeeldingen en JSON-bestanden zijn toegankelijk zonder verificatie na zodat ze beschikbaar zijn in de Azure Stack Marketplace. Zie voor meer overwegingen bij het publiceren van aangepaste marketplace-items [maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md).

De Marketplace te openen in de portal selecteert de beheerder **+ een resource maken**.

![Marketplace](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Items voor de Marketplace

Een Azure Stack Marketplace-item is een service, toepassing of resource die uw gebruikers kunnen downloaden en gebruiken. Alle Azure Stack Marketplace-items zijn zichtbaar voor alle gebruikers, met inbegrip van beheeritems zoals plannen en aanbiedingen. Deze items vereisen geen een abonnement om te bekijken, maar zijn niet functioneel voor gebruikers.

Elke Marketplace-item heeft:

* Een Azure Resource Manager-sjabloon voor de inrichting van resources.
* Metagegevens, zoals tekenreeksen, pictogrammen en andere marketingmateriaal.
* Opmaak van gegevens om weer te geven van het item in de portal.

Elk gepubliceerd naar de Marketplace-item maakt gebruik van de Azure Gallery-pakket (.azpkg)-indeling. Toevoegen-implementatie of runtime resources (code, zip-bestanden met software of installatiekopieën van virtuele machines) met Azure Stack afzonderlijk, niet als onderdeel van de Marketplace-item.

Met versie 1803 of hoger, Azure Stack afbeeldingen worden geconverteerd naar sparse bestanden wanneer ze worden gedownload vanuit Azure of wanneer het uploaden van aangepaste installatiekopieën. Dit proces voegt tijd bij het toevoegen van een afbeelding, maar bespaart ruimte en versnelt de implementatie van deze installatiekopieën. Conversie is alleen van toepassing op nieuwe afbeeldingen. Bestaande installatiekopieën worden niet gewijzigd.

## <a name="next-steps"></a>Volgende stappen

* [Marketplace-items downloaden](azure-stack-download-azure-marketplace-item.md)  
* [Een Marketplace-item maken en publiceren](azure-stack-create-and-publish-marketplace-item.md)
