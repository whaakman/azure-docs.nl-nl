---
title: Gebruik Azure Import/Export voor het overbrengen van gegevens van en naar Azure Storage | Microsoft Docs
description: Informatie over het maken van importeren en exporteren van taken in de Azure-portal voor het overbrengen van gegevens van en naar Azure Storage.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 07/11/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: ccfc91e85196cbc8295bce71e512d9cf856a23c8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453379"
---
# <a name="what-is-azure-importexport-service"></a>Wat is Azure Import/Export-service?

Azure Import/Export-service wordt gebruikt om veilig grote hoeveelheden gegevens naar Azure Blob storage en Azure-bestanden importeren door de verzending van harde schijven voor een Azure-datacenter. Deze service kan ook worden gebruikt voor gegevensoverdracht van Azure Blob-opslag op schijven en verzend deze naar uw on-premises sites. Gegevens uit een of meer schijven kunnen worden geïmporteerd naar Azure Blob-opslag of naar Azure Files. 

Geef u eigen schijfstations en gegevens overdragen met de Azure Import/Export-service. Ook kunt u schijven die door Microsoft worden verstrekt. 

Als u overdragen van gegevens met behulp van schijven die door Microsoft worden verstrekt wilt, kunt u [Azure Data Box-schijf](../../databox/data-box-disk-overview.md) gegevens importeren in Azure. Maximaal 5 versleutelde schijf SSD's (Solid-State) van Microsoft wordt geleverd met een totale capaciteit van 40 TB per order, naar uw datacenter via een regionale luchtvaartmaatschappij. U kunt snel schijfstations configureren, gegevens naar harde schijven te kopiëren via een USB-3.0-verbinding en verzend de schijven terug naar Azure. Ga voor meer informatie naar [overzicht van Azure Data Box-schijf](../../databox/data-box-disk-overview.md).

## <a name="azure-importexport-usecases"></a>Azure Import/Export-usecases

Overweeg het gebruik van Azure Import/Export-service bij het uploaden en downloaden van gegevens via het netwerk te langzaam is, of aan meer netwerkbandbreedte is te duur. Deze service gebruikt in de volgende scenario's:

* **Migratie van gegevens naar de cloud**: Voor het verplaatsen van grote hoeveelheden gegevens naar Azure is het snel en kosteneffectief.
* **Inhoudsdistributie**: Verzend snel gegevens naar uw klantlocaties.
* **Back-up**: Duren back-ups van uw on-premises gegevens op te slaan in Azure Storage.
* **Gegevensherstel**: Herstellen van de grote hoeveelheid gegevens die zijn opgeslagen in opslag en deze af te leveren aan uw on-premises locatie.

## <a name="importexport-components"></a>Import/Export-onderdelen

Import/Export-service maakt gebruik van de volgende onderdelen:

- **Import/Export service**: Deze service beschikbaar in Azure portal kan de gebruiker maken en importeren van gegevens (uploaden) bijhouden en taken (download) exporteren.  

- **WAImportExport hulpprogramma**: Dit is een opdrachtregelprogramma dat het volgende doet: 
    - Uw schijven die worden verzonden voorbereid voor het importeren van.
    - Vereenvoudigt het uitvoeren van uw gegevens kopiëren naar het station.
    - Versleutelt de gegevens op het station met BitLocker.
    - Genereert de logboekbestanden van het station gebruikt tijdens het importeren maken.
    - Aantal stations dat nodig is voor de export-taken wordt geïdentificeerd.
    
