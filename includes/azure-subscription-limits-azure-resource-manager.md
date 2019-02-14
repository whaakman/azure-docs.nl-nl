---
title: bestand opnemen
description: bestand opnemen
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 10/19/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 4cc115d068736f61f9edb4ec609ac592607e7fa0
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246832"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Virtuele machines [per abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |10.000 <sup>1</sup> per regio |10.000 per regio |
| Totaal aantal VM-cores per [abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per regio | Contact opnemen met ondersteuning |
| VM’s per reeks (Dv2, F, enz.), cores per [abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per regio | Contact opnemen met ondersteuning |
| [Medebeheerders](../articles/billing-add-change-azure-subscription-administrator.md) per abonnement |Onbeperkt |Onbeperkt |
| [Storage-accounts](../articles/storage/common/storage-quickstart-create-account.md) per regio per abonnement |200 |200<sup>2</sup> |
| [Resourcegroepen](../articles/azure-resource-manager/resource-group-overview.md) per abonnement |980 |980 |
| [Beschikbaarheidssets](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) per abonnement |2000 per regio |2000 per regio |
| Aanvraaggrootte Resource Manager-API |4.194.304 bytes |4.194.304 bytes |
| Tags per abonnement<sup>3</sup> |onbeperkt |onbeperkt |
| Unieke tagberekeningen per abonnement<sup>3</sup> | 10.000 | 10.000 |
| [Cloudservices](../articles/cloud-services/cloud-services-choose-me.md) per abonnement |Niet van toepassing<sup>4</sup> |Niet van toepassing<sup>4</sup> |
| [Affiniteitsgroepen](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) per abonnement |Niet van toepassing<sup>4</sup> |Niet van toepassing<sup>4</sup> |
| [Niveau abonnementimplementaties](../articles/azure-resource-manager/deploy-to-subscription.md) per locatie | 800 | 800 |

<sup>1</sup>Standaardlimieten zijn afhankelijk van het type aanbieding, zoals gratis proefversies, betalen naar gebruik of per serie, zoals Dv2, F, G, enz.

<sup>2</sup>Hieronder vallen zowel Standard als Premium opslagaccounts. Als u meer dan 200 opslagaccounts nodig hebt, dient u een aanvraag in te dienen via de [ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het Azure Storage-team bekijkt dan uw bedrijfsscenario en kan tot 250 opslagaccounts goedkeuren.

<sup>3</sup>U kunt u een onbeperkt aantal tags per abonnement toepassen. Het aantal tags per resource of resourcegroep is beperkt tot 15. Resource Manager retourneert alleen een [lijst met unieke tagnamen en -waarden](/rest/api/resources/tags) in het abonnement wanneer het aantal tags 10.000 of minder is. U kunt echter nog steeds resources zoeken aan de hand van tags wanneer er meer dan 10.000 tags zijn.  

<sup>4</sup>Deze functies zijn niet langer zijn vereist voor Azure-resourcegroepen en de Azure Resource Manager.

> [!NOTE]
> Het is belangrijk om te benadrukken dat er voor de cores van virtuele machine zowel regionale beperkingen gelden als regionale beperkingen per grootte (Dv2, F, enz.), die afzonderlijk worden gehandhaafd.  Neem bijvoorbeeld een abonnement met een limiet van 30 VM-cores voor US - oost, een limiet van 30 cores voor de A-serie en een limiet van 30 cores voor de D-serie.  Met dit abonnement zouden 30 A1-VM’s of 30 D1-VM’s kunnen worden geïmplementeerd, of een combinatie van deze twee typen die het totaal van 30 cores niet overschrijdt (bijvoorbeeld 10 A1-VM’s en 20 D1-VM’s).  
> <!-- -->
> 
> 

