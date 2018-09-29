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
ms.openlocfilehash: 0363b719e2587226282257e19e58e3c4e55c2219
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454382"
---
## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels

SSH is een versleutelde verbinding-protocol waarmee veilige aanmeldingen via niet-beveiligde verbindingen. SSH is het standaardprotocol voor de verbinding voor virtuele Linux-machines die worden gehost in Azure. Hoewel SSH zelf voorziet in een versleutelde verbinding, maakt met behulp van wachtwoorden met SSH-verbindingen nog steeds de virtuele machine kwetsbaar voor brute-force-aanvallen of te raden van wachtwoorden. Een veiliger en voorkeurs-methode van de verbinding te maken met een virtuele machine via SSH is met behulp van een openbaar / persoonlijk sleutelpaar, ook wel bekend als *SSH-sleutels*. 

* De *openbare sleutel* op uw Linux-VM of een andere service die u wilt gebruiken met openbare-sleutelcryptografie is geplaatst.

* De *privésleutel* biedt u voor uw Linux-VM wanneer u een SSH-verbinding om uw identiteit te verifiëren. Houd deze privésleutel geheim. Deel deze niet.

Afhankelijk van het beveiligingsbeleid van uw organisatie, kunt u een enkel openbaar / persoonlijk sleutelpaar voor toegang tot meerdere Azure-VM's en services opnieuw gebruiken. U hoeft niet een afzonderlijke combinatie van sleutel voor elke virtuele machine of service die u wilt openen. 

Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) moet beschikken over uw persoonlijke sleutel.