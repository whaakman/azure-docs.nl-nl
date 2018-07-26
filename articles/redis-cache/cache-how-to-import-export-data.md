---
title: Importeren en exporteren van gegevens in Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het importeren en exporteren van gegevens naar en van blob-opslag met uw premium Azure Redis-Cache-exemplaren
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: wesmc
ms.openlocfilehash: 6733891213f15e9ceaf08ef7fb50380db47a695f
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259191"
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Importeren en exporteren van gegevens in Azure Redis-Cache
Import/Export wordt een Azure Redis Cache gegevensbewerking voor het beheer, zodat u kunt gegevens importeren in Azure Redis-Cache in of exporteren van gegevens uit Azure Redis-Cache door te importeren en exporteren van een momentopname Redis Cache Database (RDB) van een premium-cache naar een blob in Azure Storage-Account. 

- **Exporteren** -u kunt uw Azure Redis Cache RDB-momentopnamen exporteren naar een pagina-Blob.
- **Importeren** -u kunt uw Redis-Cache RDB-momentopnamen importeren uit een pagina-Blob of een blok-Blob.

Import/Export kunt u om te migreren tussen verschillende exemplaren van Azure Redis-Cache of vullen van de cache met gegevens voor het gebruik.

Dit artikel bevat richtlijnen voor het importeren en exporteren van gegevens met Azure Redis Cache en vindt u de antwoorden op veelgestelde vragen.

> [!IMPORTANT]
> Import/Export is in preview en is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat.
>
>

## <a name="import"></a>Importeren
Invoer kan worden gebruikt om Redis compatibele RDB-bestanden vanaf een willekeurige Redis-server die wordt uitgevoerd in een cloud of de omgeving, met inbegrip van Redis die worden uitgevoerd op Linux, Windows of elke andere cloudprovider, zoals Amazon Web Services en andere. Importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingevulde gegevens. Tijdens het importproces Azure Redis Cache laadt de RDB-bestanden van Azure storage in het geheugen en voegt vervolgens de sleutels in de cache.

