---
title: Over Azure Key Vault sleutels, geheimen en certificaten - Azure Key Vault
description: Overzicht van Azure Key Vault REST-interface en developer-details voor sleutels, geheimen en certificaten.
services: key-vault
documentationcenter: ''
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 01d9f763983da2415aba0f9bae81414017bc2f02
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57842563"
---
# <a name="about-keys-secrets-and-certificates"></a>Over sleutels, geheimen en certificaten

Azure Key Vault kunt Microsoft Azure-toepassingen en gebruikers kunnen opslaan en gebruiken van verschillende gegevenstypen geheim/sleutel:

- Cryptografische sleutels: Biedt ondersteuning voor meerdere typen sleutelbestand en algoritmen, en kunt het gebruik van Hardware Security Modules (HSM) voor hoge waarde sleutels. 
- Geheimen: Biedt een veilige opslag van geheimen zoals wachtwoorden en tekenreeksen voor databaseverbindingen.
- Certificaten: Biedt ondersteuning voor certificaten die zijn gebaseerd op de sleutels en geheimen en toevoegen van een functie voor automatische verlenging.
- Azure Storage: Kan de sleutels van een Azure Storage-account voor u beheren. Intern, Key Vault kan de sleutels (sync) met een Azure Storage-Account weergeven en opnieuw te genereren (roteren) periodiek de sleutels. 

Zie voor meer algemene informatie over Key Vault [wat is Azure Key Vault?](/azure/key-vault/key-vault-whatis)

## <a name="azure-key-vault"></a>Azure Key Vault

De volgende secties bieden algemene informatie die van toepassing zijn voor de implementatie van de service Key Vault.

### <a name="supporting-standards"></a>Ondersteuning van standaarden

De JavaScript Object Notation (JSON) en de specificaties voor JavaScript Object ondertekening en versleuteling (JOSE) zijn belangrijke achtergrondinformatie.  

