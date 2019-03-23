---
title: Hot, cool en archive Storage voor blobs - Azure Storage-toegangslagen
description: Hot, cool en archive toegangslagen voor Azure storage-accounts.
services: storage
author: Xansky
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: mhopkins
ms.subservice: blobs
ms.openlocfilehash: 424ca1cccd1b82c26a801af3f85f41eb2991e2d7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370122"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Azure Blob storage: hot, cool en archive toegangslagen

Azure storage biedt verschillende toegangslagen, waarmee u kunt voor het opslaan van gegevens voor blob-object in de meest kostenefficiënte manier. De beschikbare toegangslagen zijn onder andere:

- **Hot** - geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend.
- **' Cool '** - geoptimaliseerd voor het opslaan van gegevens die niet vaak worden geraadpleegd en die gedurende ten minste 30 dagen worden opgeslagen.
- **Archief** - geoptimaliseerd voor het opslaan van gegevens die zelden worden geopend en die gedurende ten minste 180 dagen met flexibele latentievereisten (orde van grootte uur) worden opgeslagen.

De volgende overwegingen zijn van toepassing op de verschillende toegangslagen:

- De Archive Storage-toegangslaag is beschikbaar op het niveau van blob en niet op het niveau van de storage-account.
- Gegevens in de Cool Storage-toegangslaag kunt enigszins lagere beschikbaarheid verdragen, maar nog steeds vereist hoge duurzaamheid en vergelijkbare kenmerken voor de tijd voor toegang en doorvoer als gegevens. Voor gegevens in de cool, een iets lagere beschikbaarheid service level agreement (SLA) en hogere zijn kosten in vergelijking met hot gegevens laag aanvaardbaar-voor lagere opslagkosten.
- Archiefopslag is offline en biedt de laagste kosten voor opslag, maar ook de hoogste toegangskosten.
- Alleen de warme en koude toegangslagen kunnen worden ingesteld op het accountniveau.
- Hot, cool en archive-laag kunnen worden ingesteld op het objectniveau.

Gegevens die zijn opgeslagen in de cloud, groeit exponentieel. Voor een effectief beheer van de kosten voor uw groeiende opslagbehoeften is het vanwege kostenoptimalisatie een goed idee om de gegevens te ordenen op basis van kenmerken als toegangsfrequentie en geplande bewaarperiode. Gegevens die zijn opgeslagen in de cloud kan afwijken in termen van hoe deze is gegenereerd, verwerkt en toegankelijk tijdens hun levensduur. Sommige gegevens worden tijdens hun hele levensduur actief geopend en gewijzigd. Andere gegevens worden in het begin van hun levensduur regelmatig geopend, terwijl dit naarmate de tijd verstrijkt, aanzienlijk minder vaak gebeurt. Sommige gegevens inactief blijft in de cloud en zelden worden als tot nooit geopend nadat deze opgeslagen.

Elk van deze scenario's voor toegang tot profiteren van een andere opslaglaag die is geoptimaliseerd voor een specifiek toegangspatroon. Met hot, cool en archive toegangslagen, Azure Blob storage-adressen in deze behoefte aan gedifferentieerde toegangslagen met afzonderlijke prijsmodellen.

## <a name="storage-accounts-that-support-tiering"></a>Storage-accounts die ondersteuning bieden voor opslaglagen

U kunt alleen uw objectopslaggegevens hot, cool of archiveren in Blob-opslag en algemeen gebruik v2 (GPv2-accounts). General Purpose v1 (GPv1)-accounts bieden geen ondersteuning voor opslaglagen. U kunt echter eenvoudig bestaande GPv1- of Blob storage-accounts converteren naar GPv2-accounts via een proces met één klik in de Azure-portal. GPv2 biedt een nieuwe prijzenstructuur voor blobs, bestanden en wachtrijen en toegang tot tal van andere nieuwe opslagfuncties. Voortaan kunt zullen sommige nieuwe mogelijkheden en prijsverlagingen alleen in GPv2-accounts worden aangeboden. U moet daarom met behulp van GPv2-accounts evalueren, maar deze alleen gebruiken na herziening van de prijsstelling voor alle services. Sommige workloads kunnen op GPv2 duurder dan GPv1 zijn. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie.

