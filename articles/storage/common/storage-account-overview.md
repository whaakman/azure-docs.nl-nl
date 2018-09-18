---
title: Overzicht van Azure storage-account | Microsoft Docs
description: Informatie over opties voor het maken en gebruiken van een Azure Storage-account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/13/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: c0d028b2e6f5e759b46293bc3e1cbef6902ea8ea
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45740850"
---
# <a name="azure-storage-account-overview"></a>Overzicht van Azure storage-account

Een Azure storage-account bevat al uw Azure Storage-gegevensobjecten: blobs, bestanden, wachtrijen, tabellen en schijven. Gegevens in uw Azure storage-account zijn duurzame en maximaal beschikbare, veilige, zeer schaalbare en toegankelijk is vanaf overal ter wereld via HTTP of HTTPS. 

Zie voor meer informatie over het maken van een Azure storage-account, [een opslagaccount maken](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts

Azure Storage biedt drie typen opslagaccounts. Elk type biedt ondersteuning voor verschillende functies en heeft een eigen prijsmodel. Houd rekening met deze verschillen voordat u een opslagaccount om te bepalen welk type account dat wordt aanbevolen voor uw toepassingen kunt maken. De typen opslagaccounts zijn:

* **Algemeen gebruik v2** accounts (aanbevolen voor de meeste scenario's)
* **Algemeen gebruik v1** accounts
* **Blob Storage**-accounts

De volgende tabel beschrijft de soorten opslagaccounts en de bijbehorende mogelijkheden:

| Type opslagaccount | Ondersteunde services                       | Ondersteunde prestatielagen | Ondersteunde toegangslagen               | Opties voor gegevensreplicatie                                                | Implementatiemodel<sup>1</sup>  | Versleuteling<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Algemeen gebruik V2   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium           | Hot, Cool archief<sup>3</sup> | LRS-, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager | Versleuteld  |
| Algemeen gebruik V1   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium           | N/A                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, klassiek  | Versleuteld  |
| Blob Storage         | BLOB (blok-blobs en toevoeg-blobs alleen) | Standard                    | Hot, Cool archief<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Resource Manager  | Versleuteld  |

<sup>1</sup>met het Azure Resource Manager-implementatiemodel wordt aanbevolen. Opslagaccounts met behulp van het klassieke implementatiemodel kunnen nog steeds worden gemaakt op bepaalde locaties en bestaande klassieke accounts worden nog steeds ondersteund. Zie voor meer informatie, [Azure Resource Manager en klassieke implementatie: implementatiemodellen en de status van uw resources begrijpen](../../azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>alle storage-accounts zijn versleuteling voor data-at-rest met behulp van Storage Service Encryption (SSE). Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](storage-service-encryption.md).

<sup>3</sup>de archive-laag is beschikbaar op het niveau van een afzonderlijke blob alleen, niet op het niveau van de storage-account. Alleen blok-blobs en toevoeg-blobs kunnen worden gearchiveerd. Zie voor meer informatie, [Azure Blob storage: Hot, cool en archive storage-lagen](../blobs/storage-blob-storage-tiers.md).

<sup>4</sup>zone-redundante opslag (ZRS) is alleen beschikbaar voor algemeen gebruik v2 standard storage-accounts. Zie voor meer informatie over ZRS [Zone-redundante opslag (ZRS): toepassingen met hoge beschikbaarheid Azure Storage](storage-redundancy-zrs.md). Zie voor meer informatie over andere opties voor gegevensreplicatie [Azure Storage-replicatie](storage-redundancy.md).

### <a name="general-purpose-v2-accounts"></a>V2-accounts voor algemeen gebruik

Opslagaccounts voor algemeen gebruik v2 ondersteuning voor de nieuwste opslagfuncties van Azure en alle functionaliteit van voor algemeen gebruik v1 en Blob storage-accounts. Voor algemeen gebruik v2-accounts bieden de laagste per GB capaciteit prijzen voor Azure Storage, evenals de bedrijfstak van concurrerende transactieprijzen in. Algemeen gebruik v2-opslagaccounts bieden ondersteuning voor deze Azure Storage-services:

- BLOBs (alle typen)
- Bestanden
- Disks
- Wachtrijen
- Tabellen

Microsoft raadt u aan met behulp van een opslagaccount voor algemeen gebruik v2 voor de meeste scenario's. U kunt gemakkelijk een voor algemeen gebruik v1- of Blob storage-account upgraden naar een algemeen gebruik v2-account geen downtime of toepassing regeneraties en zonder de noodzaak om gegevens te kopiëren. Zie voor meer informatie over het bijwerken naar een account voor algemeen gebruik v2 [upgraden naar een opslagaccount voor algemeen gebruik v2](storage-account-upgrade.md). 

Opslagaccounts voor algemeen gebruik v2 bieden meerdere toegangslagen voor het opslaan van gegevens op basis van uw gebruikspatronen. Zie voor meer informatie, [Toegangslagen voor blob-gegevens](#access-tiers-for-blob-data).

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

### <a name="blob-storage-accounts"></a>Blob Storage-accounts

Een Blob storage-account is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde objectgegevens als blok-blobs. BLOB storage-accounts bieden de dezelfde duurzaamheid, beschikbaarheid, schaalbaarheid en prestatieverhogende functies die beschikbaar met opslagaccounts voor algemeen gebruik v2 zijn. BLOB storage-accounts ondersteunen opslaan blok-blobs en toevoeg-blobs, maar niet voor pagina-blobs.

BLOB storage-accounts bieden meerdere toegangslagen voor het opslaan van gegevens op basis van uw gebruikspatronen. Zie voor meer informatie, [Toegangslagen voor blob-gegevens](#access-tiers-for-blob-data).

## <a name="naming-storage-accounts"></a>Naamgeving van opslagaccounts

Neem de volgende regels in acht als u het opslagaccount een naam geeft:

- Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.
- De naam van uw opslagaccount moet uniek zijn binnen Azure. Een opslagaccount kan niet dezelfde naam hebben als een ander opslagaccount.

## <a name="performance-tiers"></a>Prestatielagen

Algemene opslagaccounts kunnen worden geconfigureerd voor een van de volgende prestatielagen:

* Een standard-opslag-prestatielaag voor het opslaan van blobs, bestanden, tabellen, wachtrijen en schijven van de virtuele machine van Azure.
* Een premium storage-prestatielaag voor het opslaan van virtuele-machineschijven van Azure alleen. Zie [Premium Storage: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md) voor een gedetailleerd overzicht van Premium-opslag.

## <a name="access-tiers-for-block-blob-data"></a>Toegangslagen voor blok-blob-gegevens

Azure Storage biedt verschillende opties voor toegang tot blok-blobgegevens op basis van gebruikspatronen. Elke toegangslaag in Azure Storage is geoptimaliseerd voor een bepaalde patroon van de gebruikte gegevens. Als u de juiste toegangslaag voor uw behoeften, kunt u uw blok-blob-gegevens opslaan in de meest kostenefficiënte manier.

De laag beschikbaar zijn:

* De **hot** toegangslaag, die is geoptimaliseerd voor frequente toegang krijgen tot objecten in de storage-account. Toegang tot gegevens in de warme laag is meest rendabele, terwijl de kosten voor opslag enigszins hoger zijn. Nieuwe storage-accounts worden gemaakt in de warme laag standaard.
* De **cool** toegangslaag, die is geoptimaliseerd voor het opslaan van grote hoeveelheden gegevens die niet vaak worden geraadpleegd en die gedurende ten minste 30 dagen worden opgeslagen. Opslaan van gegevens in de koude laag rendabeler is, maar toegang tot die gegevens mogelijk iets duurder dan de toegang tot gegevens in de warme laag.
* De **archief** laag, die alleen beschikbaar voor afzonderlijke blok-blobs is. De archive-laag is geoptimaliseerd voor gegevens die enkele uren latentie bij het ophalen kan tolereren en blijven in de archive-laag voor ten minste 180 dagen. De archive-laag is de meest voordelige optie zijn voor het opslaan van gegevens, maar toegang tot die gegevens is duurder dan de toegang tot gegevens in de warme als koude opslaglagen. 

Als er een wijziging in het gebruikspatroon van uw gegevens is, kunt u schakelen tussen deze toegangslagen op elk gewenst moment. 

> [!IMPORTANT]
> Wijzigen van de toegangslaag voor een bestaand opslagaccount of blob kan leiden tot extra kosten in rekening gebracht.

Zie voor meer informatie over de toegangslagen, [Azure Blob storage: Hot, cool en archive storage-lagen](../blobs/storage-blob-storage-tiers.md).

## <a name="replication"></a>Replicatie

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Zie voor meer informatie over de storage-replicatie, [Azure Storage-replicatie](storage-redundancy.md).

## <a name="encryption"></a>Versleuteling

Alle gegevens in uw storage-account is versleuteld aan de servicezijde. Zie voor meer informatie over versleuteling [Azure Storage-Serviceversleuteling voor data-at-rest](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Eindpunten van opslagaccount

Een opslagaccount biedt een unieke naamruimte in Azure voor uw gegevens. Elk object dat u in Azure Storage opslaat, heeft een adres met de naam van uw unieke account. De combinatie van de accountnaam en het Azure Storage service-eindpunt vormt de eindpunten voor uw storage-account.

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

- **Azure Active Directory:** gebruik Azure Active Directory (Azure AD) referenties voor het verifiëren van een gebruiker, groep of andere identiteit voor toegang tot blob- en wachtrijservices gegevens (preview). Als verificatie van een identiteit geslaagd is, retourneert een token maken voor gebruik in het autoriseren van de aanvraag voor Azure Blob-opslag of Queue storage met Azure AD. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (preview)](storage-auth-aad.md).
- **Gedeelde sleutel autorisatie:** toegangssleutel voor uw opslagaccount te maken van een verbindingsreeks die uw toepassing tijdens runtime worden gebruikt voor toegang tot Azure-opslag gebruiken. De waarden in de verbindingsreeks gebruikt om samen te stellen de *autorisatie* -header die wordt doorgegeven aan Azure Storage. Zie voor meer informatie, [configureren van Azure Storage-verbindingsreeksen](storage-configure-connection-string.md).
- **Handtekening voor gedeelde toegang:** gebruiken een shared access signature voor toegang tot resources in uw opslagaccount delegeren als u geen Azure AD-verificatie. Een shared access signature is een token dat alle informatie die nodig zijn voor het autoriseren van een aanvraag naar Azure Storage op de URL wordt ingekapseld. U kunt de storage-resource, machtigingen en het interval op waarover de machtigingen geldig zijn opgeven als onderdeel van de shared access signature. Zie voor meer informatie, [Using shared access signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).

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
