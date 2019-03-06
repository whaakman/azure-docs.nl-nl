---
title: Premium, Hot, Cool en Archive storage voor blobs - Azure Storage
description: Premium, Hot, Cool en Archive storage voor Azure storage-accounts.
services: storage
author: kuhussai
ms.service: storage
ms.topic: article
ms.date: 03/06/2019
ms.author: kuhussai
ms.subservice: blobs
ms.openlocfilehash: ec024f8f1cd411455a6cbb2a0b12e7b4751af5fe
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405331"
---
# <a name="azure-blob-storage-premium-preview-hot-cool-and-archive-storage-tiers"></a>Azure Blob-opslag: Premium (preview), Hot, Cool en Archive storage-lagen

## <a name="overview"></a>Overzicht

Azure storage biedt verschillende opslaglagen, waarmee u kunt voor het opslaan van gegevens voor Blob-object in de meest kostenefficiënte manier. De beschikbare categorieën zijn onder andere:

- **Premium storage (voorbeeld)** biedt krachtige hardware voor gegevens die regelmatig worden geopend.
 
- **Hot storage**: is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend. 

- **Cool storage** is geoptimaliseerd voor het opslaan van gegevens die niet vaak worden geraadpleegd en die gedurende ten minste 30 dagen worden opgeslagen.
 
- **Archiefopslag** is geoptimaliseerd voor het opslaan van gegevens die zelden worden geopend en die gedurende ten minste 180 dagen met flexibele latentievereisten (orde van grootte uur) worden opgeslagen.

De volgende overwegingen vergezeld gaan van de verschillende opslaglagen:

- De Archive storage-laag is alleen beschikbaar op blob-niveau en niet op het niveau van de storage-account.
 
- Gegevens in de Cool storage-laag kan enigszins lagere beschikbaarheid verdragen, maar nog steeds vereist hoge duurzaamheid en vergelijkbare kenmerken voor de tijd voor toegang en doorvoer als gegevens. Voor gegevens in de Cool, een SLA met een iets lagere beschikbaarheid en hogere zijn kosten in vergelijking met Hot gegevens laag aanvaardbaar-voor lagere opslagkosten.

- Archiefopslag is offline en biedt de laagste kosten voor opslag, maar ook de hoogste toegangskosten.
 
- Alleen de opslaglagen warme en koude kunnen worden ingesteld op het accountniveau. Op dit moment kan niet de Archive-laag worden ingesteld op het accountniveau.
 
- Hot, Cool, en archiefopslaglagen kunnen worden ingesteld op het objectniveau.

Gegevens die zijn opgeslagen in de cloud, groeit exponentieel. Voor een effectief beheer van de kosten voor uw groeiende opslagbehoeften is het vanwege kostenoptimalisatie een goed idee om de gegevens te ordenen op basis van kenmerken als toegangsfrequentie en geplande bewaarperiode. Er bestaan verschillen in de manier waarop gegevens die in de cloud zijn opgeslagen, tijdens hun levensduur worden gegenereerd, benaderd en verwerkt. Sommige gegevens worden tijdens hun hele levensduur actief geopend en gewijzigd. Andere gegevens worden in het begin van hun levensduur regelmatig geopend, terwijl dit naarmate de tijd verstrijkt, aanzienlijk minder vaak gebeurt. Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend.

Het is nuttig om voor elk van deze scenario‘s voor toegang tot gegevens een andere opslaglaag te maken die is geoptimaliseerd voor een specifiek toegangspatroon. Met Hot, Cool en Archive storage-lagen, Azure Blob storage-adressen deze behoefte aan opslaglagen met gedifferentieerde afzonderlijke prijsmodellen.

## <a name="storage-accounts-that-support-tiering"></a>Storage-accounts die ondersteuning bieden voor opslaglagen

