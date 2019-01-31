---
title: Exporteren van een taak voor Azure Import/Export maken | Microsoft Docs
description: Informatie over het maken van een exporttaak voor de Microsoft Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 935af10c2ebcdc5273671ed058fdf72099059da3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475615"
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Een exporttaak voor de Azure Import/Export-service maken
Het maken van een exporttaak voor de Microsoft Azure Import/Export-service met behulp van de REST-API omvat de volgende stappen:

-   Selecteren van de blobs te exporteren.

-   Het verkrijgen van een locatie voor verzending.

-   Het maken van de taak voor het exporteren.

-   Verzend uw lege schijven naar Microsoft via een ondersteunde Provider-service.

-   De exporttaak bijwerken met de pakketgegevens.

-   De schijven ontvangt terug van Microsoft.

 Zie [met behulp van de Windows Azure Import/Export-service gegevens overbrengen naar Blob-opslag](storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie waarin wordt gedemonstreerd hoe u de [Azure-portal](https://portal.azure.com/) te maken en beheren van importeren en exporteren van taken.

## <a name="selecting-blobs-to-export"></a>Te exporteren blobs selecteren
 Voor het maken van een exporttaak bekijken, moet u een lijst met blobs die u wilt exporteren uit uw opslagaccount opgeven. Er zijn een aantal manieren om te selecteren van blobs worden geëxporteerd:

-   U kunt een relatief pad naar één blob en alle bijbehorende momentopnamen te selecteren.

-   U kunt een relatief pad naar gebruiken om te selecteren van één blob met uitzondering van de momentopnamen ervan verwijderd.

-   U kunt een relatief pad naar en de tijd van een momentopname gebruiken om te selecteren van een momentopname van een enkele.

-   Een blob-voorvoegsel kunt u alle blobs en -momentopnamen met het opgegeven voorvoegsel selecteren.

-   U kunt exporteren alle blobs en momentopnamen in de storage-account.

 Zie voor meer informatie over het opgeven van blobs voor het exporteren van de [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking.

## <a name="obtaining-your-shipping-location"></a>Het ophalen van de locatie van uw verzending
Voordat u een exporttaak bekijken, moet u eerst een verzending locatienaam en adres door het aanroepen van de [locatie ophalen](https://portal.azure.com) of [lijst met locaties](https://docs.microsoft.com/rest/api/storageimportexport/locations/list) bewerking. `List Locations` retourneert een lijst met locaties en hun e-mailadressen. U kunt een locatie in de geretourneerde lijst selecteren en verzending van uw harde schijven naar dat adres. U kunt ook de `Get Location` bewerking rechtstreeks verkrijgen van het verzendadres voor een specifieke locatie.

Volg de stappen hieronder om de locatie van de verzending verkrijgen:

-   Geef de naam van de locatie van uw opslagaccount. Deze waarde kunt u vinden onder de **locatie** veld in de storage-account **Dashboard** in Azure portal of opgevraagde voor met behulp van de service management API-bewerking [Opslagaccount ophalen Eigenschappen van](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Ophalen van de locatie die beschikbaar zijn voor het verwerken van dit opslagaccount wordt gebruikt door het aanroepen van de `Get Location` bewerking.

-   Als de `AlternateLocations` eigenschap van de locatie van de locatie zelf bevat, wordt dit klopt om deze locatie te gebruiken. Anders aanroepen de `Get Location` bewerking opnieuw uit met een van de alternatieve locaties. De oorspronkelijke locatie mogelijk tijdelijk worden gesloten voor onderhoud.

## <a name="creating-the-export-job"></a>De exporttaak maken
 Aanroepen voor het maken van de taak voor het exporteren, de [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. U moet de volgende informatie:

-   Een naam voor de taak.

-   De naam van het opslagaccount.

-   De verzending locatienaam, in de vorige stap hebt verkregen.

-   Een taaktype (exporteren).

-   Het retouradres waar de stations moeten worden verzonden nadat de taak voor het exporteren is voltooid.

-   De lijst met blobs (of een blob-voorvoegsels) moet worden geëxporteerd.

## <a name="shipping-your-drives"></a>Verzend uw schijven
 Gebruik vervolgens de Azure Import/Export-hulpprogramma om te bepalen het aantal stations dat u verzenden wilt, op basis van de blobs die u hebt geselecteerd om te worden geëxporteerd en de grootte van de schijf. Zie de [naslaginformatie voor Azure Import/Export-hulpprogramma](storage-import-export-tool-how-to-v1.md) voor meer informatie.

 De stations in één pakket verpakken en ze verzenden naar het adres dat is verkregen in de vorige stap. Houd er rekening mee het volgnummer van het pakket voor de volgende stap.

> [!NOTE]
>  U moet uw stations via een ondersteunde Provider-service, die het pakket van een volgnummer voorzien verzenden.

## <a name="updating-the-export-job-with-your-package-information"></a>De exporttaak bijwerken met de pakketgegevens
 Nadat u uw traceringsnummer hebt, roept u de [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking is de naam van de provider en traceringsnummer voor de taak bijgewerkt. U kunt het aantal stations, adres van de afzender en de datum van verzending ook optioneel opgeven.

## <a name="receiving-the-package"></a>Het pakket ontvangen
 Nadat de taak voor het exporteren is verwerkt, wordt uw schijven aan u worden geretourneerd met de versleutelde gegevens. U kunt de BitLocker-sleutel voor elk van de stations ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. U kunt vervolgens het station met de sleutel te ontgrendelen. Het manifestbestand van de schijf op elke schijf bevat de lijst met bestanden op het station, evenals het oorspronkelijke adres van de blob voor elk bestand.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
