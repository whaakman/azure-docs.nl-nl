---
title: Een volume maken voor Azure NetApp Files | Microsoft Docs
description: Dit artikel beschrijft hoe u een volume kunt maken voor Azure NetApp Files.
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
ms.topic: get-started-article
ms.date: 12/17/2018
ms.author: b-juche
ms.openlocfilehash: 937ff9cf4c099f229df28070be07ba76339704e0
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583991"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Een volume maken voor Azure NetApp Files

Capaciteitsgebruik van een volume wordt in mindering gebracht op de ingerichte capaciteit van de pool.  U kunt meerdere volumes maken in een capaciteitspool, maar het totale capaciteitsverbruik van deze volumes mag niet groter zijn dan de grootte van de pool. 

## <a name="before-you-begin"></a>Voordat u begint 
U dient al een capaciteitspool te hebben ingesteld.   
[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)   
Er moet een subnet zijn gedelegeerd aan Azure NetApp Files.  
[Een subnet delegeren aan Azure NetApp Files](azure-netapp-files-delegate-subnet.md)


## <a name="steps"></a>Stappen 
1.  Klik in de blade Capaciteitspools beheren op de blade **Volumes**. 
2.  Klik op **+ Volume toevoegen** om een volume te maken.  
    Het venster Nieuw volume wordt nu weergegeven.

3.  Klik in het venster Nieuw volume op **Maken** en vul de volgende velden in:   
    * **Naam**      
        Geef de naam op voor het volume dat u wilt maken.   

        De naam moet uniek zijn binnen de resourcegroep. De naam moet minstens drie tekens bevatten.  U kunt hiervoor alle alfanumerieke tekens gebruiken.

    * **Bestandspad**  
        Geef het bestandspad op dat wordt gebruikt om het exportpad voor het nieuwe volume te maken. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.   
     
        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn.  

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  

        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. De Azure NetApp Files-service is alleen toegankelijk vanuit hetzelfde VNet, of vanuit een VNet in dezelfde regio als het volume via VNet-peering. Het volume is ook toegankelijk vanuit uw on-premises netwerk via ExpressRoute.   

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het opgegeven subnet moet zijn gedelegeerd aan Azure NetApp Files. 
        
        Als u geen subnet hebt gedelegeerd, kunt u klikken op **Nieuwe maken** op de pagina Een volume maken. Geef vervolgens op de pagina Subnet maken de subnetgegevens op, en selecteer **Microsoft.NetApp/volumes** om het subnet te delegeren aan Azure NetApp Files. In elk VNet kan er slechts één subnet aan Azure NetApp Files worden gedelegeerd.   
 
        ![Nieuw volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Subnet maken](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)


4.  Klik op **OK**. 
 
Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="next-steps"></a>Volgende stappen  
* [Exportbeleid voor een volume configureren (optioneel)](azure-netapp-files-configure-export-policy.md)
* Meer informatie over [Integratie van virtuele netwerken voor Azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)

