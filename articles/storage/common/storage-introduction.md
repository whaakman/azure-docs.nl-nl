---
title: Inleiding tot Azure Storage - cloudopslag in Azure | Microsoft Docs
description: Azure Storage is Microsoft's oplossing voor opslag in de cloud. Azure Storage biedt opslag voor gegevensobjecten die maximaal beschikbaar, veilig, duurzaam, in hoge mate schaalbaar en redundant is.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ae2384d0ac6773ccd362778d2913cdcaa9cb4d6c
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446710"
---
# <a name="introduction-to-azure-storage"></a>Kennismaking met Azure Storage

Azure Storage is Microsoft's cloudoplossing bedoeld voor scenario's voor gegevensopslag. Azure Storage biedt een in hoge mate schaalbare opslag voor gegevensobjecten, een bestandssysteemservice voor de cloud, een berichtenarchief voor betrouwbaar gebruik van berichten en een NoSQL-archief. Azure Storage is:

- **Duurzaam en maximaal beschikbaar.** Redundantie zorgt ervoor dat uw gegevens veilig zijn in geval van tijdelijke hardwarefouten. U kunt er ook voor kiezen gegevens te repliceren in datacenters of geografische regio's voor extra beveiliging tegen lokale rampen of natuurrampen. Op deze manier gerepliceerde gegevens blijven maximaal beschikbaar in het geval van een stroomstoring. 
- **Veilig.** Alle gegevens die naar Azure Storage worden geschreven, worden versleuteld door de service. Azure Storage biedt u gedetailleerde controle over wie toegang tot uw gegevens heeft.
- **Schaalbaar.** Azure Storage is in hoge mate schaalbaar om te voldoen aan de gegevensopslag- en prestatiebehoeften van de huidige toepassingen. 
- **Beheerd.** Microsoft Azure zorgt voor het onderhoud van hardware, updates en het oplossen van ernstige problemen.
- **Toegankelijk.** Gegevens in Azure Storage zijn overal ter wereld toegankelijk via HTTP of HTTPS. Microsoft biedt SDK's voor Azure Storage in verschillende talen, te weten .NET, Java, Node.js, Python, PHP, Ruby, Go en andere, samen met een goed ontwikkelde REST-API. Azure Storage ondersteunt scripts in Azure PowerShell of Azure CLI. En Azure Portal en Azure Storage Explorer bieden handige visuele oplossingen voor het werken met uw gegevens.  

## <a name="azure-storage-services"></a>Azure Storage-services

Azure Storage omvat deze gegevensservices: 

- [Azure-blobs](../blobs/storage-blobs-introduction.md): Een in hoge mate schaalbaar objectarchief voor tekst en binaire gegevens.
- [Azure-bestanden](../files/storage-files-introduction.md): Beheerde bestandsshares voor implementaties in de cloud of on-premises.
- [Azure-wachtrijen](../queues/storage-queues-introduction.md): Een berichtenarchief voor betrouwbare uitwisseling van berichten tussen toepassingsonderdelen. 
- [Azure-tabellen](../tables/table-storage-overview.md): Een NoSQL-archief voor schemaloze opslag van gestructureerde gegevens.

Elke service kan worden geopend via een opslagaccount. Zie [Een opslagaccount maken](storage-quickstart-create-account.md) om aan de slag te gaan.

## <a name="blob-storage"></a>Blob Storage

Azure Blob Storage is Microsoft's oplossing voor opslag van objecten in de cloud. Blob Storage is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens. 

Blob-opslag is ideaal voor:

* Het rechtstreeks aan een browser leveren van afbeeldingen of documenten.
* De opslag van bestanden voor gedistribueerde toegang.
* Streaming van video en audio.
* De opslag van gegevens voor back-up en herstel, herstel na noodgevallen en archivering.
* De opslag van gegevens voor analyse door een on-premises of in Azure gehoste service.

Objecten in Blob-opslag zijn overal ter wereld toegankelijk via HTTP of HTTPS. Gebruikers of clienttoepassingen hebben toegang tot blobs via URL's, de [REST-API van Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) of een Azure Storage-clientbibliotheek. Deze clientbibliotheken zijn beschikbaar voor meerdere talen, waaronder [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) en [Ruby](https://azure.github.io/azure-storage-ruby).

