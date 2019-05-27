---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165376"
---
Vervolgens als alle servers op het cluster wordt uitgevoerd Windows Server 2008 R2 of Windows Server 2012, moet u controleren of de hotfix [KB2854082](https://support.microsoft.com/kb/2854082) is geïnstalleerd op elk van de on-premises servers of virtuele Azure-machines die deel van het cluster uitmaken. Een server of virtuele machine die in het cluster, maar niet in de beschikbaarheidsgroep, moet u ook deze hotfix geïnstalleerd hebben.

Downloaden in de extern bureaublad-sessiehost voor elk van de clusterknooppunten, [KB2854082](https://support.microsoft.com/kb/2854082) naar een lokale map. Vervolgens installeert u de hotfix op elk clusterknooppunt sequentieel worden verwerkt. Als de cluster-service wordt momenteel uitgevoerd op het clusterknooppunt bevindt, wordt de server opnieuw opgestart aan het einde van de hotfixinstallatie.

> [!WARNING]
> De clusterservice wordt gestopt of de server opnieuw wordt opgestart invloed heeft op de status van het quorum van het cluster en de beschikbaarheidsgroep en dit kan ertoe leiden dat uw cluster offline te gaan. Als u wilt behouden de hoge beschikbaarheid van uw cluster tijdens de installatie, controleert u of:
> 
> * Het cluster zich in de gezondheid van optimale quorum. 
> * Voordat u de hotfix op een willekeurig knooppunt installeert, wordt alle clusterknooppunten online zijn.
> * Voordat u de hotfix op een ander knooppunt in het cluster installeert, kunt u de installatie van de hotfix voor uitvoering op één knooppunt, met inbegrip van de server volledig opnieuw te starten.
> 
> 

