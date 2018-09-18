---
title: Azure Hot, Cool en Archive Storage voor blobs | Microsoft Docs
description: Dynamische, statische en archiefopslag voor Azure Storage-accounts.
services: storage
author: kuhussai
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: kuhussai
ms.component: blobs
ms.openlocfilehash: 66c47a97eee6759eb963db43d5c573fb6612bde6
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735915"
---
# <a name="azure-blob-storage-hot-cool-and-archive-storage-tiers"></a>Azure Blob Storage: dynamische, statische en archiefopslaglaag

## <a name="overview"></a>Overzicht

Azure Storage biedt drie opslaglagen voor de opslag van blob-objecten, zodat u gegevens zeer voordelig kunt opslaan afhankelijk van hoe u deze gebruikt. De Azure **Hot Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend. De Azure **Cool Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die niet regelmatig worden geopend en die gedurende minimaal dertig dagen worden opgeslagen. De Azure **Archive Storage-laag** is geoptimaliseerd voor het opslaan van gegevens die zelden worden geraadpleegd en die gedurende ten minste 180 dagen worden opgeslagen met flexibele latentievereisten (orde van grootte uur). De Archive Storage-laag is slechts beschikbaar op blob-niveau en niet op opslagaccountniveau. Voor gegevens in de Cool Storage-laag is een iets lagere beschikbaarheid toegestaan, maar ze vereisen nog steeds een hoge duurzaamheid en een gelijke tijdsduur voor toegang en doorvoer als gegevens in de Hot Storage-laag. Voor gegevens in de Cool Storage-laag zijn een SLA met een iets lagere beschikbaarheid en hogere toegangskosten (in vergelijking met gegevens in de Hot Storage-laag) aanvaardbaar vanwege de veel lagere opslagkosten. Archiefopslag is offline en biedt de laagste kosten voor opslag, maar ook de hoogste toegangskosten. Alleen de dynamische-opslaglaag en statische-opslaglaag (niet archief) kunnen worden ingesteld op het accountniveau. De lagen kunnen alle drie op het objectniveau worden ingesteld.

Het aantal gegevens dat is opgeslagen in de cloud, groeit vandaag de dag exponentieel. Voor een effectief beheer van de kosten voor uw groeiende opslagbehoeften is het vanwege kostenoptimalisatie een goed idee om de gegevens te ordenen op basis van kenmerken als toegangsfrequentie en geplande bewaarperiode. Er bestaan verschillen in de manier waarop gegevens die in de cloud zijn opgeslagen, tijdens hun levensduur worden gegenereerd, benaderd en verwerkt. Sommige gegevens worden tijdens hun hele levensduur actief geopend en gewijzigd. Andere gegevens worden in het begin van hun levensduur regelmatig geopend, terwijl dit naarmate de tijd verstrijkt, aanzienlijk minder vaak gebeurt. Weer andere gegevens in de cloud zijn inactief en worden, als ze eenmaal zijn opgeslagen, zelden tot nooit geopend.

Het is nuttig om voor elk van deze scenario‘s voor toegang tot gegevens een andere opslaglaag te maken die is geoptimaliseerd voor een specifiek toegangspatroon. Door middel van de lagen Hot, Cool en Archive Storage wordt in Azure Blob Storage voorzien in deze behoefte aan gedifferentieerde opslaglagen met afzonderlijke prijsmodellen.

## <a name="storage-accounts-that-support-tiering"></a>Storage-accounts die ondersteuning bieden voor opslaglagen

