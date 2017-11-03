---
title: Met behulp van shared access signatures (SAS) in Azure Storage | Microsoft Docs
description: Informatie over het gebruik van handtekeningen voor gedeelde toegang (SAS) om te delegeren van toegang tot Azure Storage-resources, met inbegrip van blobs, wachtrijen, tabellen en bestanden.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: tamram
ms.openlocfilehash: 32e92e6ffc376d27297810596691f0371770e86d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-shared-access-signatures-sas"></a>Met behulp van handtekeningen voor gedeelde toegang (SAS)

Een shared access signature (SAS) biedt een manier om beperkte toegang tot objecten in uw opslagaccount verlenen aan andere clients, zonder dat de sleutel van uw account. In dit artikel wordt een overzicht gegeven van SAS-model, SAS aanbevolen procedures en kijken naar enkele voorbeelden.

Zie voor aanvullende codevoorbeelden via SAS afgezien van wat die hier wordt gepresenteerd, [aan de slag met Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) en andere voorbeelden die beschikbaar zijn in de [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=storage) bibliotheek. U kunt downloaden van de voorbeeldtoepassingen en ze uitvoeren of de code op GitHub bladeren.

## <a name="what-is-a-shared-access-signature"></a>Wat is er een shared access signature?
Een shared access signature biedt gedelegeerde toegang tot bronnen in uw opslagaccount. Met een SAS kunt u clients toegang verlenen tot bronnen in uw opslagaccount zonder het delen van de sleutels van uw account. Dit is de sleutel van het gebruik van handtekeningen voor gedeelde toegang in uw toepassingen--een SAS is een veilige manier voor het delen van uw opslagresources zonder uw sleutels account gevaar te brengen.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

Een SAS biedt gedetailleerde controle over het type toegang u aan clients die beschikken over de SA's verleent, waaronder:

* De tijdsinterval waarover de SAS geldig zijn, inclusief de begintijd en de verlooptijd is.
* De machtigingen verleend door de SAS. Een SAS voor een blob kan bijvoorbeeld verleent u lees en schrijfmachtigingen voor blob maar machtigingen niet verwijderen.
* Een optionele IP-adres of het bereik van IP-adressen waarvan van Azure Storage de SAS accepteert. U kunt bijvoorbeeld een bereik met IP-adressen die horen bij uw organisatie opgeven.
* Het protocol waarvoor Azure Storage de SA's worden geaccepteerd. U kunt deze optionele parameter gebruiken om toegang te beperken op clients met behulp van HTTPS.

## <a name="when-should-you-use-a-shared-access-signature"></a>Wanneer moet u een shared access signature gebruiken?
U kunt een SAS gebruiken wanneer u wilt toegang bieden tot bronnen in uw opslagaccount voor elke client die niet beschikken toegangssleutels voor uw opslagaccount. Uw opslagaccount omvat zowel een primaire en secundaire toegangssleutel voor, die beide Verleen beheerderstoegang tot uw account en alle resources binnen het. Uw account met de mogelijkheid van kwaadwillende of Achteloos gebruik blootstellen van een van deze sleutels worden geopend. Handtekeningen voor gedeelde toegang bieden een veilige alternatief waarmee clients kunnen lezen, schrijven en verwijderen van gegevens in uw opslagaccount volgens de machtigingen die u expliciet hebt verleend en zonder dat nodig is voor de accountsleutel van een.

Een veelvoorkomend scenario waarin een SAS handig is, is een service waar gebruikers lezen en schrijven van hun eigen gegevens naar uw opslagaccount. In een scenario waar de gebruikersgegevens worden opgeslagen in een opslagaccount, zijn er twee typische ontwerppatronen:

1. Clients uploaden en downloaden van gegevens via een front-proxyservice, die verificatie uitvoert. Deze front-proxyservice heeft het voordeel van het toestaan van validatie van bedrijfsregels, maar voor grote hoeveelheden gegevens of transacties, hoog volume, maken van een service die kan worden geschaald zodat deze overeenkomen met de vraag mogelijk dure of moeilijk.

  ![Scenario-diagram: front-proxy-service](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Een lichtgewicht service verifieert de client, indien nodig en genereert vervolgens een SAS. Nadat de client de SA's ontvangt, toegang te krijgen tot resources voor storage-account rechtstreeks met de machtigingen die zijn gedefinieerd door de SAS en voor het interval dat is toegestaan door de SAS. De SA's vermindert de noodzaak voor het doorsturen van alle gegevens via de front-end-proxy-service.

  ![Scenario-diagram: SAS provider-service](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Veel echte services kunnen een hybride versie van deze twee methoden gebruiken. Sommige gegevens kunnen bijvoorbeeld worden verwerkt en gevalideerd via de front-end-proxy, terwijl andere gegevens wordt opgeslagen en/of rechtstreeks via SAS lezen.

Bovendien moet u een SAS gebruiken om te verifiëren van het bronobject in een kopieerbewerking in bepaalde scenario's:

* Wanneer u een blob naar een andere blob die zich in een ander opslagaccount bevindt kopieert, moet u een SAS gebruiken om te verifiëren van de bron-blob. U kunt desgewenst een SAS gebruiken om te verifiëren van de bestemmings-blob.
* Wanneer u een bestand naar een ander bestand dat zich in een ander opslagaccount kopieert bevindt, moet u een SAS gebruiken om te verifiëren van het bronbestand. U kunt desgewenst een SAS gebruiken om te verifiëren van het doel-bestand.
* Wanneer u een blob naar een bestand of een bestand naar een blob kopieert, moet u een SAS gebruiken om te verifiëren van het bronobject, zelfs als de bron- en doelserver objecten bevinden zich in hetzelfde opslagaccount.

## <a name="types-of-shared-access-signatures"></a>Typen handtekeningen voor gedeelde toegang
U kunt twee soorten handtekeningen voor gedeelde toegang maken:

* **Service-SAS.** De service-SAS biedt toegang tot een resource in slechts een van de opslagservices: de Blob-, Queue-, Tabel- of File-service. Zie [samenstellen van een Service-SAS](https://msdn.microsoft.com/library/dn140255.aspx) en [voorbeelden van Service-SAS](https://msdn.microsoft.com/library/dn140256.aspx) voor gedetailleerde informatie over het maken van het service-SAS-token.
* **Account-SAS.** De account SAS gemachtigden toegang tot bronnen in een of meer van de storage-services. Alle bewerkingen die beschikbaar zijn via een service-SAS zijn ook beschikbaar via een account-SAS. Met het account-SAS kunt u bovendien toegang tot de bewerkingen die betrekking hebben op een bepaalde service, zoals delegeren **Get/Set-Service-eigenschappen** en **Service statistieken ophalen**. U kunt ook toegang tot het lezen, schrijven en verwijderen van bewerkingen delegeren voor blobcontainers, tabellen, wachtrijen en bestandsshares die niet zijn toegestaan bij een service-SAS. Zie [samenstellen van een Account-SAS](https://msdn.microsoft.com/library/mt584140.aspx) voor gedetailleerde informatie over het maken van het account-SAS-token.

## <a name="how-a-shared-access-signature-works"></a>De werking van een shared access signature
Een shared access signature is een ondertekende URI die verwijst naar een of meer opslagbronnen en bevat een token dat een speciale reeks queryparameters bevat. Het token geeft aan hoe de resources zijn toegankelijk voor de client. Een van de queryparameters, de handtekening is gemaakt op basis van de SAS-parameters en ondertekend met de accountsleutel. Deze handtekening wordt gebruikt door Azure Storage voor het verifiëren van de SAS.

Hier volgt een voorbeeld van een SAS-URI met de bron-URI en de SAS-token:

![Onderdelen van een SAS-URI](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

De SAS-token is een tekenreeks die u genereren op de *client* kant (Zie de [SAS-voorbeelden](#sas-examples) sectie voor codevoorbeelden). Een SAS-token met de storage-clientbibliotheek genereren bijvoorbeeld wordt niet bijgehouden door Azure Storage op elke manier. Aan de clientzijde kunt u een onbeperkt aantal SAS-tokens.

Wanneer een client een SAS-URI naar Azure Storage als onderdeel van een aanvraag biedt, wordt de service controleert de SAS-parameters en handtekening om te controleren of deze geldig is voor het verifiëren van de aanvraag. Als de service controleert de handtekening ongeldig is, wordt de aanvraag is geverifieerd. Anders wordt is de aanvraag geweigerd met foutcode 403 (verboden).

## <a name="shared-access-signature-parameters"></a>Parameters voor gedeelde toegang handtekening
De account-SAS en service-SAS-tokens bevatten enkele algemene parameters en ook rekening houden met een aantal parameters die niet overeenkomen.

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a>Parameters die gemeenschappelijk zijn voor account-SAS en service-SAS-tokens
* **API-versie** een optionele parameter waarmee de versie van de storage-service moet worden gebruikt voor het uitvoeren van de aanvraag.
* **Service-versie** een vereiste parameter waarmee de versie van de storage-service gebruiken om te verifiëren van de aanvraag.
* **De begintijd.** Dit is de tijd waarop de SAS geldig wordt. De begintijd voor een shared access signature is optioneel. Als een begintijd wordt weggelaten, wordt de SAS onmiddellijk van kracht. De begintijd moet worden uitgedrukt in UTC (Coordinated Universal Time), met een speciale UTC-aanduiding ('Z'), bijvoorbeeld `1994-11-05T13:15:30Z`.
* **Verlooptijd.** Dit is de tijd waarna de SAS niet meer geldig is. Aanbevolen dat u een verlooptijd voor een SAS opgeven, of deze aan een opgeslagen toegangsbeleid koppelen. Het verlooptijdstip moet worden uitgedrukt in UTC (Coordinated Universal Time), met een speciale UTC-aanduiding ('Z'), bijvoorbeeld `1994-11-05T13:15:30Z` (Zie meer hieronder).
* **Machtigingen.** De machtigingen die zijn opgegeven voor de SAS aangeven welke bewerkingen die de client op basis van de storage-resource met behulp van de SAS kunt uitvoeren. Beschikbare machtigingen verschillen voor een account-SAS en een service-SAS.
* **IP-ADRES.** Een optionele parameter waarmee een IP-adres of een bereik met IP-adressen buiten Azure (Zie de sectie [routering configuratie sessiestatus](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) voor Express Route) waarin om aanvragen te accepteren.
* **Protocol.** Een optionele parameter waarmee het protocol is toegestaan voor een aanvraag. Mogelijke waarden zijn zowel HTTP als HTTPS (`https,http`), wordt de standaardwaarde of HTTPS (`https`). Houd er rekening mee dat HTTP alleen geen toegestane waarde is.
* **Handtekening.** De handtekening is samengesteld uit de andere parameters opgegeven als deel token, en vervolgens versleuteld. Deze wordt gebruikt voor verificatie van de SAS.

### <a name="parameters-for-a-service-sas-token"></a>Parameters voor een service-SAS-token
* **Storage-resource.** Storage-resources die u kunt toegang met een service te delegeren SAS zijn onder andere:
  * Containers en blobs
  * Bestandsshares en bestanden
  * Wachtrijen
  * Tabellen en -bereiken van de tabelentiteiten.

### <a name="parameters-for-an-account-sas-token"></a>Parameters voor een account-SAS-token
* **De service of services.** Een account-SAS kunt toegang tot een of meer van de storage-services delegeren. U kunt bijvoorbeeld een account-SAS waarmee de gemachtigden toegang tot de service-Blob en het bestand maken. Of u een SAS dat gemachtigden toegang tot alle vier services (Blob, Queue, Table en -bestand) kunt maken.
* **Opslag resourcetypen.** Een account SAS is van toepassing op een of meer klassen van opslagbronnen in plaats van een specifieke bron. U kunt een account-SAS delegeren van toegang tot maken:
  * Serviceniveau-API's, die worden aangeroepen voor de account opslagbronnen. Voorbeelden zijn onder meer **Get/Set-Service-eigenschappen**, **ophalen Service statistieken**, en **lijst Containers/wachtrijen/tabellen, Shares**.
  * Niveau van de container-API's die worden aangeroepen met de containerobjecten voor elke service: blob-containers, wachtrijen, tabellen en bestandsshares. Voorbeelden zijn **maken/verwijderen Container**, **wachtrij maken/verwijderen**, **maken/verwijderen tabel**, **Share maken/verwijderen**, en  **Blobs/bestanden en mappen weergeven**.
  * Op objectniveau API's, die worden aangeroepen voor blobs, Wachtrijberichten, tabelentiteiten en bestanden. Bijvoorbeeld: **Blob plaatsen**, **Query entiteit**, **berichten ophalen**, en **bestand maken**.

## <a name="examples-of-sas-uris"></a>Voorbeelden van SAS-URI 's

### <a name="service-sas-uri-example"></a>Voorbeeld van de service-SAS-URI

Hier volgt een voorbeeld van een SAS-URI waarmee machtigingen lezen en schrijven naar een blob-service. De tabel uitsplitsing van elk deel van de te begrijpen hoe het bijdraagt aan de SAS URI:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Naam | SAS gedeelte | Beschrijving |
| --- | --- | --- |
| BLOB-URI |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |Het adres van de blob. Houd er rekening mee dat via HTTPS wordt sterk aanbevolen. |
| Storage services versie |`sv=2015-04-05` |Voor services versie 2012-02-12-opslag en later, de versie moet worden gebruikt door deze parameter wordt aangegeven. |
| Begintijd |`st=2015-04-29T22%3A18%3A26Z` |Opgegeven in UTC-tijd. Als u wilt dat de SAS onmiddellijk geldig zijn, laat u de begintijd. |
| Verlooptijd |`se=2015-04-30T02%3A23%3A26Z` |Opgegeven in UTC-tijd. |
| Resource |`sr=b` |De bron is een blob. |
| Machtigingen |`sp=rw` |De machtigingen verleend door de SAS Read (r) opnemen en (b) schrijven. |
| IP-bereik |`sip=168.1.5.60-168.1.5.70` |Het bereik van IP-adressen waaruit een aanvraag wordt geaccepteerd. |
| Protocol |`spr=https` |Alleen-aanvragen via HTTPS zijn toegestaan. |
| Handtekening |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Gebruikt voor het verifiëren van toegang tot de blob. De handtekening is een HMAC berekend over een tekenreeks te ondertekenen en de sleutel met behulp van het algoritme SHA256 en vervolgens gecodeerd met Base64-codering. |

### <a name="account-sas-uri-example"></a>Voorbeeld van account-SAS-URI

Hier volgt een voorbeeld van een account-SAS die gebruikmaakt van dezelfde algemene parameters op het token. Sinds deze parameters hierboven zijn beschreven, worden ze hier niet beschreven. Alleen de parameters die specifiek zijn voor account-SAS in de onderstaande tabel worden beschreven.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Naam | SAS gedeelte | Beschrijving |
| --- | --- | --- |
| Resource-URI |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |Het eindpunt Blob-service, en de parameters voor service-eigenschappen (als deze wordt aangeroepen met GET) ophalen of instellen van eigenschappen (als deze wordt aangeroepen met SET) van de service. |
| Services |`ss=bf` |De SAS is van toepassing op de services Blob en het bestand |
| Brontypen |`srt=s` |De SAS geldt voor serviceniveau-bewerkingen. |
| Machtigingen |`sp=rw` |De machtigingen verlenen toegang tot de lees- en schrijfbewerkingen. |

Gezien het feit dat machtigingen beperkt tot het serviceniveau zijn, toegankelijk bewerkingen met deze SAS zijn **Blob-Service-eigenschappen ophalen** (gelezen) en **Blob-Service-eigenschappen instellen** (schrijven). Echter met een andere resource URI hetzelfde SAS-token kan ook worden gebruikt voor toegang tot delegeren **statistieken voor Blob-Service ophalen** (gelezen).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Een SAS met een opgeslagen-beleid beheren
Een shared access signature kan duren voordat een van twee vormen:

* **Ad-hoc SAS:** wanneer u een ad-hoc SAS, de begintijd, de verlooptijd, maakt en machtigingen voor de SA's worden alle opgegeven in de SAS-URI (of impliciet, in het geval waarin de begintijd wordt weggelaten). Dit type SAS kan worden gemaakt als een account-SAS of een service-SAS.
* **SAS met opgeslagen toegangsbeleid:** een opgeslagen-beleid is gedefinieerd in een resourcecontainer een blob-container, tabel, wachtrijen of bestandsshare-- en kan worden gebruikt voor het beheren van beperkingen voor een of meer handtekeningen voor gedeelde toegang. Wanneer u een SAS aan een opgeslagen toegangsbeleid koppelt, neemt de SAS de beperkingen--de begintijd, verlooptijd en machtigingen--gedefinieerd voor het opgeslagen toegangsbeleid.

> [!NOTE]
> Op dit moment moet een SAS-account een ad-hoc SAS. Opgeslagen-beleidsregels worden nog niet ondersteund voor account-SAS.

Het verschil tussen de twee formulieren is belangrijk voor één key '-scenario: intrekken. Omdat een SAS-URI een URL is, kunt iedereen die de SAS verkrijgt gebruiken, ongeacht wie deze oorspronkelijk is gemaakt. Als een SAS openbaar wordt gepubliceerd, kan deze worden gebruikt door iedereen in de hele wereld. Een SAS verleent toegang tot bronnen voor iedereen die deze totdat een van de vier dingen gebeurt:

1. De verlooptijd opgegeven voor de SAS is bereikt.
2. De verlooptijd opgegeven op het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is bereikt (als een opgeslagen-beleid wordt verwezen, en als een verlooptijd is opgegeven). Dit kan optreden omdat het interval is verstreken of omdat u het opgeslagen toegangsbeleid met een verlooptijd in het verleden is één manier om in te trekken van de SAS hebt gewijzigd.
3. Het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS wordt verwijderd, die een andere manier om in te trekken van de SAS is. Houd er rekening mee dat als u het opgeslagen toegangsbeleid met dezelfde naam opnieuw maken, alle bestaande SAS-tokens opnieuw ongeldig volgens de machtigingen die zijn gekoppeld aan deze opgeslagen toegangsbeleid is (ervan uitgaande dat de verlooptijd van de SAS is niet verstreken). Als u wilde weet in te trekken van de SA's, zorg er dan voor dat een andere naam gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw maken.
4. De sleutel van het account dat is gebruikt voor het maken van de SAS is gegenereerd. Een account lezensleutel zorgt ervoor dat alle toepassingsonderdelen met deze sleutel niet kunnen verifiëren totdat ze zijn bijgewerkt voor het gebruik van andere geldige sleutel of de accountsleutel zojuist opnieuw gegenereerd.

> [!IMPORTANT]
> Een shared access signature-URI is gekoppeld aan de sleutel van het account gebruikt voor het maken van de handtekening en de bijbehorende opgeslagen-beleid (indien aanwezig). Als er geen opgeslagen toegangsbeleid is opgegeven, is de enige manier om een shared access signature intrekken om de accountsleutel te wijzigen.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Verificatie van een clienttoepassing met een SAS
Een client die in bezit van een SAS kan de SAS gebruiken om te verifiëren van een aanvraag in voor een opslagaccount waarvoor ze geen sleutels van de bezitten. Een SAS worden opgenomen in een verbindingsreeks, of gebruik rechtstreeks vanuit de juiste constructor of methode.

### <a name="using-a-sas-in-a-connection-string"></a>Met behulp van een SAS in een verbindingsreeks
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Met behulp van een SAS in een constructor of methode
Verschillende Azure Storage client bibliotheek constructors en methode overloads bieden een SAS-parameter, zodat u kunt een aanvraag om de service met een SAS te verifiëren.

Bijvoorbeeld, wordt hier een SAS-URI gebruikt voor het maken van een verwijzing naar een blok-blob. De SAS biedt de enige referenties die nodig zijn voor de aanvraag. De blok-blobverwijzing wordt vervolgens gebruikt voor een schrijfbewerking:

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

## <a name="best-practices-when-using-sas"></a>Aanbevolen procedures voor via SAS
Wanneer u handtekeningen voor gedeelde toegang in uw toepassingen gebruikt, moet u rekening houden met twee mogelijke risico's:

* Als een SAS is gelekt, kan deze worden gebruikt door iedereen die verkrijgt, die mogelijk uw storage-account kan vormen.
* Als een SAS opgegeven voor een clienttoepassing verloopt en de toepassing kan niet worden opgehaald van een nieuwe SAS van uw service, wordt de functionaliteit van de toepassing worden gehinderd uit.

De volgende aanbevelingen voor het gebruik van handtekeningen voor gedeelde toegang kunnen deze risico's verminderen:

1. **Altijd gebruik van HTTPS** maken of een SAS te distribueren. Als een SAS is doorgegeven via HTTP en onderschept, is een aanvaller uitvoeren van een man-in-the-middle-aanval kunnen de SAS lezen en gebruiken net zoals de beoogde gebruiker zijn kan, potentieel inbreuk op gevoelige gegevens of waardoor het beschadigde gegevens door de kwaadwillende gebruiker.
2. **Verwijzing opgeslagen toegangsbeleid waar mogelijk.** Opgeslagen toegangsbeleid bieden u de optie voor het intrekken van machtigingen zonder de sleutels van opslagaccount opnieuw genereren. De vervaldatum instellen voor deze zeer ver in de toekomst (of oneindige) en zorg ervoor dat deze wordt regelmatig bijgewerkt om deze te verplaatsen verder in de toekomst.
3. **Korte termijn verlopen keren gebruiken op een ad-hoc SAS.** Op deze manier, zelfs als een SAS is geknoeid, is dit alleen geldig voor een korte periode. Deze procedure is vooral belangrijk als u niet verwijzen naar een opgeslagen toegangsbeleid. Korte termijn verlopen tijden worden ook de hoeveelheid gegevens die kunnen worden geschreven naar een blob door het beperken van de beschikbare uploaden naar deze tijd beperken.
4. **Clients automatisch vernieuwen van de SAS indien nodig hebt.** Clients moeten de SAS vóór de vervaldatum vernieuwen om de tijd voor nieuwe pogingen toestaan als de service die de SAS niet beschikbaar is. Als uw SAS is bedoeld om te worden gebruikt voor een klein aantal onmiddellijke, tijdelijke bewerkingen die moeten worden voltooid binnen de verloopperiode, is dit mogelijk niet nodig als de SAS naar verwachting niet worden vernieuwd. Echter, hebt u een client die regelmatig aanvragen via SAS aan te brengen, klikt u vervolgens de mogelijkheid van verlopen binnenkomt in play. De belangrijkste overweging is om de noodzaak van de SA's worden tijdelijke balans (zoals eerder vermeld) met de noodzaak om ervoor te zorgen dat de client een vernieuwing vroeg vraagt genoeg (om te voorkomen dat wordt onderbroken vanwege de SAS verloopt voordat geslaagde vernieuwen).
5. **Wees voorzichtig met SAS-begintijd.** Als u de begintijd instellen voor een SAS voor **nu**, vervolgens vanwege klok leiden tot onjuiste (verschillen in de huidige tijd overeenkomstig andere machines), fouten kunnen worden waargenomen tijdelijk voor de eerste enkele minuten. In het algemeen de begintijd moet ten minste 15 minuten in het verleden instellen. Of niet zijn ingesteld dat deze, waarmee u het geldige direct in alle gevallen. In het algemeen geldt ook voor verlooptijd ook--Houd er rekening mee dat u maximaal 15 minuten klok scheeftrekken in beide richtingen op een aanvraag waarnemen. De maximale tijdsduur voor een SAS die niet verwijst naar een opgeslagen toegangsbeleid is voor clients met behulp van een REST-versie dan 2012-02-12, 1 uur en alle regels voor het opgeven van de lange termijn dan die zal mislukken.
6. **Moet specifiek met de bron worden geopend.** Aanbevolen beveiligingsprocedure is om te voorzien van een gebruiker de minimaal vereiste machtigingen. Als een gebruiker alleen leestoegang tot één entiteit moet, verleen hen leestoegang tot die één entiteit en niet lezen, schrijven, verwijderen toegang tot alle entiteiten. Hierdoor kunnen ook verkleinen van de schade, als er een SAS is geknoeid omdat de SAS minder stroom in handen valt van een aanvaller heeft.
7. **Begrijpen dat uw account wordt gefactureerd voor gebruik, met inbegrip van die klaar met SAS.** Als u toegang voor schrijven naar een blob opgeeft, wordt een gebruiker kan kiezen voor het uploaden van een blob 200GB. Als u ze ook leestoegang gegeven, kiezen ze voor het downloaden van 10 keer steeds 2 TB in uitgaande kosten voor u. Opnieuw, Geef beperkte machtigingen om te beperken van de mogelijke acties van kwaadwillende gebruikers. Tijdelijke SAS deze bedreiging beperken (maar houd ook rekening met het tijdsverschil op de eindtijd) gebruiken.
8. **Gegevens die zijn geschreven met behulp van SAS valideren.** Wanneer een client-toepassing gegevens naar uw opslagaccount schrijft, houd er rekening mee dat er problemen met deze gegevens kunnen worden. Als uw toepassing vereist dat gegevens worden gevalideerd of geautoriseerd voordat deze is gereed om te gebruiken, moet u deze validatie uitvoeren nadat de gegevens worden geschreven en voordat deze wordt gebruikt door uw toepassing. Deze procedure biedt ook bescherming tegen beschadigd of schadelijke gegevens naar uw account worden geschreven door een gebruiker die de SAS correct hebt verkregen of door een gebruiker die misbruik van een gelekte SAS.
9. **Altijd gebruik geen SAS.** Soms is de risico's die zijn gekoppeld aan een bepaalde bewerking op basis van uw opslagaccount opwegen tegen de voordelen van SAS. Maken van een middelste laag-service die naar uw storage-account na het uitvoeren van zakelijke schrijft voor dergelijke bewerkingen regel validatie, verificatie en controle. Soms is ook eenvoudiger te beheren van toegang op andere manieren. Bijvoorbeeld, als u alle blobs in een container openbaar leesbaar maken wilt, kunt u de container openbaar, in plaats van een SAS voor toegang voor elke client.
10. **Storage Analytics gebruiken om uw toepassing te bewaken.** U kunt logboekregistratie en metrische gegevens gebruiken om te observeren een piek in de verificatiefouten vanwege een storing in uw SAS provider-service of op het onbedoeld verwijderen van een opgeslagen toegangsbeleid. Zie de [Blog van Azure Storage-Team](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) voor meer informatie.

## <a name="sas-examples"></a>SAS-voorbeelden
Hieronder volgen enkele voorbeelden van beide typen handtekeningen voor gedeelde toegang, account-SAS en service-SAS.

Deze voorbeelden C#, hebt u nodig om te verwijzen naar de volgende NuGet-pakketten in uw project:

* [Azure Storage-clientbibliotheek voor .NET](http://www.nuget.org/packages/WindowsAzure.Storage), versie 6.x of hoger (account-SAS gebruiken).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Zie voor aanvullende voorbeelden die laten zien hoe u maken en testen van een SAS [Azure codevoorbeelden voor opslag](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Voorbeeld: Maken en gebruiken van een account-SAS
Het volgende codevoorbeeld maakt een account SA's dat is geldig voor de services Blob en het bestand en krijgt de client machtigingen voor lezen, schrijven en lijst met machtigingen voor toegang tot serviceniveau-API's. De account-SAS beperkt het protocol HTTPS, zodat de aanvraag moet worden uitgevoerd met HTTPS.

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

Als u wilt de SAS-account gebruiken voor toegang tot serviceniveau-API's voor de Blob-service maken op het object van een Blob-client met behulp van de SAS- en eindpunt van de Blob-opslag voor uw opslagaccount.

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

### <a name="example-create-a-stored-access-policy"></a>Voorbeeld: Een opgeslagen-beleid maken
De volgende code maakt een toegangsbeleid opgeslagen in een container. Het toegangsbeleid kunt u beperkingen voor een service-SAS opgeven op de container of de blobs.

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

### <a name="example-create-a-service-sas-on-a-container"></a>Voorbeeld: Een SAS-service op een container maken
De volgende code maakt een SAS in een container. Als de naam van een bestaande opgeslagen toegangsbeleid is opgegeven, wordt dat beleid is gekoppeld aan de SAS. Als er geen opgeslagen toegangsbeleid is opgegeven, maakt de code een ad-hoc-SAS voor de container.

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

### <a name="example-create-a-service-sas-on-a-blob"></a>Voorbeeld: Een SAS-service op een blob maken
De volgende code wordt een SAS maakt op een blob. Als de naam van een bestaande opgeslagen toegangsbeleid is opgegeven, wordt dat beleid is gekoppeld aan de SAS. Als er geen opgeslagen toegangsbeleid is opgegeven, maakt de code een ad-hoc-SAS op de blob.

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
Shared access signatures zijn nuttig voor het bieden van beperkte machtigingen naar uw opslagaccount aan clients die niet de accountsleutel moeten hebben. Ze zijn als zodanig een essentieel onderdeel van het beveiligingsmodel voor elke toepassing met behulp van Azure Storage. Als u Volg de aanbevolen procedures die hier worden vermeld, kunt u SAS gebruiken om toegang te bieden meer flexibiliteit van toegang tot bronnen in uw opslagaccount, zonder de beveiliging van uw toepassing.

## <a name="next-steps"></a>Volgende stappen
* [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met Blob-opslag](../blobs/storage-dotnet-shared-access-signature-part-2.md)
* [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md)
* [Toegang delegeren met een Shared Access Signature](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Inleiding tot Table en Queue SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