BLOB storage en GPv2-accounts geven het **Toegangslaag** kenmerk op accountniveau. Dit kenmerk kunt u de standaardlaag als hot of cool voor elke blob in het opslagaccount waarvoor geen expliciete laag is ingesteld op het objectniveau van het opgeven. Voor objecten met de op objectniveau ingestelde opslaglaag is het accountniveau niet van toepassing. De archive-laag kan alleen op objectniveau worden toegepast. U kunt schakelen tussen deze toegangslagen op elk gewenst moment.

## <a name="premium-performance-block-blob-storage"></a>Premium-prestaties blok-blobopslag

Premium-prestaties blok-blobopslag maakt vaak gebruikte gegevens beschikbaar zijn via krachtige hardware. Gegevens in deze prestatielaag is opgeslagen op SSD (Solid-State drives), die zijn geoptimaliseerd voor latentie laag en consistent is. SSD's bieden een hogere transactionele tarieven en doorvoer die vergeleken met traditionele harde schijven.

Premium-prestaties blok-blobopslag is ideaal voor workloads waarvoor snelle en consistente reactietijden. Het is raadzaam voor werkbelastingen die veel kleine transacties, zoals het vastleggen van telemetriegegevens, berichten en gegevenstransformatie van uitvoeren. Gegevens die betrekking heeft op eindgebruikers, zoals interactieve videobewerking, statische webinhoud en online transacties zijn ook geschikt.

Premium prestaties blok-blobopslag is alleen toegankelijk zijn voor het accounttype van block blob storage en ondersteunt momenteel geen lagen voor warm, koud of archief toegangslagen.

## <a name="hot-access-tier"></a>Hot Storage-toegangslaag

De hot Storage-toegangslaag heeft hogere opslagkosten dan cool en archive-laag, maar de laagste toegangskosten. Voorbeelden van gebruiksscenario voor de hot Storage-toegangslaag zijn onder andere:

- Gegevens die actief worden gebruikt of worden geopend (Lees- en schrijfbewerkingen) vaak wordt verwacht.
- Gegevens die tijdelijk worden opgeslagen voor verwerking en uiteindelijk voor migratie naar de Cool Storage-toegangslaag.

## <a name="cool-access-tier"></a>Cool Storage-toegangslaag

De Cool Storage-toegangslaag heeft lagere opslagkosten en hogere toegangskosten in vergelijking met hot storage. Deze laag is bedoeld voor gegevens die ten minste dertig dagen in de Cold Storage verblijven. Voorbeelden van gebruiksscenario voor de koude laag zijn onder andere:

- Gegevenssets waarvan voor de korte termijn een back-up is gemaakt en die na een noodgeval zijn hersteld.
- Oudere media-inhoud die niet meer regelmatig wordt bekeken, maar onmiddellijk beschikbaar moet zijn wanneer deze wordt geopend.
- Grote gegevenssets die voordelig moeten worden opgeslagen, terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*Bijvoorbeeld*: langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een productiefaciliteit)

## <a name="archive-access-tier"></a>Archive-toegangslaag

De Archive Storage-toegangslaag heeft de laagste opslagkosten en hogere kosten voor gegevens ophalen in vergelijking met warme en koude lagen. Deze laag is bedoeld voor gegevens die enkele uren latentie kunnen verdragen bij terughalen en die ten minste 180 dagen in de archieflaag verblijven.

Hoewel een blob in archive storage, wordt de blob-gegevens offline is en kan niet worden gelezen, gekopieerd, overschreven of gewijzigd. U kunt geen momentopnamen maken van een blob in archiefopslag. De metagegevens van de blob blijft echter online en beschikbaar, zodat u om de blob en de bijbehorende eigenschappen weer te geven. Voor blobs in archief zijn de enige geldige bewerkingen GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier en DeleteBlob.

