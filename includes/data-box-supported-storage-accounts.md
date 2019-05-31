---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: bc156b8c18f46cccf6fc775b82f76383b8c43861
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242138"
---
Hier volgt een lijst van de ondersteunde opslagaccounts en de opslagtypen voor de Data Box-apparaat. Zie voor een volledige lijst van alle verschillende soorten opslagaccounts en de bijbehorende volledige mogelijkheden [typen opslagaccounts](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Storage-account / opslagtypen ondersteund** | **Blok-blob** |**Pagina-blobs*** |**Met Azure files** |**Opmerkingen**|
| --- | --- | -- | -- | -- |
| Klassieke Standard | J | J | J |
| Standaard voor algemeen gebruik v1  | J | J | J | Zowel warme als koude worden ondersteund.|
| Premium voor algemeen gebruik v1  |  | J| | |
| Algemeen gebruik v2 Standard  | J | J | J | Zowel warme als koude worden ondersteund.|
| Algemeen gebruik v2 Premium  |  |J | | |
| BLOB-opslag Standard |J | | |Zowel warme als koude worden ondersteund. |

\* *-Gegevens geüpload naar de pagina-blobs moet 512 bytes uitgelijnd, zoals VHD's.*

>[!NOTE]
> Azure Data Lake Storage Gen 2-accounts worden niet ondersteund.
