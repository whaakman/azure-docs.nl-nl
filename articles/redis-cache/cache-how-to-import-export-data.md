---
title: Importeren en exporteren van gegevens in Azure Redis-Cache | Microsoft Docs
description: Meer informatie over het importeren en exporteren van gegevens naar en van blob storage met uw premium Azure Redis-Cache-exemplaren
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: wesmc
ms.openlocfilehash: db488f759752880a47a78dfeec13b14f65bd503c
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Importeren en exporteren van gegevens in Azure Redis-Cache
Import/Export is een Azure Redis-Cache gegevensbewerking voor het beheer, zodat u kunt gegevens in Azure Redis-Cache importeren of exporteren van gegevens in Azure Redis-Cache door importeren en exporteren van een momentopname van een Redis-Cache Database (RDB) uit de cache premium naar een blob in een Azure Storage-Account. 

- **Exporteren** -kunt u uw Azure Redis-Cache RDB momentopnamen exporteren naar een pagina-Blob.
- **Importeren** -u kunt de momentopnamen van uw Redis-Cache RDB importeren uit een pagina-Blob of een blok-Blob.

Voor importeren/exporteren kunt u migreren tussen verschillende exemplaren van Azure Redis-Cache of het vullen van de cache met gegevens voor het gebruik.

In dit artikel biedt een handleiding voor het importeren en exporteren van gegevens met Azure Redis-Cache en vindt u de antwoorden op veelgestelde vragen.

> [!IMPORTANT]
> Import/Export is Preview-versie en is alleen beschikbaar voor [premium-laag](cache-premium-tier-intro.md) in de cache opslaat.
>
>

## <a name="import"></a>Importeren
Invoer kan worden gebruikt om Redis compatibele RDB bestanden vanaf een willekeurige Redis-server uitgevoerd in een cloud of de omgeving, inclusief Redis uitgevoerd op Linux, Windows of elke cloudprovider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier voor het maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure Redis-Cache laadt de bestanden RDB uit Azure storage in het geheugen en voegt vervolgens de sleutels in de cache.

