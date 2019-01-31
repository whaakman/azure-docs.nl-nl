---
title: Een importtaak voor Azure Import/Export maken | Microsoft Docs
description: Informatie over het maken van een invoer voor de Microsoft Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: efe7b6722a9cfa24ea86130a13a0599115e89821
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55451662"
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a>Een importtaak voor de Azure Import/Export-service maken

Het maken van een import-taak voor de Microsoft Azure Import/Export-service met behulp van de REST-API omvat de volgende stappen:

-   Voorbereiden van harde schijven met de Azure Import/Export-hulpprogramma.

-   Het ophalen van de locatie waarnaar u wilt het station verzenden.

-   Het maken van de import-taak.

-   Verzending van de schijven naar Microsoft via een ondersteunde Provider-service.

-   De import-taak bijwerken met de verzendgegevens.

 Zie [met behulp van de Microsoft Azure Import/Export-service gegevens overbrengen naar Blob-opslag](storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie waarin wordt gedemonstreerd hoe u de [Azure-portal](https://portal.azure.com/) maken en beheren van importeren en exporteren van taken.

## <a name="preparing-drives-with-the-azure-importexport-tool"></a>Schijven met het hulpprogramma Azure Import/Export voorbereiden

De stappen voor het voorbereiden van schijven voor een importtaak zijn hetzelfde, of u nu de jobvia de portal maakt of via de REST-API.

Hieronder volgt een kort overzicht van de voorbereiding van station. Raadpleeg de [naslaginformatie voor Azure Import-ExportTool](storage-import-export-tool-how-to-v1.md) voor volledige instructies. U kunt het hulpprogramma Azure Import/Export downloaden [hier](https://go.microsoft.com/fwlink/?LinkID=301900).

Voorbereiden van het station bestaat uit:

-   Identificeren van de gegevens worden geïmporteerd.

-   Hoe identificeert u de blobs doel in Windows Azure Storage.

-   Met behulp van de Azure Import/Export-hulpprogramma gegevens kopiëren naar een of meer harde schijven.

 De Azure Import/Export-hulpprogramma wordt ook een manifestbestand voor elk van de stations gegenereerd als deze wordt voorbereid. Een manifestbestand bevat:

-   Een inventarisatie van alle bestanden die zijn bedoeld voor het uploaden en de toewijzingen van deze bestanden tot blobs.

-   Controlesommen van de segmenten van elk bestand.

-   Informatie over de metagegevens en eigenschappen om te koppelen aan elke blob.

-   Een overzicht van de actie moet worden uitgevoerd als een blob die wordt geüpload dezelfde naam als een bestaande blob in de container heeft. Mogelijke opties zijn: a) de blob wordt overschreven door het bestand, (b) het bewaren van de bestaande blob en het bestand te uploaden overslaan, c) een achtervoegsel aan de naam toegevoegd zodat deze niet in strijd met andere bestanden.

## <a name="obtaining-your-shipping-location"></a>Het ophalen van de locatie van uw verzending

Voordat u een import-taak maakt, moet u eerst een verzending locatienaam en adres door het aanroepen van de [lijst met locaties](https://docs.microsoft.com/rest/api/storageimportexport/locations/list) bewerking. `List Locations` retourneert een lijst met locaties en hun e-mailadressen. U kunt een locatie in de geretourneerde lijst selecteren en verzending van uw harde schijven naar dat adres. U kunt ook de `Get Location` bewerking rechtstreeks verkrijgen van het verzendadres voor een specifieke locatie.

 Volg de stappen hieronder om de locatie van de verzending verkrijgen:

-   Geef de naam van de locatie van uw opslagaccount. Deze waarde kunt u vinden onder de **locatie** veld in de storage-account **Dashboard** in Azure portal of opgevraagde voor met behulp van de service management API-bewerking [Opslagaccount ophalen Eigenschappen van](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Ophalen van de locatie die beschikbaar is voor het verwerken van dit opslagaccount wordt gebruikt door het aanroepen van de `Get Location` bewerking.

-   Als de `AlternateLocations` eigenschap van de locatie van de locatie zelf bevat, wordt dit klopt om deze locatie te gebruiken. Anders aanroepen de `Get Location` bewerking opnieuw uit met een van de alternatieve locaties. De oorspronkelijke locatie mogelijk tijdelijk worden gesloten voor onderhoud.

## <a name="creating-the-import-job"></a>Het maken van de import-taak
Aanroepen voor het maken van de import-taak, de [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. U moet de volgende informatie:

-   Een naam voor de taak.

-   De naam van het opslagaccount.

-   De verzending locatienaam, verkregen uit de vorige stap.

-   Een taaktype (importeren).

-   Het retouradres waar de stations moeten worden verzonden nadat de import-taak is voltooid.

-   De lijst met stations in de taak. Voor elk station, moet u de volgende informatie die is verkregen tijdens de voorbereiding van het station opnemen:

    -   Het station Id

    -   De BitLocker-sleutel

    -   Het relatieve pad van de manifest-bestand op de harde schijf

    -   De Base16 gecodeerd manifestbestand MD5-hash

## <a name="shipping-your-drives"></a>Verzend uw schijven
U moet verzend uw schijven naar het adres dat u in de vorige stap hebt verkregen, en geeft u de Import/Export-service met het volgnummer van het pakket.

> [!NOTE]
>  U moet uw stations via een ondersteunde Provider-service, die het pakket van een volgnummer voorzien verzenden.

## <a name="updating-the-import-job-with-your-shipping-information"></a>De import-taak bijwerken met uw verzendgegevens
Nadat u uw traceringsnummer hebt, roept u de [taakeigenschappen bijwerken](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Update) bewerking voor het bijwerken van de naam van de vervoerder, het aantal voor de taak en de accountnummer van vervoerder voor retourneren verzending. U kunt het aantal stations en de datum van verzending ook optioneel opgeven.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
