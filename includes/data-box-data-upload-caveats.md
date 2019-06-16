---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: b159ec8620fa8c93e4917f73be9b9898e1b4fbcc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244602"
---
- Kopieer de bestanden rechtstreeks aan een van de precreated shares niet. U moet een map in de bestandsshare maken en kopieer vervolgens bestanden naar die map.
- Een map onder de *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* is een container. Bijvoorbeeld, containers worden gemaakt als *StorageAccount_BlockBlob/container* en *StorageAccount_PageBlob/container*.
- Elke gemaakt direct onder de map *StorageAccount_AzureFiles* wordt omgezet in een Azure-bestandsshare.
- Hebt u een bestaande Azure-object (zoals een blob of een bestand) in de cloud met dezelfde naam als het object dat wordt gekopieerd, overschrijft de gegevens in het bestand in de cloud.
- Alle bestanden die worden weggeschreven naar *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* shares respectievelijk als een blok-blobs en pagina-blob is geüpload.
- Azure blob-opslag biedt geen ondersteuning voor mappen. Als u een map onder de *StorageAccount_BlockBlob* map, en vervolgens virtuele mappen worden gemaakt in de blob-naam. Voor Azure Files, wordt de werkelijke mapstructuur behouden.
- Een directory-hiërarchie (zonder bestanden) gemaakt op basis van lege *StorageAccount_BlockBlob* en *StorageAccount_PageBlob* mappen is niet geüpload.
- Als er fouten optreden tijdens het uploaden van gegevens naar Azure, wordt een foutenlogboek gemaakt in het doelopslagaccount. Het pad naar dit foutenlogboek is beschikbaar wanneer het uploaden voltooid is en kunt u corrigerende maatregelen nemen in het logboek bekijken. Geen gegevens verwijderen uit de bron zonder de geüploade gegevens te verifiëren.