> [!NOTE]
> Zorg ervoor dat uw Redis-Database (RDB) of meer bestanden zijn geüpload naar de pagina of het blok-blobs in Azure-opslag in de dezelfde regio en het abonnement als uw exemplaar van Azure Redis-Cache vóór de importbewerking. Zie voor meer informatie [aan de slag met Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Als u hebt geëxporteerd uw RDB-bestand met de [exporteren van Azure Redis-Cache](#export) functie, het bestand RDB is al opgeslagen in een pagina-blob en is gereed om te importeren.
>
>

1. Voor het importeren van een of meer geëxporteerde cache blobs [Blader naar uw cache](cache-configure.md#configure-redis-cache-settings) in de Azure-portal en klik op **gegevens importeren** van de **Resource menu**.

    ![Gegevens importeren][cache-import-data]
2. Klik op **kiezen Blob(s)** en selecteer het opslagaccount met de gegevens te importeren.

    ![Opslagaccount kiezen][cache-import-choose-storage-account]
3. Klik op de container waarin de gegevens te importeren.

    ![Kies container][cache-import-choose-container]
4. Selecteer een of meer blobs te importeren door te klikken op het gebied aan de linkerkant van de blob-naam en klik vervolgens op **Selecteer**.

    ![Kies BLOB 's][cache-import-choose-blobs]
5. Klik op **importeren** om te beginnen met het importproces.

   > [!IMPORTANT]
   > De cache is niet toegankelijk is voor cacheclients tijdens het importeren en alle bestaande gegevens in de cache wordt verwijderd.
   >
   >

    ![Importeren][cache-import-blobs]

    U kunt de voortgang van de importbewerking door de meldingen van de Azure-portal of door het bekijken van de gebeurtenissen in de [controlelogboek](../azure-resource-manager/resource-group-audit.md).

    ![Voortgang importeren][cache-import-data-import-complete]

## <a name="export"></a>Exporteren
Exporteren kunt u de gegevens die zijn opgeslagen in Azure Redis-Cache voor Redis compatibel RDB bestand(en) exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van een Azure Redis-Cache-exemplaar naar een andere of een andere Redis-server. Tijdens het exportproces een tijdelijk bestand wordt gemaakt op de virtuele machine die als host fungeert voor de server-exemplaar van Azure Redis-Cache en het bestand is geüpload naar de aangewezen storage-account. Wanneer de exportbewerking is voltooid met de status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

1. De huidige inhoud van de cache exporteren naar opslag, [Blader naar uw cache](cache-configure.md#configure-redis-cache-settings) in de Azure-portal en klik op **gegevens exporteren** van de **Resource menu**.

    ![Kies de opslagcontainer][cache-export-data-choose-storage-container]
2. Klik op **Opslagcontainer Kies** en selecteer het gewenste opslagaccount. Het opslagaccount moet zich in hetzelfde abonnement en dezelfde regio als uw cache.

   > [!IMPORTANT]
   > Werkt met pagina-blobs, die worden ondersteund door zowel klassieke als Resource Manager storage-accounts, maar worden niet ondersteund door exporteren [Blob storage-accounts](../storage/common/storage-account-options.md#blob-storage-accounts) op dit moment.
   >
   >

    ![Storage-account][cache-export-data-choose-account]
3. Kies de gewenste blob-container en op **Selecteer**. Als u nieuwe container **Container toevoegen** eerst toevoegen en selecteer vervolgens in de lijst.

    ![Kies de opslagcontainer][cache-export-data-container]
4. Typ een **Blob voorvoegsel** en klik op **exporteren** naar het exportproces start. Het voorvoegsel van de blob wordt gebruikt voor het voorvoegsel van de namen van bestanden die door deze bewerking uitvoer gegenereerd.

    ![Exporteren][cache-export-data]

    U kunt de voortgang van de exportbewerking door de meldingen van de Azure-portal of door het bekijken van de gebeurtenissen in de [controlelogboek](../azure-resource-manager/resource-group-audit.md).

    ![Gegevens exporteren voltooien][cache-export-data-export-complete]

    Caches blijven beschikbaar voor gebruik tijdens het exporteren.

## <a name="importexport-faq"></a>Veelgestelde vragen over het importeren/exporteren
Deze sectie bevat veelgestelde vragen over de functie voor importeren/exporteren.

* [Welke prijzen lagen kunt importeren/exporteren?](#what-pricing-tiers-can-use-importexport)
* [Kan ik gegevens importeren vanaf een willekeurige Redis-server?](#can-i-import-data-from-any-redis-server)
* [Welke versies RDB kan ik importeren?](#what-rdb-versions-can-i-import)
* [Mijn cache is beschikbaar tijdens een bewerking voor importeren/exporteren?](#is-my-cache-available-during-an-importexport-operation)
* [Kan ik voor importeren/exporteren met Redis-cluster gebruiken?](#can-i-use-importexport-with-redis-cluster)
* [Hoe werkt voor importeren/exporteren met een aangepaste databases instellen?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Hoe verschilt voor importeren/exporteren van Redis-persistentie?](#how-is-importexport-different-from-redis-persistence)
* [Kan ik automatiseren met behulp van PowerShell, CLI of andere clients management voor importeren/exporteren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Ik heb ontvangen een time-outfout tijdens de Import/Export-bewerking. Wat betekent dit?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Ik krijg een fout opgetreden bij het exporteren van mijn gegevens naar Azure Blob Storage. Wat is er gebeurd?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Welke prijzen lagen kunt importeren/exporteren?
Import/Export is alleen beschikbaar in de prijscategorie premium.

### <a name="can-i-import-data-from-any-redis-server"></a>Kan ik gegevens importeren vanaf een willekeurige Redis-server?
Ja, naast het importeren van gegevens van exemplaren van Azure Redis-Cache hebt geëxporteerd, kunt u RDB bestanden importeren vanaf een willekeurige Redis-server uitgevoerd in een cloud of de omgeving, zoals Linux, Windows, of cloud providers zoals Amazon Web Services. Upload het bestand RDB van de gewenste Redis-server naar een pagina of het blok-blob in Azure Storage-Account om dit te doen, en vervolgens importeren in uw premium Azure Redis-Cache-exemplaar. U wilt bijvoorbeeld de gegevens van uw cache productie exporteren en importeren in een cache die wordt gebruikt als onderdeel van een testomgeving voor testdoeleinden of migratie.

> [!IMPORTANT]
> Is om gegevens te importeren wanneer u een pagina-blob van Redis-servers dan de Azure Redis-Cache hebt geëxporteerd, moet het formaat van de blob worden afgestemd op de grens van een sectorgrootte van 512 bytes. Zie voor voorbeeldcode om uit te voeren van eventuele vereiste byte opvulling, [voorbeeld pagina blog uploaden](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Welke versies RDB kan ik importeren?

Azure Redis-Cache ondersteunt RDB importeren tot via RDB versie 7.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Mijn cache is beschikbaar tijdens een bewerking voor importeren/exporteren?
* **Exporteren** - Caches beschikbaar blijven en u kunt blijven gebruiken van uw cache tijdens een exportbewerking.
* **Importeren** - Caches niet beschikbaar wanneer een importbewerking wordt gestart en worden beschikbaar voor gebruik wanneer de importbewerking is voltooid.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Kan ik voor importeren/exporteren met Redis-cluster gebruiken?
Ja, en u kunt importeren/exporteren tussen een geclusterde cache en een niet-geclusterde cache. Sinds de Redis-cluster [alleen ondersteunt database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), alle gegevens in databases dan 0 is niet geïmporteerd. Wanneer geclusterde cache-gegevens worden geïmporteerd, wordt de sleutels herverdeeld over de shards van het cluster.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hoe werkt voor importeren/exporteren met een aangepaste databases instellen?
Sommige Prijscategorieën hebben verschillende [databases limieten](cache-configure.md#databases), dus er enkele overwegingen zijn bij het importeren als u een aangepaste waarde voor geconfigureerd de `databases` instellen tijdens het maken van de cache.

* Bij het importeren van een prijscategorie met een lagere `databases` limiet dan de laag die u hebt geëxporteerd:
  * Als u het standaardaantal `databases`, namelijk 16 voor alle Prijscategorieën, gegevens niet verloren.
  * Als u een aangepaste aantal `databases` dat valt binnen de grenzen voor de laag waarop u importeren wilt, gegevens niet verloren.
  * Als de geëxporteerde gegevens gegevens in een database die langer is dan de grenzen van de nieuwe laag bevat, worden de gegevens van die databases hoger is niet geïmporteerd.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Hoe verschilt voor importeren/exporteren van Redis-persistentie?
Azure Redis-Cache-persistentie kunt u voor het persistent maken van gegevens die zijn opgeslagen in Redis naar Azure Storage. Wanneer de persistentie is geconfigureerd, persistente Azure Redis-Cache een momentopname van de Redis-cache in een binaire indeling Redis op schijf op basis van een back-upfrequentie die kunnen worden geconfigureerd. Als er een onherstelbare gebeurtenis optreedt die zowel de primaire als de replica-cache wordt uitgeschakeld, wordt automatisch met behulp van de momentopname van de meest recente gegevens in de cache teruggezet. Zie voor meer informatie [gegevenspersistentie voor een Premium Azure Redis-Cache configureren](cache-how-to-premium-persistence.md).

Import / Export kunt u gegevens in of exporteren uit Azure Redis-Cache. Deze niet configureren, back-up en herstellen met behulp van Redis-persistentie.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kan ik automatiseren met behulp van PowerShell, CLI of andere clients management voor importeren/exporteren?
Ja, voor PowerShell instructies raadpleegt u [voor het importeren van een Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) en [exporteren van een Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ik heb ontvangen een time-outfout tijdens de Import/Export-bewerking. Wat betekent dit?
Als u blijven op de **gegevens importeren** of **gegevens exporteren** blade voor langer dan 15 minuten voordat de bewerking te starten, wordt er een fout opgetreden bij een foutbericht weergegeven dat vergelijkbaar is met het volgende voorbeeld:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Om dit probleem oplossen, initieert de import- of exportbewerking vóór 15 minuten zijn verstreken.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ik krijg een fout opgetreden bij het exporteren van mijn gegevens naar Azure Blob Storage. Wat is er gebeurd?
Export werkt alleen met RDB-bestanden die zijn opgeslagen als pagina-blobs. Andere typen blob worden momenteel niet ondersteund, met inbegrip van blob storage-accounts met hot en cool lagen. Zie voor meer informatie [Blob storage-accounts](../storage/common/storage-account-options.md#blob-storage-accounts).

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer Premiumfuncties cache.

* [Inleiding tot de Azure Redis-Cache Premium-laag](cache-premium-tier-intro.md)    

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
