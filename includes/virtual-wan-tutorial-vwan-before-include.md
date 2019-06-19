---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 66e21aefa5648262ca2fcc61501905f725ac0e4b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175939"
---
Controleer voordat u met de configuratie begint of u aan de volgende criteria hebt voldaan:

* Als u al een virtueel netwerk hebt waarmee u verbinding wilt maken, dient u te controleren of geen van de subnetten van uw on-premises netwerk overlappen met de virtuele netwerken waarmee u verbinding wilt maken. Uw virtuele netwerk heeft geen gateway-subnet nodig en mag geen virtuele netwerkgateways hebben. Als u geen virtueel netwerk hebt, kunt u er een maken met behulp van de stappen in dit artikel.
* Zorg dat u een IP-adresbereik krijgt voor uw hubregio. De hub is een virtueel netwerk en het adresbereik dat u voor de hubregio opgeeft mag niet overlappen met een van de bestaande virtuele netwerken waarmee u verbinding wilt maken. Dit bereik mag ook niet overlappen met de adresbereiken waarmee u on-premises verbinding wilt maken. Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.