Zie [Inleiding tot blob-opslag](../blobs/storage-blobs-introduction.md) voor meer informatie over blob-opslag.

## <a name="azure-files"></a>Azure Files
Met [Azure Files](../files/storage-files-introduction.md) kunt u zeer netwerkbestandsshares met een hoge beschikbaarheid instellen die toegankelijk zijn via het standaard SMB-protocol (Server Message Block). Dit betekent dat meerdere VM's dezelfde bestanden kunnen delen met zowel lees- als schrijftoegang. U kunt de bestanden ook lezen met behulp van de REST-interface of de opslagclientbibliotheken.

Eén ding dat Azure Files onderscheidt van bestanden in een zakelijke bestandsshare is het feit dat u overal ter wereld toegang hebt tot de bestanden via een URL die verwijst naar het bestand en een SAS-token (Shared Access Signature) bevat. U kunt SAS-tokens genereren. Met deze tokens hebt u gedurende een opgegeven tijdperiode toegang tot een specifiek privéasset.

Bestandsshares kunnen worden gebruikt voor veelvoorkomende scenario's:

* Veel on-premises toepassingen maken gebruik van bestandsshares. Met deze functie kunt u gemakkelijker toepassingen die gegevens delen, migreren naar Azure. Als u de bestandsshare koppelt aan dezelfde stationsletter die wordt gebruikt voor de on-premises toepassing, werkt het gedeelte van de toepassing dat toegang heeft tot de bestandsshare, (vrijwel) ongewijzigd.

* Configuratiebestanden kunnen worden opgeslagen in een bestandsshare en zijn toegankelijk vanaf meerdere VM's. Hulpprogramma's en hulpmiddelen die worden gebruikt door meerdere ontwikkelaars in een groep, kunnen worden opgeslagen in een bestandsshare. Hierdoor kan iedereen ze vinden en maakt iedereen ook gebruik van dezelfde versie.

* Diagnostische logboeken, metrische gegevens en crashdumps zijn slechts drie voorbeelden van gegevens die naar een bestandsshare kunnen worden geschreven, en later verwerkt of geanalyseerd.

