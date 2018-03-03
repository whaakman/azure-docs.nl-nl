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
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Een installatiekopie met Marketplace voorwaarden implementeren

Bepaalde VM-installatiekopieën in Azure Marketplace hebben aanvullende licentievoorwaarden en inkoop termen die u accepteren moet voordat u programmatisch kunt implementeren.  

Voor het implementeren van een virtuele machine van een dergelijke installatiekopie, moet u akkoord met de voorwaarden van de afbeelding en programmatische implementatie inschakelen. U hoeft alleen te doen dit één keer in uw abonnement. Vervolgens telkens wanneer u een virtuele machine via een programma van de installatiekopie implementeren moet u ook opgeven *aanschaffen abonnement* parameters.

De volgende secties tonen hoe:

* Uitzoeken of een Marketplace-installatiekopie aanvullende licentievoorwaarden heeft 
* Programmatisch de voorwaarden accepteren
* Aankoop plan parameters opgeven wanneer u een virtuele machine programmatisch implementeren

