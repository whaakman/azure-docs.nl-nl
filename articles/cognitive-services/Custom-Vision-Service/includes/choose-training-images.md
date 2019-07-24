---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ee2c1fd1fc1cad07b14a2c99318be20be30db9c5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423584"
---
We raden u ten zeerste aan om ten minste 30 afbeeldingen per tag in de eerste Trainingsset te gebruiken. U wilt ook een paar extra installatie kopieën verzamelen om uw model te testen zodra het is getraind.

Gebruik installatie kopieën met visualisatie om uw model effectief te trainen. Selecteer installatie kopieën die variëren per:
* camera hoek
* verlichting
* achtergrondbitmap
* visuele stijl
* afzonderlijke/gegroepeerde items
* size
* type

Zorg er bovendien voor dat al uw trainings beeldafbeeldingen voldoen aan de volgende criteria:
* jpg-, PNG-of BMP-indeling
* niet groter dan 6 MB grootte (4 MB voor Voorspellings installatie kopieën)
* Maxi maal 256 pixels op de kortste rand; afbeeldingen die korter zijn dan deze worden automatisch omhoog geschaald door de Custom Vision Service