---
title: Met behulp van shared access signatures (SAS) in Azure Storage | Microsoft Docs
description: Meer informatie over handtekeningen voor gedeelde toegang (SAS) gebruiken voor toegang tot Azure Storage-resources, met inbegrip van blobs, wachtrijen, tabellen en bestanden delegeren.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/18/2017
ms.author: tamram
ms.component: common
ms.openlocfilehash: 1bc93b083b0f6f0d813f209c9371ce38e8a9daa6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228807"
---
# <a name="using-shared-access-signatures-sas"></a>Shared access signatures (SAS) gebruiken

Een shared access signature (SAS) biedt een manier om beperkte toegang verlenen tot objecten in uw storage-account aan andere clients, zonder dat uw accountsleutel. In dit artikel, we bieden een overzicht van het SAS-model, SAS aanbevolen procedures en kijken naar enkele voorbeelden.

Zie voor voorbeelden van aanvullende code met behulp van SAS afgezien van wat die hier wordt gepresenteerd, [aan de slag met Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) en andere voorbeelden beschikbaar zijn in de [Azure-codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=storage) bibliotheek. U kunt downloaden van de voorbeeldtoepassingen en ze uit te voeren of de code bekijken op GitHub.

## <a name="what-is-a-shared-access-signature"></a>Wat is een shared access signature?
Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. Met een SAS kunt u clients toegang verlenen tot bronnen in uw opslagaccount zonder het delen van sleutels van uw account. De belangrijkste reden voor het gebruik van handtekeningen voor gedeelde toegang in uw toepassingen is dat een SAS een veilige manier is voor het delen van uw opslagresources zonder dat uw accountsleutels in gevaar komen.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

Een SAS kunt u nauwkeurige controle over het type toegang dat u aan clients die de SAS toewijst, met inbegrip van:

* Het interval op waarover de SAS geldig is, met inbegrip van de begin- en het verlooptijdstip is.
* De machtigingen verleend door de SAS. Bijvoorbeeld, kan een SAS voor een blob verleent u lees en schrijfmachtigingen heeft tot de blob, maar de machtigingen niet verwijderen.
* Een optionele IP-adres of het bereik van IP-adressen waarop Azure Storage de SAS accepteert. Bijvoorbeeld, kunt u een bereik van IP-adressen die horen bij uw organisatie opgeven.
* Het protocol waarvoor Azure Storage de SA's worden geaccepteerd. U kunt deze optionele parameter gebruiken om toegang te beperken met clients met behulp van HTTPS.

## <a name="when-should-you-use-a-shared-access-signature"></a>Wanneer moet u een shared access signature gebruiken?
U kunt een SAS gebruiken als u wilt toegang bieden tot bronnen in uw opslagaccount voor elke client die niet beschikken de toegangssleutels van uw opslagaccount. Uw storage-account omvat zowel een primaire en secundaire toegangssleutel, die beide beheerderstoegang verlenen tot uw account en alle resources binnen deze. Uw account met de mogelijkheid van kwaadwillende of uit nalatigheid wordt geopenbaard gebruik blootstellen van een van deze sleutels worden geopend. Handtekeningen voor gedeelde toegang bieden een veilige alternatief waarmee clients kunnen lezen, schrijven en verwijderen van gegevens in uw storage-account volgens de machtigingen die u expliciet hebt toegewezen, en zonder de noodzaak voor een accountsleutel.

Een veelvoorkomend scenario waarin een SAS handig is, is een service waarbij gebruikers lezen en schrijven van hun eigen gegevens naar uw opslagaccount. In een scenario waarbij gebruikersgegevens worden opgeslagen in een storage-account, moet u er twee typische ontwerppatronen zijn:

