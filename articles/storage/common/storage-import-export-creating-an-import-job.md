---
title: Maken van een Import-taak voor de Azure Import/Export | Microsoft Docs
description: Informatie over het maken van een import voor de Microsoft Azure Import/Export-service.
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d373d2a0e601f2796719fc5efb8761f276ab24d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a>Maken van een import-taak voor de service Azure Import/Export

Maken van een import-taak voor de Microsoft Azure Import/Export-service met de REST API omvat de volgende stappen:

-   Schijven met de Azure-hulpprogramma voor importeren/exporteren wordt voorbereid.

-   Het verkrijgen van de locatie op waarnaar u het station verzenden.

-   De import-taak maken.

-   Verzending van de stations met Microsoft via een ondersteunde Provider-service.

-   Bijwerken van de import-taak met de back-upfunctie voor details.

 Zie [via de Microsoft Azure Import/Export-service gegevens overdragen naar Blob Storage](storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie waarin wordt getoond hoe u de [Azure-portal](https://portal.azure.com/) te maken en beheren van importeren en exporteren van taken.

## <a name="preparing-drives-with-the-azure-importexport-tool"></a>Stations met het hulpprogramma voor Azure Import/Export voorbereiden

De stappen voor het voorbereiden van stations voor een import-taak zijn hetzelfde, of u de jobvia de portal maakt of via de REST-API.

Hieronder vindt u een kort overzicht van de voorbereiding van het station. Raadpleeg de [Azure Import-ExportTool verwijzing](storage-import-export-tool-how-to-v1.md) voor volledige instructies. U kunt de Azure-hulpprogramma voor importeren/exporteren downloaden [hier](http://go.microsoft.com/fwlink/?LinkID=301900).

Voorbereiden van de schijf omvat:

-   Identificeren van de gegevens moeten worden geïmporteerd.

-   Identificeren van de bestemming blobs in Windows Azure Storage.

-   Gebruik de Azure-hulpprogramma voor importeren/exporteren van uw gegevens kopiëren naar een of meer harde schijven.

 De Azure-hulpprogramma voor importeren/exporteren wordt ook een manifestbestand genereren voor elk van de stations als deze wordt voorbereid. Een manifestbestand bevat:

-   Een opsomming van alle bestanden die zijn bedoeld voor het uploaden en de toewijzingen van deze bestanden in BLOB's.

-   Controlesommen van de segmenten van elk bestand.

-   Informatie over de metagegevens en de eigenschappen die u wilt koppelen aan elke blob.

-   Een lijst met de actie moet worden uitgevoerd als een blob die wordt geüpload dezelfde naam als een bestaande blob zijn in de container heeft. Mogelijke opties zijn: a) de blob wordt overschreven door het bestand, b) Houd de bestaande blob en het uploaden van het bestand overslaan, c) een achtervoegsel aan de naam toegevoegd zodat deze niet in strijd is met andere bestanden.

## <a name="obtaining-your-shipping-location"></a>Het verkrijgen van de locatie van uw back-ups

Voordat u een import-taak maakt, moet u een back-upfunctie locatienaam en adres ophalen door het aanroepen van de [lijst locaties](/rest/api/storageimportexport/listlocations) bewerking. `List Locations`retourneert een lijst met locaties en hun e-mailadressen. U kunt een locatie in de geretourneerde lijst selecteren en de harde schijven naar dit adres verzenden. U kunt ook de `Get Location` bewerking rechtstreeks verkrijgen van het adres van de back-upfunctie voor een specifieke locatie.

 Volg onderstaande stappen voor het verkrijgen van de locatie van de back-ups:

-   Geef de naam van de locatie van uw opslagaccount. Deze waarde kan worden gevonden in de **locatie** op het opslagaccount **Dashboard** in de Azure portal of voor de query met de service management API-bewerking [eigenschappen van het Opslagaccount ophalen](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Ophalen van de locatie die beschikbaar is voor het verwerken van dit opslagaccount door het aanroepen van de `Get Location` bewerking.

-   Als de `AlternateLocations` eigenschap van de locatie van de locatie zelf bevat, wordt dit klopt voor gebruik van deze locatie. Anders Roep de `Get Location` opnieuw met een van de alternatieve locaties. De oorspronkelijke locatie mogelijk tijdelijk worden gesloten voor onderhoud.

## <a name="creating-the-import-job"></a>De import-taak maken
Aanroepen voor het maken van de import-taak, de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking. U moet de volgende informatie:

-   Een naam voor de taak.

-   Naam van het opslagaccount.

-   De back-upfunctie locatienaam, uit de vorige stap hebt verkregen.

-   Een taaktype (importeren).

-   Het retouradres waarin de schijven moeten worden verzonden nadat de import-taak is voltooid.

-   De lijst met stations in de taak. U moet de volgende informatie die is verkregen tijdens de voorbereiding van station stap bevat voor elk station.

    -   De schijf-Id

    -   De BitLocker-sleutel

    -   Het manifestbestand voor het relatieve pad op de harde schijf

    -   De Base16 gecodeerd manifestbestand MD5-hash

## <a name="shipping-your-drives"></a>Verzending van uw schijven
U moet uw schijven naar het adres dat u hebt ontvangen van de vorige stap verzendt en u moet de Import/Export-service met het volgnummer van het pakket opgeven.

> [!NOTE]
>  U moet uw stations via een ondersteunde Provider-service, die het pakket van een volgnummer voorzien verzenden.

## <a name="updating-the-import-job-with-your-shipping-information"></a>De import-taak wordt bijgewerkt met uw back-upfunctie informatie
Nadat u het volgnummer hebt, roept u de [taakeigenschappen bijwerken](/api/storageimportexport/jobs#Jobs_Update) bewerking voor het bijwerken van de naam van de provider, het volgnummer voor de taak en nummer van de provider voor back-ups van return. Desgewenst kunt u het aantal stations en ook de datum van de back-upfunctie.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
