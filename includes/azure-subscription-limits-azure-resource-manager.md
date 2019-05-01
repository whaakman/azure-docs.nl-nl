---
title: bestand opnemen
description: bestand opnemen
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 04/22/2019
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 712b70960e09a9c2b0e7a998bc0bddbc28c1e112
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64733816"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Virtuele machines [per abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |25.000<sup>1</sup> per regio. |25.000 per regio. |
| Totaal aantal VM-cores per [abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per regio. | Neem contact op met ondersteuning. |
| Virtuele machine per serie, zoals Dv2- en F-cores per [abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per regio. | Neem contact op met ondersteuning. |
| [Medebeheerders](../articles/billing-add-change-azure-subscription-administrator.md) per abonnement |Onbeperkt. |Onbeperkt. |
| [Storage-accounts](../articles/storage/common/storage-quickstart-create-account.md) per regio per abonnement |250 |250 |
| [Resourcegroepen](../articles/azure-resource-manager/resource-group-overview.md) per abonnement |980 |980 |
| [Beschikbaarheidssets](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) per abonnement |2000 per regio. |2000 per regio. |
| Grootte van Azure Resource Manager-API-aanvraag |4,194,304 bytes. |4,194,304 bytes. |
| Tags per abonnement<sup>2</sup> |Onbeperkt. |Onbeperkt. |
| De unieke code berekeningen per abonnement<sup>2</sup> | 10.000 | 10.000 |
| [Cloudservices](../articles/cloud-services/cloud-services-choose-me.md) per abonnement |N.V.T.<sup>3</sup> |N.V.T.<sup>3</sup> |
| [Affiniteitsgroepen](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) per abonnement |N.V.T.<sup>3</sup> |N.V.T.<sup>3</sup> |
| [Abonnementsniveau implementaties](../articles/azure-resource-manager/deploy-to-subscription.md) per locatie | 800<sup>4</sup> | 800 |

<sup>1</sup>standaardlimieten variëren per type categorie aanbieding, zoals gratis proefversie en betalen per gebruik, en door, zoals Dv2, F en G.

<sup>2</sup>kunt u een onbeperkt aantal tags per abonnement toepassen. Het aantal tags per resource of resourcegroep is beperkt tot 15. Resource Manager retourneert een [lijst met unieke tagnamen en -waarden](/rest/api/resources/tags) in het abonnement alleen wanneer het aantal tags 10.000 of minder. Nog steeds vindt u een resource op label wanneer meer dan 10.000.  

<sup>3</sup>deze functies zijn niet langer vereist met Azure-resourcegroepen en Resource Manager.

<sup>4</sup>als u de limiet van 800 implementaties is bereikt, verwijdert u implementaties uit de geschiedenis die niet meer nodig zijn. Als u wilt verwijderen van abonnement niveau implementaties, gebruikt u [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) of [az-implementatie verwijderen](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).

> [!NOTE]
> Cores van virtuele machine hebben een regionale totale limiet. Ze hebben ook een limiet voor de regionale per grootte-serie, zoals Dv2- en f kan zijn. Deze limieten zijn afzonderlijk worden gehandhaafd. Neem bijvoorbeeld een abonnement met een limiet van 30 VM-cores voor US - oost, een limiet van 30 cores voor de A-serie en een limiet van 30 cores voor de D-serie. Dit abonnement kunt 30 A1-VM's of 30 D1-VM's of een combinatie van beide om meer dan een totaal van 30 cores niet te implementeren. Een voorbeeld van een combinatie is 10 A1-VM's en 20 D1-VM's.  
> <!-- -->
> 
> 