U kunt uw objectopslaggegevens alleen in dynamische, statische en archiefopslaglagen opslaan voor Blob Storage of General Purpose v2 (GPv2-accounts). General Purpose v1 (GPv1)-accounts bieden geen ondersteuning voor opslaglagen. Klanten kunnen echter eenvoudig hun bestaande GPv1- of Blob Storage-accounts met één muisklik converteren naar GPv2-accounts in Azure Portal. GPv2 biedt ook een nieuwe prijzenstructuur voor blobs, bestanden en wachtrijen en toegang tot tal van andere nieuwe opslagfuncties. Verder zullen sommige nieuwe mogelijkheden en prijsverlagingen alleen in GPv2-accounts worden aangeboden. Daarom moeten klanten het gebruik van GPv2-accounts evalueren, maar deze alleen gebruiken na herziening van de prijsstelling voor alle diensten, aangezien sommige workloads op GPv2 duurder kunnen zijn dan op GPv1. Zie voor meer informatie, [overzicht van Azure storage-account](../common/storage-account-overview.md).

Blob Storage- en GPv2-accounts maken het kenmerk **Toegangslaag** beschikbaar op het accountniveau waarmee u de standaard opslaglaag als dynamisch of statisch kunt opgeven voor elke blob in het opslagaccount die niet beschikt over de opslaglaag die is ingesteld op het objectniveau. Voor objecten met de op objectniveau ingestelde opslaglaag is het accountniveau niet van toepassing. De Archive-laag kan alleen worden toegepast op objectniveau. U kunt op elk gewenst moment schakelen tussen deze opslaglagen.

## <a name="hot-access-tier"></a>Hot Storage-toegangslaag

Voor Hot Storage gelden hogere opslagkosten dan Cold Storage en Archive Storage, maar Hot Storage heeft wel de laagste toegangskosten. Enkele voorbeelden van gebruiksscenario's voor de opslaglaag voor 'hot' blobs:

* Gegevens die actief worden gebruikt of waarvan wordt verwacht dat ze regelmatig worden geopend (lees- en schrijfbewerkingen).
* Gegevens die tijdelijk worden opgeslagen voor verwerking en uiteindelijk voor migratie naar de Cool Storage-laag.

## <a name="cool-access-tier"></a>Cool Storage-toegangslaag

De Cool Storage-toegangslaag heeft lagere opslagkosten en hogere toegangskosten in vergelijking met Hot Storage. Deze laag is bedoeld voor gegevens die ten minste dertig dagen in de Cold Storage verblijven. Enkele voorbeelden van gebruiksscenario's voor de Cool Storage-laag:

* Gegevenssets waarvan voor de korte termijn een back-up is gemaakt en die na een noodgeval zijn hersteld.
* Oudere media-inhoud die niet meer regelmatig wordt bekeken, maar onmiddellijk beschikbaar moet zijn wanneer deze wordt geopend.
* Grote gegevenssets die voordelig moeten worden opgeslagen, terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*Bijvoorbeeld*: langetermijnopslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een productiefaciliteit)

## <a name="archive-access-tier"></a>Archive-toegangslaag

Archive Storage heeft de laagste opslagkosten en hogere kosten voor het ophalen van gegevens in vergelijking met Hot en Cold Storage. Deze laag is bedoeld voor gegevens die enkele uren latentie kunnen verdragen bij terughalen en die ten minste 180 dagen in de archieflaag verblijven.

Als een blob in Archive Storage verblijft, is deze offline en kan niet worden gelezen (dit geldt niet voor de metagegevens, die online en beschikbaar zijn), gekopieerd, overschreven of gewijzigd. Ook kunt u geen momentopnamen maken van een blob in Archive Storage. U kunt echter bestaande bewerkingen gebruiken voor een blob om deze te verwijderen, weer te geven in een lijst, de eigenschappen/metagegevens van de blob weer te geven of de opslaglaag van de blob wijzigen.

Enkele voorbeelden van gebruiksscenario's voor de Archive Storage-laag:

