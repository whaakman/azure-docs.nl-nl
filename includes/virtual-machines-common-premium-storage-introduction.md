---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b1287f9c7e946c7b4d035b2ad6301947ffad3cea
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717118"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure premium storage: ontwerp voor hoge prestaties

In dit artikel bevat richtlijnen voor het bouwen van toepassingen met hoge prestaties met behulp van Azure Premium Storage. U kunt de instructies in dit document in combinatie met aanbevolen procedures voor prestaties van toepassing op de technologieën die worden gebruikt door uw toepassing gebruiken. Ter illustratie van de richtlijnen, hebben we met SQL Server op Premium-opslag als voorbeeld in dit document gebruikt.

Terwijl de prestaties van scenario's voor de opslaglaag in dit artikel behandelen we, moet u het niveau van de toepassing optimaliseren. Bijvoorbeeld, als u een SharePoint-Farm op Azure Premium Storage host, kunt u de SQL Server-voorbeelden uit dit artikel om te optimaliseren van de database-server. Bovendien het Optimaliseer de webserver en de toepassingsserver op de meeste prestaties van de SharePoint-Farm.

Dit artikel voor het antwoord volgen Veelgestelde vragen over het optimaliseren van prestaties van toepassingen op Azure Premium Storage

* Hoe kunt u voor het meten van prestaties van uw toepassing?  
* Waarom ziet u niet verwacht hoge prestaties?  
* Welke factoren van invloed zijn op de prestaties van uw toepassing op Premium Storage?  
* Hoe deze factoren van invloed op prestaties van uw toepassing op Premium Storage?  
* Hoe kunt u optimaliseren voor IOPS, bandbreedte en wachttijden?  

We hebben deze richtlijnen die specifiek voor Premium-opslag omdat workloads die worden uitgevoerd op de Premium-opslag zeer gevoelige prestaties zijn. Indien van toepassing zijn zijn er voorbeelden beschikbaar. U kunt sommige van deze richtlijnen ook toepassen op toepassingen die worden uitgevoerd op virtuele IaaS-machines met Standard Storage-schijven.