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
ms.openlocfilehash: 50d71a3967e61e5d531f4bfeae3582b56230a0d2
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56825539"
---
## <a name="terminology"></a>Terminologie

Een Marketplace-installatiekopie in Azure heeft de volgende kenmerken:

* **Publisher**: De organisatie waarvoor de installatiekopie is gemaakt. Voorbeelden: Canonical, MicrosoftWindowsServer
* **Bieden**: De naam van een groep gerelateerde afbeeldingen gemaakt door een uitgever. Voorbeelden: UbuntuServer, WindowsServer
* **SKU**: Een exemplaar van een aanbieding, zoals een grote release van een distributie. Voorbeelden: 18.04-LTS, 2019 Datacenter
* **Versie**: Het versienummer van de SKU van een afbeelding. 

Geef deze waarden afzonderlijk als parameters voor het identificeren van een Marketplace-installatiekopie wanneer u een virtuele machine via een programma implementeert. Sommige hulpprogramma's voor een afbeelding accepteren *URN*, die deze waarden, gescheiden door het teken dubbele punt (:) worden gecombineerd: *Uitgever*:*bieden*:*Sku*:*versie*. In een URN, kunt u vervangen het versienummer met 'nieuwste', de meest recente versie van de installatiekopie te selecteren. 

Als de installatiekopie-uitgever biedt een extra licentie en Aankoopvoorwaarden, moet u deze voorwaarden akkoord en programmatische implementatie inschakelen. U moet ook opgeven *abonnement aanschaffen* parameters bij het implementeren van een virtuele machine via een programma. Zie [een installatiekopie implementeren met de voorwaarden van Marketplace](#deploy-an-image-with-marketplace-terms).