U kunt alleen objectopslaggegevens uw voor warm, koud of archief in Blob-opslag en algemeen gebruik v2 (GPv2-accounts). General Purpose v1 (GPv1)-accounts bieden geen ondersteuning voor opslaglagen. Klanten kunnen echter eenvoudig hun bestaande GPv1- of Blob Storage-accounts met één muisklik converteren naar GPv2-accounts in Azure Portal. GPv2 biedt ook een nieuwe prijzenstructuur voor blobs, bestanden en wachtrijen en toegang tot tal van andere nieuwe opslagfuncties. Verder zullen sommige nieuwe mogelijkheden en prijsverlagingen alleen in GPv2-accounts worden aangeboden. Daarom moeten klanten het gebruik van GPv2-accounts evalueren, maar deze alleen gebruiken na herziening van de prijsstelling voor alle diensten, aangezien sommige workloads op GPv2 duurder kunnen zijn dan op GPv1. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie.

BLOB storage en GPv2-accounts geven het **Toegangslaag** kenmerk op het accountniveau waarmee u om op te geven van de standaardopslaglaag als warm of koud voor elke blob in de storage-account dat geen expliciete laag is ingesteld op de objectniveau. Voor objecten met de op objectniveau ingestelde opslaglaag is het accountniveau niet van toepassing. De Archive-laag kan alleen worden toegepast op objectniveau. U kunt op elk gewenst moment schakelen tussen deze opslaglagen.

## <a name="premium-access-tier"></a>Premium-laag voor access

Beschikbaar in preview is een Premium-toegangslaag, waardoor gegevens beschikbaar zijn via krachtige hardware vaak worden gebruikt. Gegevens die zijn opgeslagen in deze laag is opgeslagen op SSD-schijven die zijn geoptimaliseerd voor lagere latentie en hogere transactionele tarieven vergeleken met traditionele harde schijven. De laag Premium is beschikbaar via de blok-Blob opslagaccounttype alleen.

Deze laag is ideaal voor workloads waarvoor snelle en consistente reactietijden. Gegevens die betrekking heeft op eindgebruikers, zoals interactieve videobewerking, statische webinhoud, online transacties en dergelijke zijn goede kandidaten zijn voor de laag Premium. Deze laag is geschikt voor werkbelastingen die veel kleine transacties, zoals het vastleggen van telemetriegegevens, berichten en gegevenstransformatie van uitvoeren.

Zie voor meer informatie over de Preview-versie, [openbare preview van Azure Premium-blobopslag](https://azure.microsoft.com/blog/azure-premium-blob-storage-public-preview/).

## <a name="hot-access-tier"></a>Hot Storage-toegangslaag

Hot storage heeft hogere opslagkosten dan Cool en Archive storage, maar de laagste toegangskosten. Voorbeelden van gebruiksscenario voor de Hot storage-laag zijn onder andere:

* Gegevens die actief worden gebruikt of waarvan wordt verwacht dat ze regelmatig worden geopend (lees- en schrijfbewerkingen).
* Gegevens die tijdelijk worden opgeslagen voor verwerking en uiteindelijk voor migratie naar de Cool storage-laag.

## <a name="cool-access-tier"></a>Cool Storage-toegangslaag

Cool storage-toegangslaag heeft lagere opslagkosten en hogere toegangskosten in vergelijking met Hot storage. Deze laag is bedoeld voor gegevens die voor ten minste 30 dagen in de koude laag blijven. Voorbeelden van gebruiksscenario voor de Cool storage-laag zijn onder andere:

* Gegevenssets waarvan voor de korte termijn een back-up is gemaakt en die na een noodgeval zijn hersteld.
* Oudere media-inhoud die niet meer regelmatig wordt bekeken, maar onmiddellijk beschikbaar moet zijn wanneer deze wordt geopend.
* Grote gegevenssets die voordelig moeten worden opgeslagen, terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*Bijvoorbeeld*: langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een productiefaciliteit)

## <a name="archive-access-tier"></a>Archive-toegangslaag

