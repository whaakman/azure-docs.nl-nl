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
ms.openlocfilehash: 47ef014f31c628fed9d7b2b005e67e8138d3e7e9
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
ms.locfileid: "29743360"
---
## <a name="terminology"></a>Terminologie

Een Marketplace-installatiekopie in Azure heeft de volgende kenmerken:

* **Publisher** -de organisatie die de installatiekopie heeft gemaakt. Voorbeelden: Canonieke, legitieme Microsoft Windows Server
* **Bieden** -naam van een groep verwante installatiekopieën die zijn gemaakt door een uitgever. Voorbeelden: Ubuntu Server, Windows Server
* **SKU** : een exemplaar van een aanbieding, zoals een grote release van een distributiepunt. Voorbeelden: 16.04-TNS, 2016 Datacenter
* **Versie** -het versienummer van een installatiekopie van het SKU. 

Voor een Marketplace-installatiekopie identificeren wanneer u een virtuele machine programmatisch implementeren, deze waarden als parameters afzonderlijk opgeven of een aantal hulpprogramma's voor de installatiekopie van het accepteren *URN*. Deze waarden, gescheiden door het teken van de dubbele punt (:) worden gecombineerd voor de URN: *Publisher*:*bieden*:*Sku*:*versie*. In een URN kunt u vervangen het versienummer met 'nieuwste', die de meest recente versie van de afbeelding wordt geselecteerd. 

Als de installatiekopie-uitgever aanvullende licentievoorwaarden en inkoop voorwaarden biedt, moet u deze voorwaarden akkoord gaan en programmatische implementatie inschakelen. Ook moet u opgeven *aanschaffen abonnement* parameters bij het implementeren van een virtuele machine via een programma. Zie [implementeren een installatiekopie met de voorwaarden van de Marketplace](#deploy-an-image-with-marketplace-terms).