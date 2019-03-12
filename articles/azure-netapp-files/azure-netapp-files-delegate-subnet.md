---
title: Een subnet delegeren aan Azure NetApp Files | Microsoft Docs
description: Hierin wordt beschreven hoe u een subnet kunt delegeren aan Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: 6c1a6bf4e7042c28239f57af6b39c0822b63b5e8
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768073"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Een subnet delegeren aan Azure NetApp Files 

U dient een subnet te delegeren aan Azure NetApp Files.   Als u een volume maakt, dient u het gedelegeerde subnet op te geven.

## <a name="about-this-task"></a>Over deze taak
* De wizard voor het maken van een nieuw subnet gaat standaard over in een /24-netwerkmasker, dat 251 beschikbare IP-adressen onderhoudt. Een /28-netwerkmasker, dat 16 bruikbare IP-adressen onderhoudt, is voldoende voor de service.
* U kunt geen netwerkbeveiligingsgroep of service-eindpunt in het gedelegeerde subnet toewijzen. Als u dit doet, mislukt het delegeren van het subnet.
* In elk Azure-VNet (virtueel netwerk) kan slechts één subnet worden gedelegeerd aan Azure NetApp Files.
* Toegang tot een volume vanaf een virtueel netwerk waarvoor een peer is ingesteld, wordt niet ondersteund.

## <a name="steps"></a>Stappen 
1.  Ga in de Azure-portal naar de blade **Virtuele netwerken** en selecteer het virtuele netwerk dat u voor Azure NetApp Files wilt gebruiken.    

1. Selecteer **Subnetten** in de blade Virtueel netwerk en klik op de knop **+Subnet**. 

1. Maak een nieuw subnet dat u gebruikt voor Azure NetApp Files door de volgende verplichte velden in te vullen op de pagina Subnet toevoegen:
    * **Naam**: geef de naam van het subnet op.
    * **Adresbereik**: geef het IP-adresbereik op.
    * **Delegatie van subnet**: selecteer **Microsoft.NetApp/volumes**. 

      ![Delegatie van subnet](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
U kunt ook een subnet maken en delegeren als u [een volume maakt voor Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Volgende stappen  
* [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
* Meer informatie over [Integratie van virtuele netwerken voor Azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