* Langetermijnback-up, secundaire back-up en gegevenssets voor archivering
* Oorspronkelijke (onbewerkte) gegevens die moeten worden bewaard, zelfs nadat deze zijn verwerkt tot hun uiteindelijke, bruikbare vorm. (*Bijvoorbeeld*: onbewerkte mediabestanden na transcodering naar andere indelingen)
* Compliance- en archiveringsgegevens die gedurende lange tijd moeten worden opgeslagen en bijna nooit worden geopend. (*Bijvoorbeeld*: beeldmateriaal van beveiligingscamera‘s, oude röntgenfoto‘s/MRI‘s in ziekenhuizen, geluidsopnamen en transcripties van verkoopgesprekken voor financiële diensten)

### <a name="blob-rehydration"></a>Rehydratatie van blobs
Als u gegevens wilt lezen die aanwezig zijn in Archive Storage, moet u de laag van de blob eerst wijzigen in Hot of Cool. Dit proces staat bekend als rehydratatie en het voltooien ervan kan wel 15 uur duren. Grote blob-opslag worden aanbevolen voor optimale prestaties. Enkele kleine blobs tegelijk reactiveren kan extra tijd toevoegen.

Tijdens rehydratatie kunt u aan de blob-eigenschap **archive status** zien of de laag is gewijzigd. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap voor archiefstatus verwijderd en geeft de blob-eigenschap **Toegangslaag** aan of de opslaglaag dynamisch of statisch is.  

## <a name="blob-level-tiering"></a>Laaginstelling op blobniveau

De functie Laaginstelling op blob-niveau maakt het mogelijk om de laag van uw gegevens op objectniveau te wijzigen met behulp van een eenmalige bewerking met de naam [Blob-laag instellen](/rest/api/storageservices/set-blob-tier). U kunt de toegangslaag van een blob gemakkelijk wijzigen van Hot in Cool of Archive, plus alle mogelijke varianten. U kunt zo snel inspelen op veranderende gebruikspatronen zonder dat u gegevens tussen accounts hoeft te verplaatsen. Alle laagwijzigingen vinden direct plaats. Een blob in archief te reactiveren kan echter enkele uren duren. 

Het tijdstip waarop de laatste wijziging aan de blob-laag heeft plaatsgevonden, wordt weergegeven via de blob-eigenschap **Access Tier Change Time**. Worden als een blob in de laag archief was, deze kan niet overschreven, zodat dezelfde blob uploaden is niet toegestaan in dit scenario. U kunt een blob in een laag hot of cool, waarin de nieuwe blob geval neemt de laag van de blob die is overschreven over overschrijven.

Blobs in alle drie de opslaglagen kunnen naast elkaar bestaan binnen hetzelfde account. Een blob waaraan niet expliciet een laag is toegewezen, leidt de laag af die is ingesteld als toegangslaag voor het account. Als de toegangslaag van het account is afgeleid, ziet u de **Access Tier Inferred** blob-eigenschap is ingesteld op 'true', en met de blob **Toegangslaag** blob-eigenschap komt overeen met de accountlaag. In Azure Portal wordt de eigenschap Access Tier Inferred weergegeven met de Blob-toegangslaag (bijvoorbeeld Hot (afgeleid) of Cool (afgeleid)).

> [!NOTE]
> Archiefopslag en laaginstelling op blobniveau ondersteunen alleen blok-blobs. U kunt evenmin de laag wijzigen van een blok-blob die momentopnamen bevat.

