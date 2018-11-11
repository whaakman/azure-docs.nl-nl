---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/08/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: cff3d7bfb89d5b03f986da32edc148efcfb7e7bd
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51506302"
---
## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels

SSH is een versleutelde verbinding-protocol waarmee veilige aanmeldingen via niet-beveiligde verbindingen. SSH is het standaardprotocol voor de verbinding voor virtuele Linux-machines die worden gehost in Azure. Hoewel SSH zelf voorziet in een versleutelde verbinding, maakt met behulp van wachtwoorden met SSH-verbindingen nog steeds de virtuele machine kwetsbaar voor brute-force-aanvallen of te raden van wachtwoorden. Een veiliger en voorkeurs-methode van de verbinding te maken met een virtuele machine via SSH is met behulp van een openbaar / persoonlijk sleutelpaar, ook wel bekend als *SSH-sleutels*. 

* De *openbare sleutel* op uw Linux-VM of een andere service die u wilt gebruiken met openbare-sleutelcryptografie is geplaatst.

* De *privésleutel* op uw lokale systeem wordt gebruikt door een SSH-client voor uw identiteit te verifiëren wanneer u verbinding met uw Linux-VM maakt. Houd deze privésleutel geheim. Deel deze niet.

Afhankelijk van het beveiligingsbeleid van uw organisatie, kunt u een enkel openbaar / persoonlijk sleutelpaar voor toegang tot meerdere Azure-VM's en services opnieuw gebruiken. U hoeft niet een afzonderlijke combinatie van sleutel voor elke virtuele machine of service die u wilt openen. 

Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) moet beschikken over uw persoonlijke sleutel.