Archive storage heeft de laagste opslagkosten en hogere kosten voor gegevens ophalen in vergelijking met Hot en Cool storage. Deze laag is bedoeld voor gegevens die enkele uren latentie bij het ophalen kan tolereren en blijven in de Archive-laag voor ten minste 180 dagen.

Hoewel een blob in Archive storage, wordt de blob-gegevens offline is en kan niet worden gelezen, gekopieerd, overschreven of gewijzigd. Ook kunt u momentopnamen maken van een blob in Archive storage. De metagegevens van de blob blijft echter online en beschikbaar, zodat u om de blob en de bijbehorende eigenschappen weer te geven. Voor blobs in Archief zijn de enige geldige bewerkingen GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier en DeleteBlob. 


Voorbeelden van gebruiksscenario voor de Archive storage-laag zijn onder andere:

* Langetermijnback-up, secundaire back-up en gegevenssets voor archivering
* Oorspronkelijke (onbewerkte) gegevens die moeten worden bewaard, zelfs nadat deze zijn verwerkt tot hun uiteindelijke, bruikbare vorm. (*Bijvoorbeeld*: onbewerkte mediabestanden na transcodering naar andere indelingen)
* Compliance- en archiveringsgegevens die gedurende lange tijd moeten worden opgeslagen en bijna nooit worden geopend. (*Bijvoorbeeld*: beeldmateriaal van beveiligingscamera‘s, oude röntgenfoto‘s/MRI‘s in ziekenhuizen, geluidsopnamen en transcripties van verkoopgesprekken voor financiële diensten)

### <a name="blob-rehydration"></a>Rehydratatie van blobs
Als u wilt lezen van gegevens in Archive storage, moet u eerst de laag van de blob naar warm of koud wijzigen. Dit proces staat bekend als rehydratatie en het voltooien ervan kan wel 15 uur duren. Grote blob-opslag worden aanbevolen voor optimale prestaties. Enkele kleine blobs tegelijk reactiveren kan extra tijd toevoegen.

Tijdens rehydratatie kunt u aan de blob-eigenschap **archive status** zien of de laag is gewijzigd. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Na voltooiing, de eigenschap Archive status wordt verwijderd, en de **Toegangslaag** blob-eigenschap geeft de nieuwe warme of koude laag.  

## <a name="blob-level-tiering"></a>Laaginstelling op blobniveau

De functie Laaginstelling op blob-niveau maakt het mogelijk om de laag van uw gegevens op objectniveau te wijzigen met behulp van een eenmalige bewerking met de naam [Blob-laag instellen](/rest/api/storageservices/set-blob-tier). U kunt de toegangslaag van een blob uit de niveaus warm, koud of archief eenvoudig wijzigen als de gebruikspatronen zonder te verplaatsen van gegevens tussen accounts. Alle laagwijzigingen vinden direct plaats. Een blob in archief te reactiveren kan echter enkele uren duren. 

Het tijdstip waarop de laatste wijziging aan de blob-laag heeft plaatsgevonden, wordt weergegeven via de blob-eigenschap **Access Tier Change Time**. Worden als een blob in de laag archief was, deze kan niet overschreven, zodat dezelfde blob uploaden is niet toegestaan in dit scenario. U kunt een blob in een warme of koude laag, waarin de nieuwe blob geval neemt de laag van de blob die is overschreven over overschrijven.

Blobs in alle drie de opslaglagen kunnen naast elkaar bestaan binnen hetzelfde account. Een blob waaraan niet expliciet een laag is toegewezen, leidt de laag af die is ingesteld als toegangslaag voor het account. Als de toegangslaag van het account is afgeleid, ziet u de **Access Tier Inferred** blob-eigenschap is ingesteld op 'true', en met de blob **Toegangslaag** blob-eigenschap komt overeen met de accountlaag. In Azure Portal wordt de eigenschap Access Tier Inferred weergegeven met de Blob-toegangslaag (bijvoorbeeld Hot (afgeleid) of Cool (afgeleid)).