Voorbeelden van gebruiksscenario voor de archive-toegangslaag zijn onder andere:

- Langetermijnback-up, secundaire back-up en gegevenssets voor archivering
- Oorspronkelijke (onbewerkte) gegevens die moeten worden bewaard, zelfs nadat deze zijn verwerkt tot hun uiteindelijke, bruikbare vorm. (*Bijvoorbeeld*: onbewerkte mediabestanden na transcodering naar andere indelingen)
- Compliance- en archiveringsgegevens die gedurende lange tijd moeten worden opgeslagen en bijna nooit worden geopend. (*Bijvoorbeeld*, beveiligingscamera's, oude röntgenfoto-beeldmateriaal/MRI's in ziekenhuizen voor organisaties in de gezondheidszorg, geluidsopnamen en transcripties van verkoopgesprekken voor financiële diensten)

### <a name="blob-rehydration"></a>Rehydratatie van blobs

Als u gegevens wilt lezen die aanwezig zijn in Archive Storage, moet u de laag van de blob eerst wijzigen in Hot of Cool. Dit proces staat bekend als rehydratatie en het voltooien ervan kan wel 15 uur duren. Grote blob-opslag worden aanbevolen voor optimale prestaties. Enkele kleine blobs tegelijk reactiveren kan extra tijd toevoegen.

Tijdens rehydratatie kunt u aan de blob-eigenschap **archive status** zien of de laag is gewijzigd. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap voor archiefstatus verwijderd en geeft de blob-eigenschap **Toegangslaag** aan of de opslaglaag dynamisch of statisch is.  

## <a name="blob-level-tiering"></a>Laaginstelling op blobniveau

De functie Laaginstelling op blob-niveau maakt het mogelijk om de laag van uw gegevens op objectniveau te wijzigen met behulp van een eenmalige bewerking met de naam [Blob-laag instellen](/rest/api/storageservices/set-blob-tier). U kunt de toegangslaag van een blob gemakkelijk wijzigen van Hot in Cool of Archive, plus alle mogelijke varianten. U kunt zo snel inspelen op veranderende gebruikspatronen zonder dat u gegevens tussen accounts hoeft te verplaatsen. Alle laagwijzigingen vinden direct plaats. Een blob in archief te reactiveren kan echter enkele uren duren.

Het tijdstip waarop de laatste wijziging aan de blob-laag heeft plaatsgevonden, wordt weergegeven via de blob-eigenschap **Access Tier Change Time**. Worden als een blob in de laag archief was, deze kan niet overschreven, zodat dezelfde blob uploaden is niet toegestaan in dit scenario. U kunt een blob in een laag hot of cool, waarin de nieuwe blob geval neemt de laag van de blob die is overschreven over overschrijven.

BLOBs in alle drie toegangslagen kunnen naast elkaar bestaan binnen hetzelfde account. Een blob waaraan niet expliciet een laag is toegewezen, leidt de laag af die is ingesteld als toegangslaag voor het account. Als de toegangslaag van het account is afgeleid, ziet u de **Access Tier Inferred** blob-eigenschap is ingesteld op 'true', en met de blob **Toegangslaag** blob-eigenschap komt overeen met de accountlaag. In de Azure-portal, de toegangslaag afgeleid eigenschap wordt weergegeven met de blob-toegangslaag (bijvoorbeeld **Hot (afgeleid)** of **Cool (afgeleid)**).

> [!NOTE]
> Archiefopslag en laaginstelling op blobniveau ondersteunen alleen blok-blobs. U kunt evenmin de laag wijzigen van een blok-blob die momentopnamen bevat.

