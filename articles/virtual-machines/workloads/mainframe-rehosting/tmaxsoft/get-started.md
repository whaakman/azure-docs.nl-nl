---
title: Aan de slag met TmaxSoft OpenFrame op Azure Virtual Machines
description: Rehost uw IBM z/OS mainframe-workloads op Azure Virtual Machines (VM's) met behulp van TmaxSoft OpenFrame omgeving.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 408e0166e52af9efd3d4c64f1b29bddcfc1cca4c
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896280"
---
# <a name="get-started-with-tmaxsoft-openframe-on-azure"></a>Aan de slag met TmaxSoft OpenFrame op Azure

Uw bestaande mainframe-activa nemen en ze te verplaatsen naar Microsoft Azure met behulp van TmaxSoft OpenFrame. Deze populaire rehosting oplossing maakt een emulatieomgeving op Azure zodat u snel om toepassingen te migreren. Er is geen formatteren is vereist.

## <a name="openframe-rehosting-environment"></a>OpenFrame opnieuw hosten omgeving

Instellen van een omgeving OpenFrame op Azure voor ontwikkeling, demo's, tests of productie-workloads. Zoals in de volgende afbeelding wordt weergegeven, bevat OpenFrame meerdere onderdelen die de mainframe-emulatie-omgeving in Azure maken. Bijvoorbeeld vervangt OpenFrame onlineservices de mainframe-middleware, zoals IBM klant informatie besturingselement System (CICS). OpenFrame Batch, vervangt met de component TJES, u de IBM-mainframes taak post subsysteem (JES). 

![OpenFrame opnieuw hosten proces](media/openframe-01.png)

> [!NOTE]
> Als u wilt de OpenFrame-omgeving op Azure uitvoert, moet u een geldige productcode licentie of proeflicentie TmaxSoft hebben.

## <a name="openframe-components"></a>OpenFrame onderdelen

De volgende componenten maken deel uit van de omgeving OpenFrame op Azure:

- **Hulpprogramma voor migratie van** met inbegrip van OFMiner, een oplossing die analyseert de activa mainframes en ze vervolgens naar Azure migreert.
- **Compilers**, met inbegrip van OFCOBOL, een compiler dat wordt geïnterpreteerd programma's van de mainframe-COBOL; OFPLI, welke van de mainframe PL interpreteert / ik programma's; en OFASM, een compiler die van de mainframe-assembler programma's worden geïnterpreteerd.
- **Front-end** onderdelen, inclusief Java Enterprise gebruiker-oplossing (JEUS), een webserver van de toepassing die is gecertificeerd voor Java Enterprise Edition 6.OFGW en het onderdeel OpenFrame gateway waarmee een listener 3270.
- **Toepassing** omgeving. OpenFrame basis is het middleware die het hele systeem beheert. OpenFrame Server Type C (OSC) vervangt de middleware en IBM CICS van de mainframe.
- **Relationele database**, zoals Tibero (weergegeven), Oracle Database, Microsoft SQL Server, IBM Db2 of MySQL. De toepassingen OpenFrame gebruiken Open Database Connectivity (ODBC)-protocol om te communiceren met de database.
- **Beveiliging** via TACF, een servicemodule waarmee de gebruikerstoegang tot systemen en bronnen. 
- **OFManager** is een oplossing die van OpenFrame uitvoeren en beheren van functies in de web-omgeving biedt.

![OpenFrame-architectuur](media/openframe-02.png)

## <a name="next-steps"></a>Volgende stappen

- [TmaxSoft OpenFrame installeren op Azure](./install-openframe-azure.md)