> [!NOTE]
> Voordat u begint de importbewerking, zorg ervoor dat uw Redis-Database (RDB)-bestand of de bestanden zijn geüpload naar de pagina of een blok-blobs in Azure storage in dezelfde regio en abonnement als uw Azure Redis-Cache-exemplaar. Zie voor meer informatie, [aan de slag met Azure Blob-opslag](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Als u uw RDB-bestand met geëxporteerde de [exporteren van Azure Redis Cache](#export) functie, uw RDB-bestand is al opgeslagen in een pagina-blob en gereed is voor het importeren.
>
>

1. Voor het importeren van een of meer geëxporteerde cache blobs [bladert u naar uw cache](cache-configure.md#configure-redis-cache-settings) in de Azure portal en klik op **gegevens importeren** uit de **resourcemenu**.

    ![Gegevens importeren][cache-import-data]
2. Klik op **Kies BLOB (s)** en selecteer het opslagaccount waarin de gegevens te importeren.

    ![Opslagaccount kiezen][cache-import-choose-storage-account]
3. Klik op de container met de gegevens te importeren.

    ![Container kiezen][cache-import-choose-container]
4. Selecteer een of meer blobs te importeren door te klikken op het gebied aan de linkerkant van de blob-naam en klik vervolgens op **Selecteer**.

    ![Kies blobs][cache-import-choose-blobs]
5. Klik op **importeren** om het importproces te starten.

   > [!IMPORTANT]
   > De cache is niet toegankelijk is voor cache-clients tijdens het importeren en alle bestaande gegevens in de cache wordt verwijderd.
   >
   >

    ![Importeren][cache-import-blobs]

    U kunt de voortgang van de importbewerking controleren door de meldingen van de Azure-portal of door te bekijken van de gebeurtenissen in de [auditlogboek](../azure-resource-manager/resource-group-audit.md).

    ![Voortgang van importeren][cache-import-data-import-complete]

## <a name="export"></a>Exporteren
Export kunt u de gegevens die zijn opgeslagen in Azure Redis Cache voor compatibel RDB-bestand(en) Redis exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van één Azure Redis-Cache-exemplaar naar een andere of naar een andere Redis-server. Tijdens het exportproces een tijdelijke bestand wordt gemaakt op de virtuele machine die als host fungeert voor de server-exemplaar van Azure Redis-Cache en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status geslaagd of mislukt, wordt het tijdelijke bestand verwijderd.

1. Voor het exporteren van de huidige inhoud van de cache naar de opslag, [bladert u naar uw cache](cache-configure.md#configure-redis-cache-settings) in de Azure portal en klik op **gegevens exporteren** uit de **resourcemenu**.

    ![Opslagcontainer kiezen][cache-export-data-choose-storage-container]
2. Klik op **Opslagcontainer kiezen** en selecteer het gewenste opslagaccount. Het opslagaccount moet zich in hetzelfde abonnement en dezelfde regio als uw cache.

   > [!IMPORTANT]
   > Werkt met pagina-blobs die worden ondersteund door zowel klassieke als Resource Manager-opslagaccounts, maar worden niet ondersteund door exporteren [Blob storage-accounts](../storage/common/storage-account-options.md#blob-storage-accounts) op dit moment.
   >
   >

    ![Storage-account][cache-export-data-choose-account]
3. Kies de gewenste blob-container en klik op **Selecteer**. Voor het gebruik van nieuwe een container, klikt u op **Container toevoegen** eerst toevoegen en selecteer vervolgens in de lijst.

    ![Opslagcontainer kiezen][cache-export-data-container]
4. Typ een **voorvoegsel voor de Blobnaam** en klikt u op **exporteren** naar het exportproces start. Het voorvoegsel van de blob wordt gebruikt als voorvoegsel voor de namen van bestanden die worden gegenereerd door deze exportbewerking.

    ![Exporteren][cache-export-data]

    U kunt de voortgang van de exportbewerking door de meldingen van de Azure-portal of door te bekijken van de gebeurtenissen in de [auditlogboek](../azure-resource-manager/resource-group-audit.md).

    ![Het exporteren van gegevens voltooid][cache-export-data-export-complete]

    Caches blijven beschikbaar voor gebruik tijdens het exportproces.

## <a name="importexport-faq"></a>Veelgestelde vragen over Import/Export
In deze sectie bevat veelgestelde vragen over de Import/Export-functie.

* [De prijzen van lagen, kan de Import/Export gebruiken?](#what-pricing-tiers-can-use-importexport)
* [Kan ik gegevens importeren uit een Redis-server?](#can-i-import-data-from-any-redis-server)
* [De RDB-versies kan ik importeren?](#what-rdb-versions-can-i-import)
* [Mijn cache is beschikbaar tijdens een bewerking voor importeren/exporteren?](#is-my-cache-available-during-an-importexport-operation)
* [Kan ik Import/Export gebruiken met Redis-cluster?](#can-i-use-importexport-with-redis-cluster)
* [Hoe werkt voor importeren/exporteren met een aangepaste databases instellen?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Hoe verschilt voor importeren/exporteren van Redis-persistentie?](#how-is-importexport-different-from-redis-persistence)
* [Kan ik automatiseren met behulp van PowerShell, CLI of andere clients beheren voor importeren/exporteren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Ik heb ontvangen een time-outfout tijdens mijn Import/Export-bewerking. Wat betekent dit?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Ik kreeg een fout bij het exporteren van gegevens naar Azure Blob Storage. Wat is er gebeurd?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>De prijzen van lagen, kan de Import/Export gebruiken?
Import/Export is alleen beschikbaar in de prijscategorie premium.

### <a name="can-i-import-data-from-any-redis-server"></a>Kan ik gegevens importeren uit een Redis-server?
Ja, naast het importeren van gegevens die zijn geëxporteerd uit Azure Redis Cache-exemplaren, kunt u de RDB-bestanden importeren vanaf een willekeurige Redis-server die wordt uitgevoerd in een cloud of de omgeving, zoals Linux, Windows, of cloud-providers, zoals Amazon Web Services. U doet dit door het RDB-bestand van de gewenste Redis-server uploaden naar een pagina of een blok-blob in een Azure Storage-Account en vervolgens importeren in uw premium Azure Redis-Cache-exemplaar. U wilt bijvoorbeeld de gegevens van uw cache voor productie exporteren en importeren in een cache die wordt gebruikt als onderdeel van een faseringsomgeving voor het testen of de migratie.

> [!IMPORTANT]
> Als u wilt importeren gegevens geëxporteerd uit de Redis-servers dan Azure Redis-Cache bij het gebruik van een pagina-blob, moet het formaat van de pagina-blob worden afgestemd op een grens van 512 bytes. Zie voor de voorbeeldcode om uit te voeren van de opvulling van eventuele vereiste byte, [voorbeeld pagina blob uploaden](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>De RDB-versies kan ik importeren?

Azure Redis Cache biedt ondersteuning voor de RDB-import van via RDB versie 7.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Mijn cache is beschikbaar tijdens een bewerking voor importeren/exporteren?
* **Exporteren** - Caches beschikbaar blijven en u kunt echter ook doorgaan met uw cache tijdens een exportbewerking.
* **Importeren** - Caches zijn niet beschikbaar wanneer een importbewerking wordt gestart, en beschikbaar is voor gebruik wanneer de importbewerking is voltooid.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Kan ik Import/Export gebruiken met Redis-cluster?
Ja, en u kunt importeren/exporteren tussen een geclusterde cache en een niet-geclusterde cache. Sinds de Redis-cluster [alleen ondersteunt database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), alle gegevens in databases dan 0 is niet geïmporteerd. Wanneer geclusterde cache-gegevens worden geïmporteerd, wordt de sleutels herverdeeld over de shards van het cluster.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hoe werkt voor importeren/exporteren met een aangepaste databases instellen?
Sommige Prijscategorieën hebben verschillende [limieten voor databases](cache-configure.md#databases), dus er enkele overwegingen zijn bij het importeren als u een aangepaste waarde voor geconfigureerd de `databases` instellen tijdens het maken van de cache.

* Bij het importeren van een prijscategorie met een lagere `databases` limiet dan de laag die u hebt geëxporteerd:
  * Als u het standaardaantal `databases`, welke 16 voor alle Prijscategorieën is, geen gegevens verloren zijn gegaan.
  * Als u een aangepaste aantal `databases` die valt binnen de grenzen voor de laag waarop u importeren wilt, gegevens niet verloren.
  * Als de geëxporteerde gegevens gegevens in een database die langer is dan de grenzen van de nieuwe laag opgenomen, worden de gegevens uit deze hogere databases is niet geïmporteerd.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Hoe verschilt voor importeren/exporteren van Redis-persistentie?
Azure Redis-cachepersistentie kunt u het behoud van gegevens die zijn opgeslagen in Redis naar Azure Storage. Wanneer persistentie is geconfigureerd, persistente Azure Redis-Cache een momentopname van de Redis-cache in een binaire indeling Redis op schijf op basis van een back-upfrequentie die kunnen worden geconfigureerd. Als er een catastrofale gebeurtenis optreedt die zowel de primaire en replica-cache wordt uitgeschakeld, wordt de cachegegevens hersteld automatisch met behulp van de meest recente momentopname. Zie voor meer informatie, [persistentie van gegevens voor een Premium Azure Redis Cache configureren](cache-how-to-premium-persistence.md).

Import / Export kunt u gegevens overzetten naar in of exporteren uit Azure Redis-Cache. Het geen back-up configureren en herstellen met behulp van Redis-persistentie.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kan ik automatiseren met behulp van PowerShell, CLI of andere clients beheren voor importeren/exporteren?
Ja, voor PowerShell instructies Zie [voor het importeren van een Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) en [voor het exporteren van een Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ik heb ontvangen een time-outfout tijdens mijn Import/Export-bewerking. Wat betekent dit?
Als u blijft op de **gegevens importeren** of **gegevens exporteren** blade voor langer dan 15 minuten voordat de bewerking wordt gestart, wordt er een fout opgetreden bij een foutbericht weergegeven die vergelijkbaar is met het volgende voorbeeld:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Dit probleem oplossen, het importeren te starten of de exportbewerking is vóór 15 minuten zijn verstreken.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ik kreeg een fout bij het exporteren van gegevens naar Azure Blob Storage. Wat is er gebeurd?
Exporteren werkt alleen met de RDB-bestanden die zijn opgeslagen als pagina-blobs. Andere blob-typen worden momenteel niet ondersteund, met inbegrip van blob storage-accounts met warme en koude lagen. Zie voor meer informatie, [Blob storage-accounts](../storage/common/storage-account-options.md#blob-storage-accounts).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer functies voor premium-cache.

* [Inleiding tot de Azure Redis Cache Premium-laag](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
