---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 7e390e2134df02b0ca9c0d1752c3207aff7b9314
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279568"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| [Virtuele machines](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) per cloudservice<sup>1</sup> |50 |50 |
| Voer eindpunten per cloudservice<sup>2</sup> |150 |150 |

<sup>1</sup>virtuele machines die in Service Management (in plaats van Resource Manager) zijn gemaakt, worden automatisch opgeslagen in een cloudservice. U kunt meer virtuele machines toevoegen aan die service in de cloud voor taakverdeling en beschikbaarheid. 

<sup>2</sup>invoer-eindpunten kunt communicatie naar een virtuele machine van buiten de cloudservice van de virtuele machine. Virtuele machines in dezelfde cloudservice of virtuele netwerk automatisch met elkaar kan communiceren. Zie [uitleg over de instellingen van eindpunten aan een virtuele Machine](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