-   [JSON-Websleutel (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Encryption (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmen (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web handtekening (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="data-types"></a>Gegevenstypen

Raadpleeg de specificaties JOSE voor relevante gegevenstypen voor sleutels, versleuteling en ondertekening.  

-   **algoritme** -een ondersteunde algoritme voor een sleutel is, bijvoorbeeld RSA1_5  
-   **ciphertext-waarde** -tekst octetten, gecodeerd met Base64URL cipher  
-   **Digest-waarde** -de uitvoer van een hash-algoritme, gecodeerd met Base64URL  
-   **sleutel-type** -een van de ondersteunde typen voor sleutels, bijvoorbeeld RSA (Rivest-Shamir-Adleman).  
-   **niet-gecodeerde tekst / waarde-** -als tekst zonder opmaak octetten, gecodeerd met Base64URL  
-   **waarde handtekening** - uitvoer van een algoritme voor handtekening, gecodeerd met Base64URL  
-   **base64URL** -een Base64URL [RFC4648] gecodeerde binaire waarde  
-   **Booleaanse** -true of false  
-   **Identiteit** : een identiteit van Azure Active Directory (AAD).  
-   **IntDate** - een decimale waarde die aangeeft van het aantal seconden tussen 1970 van JSON-01-01T0:0:0Z UTC tot en met de opgegeven UTC-datum/tijd. Bekijk in het bijzonder RFC3339 voor meer informatie over date/times, in het algemeen en UTC.  

### <a name="objects-identifiers-and-versioning"></a>Objecten, -id's en versiebeheer

Objecten die zijn opgeslagen in Key Vault worden samengesteld wanneer er een nieuw exemplaar van een object wordt gemaakt. Elke versie is een unieke id en een URL toegewezen. Wanneer een object wordt gemaakt, heeft deze krijgt een unieke id en gemarkeerd als de huidige versie van het object. Het maken van een nieuw exemplaar met dezelfde naam van het object geeft het nieuwe object een unieke id, waardoor de huidige versie.  

Objecten in Key Vault kunnen worden aangepakt met behulp van de huidige id of een specifiek voor een versie-id. Bijvoorbeeld, een sleutel met de naam van de opgegeven `MasterKey`, uitvoeren van bewerkingen met de huidige id zorgt ervoor dat het systeem moet de meest recente beschikbare versie. Uitvoeren van bewerkingen met de id van de specifieke versies zorgt ervoor dat het systeem die specifieke versie van het object te gebruiken.  

Objecten kunnen worden onderscheiden in Key Vault met behulp van een URL. Er zijn geen twee objecten in het systeem hebben dezelfde URL, ongeacht de geografische locatie. De volledige URL naar een object wordt de Object-id genoemd. De URL bestaat uit een voorvoegsel dat de Key Vault, objecttype identificeert, de gebruiker opgegeven naam van Object en de versie van een Object. Naam van het Object is niet hoofdlettergevoelig en onveranderbaar. Id's die niet de Object-versie worden aangeduid als Base-id.  

Zie voor meer informatie, [verificatie, vragen en antwoorden](authentication-requests-and-responses.md)

Een object-id heeft de volgende algemene indeling:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Waar:  

|||  
|-|-|  
|`keyvault-name`|De naam voor een key vault in de Microsoft Azure Key Vault-service.<br /><br /> Key Vault-namen worden geselecteerd door de gebruiker en wereldwijd uniek zijn.<br /><br /> Key Vault-naam moet een tekenreeks van 3 tot 24, met alleen 0-9, a-z, A-Z, en -.|  
|`object-type`|Het type van het object, 'sleutels' of 'geheimen'.|  
|`object-name`|Een `object-name` is een door de gebruiker opgegeven naam voor en moet uniek zijn binnen een Key Vault. De naam moet een tekenreeks van 1-127, met alleen 0-9, a-z, A-Z, en -.|  
|`object-version`|Een `object-version` is een systeem gegenereerde, 32 tekens tekenreeks-id die eventueel wordt gebruikt om een unieke versie van een object.|  

## <a name="key-vault-keys"></a>Key Vault sleutels

### <a name="keys-and-key-types"></a>Sleutels en sleuteltypen

Cryptografische sleutels in Key Vault worden weergegeven als JSON-Websleutel [JWK]-objecten. De base JWK/JWA specificaties zijn ook uitgebreid zodat sleuteltypen uniek is voor de implementatie van de Key Vault. Sleutels die gebruikmaken van HSM's van specifieke pakketten importeert, kunt u bijvoorbeeld veilige vervoer van sleutels die alleen kan worden gebruikt in Key Vault HSM's.  

- **"Soft" keys**: Een sleutel verwerkt in software door Key Vault, maar is in rust versleuteld met behulp van een systeemsleutel die zich in een HSM. Clients kunnen importeren van een bestaande sleutel voor RSA of EG (Elliptic Curve) of Key Vault genereren een vraag.
- **"Vaste" sleutels**: Een sleutel verwerkt in een HSM (Hardware Security Module). Deze sleutels worden beveiligd in een van de Key Vault HSM-Beveiligingswerelden (Er is een Beveiligingswereld per Geografie isolatie onderhouden). Clients kunnen een RSA of EG sleutel in zachte formulier of door het exporteren van een compatibel apparaat van de HSM importeren. Clients kunnen ook aanvragen voor Key Vault om een sleutel te genereren. Dit sleuteltype voegt de T-kenmerk toe aan de JWK ophalen voor het uitvoeren van de HSM-sleutelmateriaal.

     Zie voor meer informatie over geografische grenzen [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/)  

Key Vault ondersteunt RSA- en elliptische-sleutels. 

-   **EC**: 'Soft' elliptische-sleutel.
-   **EC-HSM**: "Vaste" elliptische-sleutel.
-   **RSA**: 'Soft' RSA-sleutel.
-   **RSA-HSM**: "Vaste" RSA-sleutel.

Key Vault ondersteunt RSA-sleutels van de grootte 2048, 3072 en 4096. Key Vault ondersteunt Elliptic Curve sleutel van het type P-256, p-384, p-521 en P-256_K (SECP256K1).

### <a name="cryptographic-protection"></a>Cryptografische beveiliging

De cryptografische modules die Key Vault gebruikt, of HSM of software zijn gevalideerd voor FIPS (Federal Information Processing Standards). U hoeft te doen niets om uit te voeren in de FIPS-modus. Sleutels **gemaakt** of **geïmporteerd** als HSM-beveiliging worden verwerkt binnen een HSM, gevalideerd voor FIPS 140-2 Level 2. Sleutels **gemaakt** of **geïmporteerd** als softwarebeveiliging, worden verwerkt in de cryptografische modules die zijn gevalideerd voor FIPS 140-2 Level 1. Zie voor meer informatie, [sleutels en sleuteltypen](#keys-and-key-types).

###  <a name="ec-algorithms"></a>EG algoritmen
 De volgende algoritme-id's worden ondersteund met de EG en EG-HSM-sleutels in Key Vault. 

#### <a name="curve-types"></a>Curve-typen

-   **P-256** -curve van het NIST p-256, gedefinieerd op [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-256_K** -de seconde curve SECP256K1, gedefinieerd op [SEC 2: Aanbevolen Elliptic Curve domeinparameters](https://www.secg.org/sec2-v2.pdf).
-   **P-384** -curve van het NIST P-384, gedefinieerd op [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).
-   **P-521** -curve van het NIST p-521 gedefinieerd op [DSS FIPS PUB 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf).

#### <a name="signverify"></a>AANMELDING/CONTROLEREN

-   **ES256** - verwerkingen ECDSA voor SHA-256 en sleutels die zijn gemaakt met de curve p-256. Dit algoritme wordt beschreven op [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES256K** - verwerkingen ECDSA voor SHA-256 en sleutels die zijn gemaakt met de curve P-256_K. Dit algoritme is in behandeling zijnde standaardisatie.
-   **ES384** - verwerkingen ECDSA voor SHA-384 en sleutels die zijn gemaakt met de curve P-384. Dit algoritme wordt beschreven op [RFC7518](https://tools.ietf.org/html/rfc7518).
-   **ES512** - ECDSA voor SHA-512 verwerkingen en sleutels die zijn gemaakt met de curve p-521. Dit algoritme wordt beschreven op [RFC7518](https://tools.ietf.org/html/rfc7518).

###  <a name="rsa-algorithms"></a>RSA-algoritmen  
 De volgende algoritme-id's worden ondersteund met de RSA- en RSA-HSM-sleutels in Key Vault.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>SLEUTEL INPAKKEN/SLEUTEL UITPAKKEN, VERSLEUTELEN/ONTSLEUTELEN

-   **RSA1_5** - RSAES-PKCS1-V1_5 [RFC3447] key encryption  
-   **RSA-OAEP** - RSAES met optimale asymmetrische codering opvulling (OAEP) [RFC3447] met de standaardparameters die is opgegeven door RFC 3447 in sectie A.2.1. Deze standaardparameters worden met behulp van een hash-functie van SHA-1 en een functie voor het genereren van masker van MGF1 met SHA-1.  

#### <a name="signverify"></a>AANMELDING/CONTROLEREN

-   **RS256** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met SHA-256. De opgegeven toepassing digest-waarde moet worden berekend op basis van SHA-256 en moet 32 bytes lang zijn.  
-   **RS384** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-384. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-384 en moet 48 bytes lang zijn.  
-   **RS512** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-512. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-512 en moet 64 bytes lang zijn.  
-   **RSNULL** -Zie [RFC2437] een gespecialiseerde use-case waarmee bepaalde TLS-scenario's.  

###  <a name="key-operations"></a>Sleutelbewerkingen

Key Vault ondersteunt de volgende bewerkingen voor sleutels objecten:  

-   **Maak**: Kan een client een sleutel te maken in Key Vault. De waarde van de sleutel wordt gegenereerd door Key Vault en opgeslagen, en naar de client niet is vrijgegeven. Asymmetrische sleutels kunnen worden gemaakt in Key Vault.  
-   **Importeren**: Kan een client een bestaande sleutel importeert naar Key Vault. Asymmetrische sleutels kunnen worden geïmporteerd in Key Vault met behulp van een aantal methoden die andere pakketten in een JWK-constructie. 
-   **Update**: Kan een client met voldoende machtigingen voor het wijzigen van de metagegevens (belangrijke kenmerken) die zijn gekoppeld aan een sleutel die eerder zijn opgeslagen in Key Vault.  
-   **Verwijder**: Kan een client met voldoende machtigingen voor het verwijderen van een sleutel uit Key Vault.  
-   **Lijst met**: Hiermee kunt u een lijst van alle sleutels in een opgegeven Key Vault-client.  
-   **Versies weergeven**: Hiermee kunt u een lijst van alle versies van een opgegeven sleutel in een opgegeven Key Vault-client.  
-   **Ophalen**: Kan een client ophalen van de openbare gedeelten van een opgegeven sleutel in een Key Vault.  
-   **Back-up**: Hiermee exporteert u een sleutel in een beveiligde vorm.  
-   **Herstellen**: Hiermee importeert u een eerder back-ups van sleutel.  

Zie voor meer informatie, [belangrijke bewerkingen in de Key Vault REST API-verwijzing](/rest/api/keyvault).  

Zodra een sleutel is gemaakt in Key Vault, kunnen de volgende cryptografische bewerkingen worden uitgevoerd met behulp van de sleutel:  

-   **Meld u aan en controleer of**: Deze bewerking is er strikt, voor dat 'aanmelding hash' of 'verifiëren hash', zoals Key Vault biedt geen ondersteuning voor hashing van inhoud als onderdeel van het maken van de handtekening. Toepassingen moeten hash van de gegevens lokaal zijn ondertekend en vervolgens die Key Vault-aanmelding de hash-aanvragen. Verificatie van de ondertekende hashes wordt ondersteund als een bewerking gemak voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal. Voor optimale toepassingsprestaties, controleert u of dat bewerkingen lokaal worden uitgevoerd.  
-   **Sleutel voor versleuteling / Wrapping**: Een sleutel die is opgeslagen in Key Vault kan worden gebruikt om een andere sleutel, meestal de sleutel van een symmetrische versleuteling van de inhoud (CEK) te beveiligen. Wanneer de sleutel in Key Vault asymmetrische is, wordt key-versleuteling gebruikt. Bijvoorbeeld: RSA-OAEP en de sleutel INPAKKEN/UNWRAPKEY-bewerkingen zijn gelijk aan het coderen/decoderen. Wanneer de sleutel in Key Vault symmetrische is, wordt wrapping sleutel gebruikt. Bijvoorbeeld: AES-KW. De sleutel INPAKKEN-bewerking wordt ondersteund voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal gemakkelijker te maken. Voor de beste toepassingsprestaties, moeten de sleutel INPAKKEN operations lokaal worden uitgevoerd.  
-   **Versleutelen en ontsleutelen van**: Een sleutel die is opgeslagen in Key Vault kan worden gebruikt om te versleutelen of ontsleutelen van één blok van gegevens. De grootte van het blok wordt bepaald door het type sleutel en de geselecteerde versleutelingsalgoritme. De bewerking versleutelen is opgegeven voor het gemak voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal. Voor de beste toepassingsprestaties, versleutelen bewerkingen moeten lokaal worden uitgevoerd.  

Tijdens het WRAPKEY/sleutel uitpakken met behulp van asymmetrische sleutels lijken misschien overbodige (zoals de bewerking gelijk aan het coderen/decoderen is), het gebruik van verschillende bewerkingen is belangrijk. Het verschil biedt functionaliteit voor semantische en autorisatie scheiding van deze bewerkingen en consistentie op basis van andere typen sleutels worden ondersteund door de service.  

Key Vault biedt geen ondersteuning voor exportbewerkingen. Zodra een sleutel is ingericht in het systeem, kan niet worden uitgepakt of het sleutelmateriaal is gewijzigd. Gebruikers van Key Vault kunnen echter de sleutel nodig voor andere gevallen, zoals nadat deze is verwijderd. In dit geval kunnen ze de back-up en herstel-bewerkingen gebruiken voor exporteren/importeren van de sleutel in een beveiligde vorm. Sleutels die zijn gemaakt door de back-up zijn niet kan worden gebruikt buiten de Key Vault. De importbewerking kan ook worden gebruikt op basis van meerdere exemplaren van de Key Vault.  

Gebruikers kunnen een van de cryptografische bewerkingen die ondersteuning biedt voor Key Vault op basis van de key_ops-eigenschap van het object JWK per sleutel beperken.  

Zie voor meer informatie over JWK objecten [JSON Web sleutel (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="key-attributes"></a>Belangrijke kenmerken

Naast het sleutelmateriaal, kunnen de volgende kenmerken worden opgegeven. In een JSON-aanvraag, de kenmerken sleutelwoord en accolades, ' {' '}', zijn vereist, zelfs als er geen kenmerken opgegeven zijn.  

- *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Hiermee geeft u op of de sleutel ingeschakeld en bruikbaar voor cryptografische bewerkingen is. De *ingeschakeld* kenmerk wordt gebruikt in combinatie met *nbf* en *exp*. Wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, wordt alleen worden toegestaan als *ingeschakeld* is ingesteld op **waar**. Bewerkingen buiten de *nbf* / *exp* venster zijn automatisch toegestaan, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](#date-time-controlled-operations).
- *nbf*: IntDate, optioneel, standaard is nu. De *nbf* (niet voor) kenmerk wordt de tijd waarbinnen moet de sleutel niet worden gebruikt voor cryptografische bewerkingen, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](#date-time-controlled-operations). De verwerking van de *nbf* kenmerk vereist dat de huidige datum en tijd moeten na of gelijk aan de niet-vóór de datum/tijd die worden vermeld in de *nbf* kenmerk. Key Vault kan bieden voor enkele kleine eenheidsprofiel normaal gesproken niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *exp*: IntDate, optioneel, standaard is 'altijd'. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na die de sleutel mag niet worden gebruikt voor cryptografische bewerking, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](#date-time-controlled-operations). De verwerking van de *exp* kenmerk is vereist dat de huidige datum en tijd zijn moet voordat de vervaldatum van de datum/tijd die worden vermeld in de *exp* kenmerk. Key Vault kan bieden voor enkele kleine eenheidsprofiel doorgaans niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in elke reactie met belangrijke kenmerken:  

- *gemaakt*: IntDate, optioneel. De *gemaakt* kenmerk geeft aan wanneer deze versie van de sleutel is gemaakt. De waarde is null voor sleutels die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De *bijgewerkt* kenmerk geeft aan wanneer deze versie van de sleutel is bijgewerkt. De waarde is null voor sleutels die voor het laatst vóór het toevoegen van dit kenmerk bijgewerkt zijn. De waarde moet een getal met een IntDate-waarde.  

Zie voor meer informatie over IntDate en andere gegevenstypen [gegevenstypen](#data-types)  

#### <a name="date-time-controlled-operations"></a>Beheerde bewerkingen voor datum / tijd

Niet nog geldig en verlopen sleutels, buiten de *nbf* / *exp* venster werkt voor **ontsleutelen**, **uitpakken**, en **controleren** bewerkingen (403, niet als resultaat verboden). De logica voor het gebruik van de status niet nog geldig is om een sleutel moet worden getest voordat u gebruik in productieomgevingen. De logica voor het gebruik van de verlopen staat is om toe te staan van de recovery-bewerkingen op gegevens die is gemaakt tijdens de sleutel ongeldig is. Bovendien kunt u toegang tot een sleutel met behulp van Key Vault-beleid of door bij te werken uitschakelen de *ingeschakeld* sleutelkenmerk naar **false**.

Zie voor meer informatie over gegevenstypen [gegevenstypen](#data-types).

Zie voor meer informatie over andere kenmerken, het [JSON Web sleutel (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="key-tags"></a>Sleutel-tags

U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van tags. Key Vault ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde 256 tekens hebben.  

>[!Note]
>Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor dit objecttype (sleutels, geheimen of certificaten).

###  <a name="key-access-control"></a>Toegangsbeheer voor sleutels

Toegangsbeheer voor sleutels die worden beheerd door Key Vault is opgegeven op het niveau van een Key Vault die als de container van sleutels fungeert. Het beleid voor toegangsbeheer voor sleutels, verschilt van het beleid voor toegangsbeheer voor geheimen in de dezelfde Key Vault. Gebruikers kunnen maken van een of meer kluizen voor het opslaan van sleutels en zijn verplicht om de juiste segmentering scenario en het beheer van sleutels. Toegangsbeheer voor sleutels is onafhankelijk van toegangsbeheer voor geheimen.  

De volgende machtigingen kunnen worden verleend, op een per gebruiker / service-principal uit te voeren, in de access control entry sleutels in een kluis. Deze machtigingen spiegelen nauw de bewerkingen die zijn toegestaan op een sleutel-object:  

- Machtigingen voor sleutelbeheerbewerkingen
  - *Ophalen*: Het openbare deel van een sleutel, plus de kenmerken lezen
  - *Lijst met*: De sleutels of versies van een sleutel die zijn opgeslagen in een key vault
  - *Update*: De kenmerken voor een sleutel bijwerken
  - *Maak*: Nieuwe sleutels maken
  - *Importeren*: Een sleutel te importeren naar een sleutelkluis
  - *Verwijder*: Het belangrijkste object verwijderen
  - *recover*: Een verwijderde sleutel herstellen
  - *backup*: Back-up van een sleutel in een key vault
  - *Herstellen*: Een back-ups herstellen van de sleutel tot een key vault

- Machtigingen voor cryptografische bewerkingen
  - *ontsleutelen*: Gebruik de sleutel voor het opheffen van een reeks bytes
  - *versleutelen*: Gebruik de sleutel voor het beveiligen van een willekeurige volgorde van de bytes
  - *unwrapKey*: De sleutel gebruiken om op te heffen verpakte symmetrische sleutels
  - *wrapKey*: Gebruik de sleutel voor het beveiligen van een symmetrische sleutel
  - *Controleer of*: De sleutel gebruiken om te controleren of verwerkingen  
  - *Meld u*: Gebruik de sleutel aan te melden verwerkingen
    
- Machtigingen voor beschermde bewerkingen
  - *purge*: Verwijderen (definitief verwijderen) een verwijderde sleutel

Zie voor meer informatie over het werken met sleutels [belangrijke bewerkingen in de Key Vault REST API-verwijzing](/rest/api/keyvault). Zie voor informatie over het tot stand brengen van machtigingen, [kluizen: maken of bijwerken](/rest/api/keyvault/vaults/createorupdate) en [kluizen: Update-toegangsbeleid](/rest/api/keyvault/vaults/updateaccesspolicy). 

## <a name="key-vault-secrets"></a>Key Vault-geheimen 

### <a name="working-with-secrets"></a>Werken met geheimen

Vanuit het perspectief van een ontwikkelaar, van Key Vault-API's accepteren en geheime waarden als tekenreeksen retourneren. Intern, Key Vault worden opgeslagen en beheerd van geheimen als reeksen octetten (8-bits-bytes), met een maximale grootte van 25 k bytes. De service Key Vault biedt geen semantiek voor geheimen. Het gewoon accepteert de gegevens, versleutelt deze, slaat deze en retourneert een geheime id ('id'). De id kan worden gebruikt om op te halen van het geheim op een later tijdstip.  

Voor zeer gevoelige gegevens moeten clients kunt u extra beveiligingslagen voor gegevens. Versleutelen van gegevens met behulp van een afzonderlijke protection-tenantsleutel voorafgaand aan opslag in Key Vault is een voorbeeld.  

Key Vault ondersteunt ook het veld contentType voor geheimen. Clients kunnen het type inhoud van een geheim om te helpen bij het interpreteren van de geheime gegevens wanneer deze wordt opgehaald opgeven. De maximale lengte van dit veld is 255 tekens. Er zijn geen vooraf gedefinieerde waarden. Het voorgestelde gebruik is als een hint op voor het interpreteren van de geheime gegevens. Een van de implementatie kan bijvoorbeeld zowel wachtwoorden en certificaten als geheimen opslaan en vervolgens dit veld om onderscheid te gebruiken. Er zijn geen vooraf gedefinieerde waarden.  

### <a name="secret-attributes"></a>Geheime kenmerken

Naast de geheime gegevens kunnen de volgende kenmerken worden opgegeven:  

- *exp*: IntDate, optioneel, standaard is **altijd**. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na die de geheime gegevens moeten niet worden opgehaald, met uitzondering van in [bepaalde situaties](#date-time-controlled-operations). Dit veld is bedoeld voor **informatief** doeleinden alleen als deze gebruikers van de sleutelkluis-service geïnformeerd dat een bepaalde geheim niet kan worden gebruikt. De waarde moet een getal met een IntDate-waarde.   
- *nbf*: IntDate, optioneel, standaard is **nu**. De *nbf* (niet voor) kenmerk wordt de tijd waarbinnen moet de geheime gegevens niet worden opgehaald, met uitzondering van in [bepaalde situaties](#date-time-controlled-operations). Dit veld is bedoeld voor **informatief** uitsluitend bedoeld. De waarde moet een getal met een IntDate-waarde. 
- *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Dit kenmerk wordt opgegeven of de geheime gegevens kunnen worden opgehaald. De ingeschakelde kenmerk wordt gebruikt in combinatie met *nbf* en *exp* wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, worden pas bij inschakeling toegestaan is ingesteld op **waar**. Bewerkingen buiten de *nbf* en *exp* venster worden automatisch niet-toegestane, behalve in [bepaalde situaties](#date-time-controlled-operations).  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in elke reactie met geheime kenmerken:  

- *gemaakt*: IntDate, optioneel. De gemaakte kenmerk geeft aan wanneer deze versie van het geheim is gemaakt. Deze waarde is null voor geheimen die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De bijgewerkte kenmerk geeft aan wanneer deze versie van het geheim is bijgewerkt. Deze waarde is null voor geheimen die voor het laatst vóór het toevoegen van dit kenmerk bijgewerkt zijn. De waarde moet een getal met een IntDate-waarde.

#### <a name="date-time-controlled-operations"></a>Beheerde bewerkingen voor datum / tijd

Van een geheim **ophalen** bewerking werkt voor geen nog geldig is en verlopen geheimen, buiten de *nbf* / *exp* venster. Aanroepen van een geheim **ophalen** bewerking, voor een geheim niet nog geldig kan worden gebruikt voor testdoeleinden. Bij het ophalen van (**ophalen**appclassifica) een geheim verlopen, kan worden gebruikt voor herstelbewerkingen.

Zie voor meer informatie over gegevenstypen [gegevenstypen](#data-types).  

### <a name="secret-access-control"></a>Toegangsbeheer voor geheimen

Toegangsbeheer voor geheimen in Key Vault wordt beheerd, is opgegeven op het niveau van de Key Vault waarin deze geheimen. Het beleid voor toegangsbeheer voor geheimen, verschilt van het beleid voor toegangsbeheer voor sleutels in de dezelfde Key Vault. Gebruikers kunnen maken van een of meer kluizen voor het opslaan van geheimen en zijn verplicht om de juiste segmentering scenario en het beheer van geheimen.   

De volgende machtigingen kunnen worden gebruikt, op basis van per-principal, in de access control entry geheimen in een kluis en nauw mirror van de bewerkingen die zijn toegestaan op een geheim object:  

- Machtigingen voor geheimenbeheerbewerkingen
  - *Ophalen*: Een geheim lezen  
  - *Lijst met*: De geheimen of versies van een geheim opgeslagen in een Key Vault  
  - *Stel*: Een geheim maken  
  - *Verwijder*: Een geheim verwijderen  
  - *recover*: Herstellen van een verwijderde geheim
  - *backup*: Back-up van een geheim in een key vault
  - *Herstellen*: Een back-ups herstellen van een key vault-geheim

- Machtigingen voor beschermde bewerkingen
  - *purge*: Verwijderen (definitief verwijderen) een verwijderde geheim

Zie voor meer informatie over het werken met geheimen [geheime bewerkingen in de Key Vault REST API-verwijzing](/rest/api/keyvault). Zie voor informatie over het tot stand brengen van machtigingen, [kluizen: maken of bijwerken](/rest/api/keyvault/vaults/createorupdate) en [kluizen: Update-toegangsbeleid](/rest/api/keyvault/vaults/updateaccesspolicy). 

### <a name="secret-tags"></a>De geheime codes  
U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van tags. Key Vault ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde 256 tekens hebben.  

>[!Note]
>Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor dit objecttype (sleutels, geheimen of certificaten).

## <a name="key-vault-certificates"></a>Key Vault-certificaten

Key Vault-certificaten-ondersteuning biedt voor het beheer van uw x509 certificaten en het volgende gedrag:  

-   Kan de eigenaar van een certificaat om een certificaat via een proces voor het maken van Key Vault of via het importeren van een bestaand certificaat te maken. Bevat zowel zelfondertekende en de certificeringsinstantie die certificaten worden gegenereerd.
-   Kan de eigenaar van een Key Vault-certificaat voor het implementeren van veilige opslag en het beheer van X509 certificaten zonder interactie met privésleutelmateriaal.  
-   Kan de eigenaar van een certificaat te maken van een beleid dat zorgt ervoor dat de Key Vault voor het beheren van de levenscyclus van een certificaat.  
-   Kan certificaat eigenaren contact op met informatie voor melding over gebeurtenissen van de levenscyclus van de vervaldatum en -verlenging van het certificaat opgeven.  
-   Automatische vernieuwing ondersteunt met geselecteerde verleners - Key Vault partner X509 providers van het certificaat / certificeringsinstanties.

>[!Note]
>Providers/instanties niet gelieerde zijn ook toegestaan, maar wordt geen ondersteuning voor de functie voor automatisch verlengen.

### <a name="composition-of-a-certificate"></a>Samenstelling van een certificaat

Wanneer een Key Vault-certificaat is gemaakt, worden ook een adresseerbare sleutel en -geheim gemaakt met dezelfde naam. De Key Vault-sleutel kan sleutelbewerkingen en de Key Vault-geheim kan ophalen van de waarde van het certificaat als een geheim. Een Key Vault-certificaat bevat ook openbare x509 metagegevens van het certificaat.  

De id en de versie van de certificaten is vergelijkbaar met dat van sleutels en geheimen. Een specifieke versie van een adresseerbare sleutel en -geheim dat is gemaakt met de versie van Key Vault-certificaat is beschikbaar in het antwoord van Key Vault-certificaat.
 
![Certificaten zijn complexe objecten](media/azure-key-vault.png)

### <a name="exportable-or-non-exportable-key"></a>De sleutel exporteerbaar of niet kan worden geëxporteerd

Wanneer een Key Vault-certificaat is gemaakt, kan het worden opgehaald uit de adresseerbare geheim met de persoonlijke sleutel in pfx- of PEM-indeling. Het beleid dat wordt gebruikt voor het maken van het certificaat moet aangeven dat de sleutel exporteerbaar is. Als het beleid niet exporteerbaar maken blijkt, klikt u vervolgens de persoonlijke sleutel is niet een deel van de waarde als opgehaald als een geheim.  

De adresseerbare sleutel wordt meer relevante met niet-exporteerbare KV certificaten. Bewerkingen van de adresseerbare KV sleutel vanuit zijn toegewezen *keyusage* veld van het beleid voor KV certificaat gebruikt voor het maken van het certificaat KV.  

Twee typen sleutel worden ondersteund: *RSA* of *RSA HSM* met certificaten. Exporteerbaar is alleen toegestaan met RSA, niet wordt ondersteund door RSA HSM.  

### <a name="certificate-attributes-and-tags"></a>Certificaatkenmerken en -Tags

Naast de certificaatmetagegevens van het, een adresseerbare sleutel en adresseerbare geheim bevat een Key Vault-certificaat ook de kenmerken en tags.  

#### <a name="attributes"></a>Kenmerken

De certificaatkenmerken worden gespiegeld op kenmerken van de adresseerbare sleutel en -geheim gemaakt wanneer KV certificaat wordt gemaakt.  

Een Key Vault-certificaat heeft de volgende kenmerken:  

-   *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Om aan te geven als de gegevens van het certificaat kan worden opgehaald als geheim of bediend als een sleutel kan worden opgegeven. Ook worden gebruikt in combinatie met *nbf* en *exp* wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, alleen worden toegestaan als ingeschakeld is ingesteld op true. Bewerkingen buiten de *nbf* en *exp* venster automatisch zijn niet toegestaan.  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in het antwoord:

-   *gemaakt*: IntDate: geeft aan wanneer deze versie van het certificaat is gemaakt.  
-   *bijgewerkt*: IntDate: geeft aan wanneer deze versie van het certificaat is bijgewerkt.  
-   *exp*: IntDate: bevat de waarde van de vervaldatum van de x509 certificaat.  
-   *nbf*: IntDate: bevat de waarde van de datum van de x509 certificaat.  

> [!Note] 
> Als een Key Vault-certificaat is verlopen, is het beschikbaar maken zonder sleutel en geheim niet meer worden gebruikt.  

#### <a name="tags"></a>Tags

 Client opgegeven woordenlijst met sleutel-waardeparen, vergelijkbaar met de labels in sleutels en geheimen.  

 > [!Note]
> Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor dit objecttype (sleutels, geheimen of certificaten).

### <a name="certificate-policy"></a>Certificaatbeleid

Een certificaatbeleid bevat informatie over het maken en beheren van de levenscyclus van een Key Vault-certificaat. Wanneer een certificaat met persoonlijke sleutel wordt geïmporteerd in de key vault, een standaardbeleid is gemaakt met het lezen van de x509 certificaat.  

Wanneer een Key Vault-certificaat helemaal is gemaakt, wordt een beleid moet worden opgegeven. Het beleid bepaalt hoe u deze versie van Key Vault-certificaat of de volgende versie van de Key Vault-certificaat te maken. Wanneer een beleid is ingesteld, dit is niet vereist met opeenvolgende maakbewerkingen voor toekomstige versies. Er is slechts één exemplaar van een beleid voor alle versies van een Key Vault-certificaat.  

Op hoog niveau bevat een certificaatbeleid de volgende informatie:  

-   Eigenschappen van het X509 certificaat: Bevat de naam van het onderwerp alternatieve namen voor onderwerpen en andere eigenschappen gebruikt voor het maken van een x509 certificaataanvraag.  
-   Sleuteleigenschappen: bevat belangrijke type sleutellengte, kan worden geëxporteerd, en velden voor sleutels opnieuw te gebruiken. Deze velden vertelt u key vault voor het genereren van een sleutel.  
-   Eigenschappen van geheim: eigenschappen van geheim, zoals het inhoudstype van adresseerbare geheim voor het genereren van de geheime waarde voor het ophalen van certificaat als een geheim bevat.  
-   De levensduur van acties: bevat de levensduur van acties voor het certificaat KV. Elke actie voor de levensduur bevat:  

     - Trigger: opgegeven via dagen vóór de vervaldatum of de levensduur span percentage  

     - Actie: op te geven actietype – *emailContacts* of *automatisch verlengen*  

-   Uitgever: Parameters voor de uitgever van het certificaat te gebruiken om certificaten te verlenen x509.  
-   Kenmerken van beleid: bevat kenmerken die zijn gekoppeld aan het beleid  

#### <a name="x509-to-key-vault-usage-mapping"></a>X509 toewijzing voor gebruik van Key Vault

De volgende tabel geeft de toewijzing van x509 sleutelgebruik beleid effectieve sleutelbewerkingen van een sleutel als onderdeel van het maken van een Key Vault-certificaat gemaakt.

|**X509 sleutelgebruik wordt gemarkeerd**|**Key Vault-sleutel ops**|**Standaardgedrag**|
|----------|--------|--------|
|DataEncipherment|versleutelen, ontsleutelen| N/A |
|DecipherOnly|ontsleutelen| N/A  |
|DigitalSignature|Meld u aan, controleert u of| Key Vault standaard zonder een specificatie voor gebruik tijdens de aanmaak van certificaat | 
|EncipherOnly|encrypt| N/A |
|KeyCertSign|Meld u aan, controleert u of|N/A|
|KeyEncipherment|sleutel inpakken, sleutel uitpakken| Key Vault standaard zonder een specificatie voor gebruik tijdens de aanmaak van certificaat | 
|Niet-afwijzing|Meld u aan, controleert u of| N/A |
|crlsign|Meld u aan, controleert u of| N/A |

### <a name="certificate-issuer"></a>Certificaatverlener

Een Key Vault-certificaat-object bevat een configuratie die is gebruikt om te communiceren met een provider van de uitgever geselecteerde certificaat volgorde x509 certificaten.  

-   Key Vault-partners met de volgende certificaatverlener providers voor SSL-certificaten

|**Naam van provider**|**Locaties**|
|----------|--------|
|DigiCert|Ondersteund op alle locaties van sleutelkluis-service in de openbare cloud en Azure Government|
|GlobalSign|Ondersteund op alle locaties van sleutelkluis-service in de openbare cloud en Azure Government|

Voordat de uitgever van een certificaat kan worden gemaakt in een Key Vault, moeten, worden volgende vereiste stappen 1 en 2 bereikt.  

1. Onboarding van Certificaatproviders certificeringsinstantie (CA)  

    -   Beheerder van de organisatie aan boord moet het bedrijf (ex.) Contoso) met ten minste één CA-provider.  

2. De beheerder maakt een aanvrager referenties voor Key Vault te registreren (vernieuwen) SSL-certificaten  

    -   De configuratie die moet worden gebruikt voor het maken van een object van de uitgever van de provider in de key vault biedt  

Zie voor meer informatie over het maken van objecten van certificaatverlener uit de Certificates-portal, de [Key Vault-certificaten-blog](https://aka.ms/kvcertsblog)  

Key Vault kunt u het maken van meerdere verlener objecten met andere verlener-providerconfiguratie. Zodra een verlener-object is gemaakt, kan de naam ervan in een of meerdere beleidsregels van het certificaat worden verwezen. Key Vault configuratie zoals opgegeven in het object verlener gebruiken bij het aanvragen van de x509 verwijzen naar het object verlener geeft certificaten van CA-provider tijdens het maken van het certificaat en de verlenging.  

Objecten van certificaatverlener worden gemaakt in de kluis en kunnen alleen worden gebruikt met KV certificaten in dezelfde kluis.  

### <a name="certificate-contacts"></a>Certificaatcontactpersonen

Certificaatcontactpersonen bevatten informatie voor het verzenden van meldingen die worden geactiveerd door gebeurtenissen voor certificaat-levensduur. De gegevens van contactpersonen wordt gedeeld door alle certificaten in de key vault. Een melding wordt verzonden naar de opgegeven contactpersonen voor een gebeurtenis voor alle certificaten in de key vault.  

Als het beleid van het certificaat is ingesteld op automatische verlenging, wordt op de volgende gebeurtenissen een melding verzonden.  

- Voordat u certificaatvernieuwing
- Na het vernieuwen van een certificaat, waarin wordt vermeld als het certificaat is verlengd of als er een fout optreedt, waarvoor handmatige verlenging van het certificaat.  

  Wanneer een certificaatbeleid of is ingesteld op handmatig worden vernieuwd (alleen voor e-mail), wordt een melding wordt verzonden wanneer het is nu tijd om het certificaat te vernieuwen.  

### <a name="certificate-access-control"></a>Certificaat-toegangsbeheer

 Toegangsbeheer voor certificaten die wordt beheerd door Key Vault en wordt geleverd door de Key Vault waarin deze certificaten. Het beleid voor toegangsbeheer voor certificaten die verschilt van het beleid voor toegangsbeheer voor sleutels en geheimen in de dezelfde Key Vault. Gebruikers kunnen een of meer kluizen voor het opslaan van certificaten, zodat de juiste segmentering scenario en het beheer van certificaten maken.  

 De volgende machtigingen kunnen worden gebruikt, op basis van per-principal, in de access control entry geheimen in een key vault en nauw mirrors de bewerkingen die zijn toegestaan op een geheim object:  

- Machtigingen voor certificate management-bewerkingen
  - *Ophalen*: De huidige certificaatversie van het of een willekeurige versie van een certificaat ophalen 
  - *Lijst met*: De huidige certificaten of versies van een certificaat  
  - *Update*: Een certificaat bijwerken
  - *Maak*: Een Key Vault-certificaat maken
  - *Importeren*: Certificaat materiaal in een Key Vault-certificaat importeren
  - *Verwijder*: Een certificaat, het beleid en alle versies ervan verwijderen  
  - *recover*: Herstellen van een certificaat is verwijderd
  - *backup*: Back-up van een certificaat in een key vault
  - *Herstellen*: Een certificaat van een back-up herstellen naar een key vault
  - *managecontacts*: Key Vault-certificaatcontactpersonen beheren  
  - *manageissuers*: Key Vault instanties/certificaatverleners beheren
  - *getissuers*: Ophalen van een certificaat instanties/uitgevers van certificaten
  - *listissuers*: Lijst van een certificaat instanties/uitgevers van certificaten  
  - *setissuers*: Maken of bijwerken van een Key Vault-certificaat instanties/uitgevers van certificaten  
  - *deleteissuers*: Verwijderen van een Key Vault-certificaat instanties/uitgevers van certificaten  
 
- Machtigingen voor beschermde bewerkingen
  - *purge*: Verwijderen (definitief verwijderen) een certificaat is verwijderd

Zie voor meer informatie de [certificaatbewerkingen in de Key Vault REST API-verwijzing](/rest/api/keyvault). Zie voor informatie over het tot stand brengen van machtigingen, [kluizen: maken of bijwerken](/rest/api/keyvault/vaults/createorupdate) en [kluizen: Update-toegangsbeleid](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="azure-storage-account-key-management"></a>Azure Storage-account-Sleutelbeheer

Key Vault kunnen Azure storage-accountsleutels worden beheerd:

- Key Vault kan intern (sync) sleutels met een Azure storage-account weergeven. 
- Key Vault wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
- Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.
- Key Vault beheren sleutels van opslagaccounts en klassieke opslagaccounts

Zie voor meer informatie, [Azure Key Vault-Opslagaccountsleutels](key-vault-ovw-storage-keys.md)

### <a name="storage-account-access-control"></a>Toegangsbeheer voor Storage-account

De volgende machtigingen kunnen worden gebruikt wanneer een gebruiker of de principal-toepassing autoriseren bewerkingen op een beheerde storage-account uit te voeren:  

- Machtigingen voor een beheerd opslagaccount en SaS-definition-bewerkingen
  - *Ophalen*: Hiermee haalt u informatie over een storage-account 
  - *Lijst met*: Lijst met opslagaccounts beheerd door een Key Vault
  - *Update*: Een storage-account bijwerken
  - *Verwijder*: Een opslagaccount verwijderen  
  - *recover*: Herstellen van een verwijderd opslagaccount
  - *backup*: Back-up van een storage-account
  - *Herstellen*: Een opslagaccount van een back-up herstellen naar een Key Vault
  - *Stel*: Maken of bijwerken van een storage-account
  - *regeneratekey*: Opnieuw genereren van een opgegeven sleutelwaarde voor een opslagaccount
  - *getsas*: Informatie over de definitie van een SAS voor een opslagaccount ophalen
  - *listsas*: Lijst met opslag SAS-definities voor een opslagaccount
  - *deletesas*: Een SAS-definitie van een storage-account verwijderen
  - *setsas*: Maken of bijwerken van een nieuwe SAS-definitie/kenmerken voor een opslagaccount

- Machtigingen voor beschermde bewerkingen
  - *purge*: Verwijderen (definitief verwijderen) een beheerd opslagaccount

Zie voor meer informatie de [opslag accountbewerkingen in de Key Vault REST API-verwijzing](/rest/api/keyvault). Zie voor informatie over het tot stand brengen van machtigingen, [kluizen: maken of bijwerken](/rest/api/keyvault/vaults/createorupdate) en [kluizen: Update-toegangsbeleid](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="see-also"></a>Zie ook

- [Verificatie, vragen en antwoorden](authentication-requests-and-responses.md)
- [Key Vault-versies](key-vault-versions.md)
- [Gids voor Key Vault-ontwikkelaars](/azure/key-vault/key-vault-developers-guide)