> [!NOTE]
> Archiefopslag en laaginstelling op blobniveau ondersteunen alleen blok-blobs. U kunt evenmin de laag wijzigen van een blok-blob die momentopnamen bevat.

> [!NOTE]
> Gegevens die zijn opgeslagen in de laag Premium kan niet op dit moment worden lagen Hot, Cool of Archive met behulp van [Blob-laag instellen](/rest/api/storageservices/set-blob-tier) of met behulp van Azure Blob Storage-levenscyclusbeheer. Om gegevens te verplaatsen, moet u synchroon kopiëren blobs uit de Premium-toegang voor het gebruik van ' hot ' de [blok plaatsen van URL API](/rest/api/storageservices/put-block-from-url) of een versie van AzCopy die ondersteuning biedt voor deze API. De *blok plaatsen van URL* API synchroon worden gegevens gekopieerd op de server, wat betekent dat de aanroep is voltooid maar één keer alle de gegevens van de oorspronkelijke serverlocatie wordt verplaatst naar de doellocatie.

### <a name="blob-lifecycle-management"></a>Beheer van de BLOB-levenscyclus
BLOB Storage lifecycle management (Preview) biedt een uitgebreide, op basis van regels beleid dat u gebruiken kunt voor de overgang van uw gegevens naar de beste toegangslaag en verloopt gegevens aan het einde van de levenscyclus. Zie [beheren van de levenscyclus van de Azure Blob-opslag](storage-lifecycle-management-concepts.md) voor meer informatie.  

### <a name="blob-level-tiering-billing"></a>Facturering van laaginstelling op blobniveau

Wanneer een blob wordt verplaatst naar een minder dynamische laag (dynamisch -> statisch, dynamisch -> archief of statisch -> archief), de bewerking gefactureerd als een schrijfbewerking naar de bestemmingslaag, waar de schrijfbewerking (per 10.000) en de kosten voor gegevens schrijven (per GB) van de bestemmingslaag van toepassing. Wanneer een blob wordt verplaatst naar een dynamischer laag (archief -> statisch, archief -> warm of koud -> warm), de bewerking gefactureerd als een leesbewerking vanuit de bronlaag, waar de leesbewerking (per 10.000) en de kosten voor gegevens (per GB) voor het ophalen van de bronlaag van toepassing. De volgende tabel geeft een overzicht van hoe laagwijzigingen worden in rekening gebracht.

| | **Schrijven van kosten in rekening gebracht (bewerking + toegang)** | **Alleen kosten in rekening gebracht (bewerking + toegang)** 
| ---- | ----- | ----- |
| **SetBlobTier richting** | Dynamisch -> statisch, dynamisch -> archief, statisch -> archief | Archief -> statisch, archief -> dynamisch, statisch -> dynamisch

Als u het accountniveau van warm naar koud omschakelt, wordt in rekening gebracht voor schrijfbewerkingen (per 10.000) voor alle blobs zonder een vaste laag in GPv2-accounts. Er zijn geen kosten voor deze wijziging in Blob storage-accounts. U wordt in rekening gebracht voor zowel leesbewerkingen (per 10.000) als het ophalen van gegevens (per GB) als u uw Blob storage- of GPv2-account overschakelt van koud naar warm overzet. Kosten voor vroegtijdige verwijdering voor elke blob verplaatst uit de Cool of Archive-laag kunnen ook van toepassing zijn.

### <a name="cool-and-archive-early-deletion"></a>Vroege verwijdering voor Koud en Archief

Naast de per GB per maand, voor elke blob die wordt verplaatst naar de koude laag (alleen GPv2-accounts) onderworpen aan een ' Cool ' vroege verwijderingsperiode van 30 dagen en voor elke blob die wordt verplaatst naar de Archive-laag onderworpen aan een vroege verwijderingsperiode voor archief van 180 dagen. Deze kosten zijn evenredig verdeeld. Bijvoorbeeld, als een blob wordt verplaatst naar het archief en die vervolgens worden verwijderd of verplaatst naar de laag warm na 45 dagen wordt u gefactureerd een bedrag voor vroege verwijdering gelijk is aan 135 (180 minus 45) dagen van de opslag van die blob in archief.

