---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 919582745d166cf8b3f3937f9bac4fc0dc1fe64f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels

SSH is een versleutelde verbindingsprotocol waarmee beveiligde aanmeldingen via niet-beveiligde verbindingen. Het is het standaardprotocol voor de verbinding voor virtuele Linux-machines gehost in Azure. Maar SSH zelf een versleutelde verbinding biedt, wordt met wachtwoorden met SSH-verbindingen nog steeds de virtuele machine kwetsbaar voor brute-force-aanvallen of te raden van wachtwoorden. Een veiliger en gewenste methode voor het verbinding maken met een virtuele machine via SSH is via een openbaar / persoonlijk sleutelpaar, ook wel bekend als SSH-sleutels. 

* De *openbare sleutel* uw Linux-VM of een andere service die u wilt gebruiken met cryptografie met openbare sleutels is geplaatst.

* De *persoonlijke sleutel* is wat u aanwezig zijn voor uw Linux-VM wanneer u een SSH-verbinding om uw identiteit te verifiëren. Houd deze privésleutel geheim. Deel deze niet.

Afhankelijk van het beveiligingsbeleid van uw organisatie, kunt u een enkel openbaar / persoonlijk sleutelpaar voor toegang tot meerdere virtuele Azure-machines en services opnieuw gebruiken. U hoeft geen afzonderlijke paar sleutels niet voor elke virtuele machine of service die u toegang wilt. 

Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) beschikt over uw persoonlijke sleutel.