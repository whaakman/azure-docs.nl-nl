---
title: Exporteren van een taak voor Azure Import/Export maken | Microsoft Docs
description: Informatie over het maken van een taak voor het exporteren van de Microsoft Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 70f74b685f4d5b902b6f6780f7c1de4d357164b8
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Een exporttaak voor de Azure Import/Export-service maken
Maken van een taak voor het exporteren van de Microsoft Azure Import/Export-service met de REST API omvat de volgende stappen:

-   Als u de blobs te exporteren.

-   Het verkrijgen van een back-ups van locatie.

-   Maken van de taak voor het exporteren.

-   Lege stations via een ondersteunde Provider-service naar Microsoft verzenden.

-   De taak voor het exporteren bijwerken met de pakket-informatie.

-   De stations ontvangen terug van Microsoft.

 Zie [via de Windows Azure Import/Export-service gegevens overdragen naar Blob Storage](storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie waarin wordt getoond hoe u de [Azure-portal](https://portal.azure.com/) te maken en beheren van importeren en exporteren van taken.

## <a name="selecting-blobs-to-export"></a>Als u blobs exporteren
 Voor het maken van een taak voor het exporteren, moet u een lijst verstrekken van blobs die u wilt exporteren vanuit uw opslagaccount. Er zijn een aantal manieren blobs worden geëxporteerd selecteren:

-   U kunt een blobpad relatief Selecteer één blob en alle bijbehorende momentopnamen.

-   U kunt een blobpad relatief selecteren één blob met uitzondering van de momentopnamen.

-   U kunt een relatief blobpad en de tijd van een momentopname selecteren één momentopname.

-   Een blob-voorvoegsel kunt u alle blobs en momentopnamen met het opgegeven voorvoegsel selecteren.

-   U kunt alle blobs en momentopnamen in de storage-account exporteren.

 Zie voor meer informatie over het opgeven van BLOB's om te exporteren, de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking.

## <a name="obtaining-your-shipping-location"></a>Het verkrijgen van de locatie van uw back-ups
Voordat u maakt een taak voor het exporteren, moet u een back-upfunctie locatienaam en adres ophalen door het aanroepen van de [locatie ophalen](https://portal.azure.com) of [lijst locaties](/rest/api/storageimportexport/listlocations) bewerking. `List Locations`retourneert een lijst met locaties en hun e-mailadressen. U kunt een locatie in de geretourneerde lijst selecteren en de harde schijven naar dit adres verzenden. U kunt ook de `Get Location` bewerking rechtstreeks verkrijgen van het adres van de back-upfunctie voor een specifieke locatie.

Volg onderstaande stappen voor het verkrijgen van de locatie van de back-ups:

-   Geef de naam van de locatie van uw opslagaccount. Deze waarde kan worden gevonden in de **locatie** op het opslagaccount **Dashboard** in de Azure portal of voor de query met de service management API-bewerking [eigenschappen van het Opslagaccount ophalen](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Ophalen van de locatie die beschikbaar zijn voor het verwerken van dit opslagaccount door het aanroepen van de `Get Location` bewerking.

-   Als de `AlternateLocations` eigenschap van de locatie van de locatie zelf bevat, wordt dit klopt voor gebruik van deze locatie. Anders Roep de `Get Location` opnieuw met een van de alternatieve locaties. De oorspronkelijke locatie mogelijk tijdelijk worden gesloten voor onderhoud.

## <a name="creating-the-export-job"></a>Maken van de taak voor exporteren
 Aanroepen voor het maken van de taak voor het exporteren, de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. U moet de volgende informatie:

-   Een naam voor de taak.

-   Naam van het opslagaccount.

-   De back-upfunctie locatienaam, in de vorige stap hebt verkregen.

-   Een taaktype (exporteren).

-   Het retouradres waarin de schijven moeten worden verzonden nadat de taak voor het exporteren is voltooid.

-   De lijst met BLOB's (of blob voorvoegsels) moet worden geëxporteerd.

## <a name="shipping-your-drives"></a>Verzending van uw schijven
 Gebruik vervolgens de Azure-hulpprogramma voor importeren/exporteren om te bepalen het aantal stations dat u verzenden wilt, op basis van de blobs die u hebt geselecteerd om te exporteren en de grootte van de schijf. Zie de [Azure Import/Export hulpprogramma verwijzing](storage-import-export-tool-how-to-v1.md) voor meer informatie.

 De stations in een enkel pakket van het pakket en moeten worden verzonden naar het adres in de vorige stap verkregen. Noteer het nummer van het bijhouden van uw pakket voor de volgende stap.

> [!NOTE]
>  U moet uw stations via een ondersteunde Provider-service, die het pakket van een volgnummer voorzien verzenden.

## <a name="updating-the-export-job-with-your-package-information"></a>De taak voor het exporteren wordt bijgewerkt met de informatie van uw pakket
 Nadat u het volgnummer hebt, roept u de [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bijgewerkt met de bewerking op de naam van de provider en het volgnummer voor de taak. Desgewenst kunt u het aantal stations, het adres van de afzender en ook de datum van de back-upfunctie.

## <a name="receiving-the-package"></a>Het pakket wordt ontvangen
 Nadat de taak voor het exporteren is verwerkt, wordt de stations voor u met uw versleutelde gegevens geretourneerd. U kunt de BitLocker-sleutel voor elk van de stations ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. U kunt vervolgens het station met de sleutel te ontgrendelen. Het manifestbestand van de schijf op elk station bevat de lijst met bestanden op het station, evenals het oorspronkelijke blobadres voor elk bestand.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
