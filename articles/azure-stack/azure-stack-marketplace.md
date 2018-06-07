---
title: Een aangepaste marketplace-item publiceren in Azure-Stack (cloud-operator) | Microsoft Docs
description: Als een Azure-Stack-operator, informatie over het publiceren van een aangepaste marketplace-item in de Azure-Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 4ea23ed01e6432f24024d7e8cc07c2dfe42ac639
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605566"
---
# <a name="the-azure-stack-marketplace-overview"></a>Het overzicht van Azure Stack Marketplace

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De Marketplace is een verzameling van services, toepassingen en bronnen worden aangepast voor Azure-Stack. Resources omvatten netwerken, virtuele machines en opslag. Gebruikers komen hier nieuwe resources maken en implementeren van nieuwe toepassingen. Beschouwen als een webwinkel catalogus waar gebruikers kunnen bladeren en kies de items die ze willen gebruiken. Voor het gebruik van een Marketplace-item, moeten gebruikers zich abonneren op een aanbieding die ze toegang tot het item verleent.

Als een Azure-Stack-operator, besluit u welke items om toe te voegen (publiceren) naar de Marketplace. U kunt items zoals databases en App-Services publiceren. Publicatie maakt deze zichtbaar voor alle gebruikers. Aangepaste items die u maakt, kunt u publiceren. U kunt ook items uit een groeiende publiceren [lijst met items van Azure Marketplace](azure-stack-marketplace-azure-items.md). Wanneer u een item in de Marketplace publiceert, kunnen gebruikers deze zien binnen vijf minuten.

> [!Caution]  
> Alle galerie-item artefacten bekend als installatiekopieën en json-bestanden zijn toegankelijk zijn zonder verificatie na zodat ze beschikbaar zijn in de Stack van Azure marketplace. Zie voor meer overwegingen bij het publiceren van aangepaste marketplace-items [maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md).

Als de Marketplace, in de beheerconsole schakelt **nieuw**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Items voor de Marketplace
Een Azure-Stack Marketplace-item is een service, toepassing of resource die uw gebruikers kunnen downloaden en gebruiken. Alle Azure-Stack Marketplace-items zijn zichtbaar voor alle gebruikers, met inbegrip van beheeritems zoals plannen en aanbiedingen. Deze objecten hebben een abonnement op de weergave, maar zijn niet-functioneel voor gebruikers geen nodig.

Elk Marketplace-item heeft:

* Een Azure Resource Manager-sjabloon voor het inrichten van resource
* Metagegevens, zoals tekenreeksen, pictogrammen en andere marketingmateriaal
* Opmaak van gegevens naar het item wordt weergegeven in de portal

Elk item dat is gepubliceerd naar de Marketplace maakt gebruik van de Azure Gallery-pakket (.azpkg)-indeling. Toevoegen-implementatie of runtime bronnen (zoals code, zip-bestanden met software of installatiekopieën van virtuele machines) naar Azure Stack afzonderlijk, niet als onderdeel van de Marketplace-item. 

Met versie 1803 en hoger, Azure Stack afbeeldingen worden geconverteerd naar sparse bestanden wanneer ze downloaden van Azure of wanneer u aangepaste installatiekopieën uploaden. Dit proces wordt toegevoegd tijd bij het toevoegen van een installatiekopie, maar databaseruimte en de implementatie van deze installatiekopieën sneller. Conversie is alleen van toepassing op nieuwe afbeeldingen.  Bestaande installatiekopieën worden niet gewijzigd. 

## <a name="next-steps"></a>Volgende stappen
[Downloaden van de Marketplace-items](azure-stack-download-azure-marketplace-item.md)  
[Maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md)