> [!NOTE]
> Het hulpprogramma WAImportExport is beschikbaar in twee versies, versie 1 en 2. U wordt aangeraden dat u gebruikt:
> - Versie 1 voor import/export naar Azure Blob-opslag. 
> - Versie 2 voor het importeren van gegevens in Azure files.
>
> Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssysteem. Voor specifieke versies van het besturingssysteem wordt ondersteund, gaat u naar [Azure Import/Export-vereisten](storage-import-export-requirements.md#supported-operating-systems).

- **Schijfstations**: U kunt verzenden SSD-schijven (SSD's) of harde schijven (HDD's) naar het Azure-datacenter. Bij het maken van een import-taak, kunt u schijven met uw gegevens verzenden. Bij het maken van een exporttaak bekijken, verzendt u lege stations in de Azure-datacenter. Voor specifieke schijftypen, gaat u naar [schijftypen ondersteund](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>Hoe werkt de Import-/ Exportservice?

Azure Import/Export-service kunt overbrengen van gegevens in Azure-Blobs en Azure Files met het maken van taken. Azure portal of Azure Resource Manager REST API gebruiken om te maken van taken. Elke taak is gekoppeld aan één opslagaccount. 

De taken kunnen worden importeren of exporteren van taken. Een importtaak kunt u gegevens importeren in Azure-Blobs of Azure files, terwijl de exporttaak toestaat dat gegevens van Azure-Blobs worden geëxporteerd. Voor een importtaak verzenden u stations die uw gegevens. Wanneer u een exporttaak maakt, kunt u lege schijven naar een Azure-datacenter verzenden. In beide gevallen kunt u maximaal 10 schijven per taak verzenden.

### <a name="inside-an-import-job"></a>Binnen een importtaak

Op hoog niveau omvat een import-taak de volgende stappen uit:

1. Bepaalt de gegevens moet worden geïmporteerd, het aantal stations dat u nodig hebt, blob doellocatie voor uw gegevens in Azure storage.
2. Het hulpprogramma WAImportExport gebruiken om gegevens te kopiëren naar harde schijven. Versleutel de schijven met BitLocker.
3. Een importtaak maken in uw doel-opslagaccount in Azure portal. Upload de logboekbestanden van het station.
4. Geef de retouradres en accountnummer van vervoerder voor de schijven terug naar u verzonden.
5. De schijven naar het verzendadres opgegeven tijdens het maken van taak verzenden.
6. Bijwerken van de levering traceringsnummer in de taakgegevens importeren en het verzenden van de import-taak.
7. De schijven zijn ontvangen en verwerkt in de Azure-Datacenter.
8. De stations worden geleverd met uw provider-account opgegeven in de import-taak adres van de afzender.

> [!NOTE]
> Delen een nationale provider-account voor lokale (binnen data center land) verzendingen 
>
> Delen van verzendingen buitenland (buiten data center land), een internationale provider-account

 ![Afbeelding 1:Import taak stroom](./media/storage-import-export-service/importjob.png)

Voor stapsgewijze instructies voor gegevens importeert, gaat u naar:

- [Gegevens importeren in Azure-Blobs](storage-import-export-data-to-blobs.md)
- [Gegevens importeren in Azure Files](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>Binnen een exporttaak bekijken

> [!IMPORTANT]
> De service biedt alleen ondersteuning voor uitvoer van Azure-Blobs. Exporteren van Azure-bestanden wordt niet ondersteund.

Op hoog niveau omvat een exporttaak bekijken in de volgende stappen:

1. Bepaalt de gegevens worden geëxporteerd, aantal stations u zoekt, bron-BLOB's of container paden van uw gegevens in Blob-opslag.
3. Een exporttaak bekijken in de bron-storage-account maken in Azure portal.
4. Geef bron blobs of container paden voor de gegevens worden geëxporteerd.
5. Geef de retouradres en accountnummer van vervoerder voor de schijven terug naar u verzonden.
6. De schijven naar het verzendadres opgegeven tijdens het maken van taak verzenden.
7. Bijwerken van de levering traceringsnummer in de taakgegevens exporteren en het verzenden van de taak voor het exporteren.
8. De schijven zijn ontvangen en verwerkt in de Azure-Datacenter.
9. De stations zijn versleuteld met BitLocker en de sleutels zijn beschikbaar via de Azure-portal.  
10. De stations worden geleverd met uw provider-account opgegeven in de import-taak adres van de afzender.

> [!NOTE]
> Delen een nationale provider-account voor lokale (binnen data center land) verzendingen 
>
> Delen van verzendingen buitenland (buiten data center land), een internationale provider-account
  
 ![Afbeelding 2:Export taak stroom](./media/storage-import-export-service/exportjob.png)

Ga voor stapsgewijze instructies voor het exporteren van gegevens naar [gegevens exporteren uit Azure Blobs](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Beschikbaarheid in regio’s 

De Azure Import/Export-service ondersteunt kopiëren van gegevens naar en van alle Azure storage-accounts. U kunt schijven naar een van de vermelde locaties verzenden. Als uw storage-account zich in een Azure-locatie die hier niet is opgegeven, wordt de locatie van een alternatieve verzending volgen bij het maken van de taak.

### <a name="supported-shipping-locations"></a>Ondersteunde locaties voor verzending


|Land/regio  |Land/regio  |Land/regio  |Land/regio  |
|---------|---------|---------|---------|
|US - oost    | Europa - noord        | India - centraal        |US Gov - Iowa         |
|US - west     |Europa -west         | India - zuid        | US DoD - oost        |
|US - oost 2    | Azië - oost        |  India - west        | US DoD - centraal        |
|US - west 2     | Azië - zuidoost        | Canada - midden        | China East         |
|US - centraal     | Australië - oost        | Canada - oost        | China - noord        |
|US - noord-centraal     |  Australië - zuidoost       | Brazilië - zuid        | Verenigd Koninkrijk Zuid        |
|US - zuid-centraal     | Japan - west        |Korea - centraal         | Duitsland - centraal        |
|US - west-centraal     |  Japan - oost       | VS (overheid) - Virginia        | Duitsland - noordoost        |


## <a name="security-considerations"></a>Beveiligingsoverwegingen

De gegevens op de schijf worden versleuteld met BitLocker-stationsversleuteling. Deze versleuteling beschermt uw gegevens tijdens overdracht.

Voor de taken van gegevensimport, worden schijven versleuteld op twee manieren.  


- De optie opgeven wanneer u *dataset.csv* bestand tijdens het uitvoeren van het hulpprogramma WAImportExport tijdens de voorbereiding van het station. 

- BitLocker-versleuteling op de schijf handmatig inschakelen. Geef de versleutelingssleutel in de *driveset.csv* wanneer WAImportExport hulpprogramma vanaf de opdrachtregel worden uitgevoerd tijdens de voorbereiding van het station.


Nadat uw gegevens worden gekopieerd naar de stations, versleutelt de service voor export-taken, het station met BitLocker voordat deze terug naar u verzonden. De versleutelingssleutel is aan u geleverd via de Azure-portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Prijzen

**Station verzendkosten**

Er zijn de kosten van een station verwerking voor elk station verwerkt als onderdeel van het importeren of exporteren van de taak. Zie de details over de [Azure Import/Export-prijzen](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Verzendkosten**

Wanneer u schijven naar Azure verzenden, moet u de verzendkosten aan de vervoerder betaalt. Wanneer Microsoft de schijven naar u terugkeert, wordt de verzendkosten verrekend met het provider-account die u hebt opgegeven op het moment van de taak te maken.

**Transactiekosten**

Er gelden geen transactiekosten naast transactiekosten voor standard-opslag bij het importeren van gegevens in Azure Storage. De kosten voor standaard uitgaand verkeer zijn van toepassing wanneer gegevens worden geëxporteerd uit de Blob-opslag. Zie voor meer informatie over transactiekosten [prijzen voor gegevensoverdracht.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de Import/Export-service:
* [Gegevens importeren in Azure-Blobs](storage-import-export-data-to-blobs.md)
* [Gegevens exporteren uit Azure-Blobs](storage-import-export-data-from-blobs.md)
* [Gegevens importeren in Azure Files](storage-import-export-data-to-files.md)

