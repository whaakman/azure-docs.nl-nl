---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176033"
---
## <a name="issue-custom-image-provisioning-errors"></a>Probleem: Aangepaste afbeelding. fouten bij het inrichten
Inrichting fouten optreden wanneer u uploaden of vastleggen van een gegeneraliseerde VM-installatiekopie als een gespecialiseerde VM-installatiekopie of vice versa. De oude zorgt ervoor dat een inrichting time-outfout en de laatste waarvan een inrichting fout. Voor het implementeren van uw aangepaste installatiekopie zonder fouten, moet u ervoor zorgen dat het type van de afbeelding wordt niet gewijzigd tijdens het vastleggen.

De volgende tabel geeft een lijst van mogelijke combinaties van afbeeldingen gegeneraliseerde en gespecialiseerde, het fouttype dat u tegenkomt en wat u moet doen om de fouten te herstellen.

