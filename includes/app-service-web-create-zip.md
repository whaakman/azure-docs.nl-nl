---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d804cb310a8638713cabf76c2f4192a0e4d0f43d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65914282"
---
## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

Zorg ervoor dat u zich nog steeds in de hoofdmap van het voorbeeldproject bevindt. Maak een ZIP-archief van alle bestanden in uw project. De volgende opdracht maakt gebruik van het standaardhulpprogramma in de terminal:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

U kunt dit ZIP-bestand later uploaden naar Azure en dit implementeren in App Service.