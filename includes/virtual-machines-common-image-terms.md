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
ms.openlocfilehash: 6c7a9857f6d6d57dc9e314bcb1ef848a326b7ed2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437102"
---
## <a name="terminology"></a>Terminologie

Een Marketplace-installatiekopie in Azure heeft de volgende kenmerken:

* **Uitgever**: de organisatie die de installatiekopie gemaakt. Voorbeelden: Canonical, MicrosoftWindowsServer
* **Bieden**: de naam van een groep gerelateerde afbeeldingen gemaakt door een uitgever. Voorbeelden: Ubuntu, WindowsServer
* **SKU**: een exemplaar van een aanbieding, zoals een grote release van een distributiepunt. Voorbeelden: 16.04-LTS, 2016-Datacenter
* **Versie**: het versienummer van een installatiekopie-SKU. 

Geef deze waarden afzonderlijk als parameters voor het identificeren van een Marketplace-installatiekopie wanneer u een virtuele machine via een programma implementeert. Sommige hulpprogramma's voor een afbeelding accepteren *URN*, die deze waarden, gescheiden door het teken dubbele punt (:) combineert: *Publisher*:*bieden*:*Sku*: *Versie*. In een URN, kunt u vervangen het versienummer met 'nieuwste', de meest recente versie van de installatiekopie te selecteren. 

Als de installatiekopie-uitgever biedt een extra licentie en Aankoopvoorwaarden, moet u deze voorwaarden akkoord en programmatische implementatie inschakelen. U moet ook opgeven *abonnement aanschaffen* parameters bij het implementeren van een virtuele machine via een programma. Zie [een installatiekopie implementeren met de voorwaarden van Marketplace](#deploy-an-image-with-marketplace-terms).