U mag de vroegtijdige verwijdering berekenen met behulp van de blob-eigenschap **Aanmaaktijd**, als er geen toegang laagwijzigingen is. U kunt anders gebruiken als de toegangslaag voor het laatst is gewijzigd naar koud of archief door te bekijken van de blob-eigenschap: **access-tier-wijzigen-time**. Zie voor meer informatie over blob-eigenschappen [Blob-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparison-of-the-storage-tiers"></a>Vergelijking van de opslaglagen

De volgende tabel bevat een vergelijking van de Hot, Cool en Archive storage-lagen.

| | **Hot Storage-laag** | **Cool Storage-laag** | **Archive Storage-laag**
| ---- | ----- | ----- | ----- |
| **Beschikbaarheid** | 99,9% | 99% | N/A |
| **Beschikbaarheid** <br> **(RA-GRS-leesbewerkingen)**| 99,99% | 99,9% | N/A |
| **Gebruikskosten** | Hogere opslagkosten, lagere toegangs- en transactiekosten | Lagere opslagkosten, hogere toegang en transactiekosten | Laagste opslagkosten, hoogste toegangs- en transactiekosten |
| **Minimale objectgrootte** | N/A | N/A | N/A |
| **Minimale opslagduur** | N/A | 30 dagen (alleen GPv2) | 180 dagen
| **Latentie** <br> **(Tijd tot eerste byte)** | milliseconden | milliseconden | < 15 uur
| **Schaalbaarheids- en prestatiedoelen** | Dezelfde als bij opslagaccounts voor algemeen gebruik | Dezelfde als bij opslagaccounts voor algemeen gebruik | Dezelfde als bij opslagaccounts voor algemeen gebruik |

> [!NOTE]
> Blob Storage-accounts bieden ondersteuning voor dezelfde schaalbaarheids- en prestatiedoelen als opslagaccounts voor algemeen gebruik. Zie voor meer informatie, [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). 

## <a name="quickstart-scenarios"></a>Snelstartscenario's

In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

* Het wijzigen van de toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account.
* Het wijzigen van de opslaglaag van een blob in een GPv2- of Blob Storage-account.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>De toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account wijzigen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

3. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.

4. Selecteer de juiste opslaglaag voor uw behoeften: Stelt de **toegangslaag** aan **' Cool '** of **warm**.

5. Klik op Opslaan boven aan de blade.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>De opslaglaag van een blob in een GPv2- of Blob Storage-account wijzigen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Als u naar de blob in uw opslagaccount wilt gaan, selecteert u achtereenvolgens Alle resources, uw opslagaccount, uw container en uw blob.

3. Klik op de blade met blob-eigenschappen op het vervolgkeuzemenu **Toegangslaag** om de opslaglaag **Dynamisch**, **Statisch** of **Archief** te selecteren.

5. Klik op Opslaan boven aan de blade.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Alle opslagaccounts gebruik van een prijsmodel voor Blob-opslag op basis van de laag van elke blob. Houd rekening met de volgende factureringsvoorwaarden:

* **Kosten voor opslag**: Naast de hoeveelheid gegevens die zijn opgeslagen, wordt de kosten voor opslag van gegevens is afhankelijk van de storage-laag. De kosten per GB nemen af als de laag minder dynamisch ('cooler') wordt.
* **Kosten van gegevenstoegang**: Kosten voor gegevenstoegang vergroten als de laag cooler. U betaalt voor gegevens in de Cool en Archive storage-laag een toeslag voor toegang tot gegevens per GB voor leesbewerkingen.
* **Transactiekosten**: Er is een kosten per transactie voor alle lagen die wordt verhoogd als de laag cooler.
* **Kosten voor gegevensoverdracht geo-replicatie**: Deze kosten is alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.
* **Kosten voor uitgaande gegevensoverdracht**: Uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB per, consistent met opslagaccounts voor algemeen gebruik.
* **Als u de opslaglaag wijzigt**: De opslaglaag wijzigt van Cool naar Hot leidt tot kosten voor het lezen van alle bestaande gegevens in de storage-account. Echter, als u de opslaglaag wijzigt van warm naar koud leidt tot kosten voor het schrijven van alle gegevens in de koude laag (alleen GPv2-accounts).

> [!NOTE]
> Zie voor meer informatie over de prijzen voor Blob storage-accounts [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) pagina. Zie de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

## <a name="faq"></a>Veelgestelde vragen

**Moet ik Blob Storage- of GPv2-accounts gebruiken als ik mijn gegevens in lagen wil opslaan?**

Wij raden u aan GPv2 te gebruiken in plaats van Blob Storage-accounts voor opslaglagen. GPv2-accounts ondersteunen alle functies die Blob Storage-accounts ondersteunen, plus nog veel meer. Prijzen van Blob Storage en GPv2 zijn bijna identiek, maar sommige nieuwe functies en prijsverlagingen zijn alleen beschikbaar bij GPv2-accounts. GPv1-accounts bieden geen ondersteuning voor opslaglagen.

De prijsstructuur tussen GPv1- en GPv2-accounts is verschillend en klanten moeten beide zorgvuldig evalueren alvorens te besluiten GPv2-accounts te gebruiken. U kunt een bestaand Blob Storage- of GPv1-account eenvoudig met één muisklik omzetten naar GPv2. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie.

**Kan ik objecten opslaan in alle drie (Hot, Cool en Archive) opslaglagen van hetzelfde account?**

Ja. Het kenmerk **Acces Tier** dat op accountniveau is ingesteld, is de standaardlaag die van toepassing is op alle objecten in dat account zonder een expliciet ingestelde laag. Met laaginstelling op blobniveau kunt u echter de toegangslaag op objectniveau instellen, ongeacht de instelling van de toegangslaag voor het account. BLOBs in een van de drie opslaglagen (warm, koud of archief) mag bestaan binnen hetzelfde account.

**Kan ik de standaardopslaglaag van mijn Blob Storage- of GPv2-account wijzigen?**

Ja, u kunt de standaardopslaglaag in het opslagaccount wijzigen door het kenmerk **Toegangslaag** in te stellen voor het opslagaccount. Als u de opslaglaag wijzigt, geldt dit voor alle objecten waarvoor geen expliciete laag is ingesteld en die zijn opgeslagen in het account. Schrijfbewerkingen (per 10.000) bij met het omschakelen van de opslaglaag van warm naar koud worden voor alle blobs zonder een vaste laag in GPv2-accounts en het omschakelen van koud naar warm veroorzaakt kosten voor zowel leesbewerkingen (per 10.000) en het ophalen van gegevens (per GB) in rekening gebracht voor alle blobs in Blob-opslag en GPv2-accounts.

**Kan ik de toegangslaag van Mijn standaardaccount instellen in archief?**

Nee. Alleen warme en koude opslaglagen kunnen worden ingesteld als de toegangslaag van het standaardaccount. Archive kan alleen op objectniveau worden ingesteld.

**In welke regio's zijn de opslaglagen Hot, Cool en Archive beschikbaar?**

De warme en koude opslaglagen, samen met laaginstelling op blobniveau zijn beschikbaar in alle regio's. Archive-opslag is in eerste instantie alleen beschikbaar in bepaalde regio's. Zie voor een volledige lijst [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

**Gedragen blobs in de Cool storage-laag anders dan blobs in de Hot storage-laag?**

BLOBs in de warme opslaglaag hebben dezelfde latentie als blobs in GPv1-, GPv2- en Blob storage-accounts. BLOBs in de opslaglaag hebben een gelijksoortige latentie (in milliseconden) als blobs in GPv1-, GPv2- en Blob storage-accounts. BLOBs in de archiefopslaglaag kennen enkele uren latentie in GPv1-, GPv2- en Blob storage-accounts.

BLOBs in de opslaglaag hebben een iets lagere beschikbaarheid serviceniveau (SLA) dan blobs die zijn opgeslagen in de Hot storage-laag. Zie [Dienstovereenkomst voor Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) voor meer informatie.

**Zijn de bewerkingen tussen de lagen Hot, Cool en Archive hetzelfde?**

Ja. Alle bewerkingen tussen warme en koude zijn 100% consistent. Alle geldige archiefbewerkingen, inclusief verwijderen, zijn lijst, get-blob/eigenschappen/metagegevens en vaste blob-opslaglaag 100% consistent zijn met warme en koude. Een blob kan niet worden gelezen of gewijzigd in de Archive-laag.

**Wanneer een blob vanuit de archiefopslaglaag naar de warme of koude laag te reactiveren, hoe weet ik wanneer de reactivering is voltooid?**

Tijdens het reactiveren kunt u gebruikmaken van de bewerking voor het ophalen van blob-eigenschappen om polling op het kenmerk **Archiefstatus** uit te voeren en te bevestigen dat de laagwijziging is voltooid. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Na voltooiing, de eigenschap Archive status wordt verwijderd, en de **Toegangslaag** blob-eigenschap geeft de nieuwe warme of koude laag.  

**Na het instellen van de opslaglaag van een blob, wanneer wordt er dan gefactureerd tegen het juiste tarief?**

Elke blob wordt altijd gefactureerd volgens de opslaglaag aangegeven door een van de blob **Toegangslaag** eigenschap. Bij het instellen van een nieuwe laag voor een blob, het **Toegangslaag** eigenschap onmiddellijk weergegeven in de nieuwe laag voor alle overgangen weer. Een blob uit de Archive-laag naar een warme of koude laag te reactiveren kan echter enkele uren duren. In dit geval u worden gefactureerd tegen Archive-tarieven totdat de rehydratatie is voltooid, waarna de **Toegangslaag** eigenschap weerspiegelt de nieuwe laag. Op dat moment in rekening gebracht voor die blob in de Hot of Cool snelheid.

**Hoe bepaal ik als ik een vroegtijdige verwijdering in rekening in gebracht rekening bij het verwijderen of verplaatsen van een blob uit de Cool of Archive-laag?**

Elke blob die wordt verwijderd of verplaatst uit respectievelijk de statische (alleen GPv2-accounts) of archiefopslaglaag 30 dagen of 180 dagen wordt een vooraf vastgesteld bedrag vroegtijdige verwijdering in rekening gebracht. U kunt bepalen hoe lang een blob is in de Cool of Archive-laag door het controleren van de **Access Tier Change Time** blob-eigenschap, die een tijdstempel van de laatste wijziging van de laag biedt. Zie voor meer informatie, [koud en vroege verwijdering voor archief](#cool-and-archive-early-deletion).

**Welke Azure-hulpprogramma's en SDK's ondersteunen laaginstelling op blobniveau en archiefopslag?**

Azure portal, PowerShell en CLI tools en .NET, Java, Python en Node.js-clientbibliotheken ondersteuning voor laaginstelling op blobniveau en archiefopslag.  

**Hoeveel gegevens kan ik opslaan in de lagen Hot, Cool en Archive?**

Gegevensopslag en andere limieten worden ingesteld op het accountniveau en niet per opslaglaag. Daarom kunt u al uw limieten in één laag of over alle drie de lagen gebruiken. Zie voor meer informatie, [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Hot, Cool en Archive in GPv2 Blob storage-accounts evalueren

[De beschikbaarheid van Hot, Cool en Archive per regio controleren](https://azure.microsoft.com/regions/#services)

[De levenscyclus van de Azure Blob storage beheren](storage-lifecycle-management-concepts.md)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](../common/storage-enable-and-view-metrics.md)

[Hot, Cool en Archive prijzen in Blob storage en GPv2-accounts per regio controleren](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)
