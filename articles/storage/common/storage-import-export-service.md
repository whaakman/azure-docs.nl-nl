---
title: Gebruik Azure Import/Export voor het overbrengen van gegevens van en naar Azure Storage | Microsoft Docs
description: Informatie over het maken van importeren en exporteren van taken in de Azure-portal voor het overbrengen van gegevens van en naar Azure Storage.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 83ba437e699eb150e86e6c89e478377394966419
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809539"
---
# <a name="what-is-azure-importexport-service"></a>Wat is Azure Import/Export-service?

Azure Import/Export-service wordt gebruikt voor het importeren van grote hoeveelheden gegevens veilig tot Azure Blob storage en Azure-bestanden door de back-upfunctie schijven naar een Azure-datacenter. Deze service kan ook worden gebruikt voor de overdracht van gegevens uit Azure Blob storage in schijfstations en verzenden naar uw lokale sites. Gegevens uit een of meer schijven kunnen worden geïmporteerd naar Azure Blob storage of Azure-bestanden. 

## <a name="azure-importexport-usecases"></a>Azure Import/Export usecases

Overweeg het gebruik van Azure Import/Export-service wanneer uploaden of downloaden van gegevens via het netwerk te langzaam is of meer netwerkbandbreedte ophalen kostbaar is. Gebruik deze service in de volgende scenario's:

* **Gegevensmigratie naar de cloud**: grote hoeveelheden gegevens snel verplaatsen naar Azure en voordelige wijze.
* **Inhoudsdistributie**: snel gegevens verzenden naar uw klant-sites.
* **Back-up**: uitvoeren van back-ups van uw on-premises gegevens op te slaan in Azure Storage.
* **Gegevensherstel**: grote hoeveelheid gegevens die zijn opgeslagen in de opslagruimte te herstellen en bezorgd bij uw on-premises locatie.

## <a name="importexport-components"></a>Onderdelen voor importeren/exporteren

Import/Export-service maakt gebruik van de volgende onderdelen:

- **Import/Export**service: deze beschikbaar zijn in Azure portal service helpt bij de gebruiker maken en bijhouden importeren en exporteren van taken.  

