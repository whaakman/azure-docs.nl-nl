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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38941837"
---
## <a name="terminology"></a>Terminologie

Een Marketplace-installatiekopie in Azure heeft de volgende kenmerken:

* **Uitgever** -de organisatie die de installatiekopie gemaakt. Voorbeelden: Canonieke, MicrosoftWindowsServer
* **Bieden** -naam van een groep gerelateerde afbeeldingen gemaakt door een uitgever. Voorbeelden: Ubuntu-Server, WindowsServer
* **SKU** : een exemplaar van een aanbieding, zoals een grote release van een distributiepunt. Voorbeelden: 16.04-LTS, 2016-Datacenter
* **Versie** -het versienummer van een installatiekopie-SKU. 

Deze waarden afzonderlijk als parameters opgeven voor het identificeren van een Marketplace-installatiekopie wanneer u een virtuele machine via een programma implementeert, of sommige hulpprogramma's voor de installatiekopie van het accepteren *URN*. De URN combineert deze waarden, gescheiden door het teken dubbele punt (:): *Publisher*:*bieden*:*Sku*:*versie*. In een URN, kunt u vervangen het versienummer met 'nieuwste', de meest recente versie van de installatiekopie te selecteren. 

Als de installatiekopie-uitgever biedt een extra licentie en Aankoopvoorwaarden, moet u deze voorwaarden akkoord en programmatische implementatie inschakelen. U moet ook opgeven *abonnement aanschaffen* parameters bij het implementeren van een virtuele machine via een programma. Zie [een installatiekopie implementeren met de voorwaarden van Marketplace](#deploy-an-image-with-marketplace-terms).