---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 083d59a2d8720a08abc7ea8998c1fbe048db3f28
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553639"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| [Virtuele machines](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) per cloudservice<sup>1</sup> |50 |50 |
| Voer eindpunten per cloudservice<sup>2</sup> |150 |150 |

<sup>1</sup>virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel in plaats van Azure Resource Manager worden automatisch opgeslagen in een cloudservice. U kunt meer virtuele machines toevoegen aan die service in de cloud voor taakverdeling en beschikbaarheid. 

<sup>2</sup>invoer-eindpunten kunt communicatie naar een virtuele machine van buiten de cloudservice van de virtuele machine. Virtuele machines in dezelfde cloudservice of virtuele netwerk automatisch met elkaar kan communiceren. Zie voor meer informatie, [over het instellen van eindpunten aan een virtuele machine](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