> [!NOTE]
> Gegevens die zijn opgeslagen in een blok-blob storage-account kan niet op dit moment worden lagen hot, cool of archive met behulp van [Blob-laag instellen](/rest/api/storageservices/set-blob-tier) of met behulp van Azure Blob Storage-levenscyclusbeheer.
> Om gegevens te verplaatsen, moet u synchroon kopiëren blobs uit de block blob storage-account naar de hot Storage-toegangslaag in een ander account met de [blok plaatsen van URL API](/rest/api/storageservices/put-block-from-url) of een versie van AzCopy die ondersteuning biedt voor deze API.
> De *blok plaatsen van URL* API synchroon worden gegevens gekopieerd op de server, wat betekent dat de aanroep is voltooid maar één keer alle de gegevens van de oorspronkelijke serverlocatie wordt verplaatst naar de doellocatie.

### <a name="blob-lifecycle-management"></a>Beheer van de BLOB-levenscyclus

BLOB Storage lifecycle management (Preview) biedt een uitgebreide, op basis van regels beleid dat u gebruiken kunt voor de overgang van uw gegevens naar de beste toegangslaag en verloopt gegevens aan het einde van de levenscyclus. Zie [beheren van de levenscyclus van de Azure Blob-opslag](storage-lifecycle-management-concepts.md) voor meer informatie.  

### <a name="blob-level-tiering-billing"></a>Facturering van laaginstelling op blobniveau

Wanneer een blob wordt verplaatst naar een minder dynamische laag (dynamisch -> statisch, dynamisch -> archief of statisch -> archief), de bewerking gefactureerd als een schrijfbewerking naar de bestemmingslaag, waar de schrijfbewerking (per 10.000) en de kosten voor gegevens schrijven (per GB) van de bestemmingslaag van toepassing.

Wanneer een blob wordt verplaatst naar een dynamischer laag (archief -> statisch, archief -> dynamisch of statisch -> dynamisch), de bewerking gefactureerd als een leesbewerking vanuit de bronlaag, waar de leesbewerking (per 10.000) en de kosten voor gegevens (per GB) voor het ophalen van de bronlaag van toepassing. De volgende tabel geeft een overzicht van hoe laagwijzigingen worden in rekening gebracht.

| | **Schrijven van kosten in rekening gebracht (bewerking + toegang)** | **Alleen kosten in rekening gebracht (bewerking + toegang)**
| ---- | ----- | ----- |
| **SetBlobTier richting** | dynamisch -> statisch, dynamisch -> archief, koud -> archief | archief -> statisch, archief -> dynamisch, statisch -> dynamisch

Als u het accountniveau omschakelt van dynamisch naar statisch, worden alleen schrijfacties in rekening gebracht (per 10.000) voor alle blobs zonder een ingestelde opslaglaag in GPv2-accounts. Er zijn geen kosten voor deze wijziging in Blob storage-accounts. Er worden kosten in rekening gebracht voor zowel leesbewerkingen (per 10.000) als voor het ophalen van gegevens (per GB) als u uw Blob Storage- of GPv2-account omschakelt van statisch naar dynamisch. Kosten voor vroegtijdige verwijdering van een blob die is verplaatst uit de opslaglaag Cool of Archive kunnen ook van toepassing zijn.

### <a name="cool-and-archive-early-deletion"></a>Vroege verwijdering voor Koud en Archief

In aanvulling op de kosten per GB per maand is elke blob die wordt verplaatst naar de cool-opslaglaag (alleen GPv2-accounts) onderworpen aan een periode van 30 dagen van vroegtijdige verwijdering uit Cool en is elke blob die wordt verplaatst naar de Archive-laag onderworpen aan een periode van 180 dagen van vroegtijdige verwijdering uit Archive. Deze kosten zijn evenredig verdeeld. Als bijvoorbeeld een blob na 45 dagen wordt verplaatst naar de archieflaag en vervolgens wordt verwijderd of na 45 dagen wordt verplaatst naar de dynamische laag, worden kosten voor vroegtijdige verwijdering in rekening gebracht die gelijk zijn aan 135 (180 min 45) dagen van opslag van die blob in de archieflaag.

