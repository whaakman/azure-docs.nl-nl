---
title: Inleiding tot Azure Storage | Microsoft Docs
description: Inleiding tot Azure Storage, de gegevensopslag van Microsoft in de cloud.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: tamram
ms.openlocfilehash: e7b32aa2de5d6501e8d7894a936e9ab8b2f4f42f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-microsoft-azure-storage"></a>Inleiding tot Microsoft Azure Storage

Microsoft Azure Storage is een door Microsoft beheerde cloudservice waarmee u toegang krijgt tot opslag met een zeer hoge beschikbaarheid die daarnaast veilig, duurzaam, schaalbaar en redundant is. Microsoft zorgt voor het onderhoud en handelt kritieke problemen voor u af. 

Azure Storage omvat drie gegevensservices: Blob Storage, File Storage en Queue Storage. Blob Storage ondersteunt zowel Standard- als Premium-opslag, waarbij voor Premium-opslag alleen SSD's worden gebruikt om zo de snelste prestaties te leveren. Een andere functie is 'cool' opslag, waarmee u grote hoeveelheden zelden gebruikte gegevens kunt opslaan tegen lagere kosten.

In dit artikel komen de volgende onderwerpen aan bod:
* de Azure Storage-services
* de typen opslagaccounts
* toegang tot blobs, wachtrijen en bestanden
* versleuteling
* replicatie 
* gegevens overbrengen van of naar opslag
* de verschillende opslagclientbibliotheken 

Raadpleeg een van de volgende Quick Starts om snel aan de slag te gaan met Azure Storage:
* [Een opslagaccount maken met PowerShell](storage-quickstart-create-storage-account-powershell.md)
* [Een opslagaccount maken met CLI](storage-quickstart-create-storage-account-cli.md)

## <a name="introducing-the-azure-storage-services"></a>Introductie van de Azure Storage-services

U kunt de services van Azure Storage (Blob Storage, File Storage en Queue Storage) pas gebruiken nadat u een opslagaccount hebt gemaakt. Vervolgens kunt u gegevens uit een specifieke service van of naar dat opslagaccount overbrengen. 

## <a name="blob-storage"></a>Blob Storage

