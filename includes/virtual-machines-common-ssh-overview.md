---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 12/21/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: bca78e2963f19b60071b1b27c8dc65c76818e10e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54029289"
---
## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels

[SSH](https://www.ssh.com/ssh/) is een versleutelde verbinding-protocol waarmee veilige aanmeldingen via niet-beveiligde verbindingen. SSH is het standaardprotocol voor de verbinding voor virtuele Linux-machines die worden gehost in Azure. Hoewel SSH zelf voorziet in een versleutelde verbinding, maakt met behulp van wachtwoorden met SSH-verbindingen nog steeds de virtuele machine kwetsbaar voor brute-force-aanvallen of te raden van wachtwoorden. Een veiliger en voorkeurs-methode van de verbinding te maken met een virtuele machine via SSH is met behulp van een openbaar / persoonlijk sleutelpaar, ook wel bekend als *SSH-sleutels*. 

* De *openbare sleutel* op uw Linux-VM of een andere service die u wilt gebruiken met openbare-sleutelcryptografie is geplaatst.

* De *privésleutel* blijft op uw lokale systeem. Houd deze privésleutel geheim. Deel deze niet.

Als u een SSH-client gebruiken om verbinding maken met uw Linux-VM (met de openbare sleutel), de externe virtuele machine, test de client om te controleren of deze beschikt over de persoonlijke sleutel. Als de client de persoonlijke sleutel heeft, krijgt het toegang tot de virtuele machine. 

Afhankelijk van het beveiligingsbeleid van uw organisatie, kunt u een enkel openbaar / persoonlijk sleutelpaar voor toegang tot meerdere Azure-VM's en services opnieuw gebruiken. U hoeft niet een afzonderlijke combinatie van sleutel voor elke virtuele machine of service die u wilt openen. 

Uw openbare sleutel kan worden gedeeld met iedereen, maar alleen u (of uw lokale beveiligingsinfrastructuur) moet beschikken over uw persoonlijke sleutel.