### <a name="blob-lifecycle-management"></a>Beheer van de BLOB-levenscyclus
BLOB Storage lifecycle management (Preview) biedt een uitgebreide, op basis van regels beleid dat u gebruiken kunt voor de overgang van uw gegevens naar de beste toegangslaag en verloopt gegevens aan het einde van de levenscyclus. Zie [beheren van de levenscyclus van de Azure Blob-opslag](https://docs.microsoft.com/en-us/azure/storage/common/storage-lifecycle-managment-concepts) voor meer informatie.  

### <a name="blob-level-tiering-billing"></a>Facturering van laaginstelling op blobniveau

Wanneer een blob wordt verplaatst naar een minder dynamische laag (dynamisch -> statisch, dynamisch -> archief of statisch -> archief), de bewerking gefactureerd als een schrijfbewerking naar de bestemmingslaag, waar de schrijfbewerking (per 10.000) en de kosten voor gegevens schrijven (per GB) van de bestemmingslaag van toepassing. Als een blob wordt verplaatst naar een meer dynamische laag (archief->statisch, archief->dynamisch of statisch->dynamisch), wordt de bewerking gefactureerd als een leesbewerking vanuit de bronlaag, waar de kosten voor de leesbewerking (per 10.000) en het ophalen van gegevens (per GB) voor de bronlaag van toepassing zijn.

Als u het accountniveau omschakelt van dynamisch naar statisch, worden alleen schrijfacties in rekening gebracht (per 10.000) voor alle blobs zonder een ingestelde opslaglaag in GPv2-accounts. Er zijn geen kosten voor deze wijziging in Blob storage-accounts. Er worden kosten in rekening gebracht voor zowel leesbewerkingen (per 10.000) als voor het ophalen van gegevens (per GB) als u uw Blob Storage- of GPv2-account omschakelt van statisch naar dynamisch. Kosten voor vroegtijdige verwijdering van een blob die is verplaatst uit de opslaglaag Cool of Archive kunnen ook van toepassing zijn.

### <a name="cool-and-archive-early-deletion"></a>Vroege verwijdering voor Koud en Archief

In aanvulling op de kosten per GB per maand is elke blob die wordt verplaatst naar de cool-opslaglaag (alleen GPv2-accounts) onderworpen aan een periode van 30 dagen van vroegtijdige verwijdering uit Cool en is elke blob die wordt verplaatst naar de Archive-laag onderworpen aan een periode van 180 dagen van vroegtijdige verwijdering uit Archive. Deze kosten zijn evenredig verdeeld. Als bijvoorbeeld een blob na 45 dagen wordt verplaatst naar de archieflaag en vervolgens wordt verwijderd of na 45 dagen wordt verplaatst naar de dynamische laag, worden kosten voor vroegtijdige verwijdering in rekening gebracht die gelijk zijn aan 135 (180 min 45) dagen van opslag van die blob in de archieflaag.

## <a name="comparison-of-the-storage-tiers"></a>Vergelijking van de opslaglagen

In de volgende tabel ziet u een vergelijking van de dynamische, statische en archiefopslaglaag.

| | **Hot Storage-laag** | **Cool Storage-laag** | **Archive Storage-laag**
| ---- | ----- | ----- | ----- |
| **Beschikbaarheid** | 99,9% | 99% | N/A |
| **Beschikbaarheid** <br> **(RA-GRS-leesbewerkingen)**| 99,99% | 99,9% | N/A |
| **Gebruikskosten** | Hogere opslagkosten, lagere toegangs- en transactiekosten | Lagere opslagkosten, hogere toegangs- en transactiekosten | Laagste opslagkosten, hoogste toegangs- en transactiekosten |
| **Minimale objectgrootte** | N/A | N/A | N/A |
| **Minimale opslagduur** | N/A | 30 dagen (alleen GPv2) | 180 dagen
| **Latentie** <br> **(Tijd tot eerste byte)** | milliseconden | milliseconden | < 15 uur
| **Schaalbaarheids- en prestatiedoelen** | Dezelfde als bij opslagaccounts voor algemeen gebruik | Dezelfde als bij opslagaccounts voor algemeen gebruik | Dezelfde als bij opslagaccounts voor algemeen gebruik |

> [!NOTE]
> Blob Storage-accounts bieden ondersteuning voor dezelfde schaalbaarheids- en prestatiedoelen als opslagaccounts voor algemeen gebruik. Zie [Schaalbaarheids- en prestatiedoelen in Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

## <a name="quickstart-scenarios"></a>Snelstartscenario's

In deze sectie worden de volgende scenario‘s toegelicht, waarbij gebruik wordt gemaakt van Azure Portal:

* Het wijzigen van de toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account.
* Het wijzigen van de opslaglaag van een blob in een GPv2- of Blob Storage-account.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>De toegangslaag van het standaardaccount van een GPv2- of Blob Storage-account wijzigen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Ga naar uw opslagaccount, selecteer Alle resources en selecteer vervolgens uw opslagaccount.

3. Klik in de Instellingen-blade op **Configuratie** om de accountconfiguratie te bekijken en/of te wijzigen.

4. Selecteer de juiste opslaglaag voor wat u nodig hebt: stel de **Toegangslaag** in op **'Cool'** of **'Hot'**.

5. Klik op Opslaan boven aan de blade.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>De opslaglaag van een blob in een GPv2- of Blob Storage-account wijzigen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Als u naar de blob in uw opslagaccount wilt gaan, selecteert u achtereenvolgens Alle resources, uw opslagaccount, uw container en uw blob.

3. Klik op de blade met blob-eigenschappen op het vervolgkeuzemenu **Toegangslaag** om de opslaglaag **Dynamisch**, **Statisch** of **Archief** te selecteren.

5. Klik op Opslaan boven aan de blade.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Alle opslagaccounts gebruik van een prijsmodel voor Blob-opslag op basis van de laag van elke blob. Houd rekening met de volgende factureringsvoorwaarden:

* **Opslagkosten**: de kosten voor het opslaan van gegevens hangen niet alleen af van de hoeveelheid opgeslagen gegevens, maar ook van de gebruikte opslaglaag. De kosten per GB nemen af als de laag minder dynamisch ('cooler') wordt.
* **Kosten van gegevenstoegang**: de kosten voor gegevenstoegang nemen toe als de laag minder dynamisch ('cooler') wordt. Voor gegevens in de lagen Cool Storage en Archive Storage worden kosten per GB in rekening gebracht aan gegevenstoegang voor leesbewerkingen.
* **Transactiekosten**: er gelden kosten per transactie voor alle lagen. Deze kosten nemen toe als de laag minder dynamisch wordt.
* **Kosten voor gegevensoverdracht met geo-replicatie**: deze kosten zijn alleen van toepassing op accounts waarvoor geo-replicatie is geconfigureerd, inclusief GRS en RA-GRS. Kosten voor gegevensoverdracht met geo-replicatie worden in rekening gebracht per GB.
* **Kosten voor uitgaande gegevensoverdracht**: uitgaande gegevensoverdracht (gegevens die buiten een Azure-regio worden overgedragen) worden gefactureerd voor bandbreedtegebruik per GB, net zoals bij opslagaccounts voor algemeen gebruik.
* **De opslaglaag wijzigen**: als u de accountopslaglaag wijzigt van 'cool' naar 'hot', worden kosten in rekening gebracht die overeenkomen met de kosten voor het lezen van alle bestaande gegevens in het opslagaccount. Bij een wijziging van de accountopslaglaag van dynamisch naar statisch, worden echter kosten in rekening gebracht die gelijk zijn aan die voor het schrijven van alle gegevens in de statische laag (alleen GPv2-accounts).

> [!NOTE]
> Zie voor meer informatie over de prijzen voor Blob storage-accounts [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) pagina. Zie de pagina [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/data-transfers/) voor meer informatie over de kosten voor uitgaande gegevensoverdracht.

## <a name="faq"></a>Veelgestelde vragen

**Moet ik Blob Storage- of GPv2-accounts gebruiken als ik mijn gegevens in lagen wil opslaan?**

Wij raden u aan GPv2 te gebruiken in plaats van Blob Storage-accounts voor opslaglagen. GPv2-accounts ondersteunen alle functies die Blob Storage-accounts ondersteunen, plus nog veel meer. Prijzen van Blob Storage en GPv2 zijn bijna identiek, maar sommige nieuwe functies en prijsverlagingen zijn alleen beschikbaar bij GPv2-accounts. GPv1-accounts bieden geen ondersteuning voor opslaglagen.

De prijsstructuur tussen GPv1- en GPv2-accounts is verschillend en klanten moeten beide zorgvuldig evalueren alvorens te besluiten GPv2-accounts te gebruiken. U kunt een bestaand Blob Storage- of GPv1-account eenvoudig met één muisklik omzetten naar GPv2. Zie voor meer informatie, [overzicht van Azure storage-account](../common/storage-account-overview.md).

**Kan ik objecten opslaan in alle drie de opslaglagen (dynamisch, statisch en archief) in hetzelfde account?**

Ja. Het kenmerk **Acces Tier** dat op accountniveau is ingesteld, is de standaardlaag die van toepassing is op alle objecten in dat account zonder een expliciet ingestelde laag. Met laaginstelling op blobniveau kunt u echter de toegangslaag op objectniveau instellen, ongeacht de instelling van de toegangslaag voor het account. Blobs in een van de drie opslaglagen (Hot, Cool of Archive) kunnen binnen hetzelfde account aanwezig zijn.

**Kan ik de standaardopslaglaag van mijn Blob Storage- of GPv2-account wijzigen?**

Ja, u kunt de standaardopslaglaag in het opslagaccount wijzigen door het kenmerk **Toegangslaag** in te stellen voor het opslagaccount. Als u de opslaglaag wijzigt, geldt dit voor alle objecten waarvoor geen expliciete laag is ingesteld en die zijn opgeslagen in het account. Bij het omschakelen van de opslaglaag van dynamisch naar statisch worden kosten in rekening gebracht voor schrijfbewerkingen (per 10.000) voor alle blobs zonder een vaste laag in GPv2-accounts en het omschakelen van statisch naar dynamisch veroorzaakt kosten voor zowel leesbewerkingen (per 10.000) als voor het ophalen van gegevens (per GB) voor alle blobs in Blob Storage- en GPv2-accounts.

**Kan ik de toegangslaag van mijn standaardaccount instellen op Archive?**

Nee. Alleen de opslaglagen Dynamisch en Statisch kunnen worden ingesteld als de toegangslaag voor het standaardaccount. Archive kan alleen op objectniveau worden ingesteld.

**In welke regio's zijn de opslaglagen Dynamisch, Statisch en Archief beschikbaar?**

De opslaglagen Dynamisch en Statisch zijn samen met laaginstelling op blobniveau beschikbaar in alle regio's. Archive-opslag is in eerste instantie alleen beschikbaar in bepaalde regio's. Zie voor een volledige lijst [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).

**Gedragen blobs in de opslaglaag voor 'cool' blobs zich anders dan blobs in de opslaglaag voor 'hot' blobs?**

Blobs in de dynamische opslaglaag hebben dezelfde latentie als blobs in GPv1-, GPv2- en Blob Storage-accounts. Blobs in de statische opslaglaag hebben een gelijksoortige latentie (in milliseconden) als blobs in GPv1-, GPv2- en Blob Storage-accounts. Blobs in de archiefopslaglaag kennen enkele uren latentie in GPv1-, GPv2- en Blob Storage-accounts.

Blobs in de opslaglaag voor 'cool' blobs hebben een lagere SLA (Service Level Availability) dan blobs die zijn opgeslagen in de opslaglaag voor 'hot' blobs. Zie [Dienstovereenkomst voor Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) voor meer informatie.

**Zijn de bewerkingen tussen de dynamische-, statische- en archiefopslaglagen hetzelfde?**

Ja. Alle bewerkingen tussen de dynamische en statische laag zijn 100% consistent. Alle geldige archiefbewerkingen, inclusief verwijderen, in lijst opnemen, blob-eigenschappen/metagegevens ophalen en vaste blob-opslaglaag zijn 100% consistent met de dynamische en statische laag. Een blob kan niet worden gelezen of gewijzigd in de archive-laag.

**Als ik een blob reactiveer vanuit de archiefopslaglaag naar de dynamische- of statische-opslaglaag, hoe weet ik dan wanneer de reactivering is voltooid?**

Tijdens het reactiveren kunt u gebruikmaken van de bewerking voor het ophalen van blob-eigenschappen om polling op het kenmerk **Archiefstatus** uit te voeren en te bevestigen dat de laagwijziging is voltooid. De status is "rehydrate-pending-to-hot" of "rehydrate-pending-to-cool", afhankelijk van de doellaag. Als de bewerking is voltooid, wordt de blob-eigenschap voor archiefstatus verwijderd en geeft de blob-eigenschap **Toegangslaag** aan of de opslaglaag dynamisch of statisch is.  

**Na het instellen van de opslaglaag van een blob, wanneer wordt er dan gefactureerd tegen het juiste tarief?**

Elke blob wordt altijd gefactureerd volgens de opslaglaag aangegeven door een van de blob **Toegangslaag** eigenschap. Bij het instellen van een nieuwe laag voor een blob, het **Toegangslaag** eigenschap onmiddellijk weergegeven in de nieuwe laag voor alle overgangen weer. Echter, het reactiveren van een blob uit de archive-laag naar een laag hot of cool enkele uren duren. In dit geval u worden gefactureerd tegen archive-tarieven totdat de rehydratatie is voltooid, waarna de **Toegangslaag** eigenschap weerspiegelt de nieuwe laag. Op dat moment in rekening gebracht voor die blob tegen het tarief hot of cool.

**Hoe bepaal ik of ik bij het verwijderen of verplaatsen van een blob uit de cool- of archive-laag een toeslag voor vroegtijdige verwijdering moet betalen?**

Voor elke blob die binnen 30 dagen of 180 dagen wordt verwijderd of verplaatst uit respectievelijk de statische (alleen GPv2-accounts) of archiefopslaglaag, wordt een vooraf vastgesteld bedrag voor vroegtijdige verwijdering in rekening gebracht. U kunt bepalen hoe lang een blob is in de cool of archive-laag door het controleren van de **Access Tier Change Time** blob-eigenschap, die een tijdstempel van de laatste wijziging van de laag biedt. Zie [Vroegtijdige verwijdering uit Cool en Archive](#cool-and-archive-early-deletion) voor meer informatie.

**Welke Azure Tools en SDK's ondersteunen laaginstelling op blobniveau en archiefopslag?**

Azure Portal, PowerShell en CLI Tools en .NET, Java, Python en Node.js-clientbibliotheken bieden ondersteuning voor laaginstelling op blobniveau en archiefopslag.  

**Hoeveel gegevens kan ik opslaan in de dynamische-, statische- en archiefopslaglaag?**

Gegevensopslag en andere limieten worden ingesteld op het accountniveau en niet per opslaglaag. Daarom kunt u al uw limieten in één laag of over alle drie de lagen gebruiken. Zie voor meer informatie, [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Evalueren van de dynamische, statische en archieflaag in GPv2 Blob Storage-accounts

[De beschikbaarheid controleren van de dynamische, statische en archieflaag per regio](https://azure.microsoft.com/regions/#services)

[De levenscyclus van de Azure Blob storage beheren](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts)

[Gebruik van de huidige opslagaccounts evalueren door metrische gegevens voor Azure Storage in te schakelen](../common/storage-enable-and-view-metrics.md)

[Prijzen voor de dynamische, statische en archieflaag controleren in Blob Storage- en GPv2-accounts per regio](https://azure.microsoft.com/pricing/details/storage/)

[Prijzen voor gegevensoverdracht controleren](https://azure.microsoft.com/pricing/details/data-transfers/)