Op dit moment worden verificatie op basis van Active Directory en toegangsbeheerlijsten (ACL's) niet ondersteund, maar ondersteuning hiervan wordt in de toekomst beschikbaar. De opslagaccountreferenties worden gebruikt voor verificatie voor toegang tot de bestandsshare. Dit betekent dat iedereen met de gekoppelde share volledige lees-/schrijftoegang tot de share heeft.

Raadpleeg de [Inleiding tot Azure Files](../files/storage-files-introduction.md) voor meer informatie over Azure Files.

## <a name="queue-storage"></a>Queue Storage

De Azure Queue-service wordt gebruikt voor het opslaan en ophalen van berichten. Berichten in de wachtrij kunnen maximaal 64 kB groot zijn, en een wachtrij kan miljoenen berichten bevatten. Wachtrijen worden meestal gebruikt voor het opslaan van lijsten met berichten die asynchroon moeten worden verwerkt.

Stel dat u uw klanten in de gelegenheid wilt stellen om afbeeldingen te uploaden en dat u voor elke afbeelding miniaturen wilt maken. U kunt uw klant dan laten wachten totdat u tijdens het uploaden van de afbeeldingen de miniaturen hebt gemaakt. Een alternatief is het inzetten van een wachtrij. Wanneer de klant klaar is met uploaden, wordt er een bericht weggeschreven naar de wachtrij. Vervolgens gebruikt u Azure Function om het bericht op te halen uit de wachtrij en de miniaturen te maken. Al deze verwerkingsstappen kunnen afzonderlijk worden geschaald, waardoor u meer controle hebt bij het afstemmen van de procedure op uw specifieke scenario.

Raadpleeg de [Inleiding tot Azure Queues](../queues/storage-queues-introduction.md) voor meer informatie over Azure Queues.

## <a name="table-storage"></a>Table Storage

Azure Table Storage maakt nu deel uit van Cosmos DB. Voor documentatie over Azure Table Storage raadpleegt u [Overzicht van Azure Table Storage](../tables/table-storage-overview.md). Naast de bestaande Azure Table Storage-service is er een nieuwe Azure Cosmos DB tabel-API die voor doorvoer geoptimaliseerde tabellen, wereldwijde distributie en automatische secundaire indexen biedt. Bekijk [Azure Cosmos DB: tabel-API](https://aka.ms/premiumtables) voor meer informatie en om de nieuwe premium versie uit te proberen.

Zie [Overzicht van Azure Table Storage](../tables/table-storage-overview.md) voor meer informatie over Table Storage.

## <a name="disk-storage"></a>File Storage

Azure Storage omvat ook mogelijkheden voor beheerde en onbeheerde schijven die worden gebruikt door virtuele machines. Zie voor meer informatie over deze functies de [documentatie over de Compute-service](https://docs.microsoft.com/azure/#pivot=products&panel=Compute).

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

Zie [Overzicht van Azure-opslagaccounts](storage-account-overview.md) voor meer informatie over de typen opslagaccounts. 

## <a name="securing-access-to-storage-accounts"></a>Toegang tot opslagaccounts beveiligen

Elke aanvraag naar Azure Storage moet worden toegestaan. Azure Storage ondersteunt de volgende autorisatiemethoden:

- **Integratie van Azure Active Directory (Azure AD) voor blobopslag-en wachtrij.** Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure AD-referenties voor de Blob en Queue-services via op rollen gebaseerd toegangsbeheer (RBAC). Goedkeuren van aanvragen met Azure AD wordt aanbevolen voor uitstekende beveiliging en gebruiksgemak. Zie voor meer informatie, [verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory](storage-auth-aad.md).
- **Azure AD autorisatie via SMB voor Azure Files (preview).** Azure Files biedt ondersteuning voor verificatie op basis van identiteit via SMB (Server Message Block) via Azure Active Directory Domain Services. Uw domein Windows virtuele machines (VM's) hebben toegang tot Azure-bestandsshares met behulp van Azure AD-referenties. Zie voor meer informatie, [overzicht van Azure Active Directory-autorisatie via SMB voor Azure Files (preview)](../files/storage-files-active-directory-overview.md).
- **Verificatie met gedeelde sleutel.** De services van Azure Storage-Blob, wachtrijen en tabellen en Azure Files ondersteuning voor verificatie met gedeelde Key.A-client met behulp van gedeelde sleutel autorisatie geeft een koptekst met elke aanvraag die is ondertekend met behulp van de toegangssleutel voor opslagaccount. Zie voor meer informatie, [autoriseren met gedeelde sleutel](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Met behulp van autorisatie shared access signatures (SAS).** Een shared access signature (SAS) is een tekenreeks met een beveiligingstoken dat kan worden toegevoegd aan de URI voor een opslagresource. Het beveiligingstoken ingekapseld beperkingen, zoals machtigingen en het interval van toegang. Raadpleeg voor meer informatie, [met behulp van Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Anonieme toegang tot containers en blobs.** Het is mogelijk dat een container en de blobs openbaar beschikbaar. Wanneer u opgeeft dat een container of blob openbaar is, kan iedereen deze anoniem lezen; Er is geen verificatie vereist. Zie [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Anonieme leestoegang tot containers en blobs beheren) voor meer informatie.

## <a name="encryption"></a>Versleuteling

Er zijn twee basistypen versleuteling beschikbaar voor de Storage-services. Raadpleeg de Engelstalige [Azure Storage-beveiligingshandleiding](storage-security-guide.md) voor meer informatie over beveiliging en versleuteling.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'

Azure SSE (Storage Service Encryption) ondersteunt versleuteling 'at rest' om uw gegevens te beschermen en te beveiligen, zodat u aan de beveiligings- en nalevingsafspraken van uw organisatie voldoet. Met deze functie worden uw gegevens automatisch versleuteld door Azure Storage voordat deze worden opgeslagen en ontsleutelt voordat ze weer worden opgehaald. De processen van versleuteling, ontsleuteling en sleutelbeheer zijn volledig transparant voor gebruikers.


SSE versleutelt automatisch gegevens in alle prestatielagen (Standaard en Premium), alle implementatiemodellen (Azure Resource Manager en het klassieke model) en alle services van Azure Storage (Blob, Queue, Table en File). SSE heeft geen invloed op de prestaties van Azure Storage.

Meer informatie over SSE-versleuteling 'at rest' vindt u in [Azure Storage Service-versleuteling voor inactieve gegevens](storage-service-encryption.md).

### <a name="client-side-encryption"></a>Clientversleuteling

De opslagclientbibliotheken ondersteunen methoden die u kunt aanroepen om gegevens programmatisch te versleutelen voordat deze via de kabel worden verzonden van de client naar Azure. De gegevens worden versleuteld opgeslagen, wat betekent dat ze ook 'at rest' zijn versleuteld. Bij het opnieuw lezen van de gegevens, worden de gegevens na ontvangst ontsleuteld.

Zie [Versleuteling aan clientzijde met .NET voor Microsoft Azure Storage](storage-client-side-encryption.md) voor meer informatie over versleuteling op de client.

## <a name="redundancy"></a>Redundantie

Om ervoor te zorgen dat uw gegevens duurzaam zijn, worden in Azure Storage meerdere kopieën van uw gegevens gerepliceerd. Als u uw opslagaccount gaat instellen, selecteert u een type replicatie. 

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Zie voor meer informatie over herstel na noodgevallen [Herstel na noodgevallen en failover van opslagaccounts (voorbeeld) in Azure Storage](storage-disaster-recovery-guidance.md).

## <a name="transferring-data-to-and-from-azure-storage"></a>Gegevens overbrengen van en naar Azure Storage

Er zijn verschillende mogelijkheden om gegevens te verplaatsen van of naar Azure Storage. Welke optie u kiest, is afhankelijk van de grootte van de gegevensset en de bandbreedte van het netwerk. Zie [Een Azure-oplossing kiezen voor gegevensoverdracht](storage-choose-data-transfer-solution.md) voor meer informatie.

## <a name="pricing"></a>Prijzen

Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/) voor gedetailleerde informatie over prijzen voor Azure Storage.

## <a name="storage-apis-libraries-and-tools"></a>Storage-API's, -bibliotheken en -hulpprogramma's
Azure Storage-resources zijn toegankelijk voor elke taal waarvoor HTTP/HTTPS-aanvragen mogelijk zijn. Daarnaast biedt Azure Storage programmeringsbibliotheken voor verschillende veelgebruikte talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure Storage door bewerkingen zoals synchrone en asynchrone aanroepafhandeling, batchverwerking van bewerkingen, uitzonderingsbeheer, automatische nieuwe pogingen, werking, enzovoort, voor hun rekening te nemen. Bibliotheken zijn momenteel beschikbaar voor de volgende talen en platformen (deze lijst wordt in de toekomst uitgebreid):

### <a name="azure-storage-data-api-and-library-references"></a>Azure Storage-gegevens-API en bibliotheekverwijzingen
* [REST-API voor Storage-services](https://docs.microsoft.com/rest/api/storageservices/)
* [Opslagclientbibliotheek voor .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
* [Opslagclientbibliotheek voor Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Opslagclientbibliotheek voor Node.js](https://docs.microsoft.com/javascript/api/azure-storage)
* [Opslagclientbibliotheek voor Python](https://github.com/Azure/azure-storage-python)
* [Opslagclientbibliotheek voor PHP](https://github.com/Azure/azure-storage-php)
* [Opslagclientbibliotheek voor Ruby](https://github.com/Azure/azure-storage-ruby)
* [Opslagclientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Azure Storage-beheer-API en bibliotheekverwijzingen
* [REST API van opslagresourceprovider](https://docs.microsoft.com/rest/api/storagerp/)
* [Clientbibliotheek van opslagresourceprovider voor .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
* [REST API van opslagservicebeheer (klassiek)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Azure Storage-gegevensverplaatsing-API en bibliotheekverwijzingen
* [REST-API van Storage Import/Export-service](https://docs.microsoft.com/rest/api/storageimportexport/)
* [Clientbibliotheek van opslaggegevensverplaatsing voor .NET](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.datamovement)

### <a name="tools-and-utilities"></a>Hulpprogramma's
* [Azure PowerShell-cmdlets voor Storage](https://docs.microsoft.com/powershell/module/az.storage)
* [Azure CLI-cmdlets voor Storage](https://docs.microsoft.com/cli/azure/storage)
* [AzCopy-opdrachtregelprogramma](https://aka.ms/downloadazcopy)
* [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Azure Storage-clienthulpprogramma’s](../storage-explorers.md)
* [Azure Developer-hulpprogramma's](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Volgende stappen

Zie [Een opslagaccount maken](storage-quickstart-create-account.md) om aan de slag te gaan met Azure Storage.
