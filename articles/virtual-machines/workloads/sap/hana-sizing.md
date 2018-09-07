---
title: Grootte van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Grootte van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0d72a9ef592579b21d082111ab8ad00f8995de2
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028189"
---
# <a name="sizing"></a>Grootte aanpassen

Groottes voor HANA grote instantie is niet anders dan in het algemeen voor HANA formaat. Voor bestaande en systemen die u verplaatsen van andere RDBMS met HANA wilt, SAP biedt een aantal rapporten die worden uitgevoerd op uw bestaande SAP-systemen geïmplementeerd. Als de database is verplaatst naar HANA, worden deze rapporten Controleer de gegevens en berekenen geheugenvereisten voor de HANA-instantie. Lees de volgende SAP-opmerkingen voor meer informatie over het uitvoeren van deze rapporten en hun meest recente patches of versies te verkrijgen:

- [SAP-notitie #1793345 - grootte voor SAP-Suite op HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-notitie #1872170 - Suite op HANA en s/4 HANA sizing-rapport](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-notitie #2121330 - Veelgestelde vragen over: SAP BW op HANA formaat van rapport](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-notitie #1736976 - rapport van de grootte voor BW op HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-notitie #2296290 - rapport nieuwe grootte voor BW op HANA](https://launchpad.support.sap.com/#/notes/2296290)

Snel SAP-Sizer is voor implementaties van de voorafgaande beperkingen, beschikbaar voor het berekenen van geheugenvereisten van de implementatie van SAP-software boven op HANA.

Geheugenvereisten voor HANA verhogen naarmate het gegevensvolume toeneemt. Houd rekening met uw huidige geheugengebruik om te voorspellen wat er gebeurt in de toekomst zijn. Op basis van geheugenvereisten, kunt klikt u vervolgens u toewijzen uw vraag in een van de HANA grote instantie-SKU's.

**Volgende stappen**
- Raadpleeg [Onboarding-vereisten](hana-onboarding-requirements.md)