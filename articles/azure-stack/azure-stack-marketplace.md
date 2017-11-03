---
title: Een aangepaste marketplace-item publiceren in Azure-Stack (cloud-operator) | Microsoft Docs
description: Als een Azure-Stack-operator, informatie over het publiceren van een aangepaste marketplace-item in de Azure-Stack.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: erikje
ms.openlocfilehash: 7b5f976eb2d51eb86761a2bd0be6adb45ca87681
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="the-azure-stack-marketplace-overview"></a>Het overzicht van Azure Stack Marketplace

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De Marketplace is een verzameling van services, toepassingen en bronnen zoals netwerken, virtuele machines en opslag voor Azure-Stack is aangepast. Gebruikers komen hier nieuwe resources maken en implementeren van nieuwe toepassingen. Beschouwen als een webwinkel catalogus waar gebruikers kunnen bladeren en kies de items die ze willen gebruiken. Voor het gebruik van een Marketplace-item, moeten gebruikers zich abonneren op een aanbieding die ze toegang tot het item verleent.

Als een Azure-Stack-operator, besluit u welke items om toe te voegen (publiceren) naar de Marketplace. U kunt items zoals databases en App-Services publiceren. Hierdoor kunt u ze zichtbaar is voor alle gebruikers. Aangepaste items die u maakt, kunt u publiceren. U kunt ook items uit een groeiende publiceren [lijst met items van Azure Marketplace](azure-stack-marketplace-azure-items.md). Wanneer u een item in de Marketplace publiceert, kunnen gebruikers deze zien binnen vijf minuten.

De Marketplace, klikt u op **nieuw**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace-items
Een Azure-Stack Marketplace-item is een service, toepassing of resource die uw gebruikers kunnen downloaden en gebruiken. Alle Azure-Stack Marketplace-items zijn zichtbaar voor alle gebruikers.

Elk Marketplace-item heeft:

* Een Azure Resource Manager-sjabloon voor het inrichten van resource
* Metagegevens, zoals tekenreeksen, pictogrammen en andere marketingmateriaal
* Opmaak van gegevens naar het item wordt weergegeven in de portal

Elk item dat is gepubliceerd naar de Marketplace maakt gebruik van een indeling, genaamd Azure Gallery-pakket (azpkg). Toevoegen-implementatie of runtime bronnen (zoals code, zip-bestanden met software of installatiekopieën van virtuele machines) naar Azure Stack afzonderlijk, niet als onderdeel van de Marketplace-Item. 

## <a name="next-steps"></a>Volgende stappen
[Maken en publiceren van een marketplace-item](azure-stack-create-and-publish-marketplace-item.md)