1. Clients uploaden en downloaden van gegevens via een front-end-proxy-service, waarmee authenticatie wordt uitgevoerd. Deze front-proxyservice heeft het voordeel van het toestaan van validatie van bedrijfsregels kan, maar voor grote hoeveelheden gegevens of transacties, hoog volume, het maken van een service die kan worden geschaald zodat deze overeenkomt met de aanvraag duur en moeilijk.

  ![Scenario-diagram: front-proxy-service](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Een lichtgewicht service verifieert de client naar behoefte en genereert vervolgens een SAS. Nadat de client de SAS ontvangt, kunnen ze toegang tot resources voor storage-account rechtstreeks met de machtigingen die zijn gedefinieerd door de SAS en voor het interval dat is toegestaan door de SAS. De SAS vermindert de noodzaak voor de routering van alle gegevens via de front-end-proxy-service.

  ![Scenario-diagram: SAS provider-service](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Veel real-world services kunnen een hybride versie van deze twee methoden gebruiken. Sommige gegevens kunnen bijvoorbeeld worden verwerkt en gevalideerd via het front-proxy, terwijl andere gegevens is opgeslagen en/of rechtstreeks met behulp van SAS lezen.

Daarnaast moet u het gebruik van een SAS toegang verlenen aan het bronobject een kopieerbewerking in bepaalde scenario's:

* Wanneer u een blob naar een andere blob die zich in een ander opslagaccount bevinden kopieert, moet u een SAS toegang verlenen aan de bron-blob. U kunt eventueel een SAS toegang verlenen aan de bestemmings-blob.
* Wanneer u een bestand naar een ander bestand dat zich in een ander opslagaccount bevindt kopiëren, moet u een SAS toegang verlenen aan het bronbestand. U kunt eventueel een SAS toegang verlenen aan de doel-bestand.
* Wanneer u een blob naar een bestand of een bestand naar een blob kopieert, moet u een SAS toegang verlenen aan het bronobject, zelfs als de bron- en -objecten bevinden zich in hetzelfde opslagaccount.

## <a name="types-of-shared-access-signatures"></a>Typen van handtekeningen voor gedeelde toegang
U kunt twee soorten handtekeningen voor gedeelde toegang maken:

* **Service-SAS.** De service-SAS biedt toegang tot een resource in slechts een van de opslagservices: de Blob-, Queue-, Tabel- of File-service. Zie [maken van een Service-SAS](https://msdn.microsoft.com/library/dn140255.aspx) en [voorbeelden van Service-SAS](https://msdn.microsoft.com/library/dn140256.aspx) voor gedetailleerde informatie over het maken van de service-SAS-token.
* **Account-SAS.** De account SAS delegeert toegang tot bronnen in een of meer van de storage-services. Alle van de bewerkingen die beschikbaar zijn via een service-SAS zijn ook beschikbaar via een account-SAS. Met de account-SAS, kunt u bovendien toegang tot de bewerkingen die betrekking hebben op een bepaalde service, zoals delegeren **Get/Set-Service-eigenschappen** en **Zoekstatistieken ophalen**. U kunt ook toegang tot het lezen, schrijven en verwijderen van bewerkingen delegeren voor blobcontainers, tabellen, wachtrijen en bestandsshares die niet zijn toegestaan bij een service-SAS. Zie [maken van een Account-SAS](https://msdn.microsoft.com/library/mt584140.aspx) voor gedetailleerde informatie over het maken van de account-SAS-token.

## <a name="how-a-shared-access-signature-works"></a>De werking van een shared access signature
Een shared access signature is een ondertekende URI die verwijst naar een of meer resources voor storage en bevat een token dat bestaat uit een speciale set queryparameters. Het token geeft aan hoe de resources kunnen worden geopend door de client. Een van de queryparameters, de handtekening is samengesteld uit de SAS-parameters en ondertekend met de accountsleutel. Deze handtekening wordt gebruikt door Azure Storage toegang verlenen aan de storage-resource.

Hier volgt een voorbeeld van een SAS-URI van de resource-URI en de SAS-token:

![Onderdelen van een SAS-URI](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

De SAS-token is een tekenreeks die u genereert op de *client* aan clientzijde (Zie de [SAS voorbeelden](#sas-examples) sectie voor voorbeelden van code). Een SAS-token dat u met de storage-clientbibliotheek genereren, bijvoorbeeld wordt niet bijgehouden door Azure Storage op geen enkele manier. U kunt een onbeperkt aantal SAS-tokens maken op de client.

Wanneer een client een SAS-URI naar Azure Storage als onderdeel van een aanvraag biedt, wordt de service controleert de SAS-parameters en handtekening om te controleren of deze geldig is voor het verifiëren van de aanvraag. Als de service controleert de handtekening geldig is, wordt de aanvraag is geautoriseerd. Anders wordt is de aanvraag geweigerd met foutcode 403 (verboden).

## <a name="shared-access-signature-parameters"></a>Shared access signature-parameters
De account-SAS en service-SAS-tokens bevatten enkele algemene parameters en ook rekening houden met een aantal parameters die verschillen.

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a>Parameters voor account-SAS en service-SAS-tokens
* **API-versie** een optionele parameter waarmee de versie van het storage-service te gebruiken voor het uitvoeren van de aanvraag.
* **Service-versie** een vereiste parameter waarmee de versie van het storage-service te gebruiken om te autoriseren van de aanvraag.
* **De begintijd.** Dit is de tijd waarop de SAS geldig wordt. De begintijd voor een shared access signature is optioneel. Als een begintijd wordt weggelaten, wordt de SAS onmiddellijk van kracht. De begintijd moet worden uitgedrukt in UTC (Coordinated Universal Time), met een speciale aanduiding van de UTC ("Z"), bijvoorbeeld `1994-11-05T13:15:30Z`.
* **Verlooptijd.** Dit is de tijd waarna de SAS niet meer geldig is. Aanbevolen procedures raadzaam dat u ofwel een verlooptijd voor een SAS opgeven, of deze aan een opgeslagen toegangsbeleid koppelen. Het verlooptijdstip moet worden uitgedrukt in UTC (Coordinated Universal Time), met een speciale aanduiding van de UTC ("Z"), bijvoorbeeld `1994-11-05T13:15:30Z` (Zie meer hieronder).
* **Machtigingen.** De machtigingen die zijn opgegeven voor de SAS aangeven welke bewerkingen die de client kunt uitvoeren op basis van de storage-resource met behulp van de SAS. Beschikbare machtigingen verschillen voor een account-SAS en een service-SAS.
* **IP.** Een optionele parameter waarmee een IP-adres of een bereik van IP-adressen buiten Azure (Zie de sectie [Routing configuratie sessiestatus](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) voor Express Route) uit waarvoor aanvragen worden geaccepteerd.
* **Protocol.** Een optionele parameter waarmee het protocol is toegestaan voor een aanvraag. Mogelijke waarden zijn zowel HTTP als HTTPS (`https,http`), die alleen is de standaardwaarde of HTTPS (`https`). Houd er rekening mee dat HTTP alleen geen toegestane waarde is.
* **Handtekening.** De handtekening is samengesteld uit de andere parameters opgegeven als onderdeel token en vervolgens versleuteld. De handtekening wordt gebruikt voor toegang verlenen aan de opgegeven storage-resources.

### <a name="parameters-for-a-service-sas-token"></a>Parameters voor een service-SAS-token
* **Opslagresource.** Storage-resources waarvoor u toegang tot het met een service kunt delegeren SAS zijn onder andere:
  * Containers en blobs
  * Bestandsshares en bestanden
  * Wachtrijen
  * Tabellen en -bereiken van de tabelentiteiten.

### <a name="parameters-for-an-account-sas-token"></a>Parameters voor een account-SAS-token
* **Service of -services.** Een account-SAS kunt toegang tot een of meer van de storage-services delegeren. U kunt bijvoorbeeld een account-SAS toegang biedt aan de Blob en bestand service maken. Of u een SAS dat delegeert toegang tot alle vier services (Blob, wachtrij, tabel en bestand) kunt maken.
* **Opslag-resourcetypen.** Een account SAS is van toepassing op een of meer klassen van de storage-resources, in plaats van een specifieke resource. U kunt een account-SAS voor toegang tot delegeren maken:
  * Service level-API's worden aangeroepen voor de resource van het opslagaccount. Voorbeelden zijn onder meer **Get/Set-Service-eigenschappen**, **Zoekstatistieken ophalen**, en **lijst Containers/wachtrijen/tabellen/Shares**.
  * Container-niveau API's, die worden aangeroepen voor de container-objecten voor elke service: blob-containers, wachtrijen, tabellen en bestandsshares. Voorbeelden zijn onder meer **maken/verwijderen Container**, **wachtrij maken/verwijderen**, **maken/verwijderen tabel**, **maken/verwijderen Share**, en  **Blobs/bestanden en mappen weergeven**.
  * Objectniveau API's, die worden aangeroepen voor blobs, berichten in de wachtrij, tabelentiteiten en bestanden. Bijvoorbeeld, **Put Blob**, **Queryentiteit**, **berichten ophalen**, en **-bestand maken**.

## <a name="examples-of-sas-uris"></a>Voorbeelden van SAS URI 's

### <a name="service-sas-uri-example"></a>Voorbeeld van de service-SAS-URI

Hier volgt een voorbeeld van een SAS-URI waarmee machtigingen lezen en schrijven naar een blob-service. De tabel een uitsplitsing van elk onderdeel van de te begrijpen hoe het draagt bij aan de SAS URI:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Naam | SAS-gedeelte | Beschrijving |
| --- | --- | --- |
| BLOB-URI |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |Het adres van de blob. Houd er rekening mee dat met behulp van HTTPS wordt sterk aanbevolen. |
| De versie van de Storage-services |`sv=2015-04-05` |Voor storage services versie 2012-02-12 en hoger, deze parameter geeft aan dat de versie moet worden gebruikt. |
| Begintijd |`st=2015-04-29T22%3A18%3A26Z` |Opgegeven in UTC-tijd. Als u wilt dat de SAS is onmiddellijk geldig zijn, laat u de begintijd. |
| Verlooptijd |`se=2015-04-30T02%3A23%3A26Z` |Opgegeven in UTC-tijd. |
| Resource |`sr=b` |De resource is een blob. |
| Machtigingen |`sp=rw` |De machtigingen die zijn verleend door de SAS Read (r) bevatten en schrijven (s). |
| IP-bereik |`sip=168.1.5.60-168.1.5.70` |Het bereik van IP-adressen van waaruit u een aanvraag wordt geaccepteerd. |
| Protocol |`spr=https` |Alleen aanvragen via HTTPS zijn toegestaan. |
| Handtekening |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Gebruikt voor de autorisatie van toegang tot de blob. De handtekening is een HMAC berekend voor een tekenreeks te ondertekenen en een sleutel met behulp van de algoritme SHA256 en klik vervolgens gecodeerd met Base64-codering. |

### <a name="account-sas-uri-example"></a>Voorbeeld van de account-SAS-URI

Hier volgt een voorbeeld van een account-SAS die gebruikmaakt van dezelfde algemene parameters op het token. Omdat deze parameters hierboven worden beschreven, worden ze hier niet beschreven. Alleen de parameters die specifiek zijn voor account-SAS worden beschreven in de onderstaande tabel.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Naam | SAS-gedeelte | Beschrijving |
| --- | --- | --- |
| Resource-URI |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |Het eindpunt van Blob service, en de parameters voor service-eigenschappen (wanneer aangeroepen met GET) ophalen of instellen van service-eigenschappen (wanneer met de naam is ingesteld). |
| Services |`ss=bf` |De SAS is van toepassing op de services Blob en bestand |
| Resourcetypen |`srt=s` |De SAS is van toepassing op serviceniveau bewerkingen. |
| Machtigingen |`sp=rw` |De machtigingen verlenen toegang tot lees- en schrijfbewerkingen. |

Gezien het feit dat machtigingen beperkt tot het serviceniveau zijn, toegankelijk bewerkingen met deze SAS zijn **Blob Service-eigenschappen ophalen** (lezen) en **Blob Service-eigenschappen instellen** (schrijven). Echter, met een andere resource-URI, hetzelfde SAS-token kan ook worden gebruikt voor toegang tot delegeren **Blob Service-statistieken ophalen** (lezen).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Een SAS met een opgeslagen toegangsbeleid beheren
Een shared access signature kan duren voordat een van twee vormen:

* **Ad-hoc SAS:** wanneer u een ad-hoc SAS, de begintijd, verlooptijd, maken en machtigingen voor de SAS zijn alle opgegeven in de SAS-URI (of impliciet, in het geval waarin de begintijd wordt weggelaten). Dit type SAS kan worden gemaakt als een account-SAS of een service-SAS.
* **SAS met opgeslagen toegangsbeleid:** een opgeslagen toegangsbeleid is gedefinieerd in een resourcecontainer, een blob-container, tabel, wachtrij, of bestandsshare-- en kan worden gebruikt voor het beheren van beperkingen voor een of meer handtekeningen voor gedeelde toegang. Wanneer u een SAS aan een opgeslagen toegangsbeleid koppelen, neemt de SA's over de beperkingen--de begintijd, verlooptijd en machtigingen--gedefinieerd voor de opgeslagen toegangsbeleid.

> [!NOTE]
> Een account-SAS moet op dit moment een ad-hoc SAS. Opgeslagen-beleidsregels worden nog niet ondersteund voor account-SAS.

Het verschil tussen de twee vormen is belangrijk voor een key-scenario: intrekken. Een SAS-URI is een URL, kunt iedereen die de SAS verkrijgt gebruiken, ongeacht wie het oorspronkelijk heeft gemaakt. Als een SAS openbaar is gepubliceerd, kan deze worden gebruikt door iedereen in de hele wereld. Een SAS verleent toegang tot resources voor iedereen die deze totdat een van de vier wat er gebeurt:

1. Het verlooptijdstip die is opgegeven voor de SAS is bereikt.
2. Het verlooptijdstip opgegeven op het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is bereikt (als een opgeslagen toegangsbeleid wordt verwezen, en als deze een verlooptijd). Dit kan gebeuren omdat het interval is verstreken of omdat u het opgeslagen toegangsbeleid met een verlooptijd in het verleden is één manier om in te trekken van de SAS hebt gewijzigd.
3. Het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is verwijderd, die een andere manier om in te trekken van de SAS is. Houd er rekening mee dat als u opnieuw het opgeslagen toegangsbeleid met dezelfde naam maken, alle bestaande SAS-tokens opnieuw ongeldig volgens de machtigingen die zijn gekoppeld aan deze opgeslagen toegangsbeleid is (ervan uitgaande dat het verlooptijdstip voor de SAS niet is verstreken). Als u wilde zijn maken om in te trekken van de SAS, zorg er dan voor dat een andere naam gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw maken.
4. De sleutel die is gebruikt voor het maken van de SAS is gegenereerd. Een accountsleutel opnieuw genereren zorgt ervoor dat alle onderdelen van de toepassing met behulp van deze sleutel niet kunnen worden autoriseren totdat ze zijn bijgewerkt voor het gebruik van de andere geldig account-sleutel of de sleutel van het zojuist opnieuw gegenereerd.

> [!IMPORTANT]
> Een shared access signature-URI is gekoppeld aan de accountsleutel die wordt gebruikt om de handtekening te maken en de bijbehorende opgeslagen toegangsbeleid (indien aanwezig). Als er geen opgeslagen toegangsbeleid wordt opgegeven, is de enige manier om een shared access signature intrekken om de accountsleutel te wijzigen.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Verificatie van een clienttoepassing met een SAS
Een client die in het bezit van een SAS kan de SAS gebruiken om een aanvraag indient voor een opslagaccount waarvoor ze niet de accountsleutels beschikken. Een SAS kan worden opgenomen in een verbindingsreeks of rechtstreeks vanuit de juiste constructor of de methode gebruikt.

### <a name="using-a-sas-in-a-connection-string"></a>Met behulp van een SAS in een verbindingsreeks
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Met behulp van een SAS in een constructor of methode
Verschillende Azure Storage client library constructors en methode overloads bieden een SAS-parameter, zodat u kunt een aanvraag om de service met een SAS te autoriseren.

Bijvoorbeeld, wordt hier een SAS-URI gebruikt om te maken van een verwijzing naar een blok-blob. De SAS biedt de enige referenties die nodig zijn voor de aanvraag. De blok-blobverwijzing wordt vervolgens gebruikt voor een schrijfbewerking:

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with the specified name to the container.
// If the blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a>Aanbevolen procedures bij het gebruik van SAS
Wanneer u handtekeningen voor gedeelde toegang in uw toepassingen gebruiken, moet u rekening houden met twee mogelijke risico's:

* Als u een SAS is gelekt, kan deze worden gebruikt door iedereen die wordt opgehaald, waardoor mogelijk in gevaar uw storage-account komen kan.
* Als een SAS die een clienttoepassing verloopt en de toepassing kan niet worden opgehaald van een nieuwe SAS van uw service is, wordt de functionaliteit van de toepassing kan worden belemmerd.

De volgende aanbevelingen voor het gebruik van handtekeningen voor gedeelde toegang kunnen helpen deze risico's verhelpen:

1. **Gebruik altijd HTTPS** te maken of een SAS te distribueren. Als een SAS wordt doorgegeven via HTTP en onderschept, kan een aanvaller die het uitvoeren van een man-in-the-middle-aanval te lezen van de SAS en vervolgens worden gebruikt zoals de beoogde gebruiker hebben kan, mogelijk inbreuk op gevoelige gegevens of beschadiging van gegevens door de kwaadwillende gebruiker toestaan.
2. **Verwijzen naar opgeslagen toegangsbeleid waar mogelijk.** Opgeslagen toegangsbeleid bieden u de optie voor het intrekken van machtigingen zonder de storage-accountsleutels opnieuw genereren. De vervaldatum instellen voor deze zeer tot nu toe in de toekomst (of oneindige) en controleer of dat verder gaan in de toekomst wordt regelmatig bijgewerkt.
3. **Korte verlooptijd keren gebruiken op een ad-hoc SAS.** Op deze manier kunnen zelfs als een SAS is geknoeid, is dit alleen geldig voor een korte periode. Met deze procedure is vooral belangrijk als u kan niet verwijzen naar een opgeslagen toegangsbeleid. Korte verlooptijd keren beperken ook de hoeveelheid gegevens die door het beperken van de tijd beschikbaar om te uploaden naar deze kunnen worden geschreven naar een blob.
4. **Clients automatisch vernieuwen van de SAS indien nodig hebt.** Clients moeten de SAS vóór de vervaldatum vernieuwen zodat voor nieuwe pogingen als de service die de SAS niet beschikbaar is. Als de SAS is bedoeld om te worden gebruikt voor een klein aantal direct, eenvoudige bewerkingen die worden uitgevoerd binnen de verloopperiode worden verwacht, kan klikt u vervolgens dit niet nodig als de SAS naar verwachting niet worden vernieuwd. Echter, hebt u een client die regelmatig aanvragen via SAS, klikt u vervolgens de mogelijkheid van verlopen wordt geleverd aan de orde. De belangrijkste overweging is het balanceren van de noodzaak van de SAS zijn tijdelijke (zoals eerder vermeld) met de noodzaak om ervoor te zorgen dat de client vraagt om vernieuwing vroeg genoeg (zodat onderbrekingen worden voorkomen vanwege de SAS is verlopen voordat geslaagde vernieuwing).
5. **Wees voorzichtig met SAS-begintijd.** Als u de begintijd instellen voor een SAS op **nu**, klikt u vervolgens vanwege klok scheeftrekken (verschillen in de huidige tijd op basis van verschillende computers), fouten kunnen worden waargenomen af en toe voor het eerst enkele minuten. In het algemeen de begintijd moet ten minste 15 minuten in het verleden instellen. Of niet instellen op alle dat maakt het geldig onmiddellijk in alle gevallen. In het algemeen geldt ook voor verlooptijd ook--Houd er rekening mee dat u tot vijftien minuten van de klok scheeftrekken in beide richtingen op elke aanvraag zien kunt. Voor clients met behulp van een REST eerdere versie dan 2012-02-12, is de maximale duur voor een SAS die niet verwijst naar een opgeslagen toegangsbeleid 1 uur en eventuele beleidsregels voor langere termijn dan die mislukken op te geven.
6. **Worden met de resource moet worden geopend.** Er is een aanbevolen beveiligingsprocedure zodat een gebruiker beschikken over de minimaal vereiste machtigingen. Als een gebruiker alleen lezen-toegang tot een enkele entiteit moet, verleen deze leestoegang tot die één entiteit en toegang tot alle entiteiten niet lezen, schrijven en verwijderen. Dit helpt ook bij de schade standaardprocedures als er een SAS is geknoeid omdat de SAS minder stroom de beschikking over een aanvaller heeft.
7. **Krijg inzicht in uw account wordt gefactureerd voor het gebruik, met inbegrip van die klaar zijn met SAS.** Als u toegang voor schrijven naar een blob opgeeft, wordt een gebruiker kan kiezen voor het uploaden van een blob van 200GB. Als u deze leestoegang ook opgegeven hebt, kunnen ze om deze te downloaden 10 keer kiezen waarbij extra kosten voor 2 TB in de kosten voor uitgaand voor u. Nogmaals, bieden beperkte machtigingen om te beperken van de mogelijke reacties van kwaadwillende gebruikers. Gebruik eenvoudige SAS deze bedreiging beperken (maar houd ook rekening met tijdsverschil op de eindtijd).
8. **Valideren van gegevens die zijn geschreven met behulp van SAS.** Wanneer een clienttoepassing gegevens naar uw opslagaccount schrijft, houd er rekening mee dat er problemen met die gegevens kunnen worden. Als uw toepassing vereist dat gegevens worden gevalideerd of geautoriseerd voordat deze klaar voor gebruik is, kunt u deze validatie moet uitvoeren nadat de gegevens worden geschreven en voordat deze wordt gebruikt door uw toepassing. Met deze procedure biedt ook bescherming tegen beschadigd of schadelijke gegevens naar uw account worden geschreven door een gebruiker die de SAS correct hebt aangeschaft, of door een gebruiker die gebruik maken van een gelekte SAS.
9. **Altijd gebruik geen SAS.** Soms is de risico's die zijn gekoppeld aan een bepaalde bewerking op basis van uw opslagaccount opwegen tegen de voordelen van de SAS. Voor dergelijke bewerkingen plaatsvindt, maakt u een middelste laag service die naar uw opslagaccount schrijft, na het uitvoeren van zakelijke regel validatie, verificatie en controle. Bovendien is soms het eenvoudiger om toegang op een andere manier te beheren. Bijvoorbeeld, als u wilt om alle blobs in een container openbaar leesbaar, kunt u de container openbaar, in plaats van dat een SAS toegang naar elke client.
10. **Storage Analytics gebruiken om uw toepassing te bewaken.** U kunt logboekregistratie en metrische gegevens een piek in authenticatiefouten vanwege een storing in acht nemen in uw SAS-provider-service of het onbedoeld verwijderen van een opgeslagen toegangsbeleid gebruiken. Zie de [Blog van Azure Storage-Team](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) voor meer informatie.

## <a name="sas-examples"></a>SAS-voorbeelden
Hieronder volgen enkele voorbeelden van beide typen handtekeningen voor gedeelde toegang, account-SAS en service-SAS.

Als u wilt deze C#-voorbeelden uitvoeren, moet u verwijzen naar de volgende NuGet-pakketten in uw project:

* [Azure Storage-clientbibliotheek voor .NET](http://www.nuget.org/packages/WindowsAzure.Storage), versie 6.x of hoger (om te gebruiken account-SAS).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Zie voor meer voorbeelden die laten hoe u zien kunt maken en testen van een SAS [codevoorbeelden van Azure voor opslag](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Voorbeeld: Maak en gebruik een account-SAS
Het volgende codevoorbeeld maakt u een account-SAS die geldig is voor de services Blob en bestand en geeft de client machtigingen voor lezen, schrijven en lijst met machtigingen voor toegang tot serviceniveau API's. De account-SAS beperkt het protocol HTTPS, dus de aanvraag moet worden gemaakt met HTTPS.

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

Voor het gebruik van de account-SAS toegang tot serviceniveau API's voor de Blob-service, een Blob-client-object met behulp van de SAS en het eindpunt van Blob-opslag voor uw opslagaccount te maken.

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a>Voorbeeld: Een opgeslagen toegangsbeleid maken
De volgende code maakt een opgeslagen toegangsbeleid voor een container. Het toegangsbeleid kunt u beperkingen voor een service-SAS opgeven op de container en blobs.

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
        // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a>Voorbeeld: Een service-SAS voor een container maken
De volgende code wordt een SAS voor een container gemaakt. Als de naam van een bestaand opgeslagen toegangsbeleid is opgegeven, wordt dat beleid is gekoppeld aan de SAS. Als er geen opgeslagen toegangsbeleid is opgegeven, maakt de code een ad-hoc-SAS voor de container.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container, setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case, all of the constraints for the
        // shared access signature are specified on the stored access policy, which is provided by name.
        // It is also possible to specify some constraints on an ad-hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Voorbeeld: Een service-SAS voor een blob maken
De volgende code maakt een SAS voor een blob. Als de naam van een bestaand opgeslagen toegangsbeleid is opgegeven, wordt dat beleid is gekoppeld aan de SAS. Als er geen opgeslagen toegangsbeleid is opgegeven, maakt de code een ad-hoc-SAS voor de blob.

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob, setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints for the
        // shared access signature are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a>Conclusie
Shared access signatures zijn nuttig voor het aanbieden van beperkte machtigingen naar uw opslagaccount aan clients die u niet de accountsleutel hebt. Ze zijn daarom een essentieel onderdeel van het beveiligingsmodel voor elke toepassing met behulp van Azure Storage. Als u Volg de aanbevolen procedures die hier worden vermeld, kunt u SAS gebruiken om aan te bieden meer flexibiliteit van de toegang tot resources in uw opslagaccount zonder de beveiliging van uw toepassing.

## <a name="next-steps"></a>Volgende stappen
* [Shared Access Signatures, deel 2: Maken en een SAS gebruiken met Blob-opslag](../blobs/storage-dotnet-shared-access-signature-part-2.md)
* [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md)
* [Toegang delegeren met een Shared Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Maak kennis met de tabel en wachtrij SAS](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
