---
title: Exporteren-beleid voor een NFS-volume met behulp van Azure NetApp bestanden configureren | Microsoft Docs
description: Hierin wordt beschreven hoe het configureren van beleid voor het beheren van toegang tot een NFS-volume met behulp van Azure NetApp bestanden exporteren
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
ms.date: 03/20/2019
ms.author: b-juche
ms.openlocfilehash: 8cda5921a1aec86d28beabbd9cea5b07a203a0e8
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401487"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>Exporteren-beleid voor een NFS-volume configureren

U kunt desgewenst exportbeleid configureren om de toegang tot een Azure NetApp Files-volume te beheren. Exporteren-beleid is ondersteuning voor NFS-volumes. 

## <a name="steps"></a>Stappen 

1.  Klik op de blade **Exportbeleid maken** in de blade Volume beheren. 

2.  Geef voor de volgende velden informatie op om een exportbeleidsregel te maken:   
    *  **Index**   
        Geef het indexnummer voor de regel op.  
        Een exportbeleid kan uit maximaal vijf regels bestaan. Regels worden geëvalueerd volgens hun volgorde in de lijst met indexnummers. Regels met lagere indexnummers worden als eerste geëvalueerd. De regel met indexnummer 1 wordt bijvoorbeeld geëvalueerd vóór de regel met indexnummer 2. 

    * **Toegestane clients**   
        Geef de waarde in een van de volgende indelingen op:  
        * IPv4-adres, bijvoorbeeld `10.1.12.24` 
        * IPv4-adres met een subnetmasker dat wordt uitgedrukt als een aantal bits, bijvoorbeeld `10.1.12.10/4`

    * **Toegang**  
        Selecteer een van de volgende toegangstypen:  
        * Geen toegang 
        * Lezen en schrijven
        * Alleen-lezen

    * **Protocollen**   
        Geef het protocol op dat voor het exportbeleid moet worden gebruikt.   
        Beleid voor het exporteren van Azure NetApp bestanden ondersteunt momenteel alleen NFSv3.

    ![Exportbeleid](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Volgende stappen 
* [Volumes beheren](azure-netapp-files-manage-volumes.md)
* [Een volume voor virtuele machines koppelen of ontkoppelen](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Momentopnamen beheren](azure-netapp-files-manage-snapshots.md)
