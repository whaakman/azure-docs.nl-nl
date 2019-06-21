---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176672"
---
Voor gegevens tijdens de overdracht:

- Standaard TLS 1.2 wordt gebruikt voor gegevens die worden uitgewisseld tussen het apparaat en Azure. Er is er geen terugval naar TLS 1.1 en eerdere versies. Communicatie met de agent wordt geblokkeerd als TLS 1.2 wordt niet ondersteund. TLS 1.2 is ook vereist voor de portal en het beheer van de SDK.
- Wanneer clients toegang krijgen uw apparaat via de lokale webgebruikersinterface van een browser tot, wordt standaard TLS 1.2 wordt gebruikt als de standaard veilig protocol.

    - De aanbevolen procedure is het configureren van uw browser voor het gebruik van TLS 1.2.
    - Als de browser biedt geen ondersteuning voor TLS 1.2, kunt u TLS 1.1 of TLS 1.0.
- U wordt aangeraden dat u SMB 3.0 met codering gebruiken om gegevens te beschermen bij het kopiëren van uw gegevensservers.
