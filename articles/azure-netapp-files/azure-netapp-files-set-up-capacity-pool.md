---
title: Een capaciteitspool instellen voor Azure NetApp Files | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een capaciteitspool instelt, zodat u hierin volumes kunt maken.
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
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010800"
---
# <a name="set-up-a-capacity-pool"></a>Een capaciteitspool instellen
Als u een capaciteitspool instelt, kunt u hierin volumes maken.  

## <a name="before-you-begin"></a>Voordat u begint 
U moet al een NetApp-account hebben gemaakt.   

[Een NetApp-account maken](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Stappen 

1. Ga naar de beheerblade voor uw NetApp-account en selecteer vervolgens **Capaciteitspools** in het navigatiedeelvenster.

2. Klik op **+ Pools toevoegen** om een nieuwe capaciteitspool te maken.   
    Het venster Nieuwe capaciteitspool wordt weergegeven.

3. Geef de volgende informatie op voor de nieuwe capaciteitspool:  
  * **Naam**  
    Geef de naam op voor de capaciteitspool.  
    De naam van de capaciteitspool moet uniek zijn voor elk NetApp-account.

  * **Serviceniveau**   
    In dit veld worden de doelprestaties voor de capaciteitspool weergegeven.  
    Momenteel is alleen het Premium-serviceniveau beschikbaar. 

  *  **Grootte**     
      Geef de grootte op van de capaciteitspool die u aanschaft.        
      De minimale grootte van de capaciteitspool is 4 TiB. U kunt een pool maken met een grootte die een veelvoud is van 4 TiB.   
      
      ![Nieuwe capaciteitspool](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen 

1. [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
2. [Exportbeleid voor een volume configureren (optioneel)](azure-netapp-files-configure-export-policy.md)

