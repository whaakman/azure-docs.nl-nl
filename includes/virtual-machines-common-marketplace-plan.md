---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4d2235eaea457c89d01a632afa5dd5a862bec344
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437093"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Een installatiekopie implementeren met de voorwaarden van Marketplace

Enkele VM-installatiekopieën in de Azure Marketplace zijn extra licentie en aankoop voorwaarden die u accepteren moet voordat u ze via een programma kunt implementeren.  

Voor het implementeren van een virtuele machine uit een dergelijke afbeelding, moet u akkoord met de voorwaarden van de afbeelding zowel programmatische implementatie inschakelen. U hoeft alleen te doen dit maar eenmaal per abonnement. Daarna wordt telkens wanneer u een virtuele machine implementeren via een programma uit de afbeelding die u ook moet om op te geven *abonnement aanschaffen* parameters.

De volgende secties tonen hoe u:

* Uitzoeken of een Marketplace-installatiekopie aanvullende licentievoorwaarden heeft 
* Accepteer de voorwaarden via een programma
* Aankoop planning parameters opgeven wanneer u een virtuele machine via een programma implementeren