- **WAImportExport hulpprogramma**: dit is een opdrachtregelprogramma waarmee het volgende doet: 
    - Bereidt de stations die worden geleverd voor importeren.
    - Vereenvoudigt het kopiëren van uw gegevens naar het station.
    - Versleutelt de gegevens op het station met BitLocker.
    - Genereert station journaal bestanden die worden gebruikt tijdens het maken van de import.
    - Aantallen stations die nodig zijn voor exporteren wordt geïdentificeerd.

    Dit hulpprogramma is beschikbaar in twee versies, versie 1 en 2. U wordt aangeraden dat u gebruikt:

    - Versie 1 voor importeren/exporteren in Azure Blob-opslag. 
    - Versie 2 voor het importeren van gegevens in Azure files.

    Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssysteem. Voor specifieke OS-versies die worden ondersteund, gaat u naar [Azure Import/Export vereisten](storage-import-export-requirements.md#supported-operating-systems).

- **Schijven**: U kunt SSD-schijven (SSD's) kan worden verzonden of harde schijven (HDD's) naar het Azure-datacenter. Bij het maken van een import-taak verzenden harde schijven met uw gegevens. Bij het maken van een taak voor het exporteren, moet u leeg schijven naar het Azure datacenter verzenden. Voor specifieke schijftypen, gaat u naar [schijftypen ondersteund](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>Hoe werkt voor importeren/exporteren?

Azure Import/Export-service kan de overdracht van gegevens in Azure Blobs en Azure-bestanden door taken te maken. Azure-portal of REST-API van Azure Resource Manager gebruiken voor het maken van taken. Elke taak is gekoppeld aan een enkele storage-account. 

De taken worden importeren of exporteren van taken. Een import-taak kunt u gegevens importeren in Azure Blobs of Azure files terwijl de exporttaak toestaat dat gegevens uit Azure Blobs worden geëxporteerd. Voor een import-taak moet u schijven met uw gegevens verzenden. Wanneer u een exporttaak maakt, kunt u lege stations voor een Azure-datacenter verzenden. In elk geval kunt u maximaal 10 schijfstations per taak verzenden.

> [!IMPORTANT]
> Exporteren van gegevens naar Azure-bestanden wordt niet ondersteund.

In deze sectie hoog niveau stappen betrokken bij het importeren en exporteren worden beschreven. 


### <a name="inside-an-import-job"></a>Binnen een import-taak

Op een hoog niveau omvat een import-taak de volgende stappen:

1. Bepaal de gegevens moet worden geïmporteerd, het aantal stations dat u nodig hebt, blob doellocatie voor uw gegevens in Azure-opslag.
2. Het hulpprogramma WAImportExport gebruiken om gegevens te kopiëren naar schijven worden geschreven. Versleutelen van de schijven met BitLocker.
3. Een import-taak in uw doel storage-account maken in Azure portal. Het wijzigingslogboek station bestanden uploaden.
2. Geef de afzender en carrier nummer voor het verzenden van de stations voor u.
3. De schijfstations naar het opgegeven tijdens het maken van de taak back-upfunctie adres verzenden.
4. Werk de bezorging volgnummer in de taakgegevens importeren en verzenden van de importtaak.
5. Stations worden ontvangen en verwerkt in het Azure Datacenter.
6. Schijven worden geleverd met uw account carrier adres van de afzender opgegeven in de import-taak.
  
    ![Afbeelding 1:Import taak stroom](./media/storage-import-export-service/importjob.png)

Voor stapsgewijze instructies voor gegevens importeren, gaat u naar:

- [Gegevens importeren in Azure Blobs](storage-import-export-data-to-blobs.md)
- [Gegevens importeren in Azure-bestanden](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>Binnen een taak voor het exporteren

> [!IMPORTANT]
> De service ondersteunt alleen het exporteren van de Azure Blobs. Het exporteren van de Azure-bestanden wordt niet ondersteund.

Op een hoog niveau omvat een exporttaak de volgende stappen:

1. Bepaal welke gegevens worden geëxporteerd, aantal schijven u zoekt, bron blobs of paden van de container van uw gegevens in Blob-opslag.
3. Een exporttaak in uw opslagaccount bron maken in Azure portal.
4. Geef bron blobs of paden van de container voor de gegevens worden geëxporteerd.
5. Geef de afzender en carrier nummer voor het verzenden van de stations voor u.
6. De schijfstations naar het opgegeven tijdens het maken van de taak back-upfunctie adres verzenden.
7. Werk de bezorging volgnummer in de taakgegevens exporteren en verzenden van de taak voor exporteren.
8. De stations worden ontvangen en verwerkt in het Azure Datacenter.
9. De stations zijn versleuteld met BitLocker en de sleutels zijn beschikbaar via de Azure-portal.  
10. De stations worden geleverd met uw account carrier adres van de afzender opgegeven in de import-taak.
  
    ![Afbeelding 2:Export taak stroom](./media/storage-import-export-service/exportjob.png)

Ga voor stapsgewijze instructies over het exporteren van gegevens naar [gegevens exporteren uit Azure Blobs](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Beschikbaarheid in regio’s 

De Azure Import/Export-service ondersteunt kopiëren van gegevens naar en van alle Azure storage-accounts. U kunt schijven naar een van de locaties kan worden verzonden. Als uw storage-account in een Azure-locatie wordt hier niet gespecificeerd is, wordt een back-ups van alternatieve locatie wordt verstrekt wanneer u de taak maakt.

### <a name="supported-shipping-locations"></a>Back-upfunctie locaties ondersteund


|Land  |Land  |Land  |Land  |
|---------|---------|---------|---------|
|VS - oost    | Noord-Europa        | Centraal-India        |VS (overheid) - Iowa         |
|VS - west     |West-Europa         | Zuid-India        | US DoD - oost        |
|VS - oost 2    | Oost-Azië        |  West-India        | US DoD - centraal        |
|VS - west 2     | Zuidoost-Azië        | Canada - midden        | China East         |
|VS - midden     | Australië - oost        | Canada - oost        | China - noord        |
|Noord-centraal VS     |  Australië - zuidoost       | Brazilië - zuid        | Verenigd Koninkrijk Zuid        |
|Zuid-centraal VS     | Japan - west        |Korea - centraal         | Duitsland - centraal        |
|West-centraal VS     |  Japan - oost       | VS (overheid) - Virginia        | Duitsland - noordoost        |


## <a name="security-considerations"></a>Beveiligingsoverwegingen

De gegevens op het station wordt versleuteld met BitLocker-stationsversleuteling. Deze versleuteling beschermt u uw gegevens terwijl het onderweg is.

Stations zijn voor de taken van gegevensimport versleuteld op twee manieren.  


- De optie opgeven wanneer u *dataset.csv* bestand tijdens het uitvoeren van het hulpprogramma WAImportExport tijdens de voorbereiding van het station. 

- Schakel versleuteling in BitLocker handmatig op het station. Geef de versleutelingssleutel in de *driveset.csv* wanneer WAImportExport hulpprogramma vanaf de opdrachtregel uitgevoerd tijdens de voorbereiding van het station.


Nadat uw gegevens worden gekopieerd naar de stations versleutelt de service voor exporttaken, het station met BitLocker voordat deze terug naar de back-upfunctie. De versleutelingssleutel is verleend via de Azure-portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Prijzen

**Station kosten verwerken**

Er is een station verwerking vast bedrag voor elk station verwerkt als onderdeel van het importeren of exporteren van de taak. Zie de details over de [prijzen van Azure Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Back-upfunctie voor kosten**

Tijdens het verzenden van stations naar Azure, betaalt u de kosten van de back-upfunctie voor aan de back-ups van provider. Wanneer Microsoft de stations naar u terugkeert, wordt de kosten van de back-upfunctie voor verrekend met het provider-account die u hebt opgegeven op het moment van de taak maken.

**Transactiekosten**

Er zijn geen transactiekosten naast standaardopslag transactiekosten bij het importeren van gegevens in Azure Storage. De kosten voor standaard uitgaande zijn van toepassing wanneer gegevens worden geëxporteerd van Blob-opslag. Zie voor meer informatie over transactiekosten [gegevensoverdracht prijzen.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Import/Export-service te gebruiken:
* [Gegevens importeren in Azure Blobs](storage-import-export-data-to-blobs.md)
* [Gegevens exporteren uit Azure Blobs](storage-import-export-data-from-blobs.md)
* [Gegevens importeren in Azure-bestanden](storage-import-export-data-to-files.md)

