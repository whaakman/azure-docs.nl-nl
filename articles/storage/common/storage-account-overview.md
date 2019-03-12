---
title: Overzicht van Azure storage-account | Microsoft Docs
description: Informatie over opties voor het maken en gebruiken van een Azure Storage-account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ce9635c108a948b4773c7d27cb254f01d06896f8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57544236"
---
# <a name="azure-storage-account-overview"></a>Overzicht van Azure storage-account

Een Azure storage-account bevat al uw Azure Storage-gegevensobjecten: blobs, bestanden, wachtrijen, tabellen en schijven. Gegevens in uw Azure storage-account zijn duurzame en maximaal beschikbare, veilige, zeer schaalbare en toegankelijk is vanaf overal ter wereld via HTTP of HTTPS. 

Zie voor meer informatie over het maken van een Azure storage-account, [een opslagaccount maken](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>V2-accounts voor algemeen gebruik

Opslagaccounts voor algemeen gebruik v2 ondersteuning voor de nieuwste opslagfuncties van Azure en alle functionaliteit van voor algemeen gebruik v1 en Blob storage-accounts. Voor algemeen gebruik v2-accounts bieden de laagste per GB capaciteit prijzen voor Azure Storage, evenals de bedrijfstak van concurrerende transactieprijzen in. Algemeen gebruik v2-opslagaccounts bieden ondersteuning voor deze Azure Storage-services:

- BLOBs (alle typen: Blokkeren, pagina toevoegen)
- Bestanden
- Disks
- Wachtrijen
- Tabellen

> [!NOTE]
> Microsoft raadt u aan met behulp van een opslagaccount voor algemeen gebruik v2 voor de meeste scenario's. U kunt gemakkelijk een voor algemeen gebruik v1- of Blob storage-account upgraden naar een algemeen gebruik v2-account zonder uitvaltijd en zonder de noodzaak om gegevens te kopiëren.
>
> Zie voor meer informatie over het bijwerken naar een account voor algemeen gebruik v2 [upgraden naar een opslagaccount voor algemeen gebruik v2](storage-account-upgrade.md).

Opslagaccounts voor algemeen gebruik v2 bieden meerdere toegangslagen voor het opslaan van gegevens op basis van uw gebruikspatronen. Zie voor meer informatie, [toegangslagen voor blok-blobgegevens](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>V1-accounts voor algemeen gebruik

Voor algemeen gebruik v1-accounts bieden toegang tot alle Azure Storage-services, maar hebben mogelijk niet de nieuwste functies of de laagste prijzen per gigabyte. Algemeen gebruik v1-opslagaccounts bieden ondersteuning voor deze Azure Storage-services:

- BLOBs (alle typen)
- Bestanden
- Disks
- Wachtrijen
- Tabellen

Hoewel voor algemeen gebruik v2-accounts in de meeste gevallen aanbevolen zijn, zijn voor algemeen gebruik v1-accounts bij uitstek geschikt voor deze scenario's: 

* Uw toepassingen vereisen het model van de klassieke Azure-implementatie. Accounts voor algemeen gebruik v2 en Blob storage-accounts ondersteunen alleen de Azure Resource Manager-implementatiemodel.

* Uw toepassingen zijn transactie-intensieve of aanzienlijke geo-replicatie bandbreedte gebruiken, maar geen grote capaciteit vereist. In dit geval voor algemeen gebruik v1 mogelijk het meest voordelige keuze.

* U gebruikt een versie van de [REST API voor Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek met een lagere versie dan 4.x en u uw toepassing niet kunt upgraden.

### <a name="block-blob-storage-accounts"></a>Block blob storage-accounts

Een block blob storage-account is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde objectgegevens als blok-blobs of toevoeg-blobs. Block blob storage-accounts bieden meerdere toegangslagen voor het opslaan van gegevens op basis van uw gebruikspatronen. Zie voor meer informatie, [toegangslagen voor blok-blobgegevens](#access-tiers-for-block-blob-data).

## <a name="naming-storage-accounts"></a>Naamgeving van opslagaccounts

Neem de volgende regels in acht als u het opslagaccount een naam geeft:

- Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.
- De naam van uw opslagaccount moet uniek zijn binnen Azure. Een opslagaccount kan niet dezelfde naam hebben als een ander opslagaccount.

## <a name="performance-tiers"></a>Prestatielagen

Algemene opslagaccounts kunnen worden geconfigureerd voor een van de volgende prestatielagen:

* Een standard-prestatielaag voor het opslaan van blobs, bestanden, tabellen, wachtrijen en schijven van de virtuele machine van Azure.
* Een premium-prestatielaag voor het opslaan van alleen niet-beheerde VM-schijven.

## <a name="access-tiers-for-block-blob-data"></a>Toegangslagen voor blok-blob-gegevens

Azure Storage biedt verschillende opties voor toegang tot blok-blobgegevens op basis van gebruikspatronen. Elke toegangslaag in Azure Storage is geoptimaliseerd voor een bepaalde patroon van de gebruikte gegevens. Als u de juiste toegangslaag voor uw behoeften, kunt u uw blok-blob-gegevens opslaan in de meest kostenefficiënte manier.

De laag beschikbaar zijn:

> [!NOTE]
> De [premium-toegangslaag (preview)](../blobs/storage-blob-storage-tiers.md#premium-access-tier), die is geoptimaliseerd voor gevoelige toepassingen met prestaties, lage en consistente latentie met hoge doorvoer en transactie-frequenties biedt. De premium-laag voor access is alleen beschikbaar voor blok-Blob storage-accounts (preview). Zie voor meer informatie, [openbare preview van Azure Premium-blobopslag](https://azure.microsoft.com/blog/azure-premium-blob-storage-public-preview/).

* De **warm** toegangslaag, die is geoptimaliseerd voor frequente toegang krijgen tot objecten in de storage-account. Toegang tot gegevens in de warme laag is meest rendabele, terwijl de kosten voor opslag enigszins hoger zijn. Nieuwe storage-accounts worden gemaakt in de warme laag standaard.
* De **Cool** toegangslaag, die is geoptimaliseerd voor het opslaan van grote hoeveelheden gegevens die niet vaak worden geraadpleegd en die gedurende ten minste 30 dagen worden opgeslagen. Opslaan van gegevens in de koude laag rendabeler is, maar toegang tot die gegevens mogelijk iets duurder dan de toegang tot gegevens in de warme laag.
* De **archief** laag, die alleen beschikbaar voor afzonderlijke blok-blobs is. De Archive-laag is geoptimaliseerd voor gegevens die enkele uren latentie bij het ophalen kan tolereren en blijven in de Archive-laag voor ten minste 180 dagen. De Archive-laag is de meest voordelige optie zijn voor het opslaan van gegevens, maar toegang tot die gegevens is duurder dan de toegang tot gegevens in de warme of koude laag. 


Als er een wijziging in het gebruikspatroon van uw gegevens is, kunt u schakelen tussen deze toegangslagen op elk gewenst moment. Zie voor meer informatie over de toegangslagen, [Azure Blob-opslag: Premium (preview), dynamische, statische en Archiefopslaglaag](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Wijzigen van de toegangslaag voor een bestaand opslagaccount of blob kan leiden tot extra kosten in rekening gebracht. Zie voor meer informatie de [opslagaccount facturering](#storage-account-billing).



## <a name="replication"></a>Replicatie

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Zie voor meer informatie over de storage-replicatie, [Azure Storage-replicatie](storage-redundancy.md).

## <a name="encryption"></a>Versleuteling

Alle gegevens in uw storage-account is versleuteld aan de servicezijde. Zie voor meer informatie over versleuteling [Azure Storage-Serviceversleuteling voor data-at-rest](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Eindpunten van opslagaccount

Een opslagaccount biedt een unieke naamruimte in Azure voor uw gegevens. Elk object dat u in Azure Storage opslaat, heeft een adres dat uw unieke accountnaam bevat. De combinatie van de accountnaam en het service-eindpunt voor Azure Storage vormen de eindpunten voor uw opslagaccount.

Bijvoorbeeld, als de naam van uw opslagaccount voor algemeen gebruik *mystorageaccount*, en vervolgens de Standaardeindpunten voor dat account zijn:

* BLOB-opslag: http://*mystorageaccount*. blob.core.windows.net
* Tabelopslag: http://*mystorageaccount*. table.core.windows.net
* Opslag in de wachtrij: http://*mystorageaccount*. queue.core.windows.net
* Azure Files: http://*mystorageaccount*. file.core.windows.net

> [!NOTE]
> Een Blob storage-account wordt aangegeven dat alleen het eindpunt van Blob service.

De URL voor het openen van een object in een opslagaccount wordt samengesteld door de locatie van het object in het opslagaccount naar het eindpunt toe te voegen. Een blobadres kan bijvoorbeeld de volgende indeling hebben: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

U kunt ook uw storage-account voor het gebruik van een aangepast domein voor blobs configureren. Zie voor meer informatie, [een aangepaste domeinnaam voor uw Azure Storage-account configureren](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Toegang tot gegevens beheren

De gegevens in uw account zijn standaard alleen beschikbaar voor u, de eigenaar van het account. Hebt u controle over wie mogelijk toegang heeft tot uw gegevens en welke machtigingen ze hebben.

Elke aanvraag ten opzichte van uw storage-account moet worden toegestaan. Op het niveau van de service, de aanvraag moet bevatten een geldige *autorisatie* header, waaronder alle informatie die nodig zijn voor de service voor het valideren van de aanvraag voordat deze wordt uitgevoerd.

U kunt toegang verlenen tot de gegevens in uw opslagaccount met behulp van een van de volgende methoden:

- **Azure Active Directory:** Gebruik Azure Active Directory (Azure AD)-referenties voor het verifiëren van een gebruiker, groep of andere identiteit voor toegang tot blob- en wachtrijservices gegevens (preview). Als verificatie van een identiteit geslaagd is, retourneert een token maken voor gebruik in het autoriseren van de aanvraag voor Azure Blob-opslag of Queue storage met Azure AD. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (preview)](storage-auth-aad.md).
- **Gedeelde sleutel-autorisatie:** Gebruik uw toegangssleutel voor opslagaccount te maken van een verbindingsreeks die uw toepassing tijdens runtime worden gebruikt voor toegang tot Azure Storage. De waarden in de verbindingsreeks gebruikt om samen te stellen de *autorisatie* -header die wordt doorgegeven aan Azure Storage. Zie voor meer informatie, [configureren van Azure Storage-verbindingsreeksen](storage-configure-connection-string.md).
- **Handtekening voor gedeelde toegang:** Een shared access signature gebruiken voor toegang tot resources in uw opslagaccount delegeren als u geen Azure AD-verificatie. Een shared access signature is een token dat alle informatie die nodig zijn voor het autoriseren van een aanvraag naar Azure Storage op de URL wordt ingekapseld. U kunt de storage-resource, machtigingen en het interval op waarover de machtigingen geldig zijn opgeven als onderdeel van de shared access signature. Zie voor meer informatie, [Using shared access signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Verifiëren van gebruikers of toepassingen die gebruikmaken van Azure AD-referenties biedt uitstekende beveiliging en gebruiksgemak ten opzichte van andere middelen van autorisatie. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. 
>
> Microsoft raadt u aan met behulp van Azure AD-verificatie voor uw Azure Storage blob- en wachtrijservices toepassingen indien mogelijk.

## <a name="copying-data-into-a-storage-account"></a>Kopiëren van gegevens naar een opslagaccount

Microsoft biedt hulpprogramma's en bibliotheken voor het importeren van uw gegevens vanaf on-premises opslagapparaten of cloudopslagproviders van derden. Welke oplossing die u gebruikt, is afhankelijk van de hoeveelheid gegevens die u overbrengt. 

Wanneer u een upgrade naar een algemeen gebruik v2-account maakt vanuit een voor algemeen gebruik v1- of Blob storage-account uitvoert, worden uw gegevens worden automatisch gemigreerd. Microsoft raadt aan deze route voor het upgraden van uw account. Echter, als u besluit om gegevens te verplaatsen van een account voor algemeen gebruik v1 naar een Blob storage-account, moet u het handmatig migreren van uw gegevens, met behulp van de hulpprogramma's en bibliotheken die hieronder worden beschreven. 

### <a name="azcopy"></a>AzCopy

AzCopy is een Windows-opdrachtregelprogramma dat is  ontworpen voor het high-performance kopiëren van gegevens van en naar Azure Storage. U kunt AzCopy gebruiken om gegevens te kopiëren naar een Blob storage-account van een bestaand opslagaccount of het uploaden van gegevens van on-premises opslagapparaten. Zie [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

### <a name="data-movement-library"></a>Bibliotheek voor gegevensverplaatsing

De Azure Storage-bibliotheek voor gegevensverplaatsing voor .NET is gebaseerd op het basisframework voor gegevensverplaatsing voor AzCopy. De bibliotheek is ontworpen voor high-performance, betrouwbare en eenvoudige gegevensoverdracht vergelijkbaar met AzCopy. Hiermee kunt u in uw toepassing op systeemeigen wijze profiteren van de functies die AzCopy biedt, zonder dat u externe exemplaren van AzCopy hoeft uit te voeren of te controleren. Zie [Bibliotheek voor gegevensverplaatsing van Azure Storage voor .Net](https://github.com/Azure/azure-storage-net-data-movement) voor meer informatie

### <a name="rest-api-or-client-library"></a>REST-API of clientbibliotheek

U kunt een aangepaste toepassing maken om gegevens naar een Blob Storage-account te migreren met behulp van een van de Azure-clientbibliotheken of de REST API voor Azure Storage-services. Azure Storage biedt uitgebreide clientbibliotheken voor meerdere talen en platforms, zoals  .NET, Java, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden, zoals pogingslogica, logboekregistratie en parallelle uploads. U kunt ook rechtstreeks met de REST API ontwikkelen. Deze kan worden aangeroepen in elke taal waarin HTTP-/HTTPS-verzoeken kunnen worden gemaakt.

Zie voor meer informatie over de REST-API van Azure Storage, [Azure Storage-Services REST API-verwijzing](https://docs.microsoft.com/rest/api/storageservices/). 

> [!IMPORTANT]
> Blobs die aan de clientzijde zijn versleuteld, bevatten versleutelingsgerelateerde metagegevens die samen met de blob zijn opgeslagen. Als u een blob met versleuteling aan de clientzijde kopieert, zorg er dan voor dat bij het kopiëren de blobmetagegevens behouden blijven, en dan met name de versleutelingsgerelateerde metagegevens. Als u een blob kopieert zonder versleutelingsgerelateerde metagegevens, kan de inhoud van de blob niet meer worden opgehaald. Zie [Azure Storage-versleuteling aan de clientzijde](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over versleutelingsgerelateerde metagegevens.

### <a name="azure-importexport-service"></a>Azure Import/Export-service

Als u een grote hoeveelheid gegevens te importeren in uw storage-account hebt, kunt u de Azure Import/Export-service. De Import/Export-service wordt gebruikt voor het veilig grote hoeveelheden gegevens importeren naar Azure Blob storage en Azure Files door verzending schijven naar een Azure-datacenter. 

De Import/Export-service kan ook worden gebruikt voor gegevensoverdracht van Azure Blob-opslag op schijven en verzend deze naar uw on-premises sites. Gegevens uit een of meer schijven kunnen worden geïmporteerd naar Azure Blob-opslag of naar Azure Files. Zie voor meer informatie, [wat is Azure Import/Export-service?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).

## <a name="storage-account-billing"></a>Facturering voor opslagaccounts

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van een Azure storage-account, [een opslagaccount maken](storage-quickstart-create-account.md).
* Als u wilt beheren of verwijderen van een bestaand opslagaccount, Zie [beheren Azure storage-accounts](storage-account-manage.md).