Blobs zijn eigenlijk net bestanden zoals u die opslaat op uw computer (of tablet, mobiele apparaat, enzovoort). Ze kunnen afbeeldingen bevatten, Microsoft Excel-bestanden, HTML-bestanden, virtuele harde schijven (VHD's), big data zoals logboeken, back-ups van databases, eigenlijk bijna alles. Blobs worden opgeslagen in containers, die vergelijkbaar zijn met mappen. 

Nadat u bestanden hebt opgeslagen in Blob Storage, kunt u ze overal ter wereld openen met behulp van URL's, de REST-interface of een van de opslagclientbibliotheken uit de Azure SDK. Deze clientbibliotheken zijn beschikbaar voor meerdere talen, waaronder Node.js, Java, PHP, Ruby, Python en .NET. 

Er zijn drie typen blobs: blok-blobs, pagina-blobs (gebruikt voor VHD-bestanden) en toevoeg-blobs.

* Blok-blobs worden gebruikt voor het opslaan van gewone bestanden tot ongeveer 4,7 TB. 
* Pagina-blobs worden gebruikt voor het opslaan van bestanden voor willekeurige toegang tot maximaal 8 TB in grootte. Deze blobs worden gebruikt voor de VHD-bestanden die VM's ondersteunen.
* Toevoeg-blobs bestaan uit blokken zoals de blok-blobs, maar zijn geoptimaliseerd voor toevoegbewerkingen. Deze blobs worden gebruikt voor bewerkingen zoals het vastleggen van logboekgegevens uit verschillende VM's in dezelfde blob.

Voor zeer grote gegevenssets waarbij netwerkbeperkingen het downloaden of uploaden van gegevens van of naar Blob Storage via de kabel onrealistisch maken, kunt u een set harde schijven opsturen naar Microsoft om gegevens rechtstreeks in het datacenter te importeren of exporteren. Zie [De Microsoft Azure Import/Export-service gebruiken om gegevens over te brengen naar Blob Storage](../storage-import-export-service.md).

## <a name="azure-files"></a>Azure Files
Met [Azure Files](../files/storage-files-introduction.md) kunt u zeer netwerkbestandsshares met een hoge beschikbaarheid instellen die toegankelijk zijn via het standaard SMB-protocol (Server Message Block). Dit betekent dat meerdere VM's dezelfde bestanden kunnen delen met zowel lees- als schrijftoegang. U kunt de bestanden ook lezen met behulp van de REST-interface of de opslagclientbibliotheken. 

Eén ding dat Azure Files onderscheidt van bestanden in een zakelijke bestandsshare is het feit dat u overal ter wereld toegang hebt tot de bestanden via een URL die verwijst naar het bestand en een SAS-token (Shared Access Signature) bevat. U kunt SAS-tokens genereren. Met deze tokens hebt u gedurende een opgegeven tijdperiode toegang tot een specifiek privéasset. 

Bestandsshares kunnen worden gebruikt voor veelvoorkomende scenario's: 

* Veel on-premises toepassingen maken gebruik van bestandsshares. Met deze functie kunt u gemakkelijker toepassingen die gegevens delen, migreren naar Azure. Als u de bestandsshare koppelt aan dezelfde stationsletter die wordt gebruikt voor de on-premises toepassing, werkt het gedeelte van de toepassing dat toegang heeft tot de bestandsshare, (vrijwel) ongewijzigd.

* Configuratiebestanden kunnen worden opgeslagen in een bestandsshare en zijn toegankelijk vanaf meerdere VM's. Hulpprogramma's en hulpmiddelen die worden gebruikt door meerdere ontwikkelaars in een groep, kunnen worden opgeslagen in een bestandsshare. Hierdoor kan iedereen ze vinden en maakt iedereen ook gebruik van dezelfde versie.

* Diagnostische logboeken, metrische gegevens en crashdumps zijn slechts drie voorbeelden van gegevens die naar een bestandsshare kunnen worden geschreven, en later verwerkt of geanalyseerd.

Op dit moment worden verificatie op basis van Active Directory en toegangsbeheerlijsten (ACL's) niet ondersteund, maar ondersteuning hiervan wordt in de toekomst beschikbaar. De opslagaccountreferenties worden gebruikt voor verificatie voor toegang tot de bestandsshare. Dit betekent dat iedereen met de gekoppelde share volledige lees-/schrijftoegang tot de share heeft.

## <a name="queue-storage"></a>Queue Storage

De Azure Queue-service wordt gebruikt voor het opslaan en ophalen van berichten. Berichten in de wachtrij kunnen maximaal 64 kB groot zijn, en een wachtrij kan miljoenen berichten bevatten. Wachtrijen worden meestal gebruikt voor het opslaan van lijsten met berichten die asynchroon moeten worden verwerkt. 

Stel dat u uw klanten in de gelegenheid wilt stellen om afbeeldingen te uploaden en dat u voor elke afbeelding miniaturen wilt maken. U kunt uw klant dan laten wachten totdat u tijdens het uploaden van de afbeeldingen de miniaturen hebt gemaakt. Een alternatief is het inzetten van een wachtrij. Wanneer de klant klaar is met uploaden, wordt er een bericht weggeschreven naar de wachtrij. Vervolgens gebruikt u Azure Function om het bericht op te halen uit de wachtrij en de miniaturen te maken. Al deze verwerkingsstappen kunnen afzonderlijk worden geschaald, waardoor u meer controle hebt bij het afstemmen van de procedure op uw specifieke scenario.

## <a name="table-storage"></a>Table Storage

De Standard-versie van Azure Table Storage maakt nu deel uit van Cosmos DB. Voor documentatie raadpleegt u [Overzicht van Azure Table Storage](../../cosmos-db/table-storage-overview.md). Er is ook een premium-versie van Azure Table Storage beschikbaar, met tabellen die voor doorvoer zijn geoptimaliseerd, globale distributie en automatische secundaire indexen. Bekijk [Cosmos-DB Azure: tabel-API](https://aka.ms/premiumtables) voor meer informatie en om de nieuwe premium versie uit te proberen.

## <a name="disk-storage"></a>File Storage

Azure Storage omvat ook mogelijkheden voor beheerde en onbeheerde schijven die worden gebruikt door virtuele machines. Zie voor meer informatie over deze functies de [documentatie over de Compute-service](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts 

In deze tabel ziet u de verschillende soorten opslagaccounts en welke objecten met elk account kunnen worden gebruikt.

|**Type opslagaccount**|**Algemeen Standard**|**Algemeen Premium**|**Blob-opslag, 'hot' en 'cool' toegangslagen**|
|-----|-----|-----|-----|
|**Ondersteunde services**| Blob-, File- en Queue-services | Blob-service | Blob-service|
|**Typen ondersteunde blobs**|Blok-blobs, pagina-blobs en toevoeg-blobs | Pagina-blobs | Blok-blobs en toevoeg-blobs|

### <a name="general-purpose-storage-accounts"></a>Opslagaccounts voor algemeen gebruik

Er zijn twee soorten opslagaccounts voor algemeen gebruik. 

#### <a name="standard-storage"></a>Standard Storage 

De meest gebruikte opslagaccounts zijn standaardopslagaccounts, die kunnen worden gebruikt voor alle typen gegevens. Standaardopslagaccounts maken gebruiken van magnetische media voor het opslaan van gegevens.

#### <a name="premium-storage"></a>Premium Storage

Premium-opslag biedt krachtige opslag voor pagina-blobs, die voornamelijk worden gebruikt voor VHD-bestanden. Premium-opslagaccounts gebruiken SSD voor het opslaan van gegevens. Microsoft adviseert om Premium Storage te gebruiken voor al uw VM's.

### <a name="blob-storage-accounts"></a>Blob Storage-accounts

Het Blob Storage-account is een gespecialiseerd opslagaccount dat wordt gebruikt voor het opslaan van blok-blobs en toevoeg-blobs. U kunt geen pagina-blobs opslaan in deze accounts en dus ook geen VHD-bestanden. Deze accounts maken het mogelijk om een toegangslaag in te stellen als Hot of Cool. U kunt deze instelling overigens op elk gewenst moment wijzigen. 

De toegangslaag Hot wordt gebruikt voor bestanden die regelmatig worden geopend. U betaalt hogere kosten voor opslag, maar de kosten voor toegang tot de blobs zijn veel lager. Voor blobs die zijn opgeslagen in de toegangslaag Cool, betaalt u hogere kosten voor toegang tot de blobs, maar zijn de kosten voor opslag veel lager.

## <a name="accessing-your-blobs-files-and-queues"></a>Toegang tot blobs, bestanden en wachtrijen

Elk opslagaccount heeft twee verificatiesleutels, die allebei voor elke bewerking kunnen worden gebruikt. Er zijn twee sleutels, zodat u de sleutels af en toe kunt afwisselen om de beveiliging te verbeteren. Het is essentieel dat deze sleutels geheim blijven aangezien iemand met kennis van deze sleutels, plus de accountnaam, onbeperkte toegang heeft tot alle gegevens in het opslagaccount. 

In deze sectie kijken we naar twee manieren om het opslagaccount en de bijbehorende gegevens te beveiligen. Raadpleeg de Engelstalige [Azure Storage-beveiligingshandleiding](storage-security-guide.md) voor gedetailleerde informatie over het beveiligen van uw opslagaccount en uw gegevens.

### <a name="securing-access-to-storage-accounts-using-azure-ad"></a>Toegang tot opslagaccounts beveiligen met Azure AD

Eén manier om de toegang tot uw opgeslagen gegevens te beveiligen, is door de toegang tot de sleutels voor het opslagaccount te beheren. Met op rollen gebaseerd toegangsbeheer (RBAC) van Resource Manager kunt u rollen toewijzen aan gebruikers, groepen of toepassingen. Deze rollen zijn gekoppeld aan een specifieke set acties die al dan niet worden toegestaan. Als u RBAC gebruikt om toegang te verlenen tot een opslagaccount, worden alleen de beheerbewerkingen voor dat opslagaccount afgehandeld, zoals het wijzigen van de toegangslaag. U kunt RBAC niet gebruiken om toegang te verlenen tot gegevensobjecten zoals een bepaalde container of bestandsshare. U kunt RBAC echter wel gebruiken om toegang te verlenen tot de sleutels voor het opslagaccount, die vervolgens kunnen worden gebruikt om de gegevensobjecten te lezen. 

### <a name="securing-access-using-shared-access-signatures"></a>Toegang beveiligen met handtekeningen voor gedeelde toegang 

U kunt handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid gebruiken om uw gegevensobjecten te beveiligen. Een handtekening voor gedeelde toegang (SAS) is een tekenreeks met een beveiligingstoken dat kan worden toegevoegd aan de URI voor een asset, zodat u toegang tot specifieke opslagobjecten kunt delegeren en beperkingen kunt opgeven zoals machtigingen en het datum- en tijdbereik van toegang. Deze functie heeft uitgebreide mogelijkheden. Zie [Using shared access signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Handtekeningen voor gedeelde toegang (SAS) gebruiken) voor meer informatie.

### <a name="public-access-to-blobs"></a>Openbare toegang tot blobs

Via de Blob Service kunt u openbare toegang bieden tot een container en de bijbehorende blobs, of een specifieke blob. Wanneer u opgeeft dat een container of blob openbaar is, kan iedereen deze anoniem lezen. Er is dan geen verificatie vereist. Dit is bijvoorbeeld handig wanneer u een website hebt waarop afbeeldingen, video of documenten uit Blob Storage worden gebruikt. Zie [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Anonieme leestoegang tot containers en blobs beheren) voor meer informatie. 

## <a name="encryption"></a>Versleuteling

Er zijn een paar eenvoudige typen versleuteling beschikbaar voor de Storage-services. 

### <a name="encryption-at-rest"></a>Versleuteling 'at rest' 

U kunt SSE (Storage Service Encryption) inschakelen voor de Files-service (preview) of de Blob-service voor een Azure-opslagaccount. Als u SSE inschakelt, worden alle gegevens die worden weggeschreven naar de specifieke service eerst versleuteld. Wanneer u de gegevens leest, worden deze ontsleuteld en dan pas geretourneerd. 

### <a name="client-side-encryption"></a>Clientversleuteling

De opslagclientbibliotheken ondersteunen methoden die u kunt aanroepen om gegevens programmatisch te versleutelen voordat deze via de kabel worden verzonden van de client naar Azure. De gegevens worden versleuteld opgeslagen, wat betekent dat ze ook 'at rest' zijn versleuteld. Bij het opnieuw lezen van de gegevens, worden de gegevens na ontvangst ontsleuteld. 

### <a name="encryption-in-transit-with-azure-file-shares"></a>Versleuteling tijdens overdracht met Azure-bestandsshares

Zie [Using Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md) (Shared Access Signatures (SAS) gebruiken) voor meer informatie over handtekeningen voor gedeelde toegang. Zie [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md) en [Verificatie voor de Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179428.aspx) voor meer informatie over veilige toegang tot uw Storage-account.

Raadpleeg de Engelstalige [Azure Storage-beveiligingshandleiding](storage-security-guide.md) voor meer informatie over het beveiligen van uw opslagaccount en uw gegevens.

## <a name="replication"></a>Replicatie

Om ervoor te zorgen dat uw gegevens duurzaam zijn, heeft Azure Storage de mogelijkheid om meerdere kopieën van uw gegevens te onderhouden (en beheren). Dit wordt replicatie genoemd, en soms redundantie. Als u uw opslagaccount gaat instellen, selecteert u een type replicatie. In de meeste gevallen kan deze instelling worden gewijzigd nadat het opslagaccount is ingesteld. 

Alle opslagaccounts hebben **lokaal redundante opslag (LRS)**. Dit betekent dat er drie kopieën van uw gegevens worden beheerd door Azure Storage in het datacenter dat is opgegeven tijdens de configuratie van het opslagaccount. Wanneer er in één exemplaar wijzigingen worden vastgelegd, worden de andere twee kopieën bijgewerkt en wordt pas daarna de bewerking als voltooid gezien. Dit betekent dat de drie replica's altijd zijn gesynchroniseerd. Bovendien zijn de drie kopieën opgeslagen in afzonderlijke foutdomeinen en upgrade-domeinen, wat inhoudt dat de gegevens zelfs beschikbaar zijn als een opslagknooppunt met uw gegevens uitvalt of offline wordt gezet om te worden bijgewerkt. 

**Lokaal redundante opslag (LRS)**

Zoals hierboven is uitgelegd, beschikt u met LRS over drie kopieën van uw gegevens in één datacenter. Hierdoor is het probleem opgelost dat gegevens niet meer beschikbaar zijn als er een opslagknooppunt uitvalt of offline wordt gezet om te worden bijgewerkt. Er is echter wel een probleem als het hele datacenter niet meer beschikbaar is.

**Zone-redundante opslag (ZRS)**

Zone-redundante opslag (ZRS) zorgt er niet alleen voor dat de drie lokale kopieën van uw gegevens worden onderhouden, maar ook nog een andere set met drie kopieën van uw gegevens. De tweede set met drie kopieën wordt asynchroon gerepliceerd tussen datacenters binnen één of twee regio's. ZRS is alleen beschikbaar voor blok-blobs in opslagaccounts voor algemeen gebruik. Nadat u uw opslagaccount hebt gemaakt en ZRS hebt geselecteerd, kunt u het account niet meer wijzigen in een ander type replicatie, en omgekeerd.

ZRS-accounts bieden een hogere duurzaamheid dan LRS, maar ZRS-accounts ondersteunen geen metrische gegevens of logboekregistratie. 

**Geografisch redundante opslag (GRS)**

Geografisch redundante opslag (GRS) onderhoudt de drie lokale kopieën van uw gegevens in een primaire regio, plus een andere set met drie kopieën van uw gegevens in een secundaire regio, op honderden kilometers afstand van de primaire regio. Als er een storing optreedt in de primaire regio, wordt er door Azure Storage een failover uitgevoerd naar de secundaire regio. 

**Geografisch redundante opslag met leestoegang (RA-GRS)** 

Geografisch redundante opslag met leestoegang is exact hetzelfde als GRS, met als enige verschil dat u leestoegang krijgt tot de gegevens op de secundaire locatie. Als het primaire datacenter tijdelijk niet beschikbaar is, kunt u de gegevens blijven lezen vanaf de secundaire locatie. Dit kan zeer nuttig zijn. U kunt voor een webtoepassing dan bijvoorbeeld instellen dat die moet overschakelen naar de modus Alleen-lezen en verwijzen naar de secundaire kopie, zodat er nog enige toegang mogelijk is, zelfs als updates niet beschikbaar zijn. 

> [!IMPORTANT]
> U kunt wijzigen hoe uw gegevens worden gerepliceerd nadat uw opslagaccount is gemaakt, tenzij u ZRS hebt opgegeven tijdens het maken van het account. Er worden mogelijk eenmalig extra kosten in rekening gebracht voor de overdracht van gegevens als u overschakelt van LRS naar GRS of RA-GRS.
>

Zie [Azure Storage-replicatie](storage-redundancy.md) voor meer informatie over replicatie.

Zie [Wat te doen in het geval van een Azure Storage-storing](storage-disaster-recovery-guidance.md) voor informatie over herstel na noodgevallen.

Zie [Maximaal beschikbare toepassingen met RA-GRS ontwerpen](storage-designing-ha-apps-with-ragrs.md) voor een voorbeeld van het inzetten van RA-GRS-opslag om hoge beschikbaarheid te garanderen.

## <a name="transferring-data-to-and-from-azure-storage"></a>Gegevens overbrengen van en naar Azure Storage

U kunt het opdrachtregelhulpprogramma AzCopy gebruiken om blob- en bestandsgegevens binnen uw opslagaccount of tussen opslagaccounts te kopiëren. Raadpleeg een van de volgende artikelen voor informatie:

* [Transfer data with the AzCopy on Windows](storage-use-azcopy.md) (Gegevens overdragen met AzCopy voor Windows)
* [Transfer data with AzCopy on Linux](storage-use-azcopy-linux.md) (Gegevens overdragen met AzCopy voor Linux)

AzCopy is gebouwd boven op de [Azure-bibliotheek voor gegevensverplaatsing](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), die momenteel beschikbaar is als voorbeeld.

De Azure Import/Export-service kan worden gebruikt om grote hoeveelheden blob-gegevens naar of van uw opslagaccount te importeren of exporteren. U bereidt meerdere harde schijven voor en verstuurt die naar een Azure-datacenter, waar de gegevens vervolgens worden overgezet naar/van de harde schijven en de harde schijven vervolgens worden teruggestuurd. Zie [De Microsoft Azure Import/Export-service gebruiken om gegevens over te brengen naar Blob Storage](../storage-import-export-service.md) voor meer informatie over de Import/Export-service.

## <a name="pricing"></a>Prijzen

Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor gedetailleerde informatie over prijzen voor Azure Storage.

## <a name="storage-apis-libraries-and-tools"></a>Storage-API's, -bibliotheken en -hulpprogramma's
Azure Storage-resources zijn toegankelijk voor elke taal waarvoor HTTP/HTTPS-aanvragen mogelijk zijn. Daarnaast biedt Azure Storage programmeringsbibliotheken voor verschillende veelgebruikte talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure Storage door bewerkingen zoals synchrone en asynchrone aanroepafhandeling, batchverwerking van bewerkingen, uitzonderingsbeheer, automatische nieuwe pogingen, werking, enzovoort, voor hun rekening te nemen. Bibliotheken zijn momenteel beschikbaar voor de volgende talen en platformen (deze lijst wordt in de toekomst uitgebreid):

### <a name="azure-storage-data-services"></a>Azure Storage-gegevensservices
* [REST-API voor Storage-services](/rest/api/storageservices/)
* [Opslagclientbibliotheek voor .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Opslagclientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp)
* [Opslagclientbibliotheek voor Java/Android](https://azure.microsoft.com/develop/java/)
* [Opslagclientbibliotheek voor Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Opslagclientbibliotheek voor PHP](https://azure.microsoft.com/develop/php/)
* [Opslagclientbibliotheek voor Python](https://azure.microsoft.com/develop/python/)
* [Opslagclientbibliotheek voor Ruby](https://azure.microsoft.com/develop/ruby/)
* [Opslag-cmdlets voor PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)
* [Opslagopdrachten voor CLI 2.0](/cli/azure/storage)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Blob Storage](../blobs/storage-blobs-introduction.md)
* [Meer informatie over File Storage](../storage-files-introduction.md)
* [Meer informatie over Queue Storage](../queues/storage-queues-introduction.md)

Raadpleeg een van de volgende Quick Starts om snel aan de slag te gaan met Azure Storage:
* [Een opslagaccount maken met PowerShell](storage-quickstart-create-storage-account-powershell.md)
* [Een opslagaccount maken met CLI](storage-quickstart-create-storage-account-cli.md)

<!-- FIGURE OUT WHAT TO DO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for the following languages and platforms, with others in the pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
To learn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

-->

### <a name="for-administrators"></a>Voor beheerders
* [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md)
* [Azure CLI gebruiken met Azure Storage](../storage-azure-cli.md)

### <a name="for-net-developers"></a>Voor .NET-ontwikkelaars
* [Aan de slag met Azure Blob Storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Ontwikkelen voor Azure Files met .NET](../files/storage-dotnet-how-to-use-files.md)
* [Aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Aan de slag met Azure Queue Storage met .NET](../storage-dotnet-how-to-use-queues.md)

### <a name="for-javaandroid-developers"></a>Voor Java/Android-ontwikkelaars
* [Blob Storage gebruiken met Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Ontwikkelen voor Azure Files met Java](../files/storage-java-how-to-use-file-storage.md)
* [Table Storage gebruiken met Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Queue Storage gebruiken met Java](../storage-java-how-to-use-queue-storage.md)

### <a name="for-nodejs-developers"></a>Voor Node.js-ontwikkelaars
* [Blob Storage gebruiken met Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Table Storage gebruiken met Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Queue Storage gebruiken met Node.js](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Voor PHP-ontwikkelaars
* [Blob Storage gebruiken met PHP](../blobs/storage-php-how-to-use-blobs.md)
* [Table Storage gebruiken met PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Queue Storage gebruiken met PHP](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Voor Ruby-ontwikkelaars
* [Blob Storage gebruiken met Ruby](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Table Storage gebruiken met Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Queue Storage gebruiken met Ruby](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Voor Python-ontwikkelaars
* [Blob Storage gebruiken met Python](../blobs/storage-python-how-to-use-blob-storage.md)
* [Ontwikkelen voor Azure Files met Python](../files/storage-python-how-to-use-file-storage.md)
* [Table Storage gebruiken met Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Queue Storage gebruiken met Python](../storage-python-how-to-use-queue-storage.md)