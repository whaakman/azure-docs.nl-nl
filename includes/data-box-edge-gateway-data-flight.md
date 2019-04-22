---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: bea14544949f90290bf3878295b49f1e08be9eea
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684410"
---
Voor de gegevens-in-vlucht:

- Voor gegevens die tussen het apparaat en Azure gaat, wordt standaard TLS 1.2 gebruikt. Er is er geen terugval naar TLS 1.1 en eerdere versies. De communicatie met de agent wordt geblokkeerd als TLS 1.2 wordt niet ondersteund. De TLS 1.2 is ook vereist voor de portal en SDK-beheerbewerkingen.
- Wanneer de clients toegang het apparaat via de lokale webgebruikersinterface in een browser tot, wordt standaard TLS 1.2 wordt gebruikt als de standaard veilig protocol.

    - De aanbevolen procedure is het configureren van uw browser voor het gebruik van TLS 1.2.
    - Als de browser biedt geen ondersteuning voor TLS 1.2, kunt u TLS 1.1 of TLS 1.0.
- Als u wilt de gegevens te beschermen bij het kopiëren van uw gegevensservers, wordt u aangeraden dat u SMB 3.0 met codering gebruiken.
