---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a3c5290eb0179fe5842c495c2e08f22580d02bda
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38943351"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Een installatiekopie implementeren met de voorwaarden van Marketplace

Bepaalde VM-installatiekopieën in de Azure Marketplace zijn extra licentie en aankoop voorwaarden die u accepteren moet voordat u ze via een programma kunt implementeren.  

Voor het implementeren van een virtuele machine uit een dergelijke afbeelding, moet u akkoord met de voorwaarden van de installatiekopie en programmatische implementatie inschakelen. U hoeft te doen dit één keer in uw abonnement. Vervolgens, telkens wanneer u een virtuele machine via een programma uit de afbeelding, implementeert u moet ook opgeven *abonnement aanschaffen* parameters.

De volgende secties tonen hoe u:

* Uitzoeken of een Marketplace-installatiekopie aanvullende licentievoorwaarden heeft 
* Accepteer de voorwaarden via een programma
* Aankoop planning parameters opgeven wanneer u een virtuele machine via een programma implementeren