U mag de vroegtijdige verwijdering berekenen met behulp van de blob-eigenschap **Aanmaaktijd**, als er geen toegang laagwijzigingen is. U kunt anders gebruiken als de toegangslaag voor het laatst is gewijzigd naar koud of archief door te bekijken van de blob-eigenschap: **access-tier-wijzigen-time**. Zie voor meer informatie over blob-eigenschappen [Blob-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparing-block-blob-storage-options"></a>Vergelijking van de opties voor opslag van blok-blob

De volgende tabel ziet u een vergelijking van premium-prestaties blok-blobopslag en de hot, cool en archive toegangslagen.

|                                           | **Premium-prestaties** | **Warme laag** | **Koude laag** | **Archive-laag**
| ----------------------------------------- | ---------------- | ------------ | ----- | ----- |
| **Beschikbaarheid**                          | 99,9%            | 99,9%        | 99% | N/A |
| **Beschikbaarheid** <br> **(RA-GRS-leesbewerkingen)**  | N/A              | 99,99%       | 99,9% | N/A |
| **Gebruikskosten**                         | Hogere opslagkosten, lagere toegangs- en transactiekosten | Hogere opslagkosten, lagere toegangs- en transactiekosten | Lagere opslagkosten, hogere toegang en transactiekosten | Laagste opslagkosten, hoogste toegangs- en transactiekosten |
| **Minimale objectgrootte**                   | N/A | N/A | N/A | N/A |
| **Minimale opslagduur**              | N/A | N/A | 30 dagen (alleen GPv2) | 180 dagen
| **Latentie** <br> **(Tijd tot eerste byte)** | Enkele milliseconden | milliseconden | milliseconden | < 15 uur

> [!NOTE]
> Zie voor schaalbaarheids- en prestatiedoelen [schaalbaarheids- en prestatiedoelen voor Azure Storage voor storage-accounts](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="quickstart-scenarios"></a>Snelstartscenario's

In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

- Het wijzigen van de toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account.
- Het wijzigen van de opslaglaag van een blob in een GPv2- of Blob Storage-account.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>De toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account wijzigen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

1. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.

1. Selecteer de juiste toegangslaag voor uw behoeften: Stelt de **toegangslaag** aan **' Cool '** of **warm**.

1. Klik op **Opslaan** boven aan de blade.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>De opslaglaag van een blob in een GPv2- of Blob storage-account wijzigen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Ga naar de blob in uw opslagaccount, selecteer **alle Resources**, selecteer uw opslagaccount, uw container en selecteer vervolgens uw blob.

1. In de **Blob-eigenschappen** blade, selecteer de **toegangslaag** in het vervolgkeuzemenu selecteren de **warm**, **' Cool '**, of **archiveren**  toegangslaag.

1. Klik op **Opslaan** boven aan de blade.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Alle opslagaccounts gebruik van een prijsmodel voor Blob-opslag op basis van de laag van elke blob. Houd rekening met de volgende factureringsvoorwaarden:

- **Kosten voor opslag**: Naast de hoeveelheid gegevens die zijn opgeslagen, wordt de kosten voor opslag van gegevens is afhankelijk van de toegangslaag. De kosten per GB nemen af als de laag minder dynamisch ('cooler') wordt.
- **Kosten van gegevenstoegang**: Kosten voor gegevenstoegang vergroten als de laag cooler. U betaalt voor de gegevens in de toegangslaag cool en archive, een post voor toegang tot gegevens per GB voor leesbewerkingen.
- **Transactiekosten**: Er is een kosten per transactie voor alle lagen die wordt verhoogd als de laag cooler.
- **Kosten voor gegevensoverdracht geo-replicatie**: Deze kosten is alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.
- **Kosten voor uitgaande gegevensoverdracht**: Uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB per, consistent met opslagaccounts voor algemeen gebruik.
- **Als u de toegangslaag wijzigt**: De toegangslaag wijzigt van cool naar hot leidt tot kosten voor het lezen van alle bestaande gegevens in de storage-account. Echter de toegangslaag van hot wijzigen naar ' Cool ' leidt tot kosten voor het schrijven van alle gegevens in de koude laag (alleen GPv2-accounts).

> [!NOTE]
> Zie voor meer informatie over de prijzen voor Blob storage-accounts [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) pagina. Zie de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

## <a name="faq"></a>Veelgestelde vragen

**Moet ik Blob Storage- of GPv2-accounts gebruiken als ik mijn gegevens in lagen wil opslaan?**

Wij raden u aan GPv2 te gebruiken in plaats van Blob Storage-accounts voor opslaglagen. GPv2-accounts ondersteunen alle functies die Blob Storage-accounts ondersteunen, plus nog veel meer. Prijzen van Blob Storage en GPv2 zijn bijna identiek, maar sommige nieuwe functies en prijsverlagingen zijn alleen beschikbaar bij GPv2-accounts. GPv1-accounts bieden geen ondersteuning voor opslaglagen.

De prijsstructuur tussen GPv1- en GPv2-accounts is verschillend en klanten moeten beide zorgvuldig evalueren alvorens te besluiten GPv2-accounts te gebruiken. U kunt een bestaand Blob Storage- of GPv1-account eenvoudig met één muisklik omzetten naar GPv2. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie.

**Kan ik objecten opslaan in alle drie (hot, cool en archive) toegangslagen van hetzelfde account?**

Ja. Het kenmerk **Acces Tier** dat op accountniveau is ingesteld, is de standaardlaag die van toepassing is op alle objecten in dat account zonder een expliciet ingestelde laag. Met laaginstelling op blobniveau kunt u echter de toegangslaag op objectniveau instellen, ongeacht de instelling van de toegangslaag voor het account. BLOBs in een van de drie toegangslagen (hot, cool of archive) kunnen bestaan binnen hetzelfde account.

**Kan ik de standaardlaag van mijn Blob- of GPv2-opslagaccount wijzigen?**

Ja, u de standaard-toegangslaag kunt wijzigen door in te stellen de **toegangslaag** kenmerk in de storage-account. Als u de toegangslaag wijzigt van toepassing op alle objecten die zijn opgeslagen in het account waarvoor geen expliciete laag is ingesteld. Bij het omschakelen van de toegangslaag van hot naar cool schrijfbewerkingen (per 10.000) worden voor alle blobs zonder een vaste laag in GPv2-accounts en het omschakelen van statisch naar dynamisch veroorzaakt kosten voor zowel leesbewerkingen (per 10.000) en het ophalen van gegevens (per GB) in rekening gebracht voor alle blobs in Blob-opslag en GPv2-accounts.

**Kan ik de toegangslaag van mijn standaardaccount instellen op Archive?**

Nee. Warme en koude toegangslagen kunnen alleen worden ingesteld als de toegangslaag van het standaardaccount. Archive kan alleen op objectniveau worden ingesteld.

**In welke regio's zijn van de dynamische, statische en archiveren toegangslagen die beschikbaar zijn in?**

De warme en koude toegangslagen samen met laaginstelling op blobniveau zijn beschikbaar in alle regio's. Archive-opslag is in eerste instantie alleen beschikbaar in bepaalde regio's. Zie voor een volledige lijst [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

**Gedragen blobs in de Cool Storage-toegangslaag anders dan blobs in de hot Storage-toegangslaag?**

BLOBs in de hot Storage-toegangslaag hebben dezelfde latentie als blobs in GPv1-, GPv2- en Blob storage-accounts. BLOBs in de Cool Storage-toegangslaag hebben een gelijksoortige latentie (in milliseconden) als blobs in GPv1-, GPv2- en Blob storage-accounts. BLOBs in de Archive Storage-toegangslaag hebben enkele uren latentie in GPv1-, GPv2- en Blob storage-accounts.

BLOBs in de Cool Storage-toegangslaag hebben een iets lagere beschikbaarheid serviceniveau (SLA) dan blobs die zijn opgeslagen in de hot Storage-toegangslaag. Zie [Dienstovereenkomst voor Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) voor meer informatie.

**Zijn de bewerkingen tussen de dynamische-, statische- en archiefopslaglagen hetzelfde?**

Ja. Alle bewerkingen tussen de dynamische en statische laag zijn 100% consistent. Alle geldige archiefbewerkingen, inclusief verwijderen, in lijst opnemen, blob-eigenschappen/metagegevens ophalen en vaste blob-opslaglaag zijn 100% consistent met de dynamische en statische laag. Een blob kan niet worden gelezen of gewijzigd in de archive-laag.

**Als ik een blob reactiveer vanuit de archiefopslaglaag naar de dynamische- of statische-opslaglaag, hoe weet ik dan wanneer de reactivering is voltooid?**

Tijdens het reactiveren kunt u gebruikmaken van de bewerking voor het ophalen van blob-eigenschappen om polling op het kenmerk **Archiefstatus** uit te voeren en te bevestigen dat de laagwijziging is voltooid. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap voor archiefstatus verwijderd en geeft de blob-eigenschap **Toegangslaag** aan of de opslaglaag dynamisch of statisch is.  

**Na het instellen van de opslaglaag van een blob, wanneer wordt er dan gefactureerd tegen het juiste tarief?**

Elke blob wordt altijd gefactureerd volgens de opslaglaag aangegeven door een van de blob **Toegangslaag** eigenschap. Bij het instellen van een nieuwe laag voor een blob, het **Toegangslaag** eigenschap onmiddellijk weergegeven in de nieuwe laag voor alle overgangen weer. Een blob terugplaatsen van de archieflaag naar een dynamische of statische laag kan echter enkele uren duren. In dit geval u worden gefactureerd tegen archive-tarieven totdat de rehydratatie is voltooid, waarna de **Toegangslaag** eigenschap weerspiegelt de nieuwe laag. Op dat moment in rekening gebracht voor die blob tegen het tarief hot of cool.

**Hoe bepaal ik of ik bij het verwijderen of verplaatsen van een blob uit de cool- of archive-laag een toeslag voor vroegtijdige verwijdering moet betalen?**

Voor elke blob die binnen 30 dagen of 180 dagen wordt verwijderd of verplaatst uit respectievelijk de statische (alleen GPv2-accounts) of archiefopslaglaag, wordt een vooraf vastgesteld bedrag voor vroegtijdige verwijdering in rekening gebracht. U kunt bepalen hoe lang een blob is in de cool of archive-laag door het controleren van de **Access Tier Change Time** blob-eigenschap, die een tijdstempel van de laatste wijziging van de laag biedt. Zie voor meer informatie, [vroege verwijdering voor koud en archief](#cool-and-archive-early-deletion).

**Welke Azure Tools en SDK's ondersteunen laaginstelling op blobniveau en archiefopslag?**

Azure Portal, PowerShell en CLI Tools en .NET, Java, Python en Node.js-clientbibliotheken bieden ondersteuning voor laaginstelling op blobniveau en archiefopslag.  

**Hoeveel gegevens kan ik opslaan in de dynamische-, statische- en archiefopslaglaag?**

Opslag van gegevens en andere limieten worden ingesteld op het accountniveau en niet per toegangslaag. Daarom kunt u al uw limieten in één laag of over alle drie de lagen gebruiken. Zie voor meer informatie, [schaalbaarheids- en prestatiedoelen voor Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Evalueren van de dynamische, statische en archieflaag in GPv2 Blob Storage-accounts

[De beschikbaarheid controleren van de dynamische, statische en archieflaag per regio](https://azure.microsoft.com/regions/#services)

[De levenscyclus van de Azure Blob storage beheren](storage-lifecycle-management-concepts.md)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](../common/storage-enable-and-view-metrics.md)

[Prijzen voor de dynamische, statische en archieflaag controleren in Blob Storage- en GPv2-accounts per regio](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)