---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 09/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: f9974aae1e5996ffeaa6cde690a5e10ccba4cc32
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019009"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Een installatiekopie implementeren met de voorwaarden van Marketplace

Bepaalde VM-installatiekopieën in de Azure Marketplace zijn extra licentie en aankoop voorwaarden die u accepteren moet voordat u ze via een programma kunt implementeren.  

Voor het implementeren van een virtuele machine uit een dergelijke afbeelding, moet u akkoord met de voorwaarden van de installatiekopie en programmatische implementatie inschakelen. U hoeft te doen dit één keer in uw abonnement. Vervolgens, telkens wanneer u een virtuele machine via een programma uit de afbeelding, implementeert u moet ook opgeven *abonnement aanschaffen* parameters.

De volgende secties tonen hoe u:

* Uitzoeken of een Marketplace-installatiekopie aanvullende licentievoorwaarden heeft 
* Accepteer de voorwaarden via een programma
* Aankoop planning parameters opgeven wanneer u een virtuele machine via een programma implementeren

