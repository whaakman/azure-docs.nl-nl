---
title: Over sleutels, geheimen en certificaten
description: Overzicht van de REST-interface en KV developer-details
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 2c7dd89d9c2e5d50f2533101499a6e50e52047b3
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="about-keys-secrets-and-certificates"></a>Over sleutels, geheimen en certificaten
Azure Sleutelkluis kunnen gebruikers kunt opslaan en gebruiken van de cryptografische sleutels die binnen de Microsoft Azure-omgeving. Sleutelkluis ondersteunt meerdere sleuteltypen en de algoritmen en maakt het gebruik van Hardware Security Modules (HSM) voor waardevolle sleutels. Bovendien kunnen Sleutelkluis gebruikers veilig opslaan geheimen. Geheimen zijn beperkt grootte octet objecten met geen specifieke semantiek. Sleutelkluis ondersteunt ook de certificaten die zijn ingebouwd in sleutels en geheimen en het toevoegen van een functie voor automatische verlenging.

Zie voor meer algemene informatie over Azure Sleutelkluis [wat is Azure Sleutelkluis?](/azure/key-vault/key-vault-whatis)

**Algemene details Sleutelkluis**

-   [Ondersteunende standaarden](about-keys-secrets-and-certificates.md#BKMK_Standards)
-   [Gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  
-   [Id's en versiebeheer-objecten](about-keys-secrets-and-certificates.md#BKMK_ObjId)  

**Over sleutels**

-   [Sleutels en sleuteltype](about-keys-secrets-and-certificates.md#BKMK_KeyTypes)  
-   [RSA-algoritmen](about-keys-secrets-and-certificates.md#BKMK_RSAAlgorithms)  
-   [RSA-HSM-algoritmen](about-keys-secrets-and-certificates.md#BKMK_RSA-HSMAlgorithms)  
-   [Cryptografische bescherming](about-keys-secrets-and-certificates.md#BKMK_Cryptographic)
-   [Sleutelbewerkingen](about-keys-secrets-and-certificates.md#BKMK_KeyOperations)  
-   [Belangrijke kenmerken](about-keys-secrets-and-certificates.md#BKMK_KeyAttributes)  
-   [Sleutellabels](about-keys-secrets-and-certificates.md#BKMK_Keytags)  

**Over geheimen** 

-   [Werken met geheimen](about-keys-secrets-and-certificates.md#BKMK_WorkingWithSecrets)  
-   [Geheime kenmerken](about-keys-secrets-and-certificates.md#BKMK_SecretAttrs)  
-   [Geheime codes](about-keys-secrets-and-certificates.md#BKMK_SecretTags)  
-   [Geheime toegangsbeheer](about-keys-secrets-and-certificates.md#BKMK_SecretAccessControl)  

**Over certificaten**

-   [Samenstelling van een certificaat](#BKMK_CompositionOfCertificate)  
-   [Certificaat-kenmerken en labels](#BKMK_CertificateAttributesAndTags)  
-   [Certificaatbeleid](#BKMK_CertificatePolicy)  
-   [Certificaatverlener](#BKMK_CertificateIssuer)  
-   [Certificaat contactpersonen](#BKMK_CertificateContacts)  
-   [Certificaat-toegangsbeheer](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Algemene details Sleutelkluis

Algemene informatie die van toepassing zijn bieden de volgende secties tussen de uitvoering van de Azure Sleutelkluis-service.

###  <a name="BKMK_Standards"></a> Ondersteunende standaarden

De notatie JSON (JavaScript Object) en JavaScript-Object ondertekening en versleuteling (JOSE) specificaties zijn belangrijke achtergrondinformatie.  

-   [JSON Web sleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web-versleuteling (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmen (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web handtekening (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> Gegevenstypen

Raadpleeg de [JOSE specificaties](#BKMK_Standards) voor relevante gegevenstypen voor sleutels, versleuteling en ondertekening.  

-   **algoritme** -een ondersteunde algoritme voor een bewerking, bijvoorbeeld RSA1_5  
-   **gecodeerde tekst waarde** -tekst octetten, gecodeerd met Base64URL cipher  
-   **Digest-waarde** -de uitvoer van een hash-algoritme, gecodeerd met Base64URL  
-   **type sleutel** -een van de ondersteunde typen voor sleutels, bijvoorbeeld RSA.  
-   **niet-gecodeerde tekst waarde** -tekst zonder opmaak octetten, gecodeerd met Base64URL  
-   **handtekening-waarde** - uitvoer van een algoritme voor handtekening, gecodeerd met Base64URL  
-   **base64URL** -een Base64URL [RFC4648] gecodeerde binaire waarde  
-   **Booleaanse** -true of false  
-   **Identiteit** : een identiteit van Azure Active Directory (AAD).  
-   **IntDate** - een decimale waarde die aangeeft hoeveel seconden van 1970 van JSON-01-01T0:0:0Z UTC tot en met de opgegeven UTC-datum/tijd. [RFC3339] Zie voor meer informatie over datum/time-out in het algemeen en UTC in het bijzonder.  

###  <a name="BKMK_ObjId"></a> Objecten, id's en versiebeheer voor onderdelen

Objecten die zijn opgeslagen in Azure Key Vault behouden versies wanneer een nieuw exemplaar van een object wordt gemaakt, en elke versie een unieke id en een URL heeft. Wanneer een object wordt gemaakt, krijgt een unieke id en is gemarkeerd als de huidige versie van het object. Maken van een nieuw exemplaar met dezelfde objectnaam geeft het nieuwe object een unieke id en ervoor zorgt dat de huidige versie.  

Objecten in Azure Key Vault kunnen worden aangepakt met behulp van de huidige id of een specifiek voor een versie-id. Bijvoorbeeld, uitgaande van een sleutel met de naam 'Hoofdsleutel', uitvoeren van bewerkingen met de huidige id zorgt ervoor dat het systeem de meest recente versie te gebruiken. Uitvoeren van bewerkingen met de versie-specifieke id zorgt ervoor dat het systeem die specifieke versie van het object te gebruiken.  

Objecten zijn uniek aangeduid binnen Azure Key Vault met behulp van een URL dat geen twee objecten in het systeem, ongeacht de geografische locatie, dezelfde URL hebben. De volledige URL naar een object wordt aangeduid met de Object-id en bestaat uit een voorvoegsel waarmee de Sleutelkluis, het objecttype, een gebruiker objectnaam en de versie van een Object opgegeven. Naam van het Object is niet hoofdlettergevoelig en niet-wijzigbaar. Id's die de versie van het Object niet opnemen worden als Base-id's aangeduid.  

Zie voor meer informatie [verificatie, aanvragen en antwoorden](authentication-requests-and-responses.md)

Een object-id heeft de volgende algemene indeling:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Waar:  

|||  
|-|-|  
|`keyvault-name`|De naam van een sleutelkluis in de Microsoft Azure Sleutelkluis-service.<br /><br /> Sleutelkluis-namen zijn geselecteerd door de gebruiker en globaal uniek zijn.<br /><br /> Naam van de Sleutelkluis moet een lengte die alleen string 3 tot 24 tekens (0-9, a-z, A-Z, en -).|  
|`object-type`|Het type van het object 'sleutels' of 'geheimen'.|  
|`object-name`|Een `object-name` is opgegeven voor een gebruikersnaam en moet uniek zijn binnen een Sleutelkluis. De naam moet een tekenreeks van 1-127 tekens mag bevatten alleen 0-9, a-z, A-Z, en -.|  
|`object-version`|Een `object-version` is een systeem gegenereerde, tekenreeks-id van 32 tekens die eventueel wordt gebruikt om een unieke versie van een object.|  

## <a name="key-vault-keys"></a>Sleutelkluis sleutels

###  <a name="BKMK_KeyTypes"></a> Sleutels en sleuteltype

Cryptografische sleutels in Azure Key Vault worden weergegeven als JSON Web sleutel [JWK]-objecten. De base JWK/JWA specificaties ook worden uitgebreid zodat sleuteltypen uniek is voor de implementatie van Azure Sleutelkluis, bijvoorbeeld het importeren van sleutels voor Azure Sleutelkluis gebruik het specifieke inpakken van HSM-leverancier (Thales) voor het inschakelen van beveiligde vervoer van deze sleutels die ze kunnen alleen worden gebruikt in de Azure Key Vault HSM's.  

De initiële versie van Azure Sleutelkluis ondersteunt alleen; RSA-sleutels andere typen sleutels zoals symmetrische en elliptic curve kunnen ondersteuning voor toekomstige releases.  

-   **RSA**: een 2048-bits RSA-sleutel. Dit is een 'soft' sleutel die in de software wordt verwerkt door de Sleutelkluis, maar worden versleuteld in rust met behulp van een systeemsleutel die in een HSM is opgeslagen. Clients kunnen importeren van een bestaande RSA-sleutel maken of om Azure Sleutelkluis een genereren.  
-   **RSA-HSM**: een RSA-sleutel die in een HSM is verwerkt. RSA-HSM-sleutels worden beveiligd in een van de Azure Key Vault HSM-Beveiligingswerelden (Er is een Beveiligingswereld per Geografie isolatie onderhouden). Clients kunnen importeren van een RSA-sleutel in zachte formulier of door het exporteren van een compatibel apparaat van de HSM maken of om Azure Sleutelkluis een genereren. Dit sleuteltype wordt het kenmerk T toegevoegd aan de JWK verkrijgen voor het uitvoeren van de HSM-sleutelmateriaal.  

     Zie voor meer informatie over geografische grenzen [Microsoft Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/privacy/)  

###  <a name="BKMK_RSAAlgorithms"></a> RSA-algoritmen  
 De volgende algoritme-id's worden ondersteund met de RSA-sleutels in Azure Sleutelkluis.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY VERSLEUTELEN/ONTSLEUTELEN

-   **RSA1_5** -sleutelcodering RSAES PKCS1 V1_5 [RFC3447]  
-   **RSA OAEP** - RSAES met optimale asymmetrische versleuteling opvulling (OAEP) [RFC3447] met de standaardparameters die is opgegeven door RFC 3447 in de sectie A.2.1. Deze standaardparameters gebruikmaakt van een hash-functie van SHA-1 en een functie van de generatie masker van MGF1 met SHA-1.  

#### <a name="signverify"></a>AANMELDING/CONTROLEREN

-   **RS256** - RSASSA-PKCS-v1_5 met SHA-256. De opgegeven toepassing digest-waarde moet worden berekend met SHA-256 en moet 32 bytes lang.  
-   **RS384** - RSASSA-PKCS-v1_5 met SHA-384. De opgegeven toepassing digest-waarde moet worden berekend met behulp van SHA-384 en moet 48 bytes lang.  
-   **RS512** - RSASSA-PKCS-v1_5 met SHA-512. De opgegeven toepassing digest-waarde moet worden berekend met behulp van SHA-512 en moet 64 bytes lang.  
-   **RSNULL** -Zie [RFC2437] een gespecialiseerde gebruiksvoorbeeld bepaalde TLS-scenario's te maken.  

###  <a name="BKMK_RSA-HSMAlgorithms"></a> RSA-HSM-algoritmen  
De volgende algoritme-id's worden ondersteund met de RSA-HSM-sleutels in Azure Sleutelkluis.  

### <a name="BKMK_Cryptographic"></a> Cryptografische bescherming

De cryptografische modules die gebruikmaakt van Azure Sleutelkluis, of HSM of software, FIPS-gevalideerde zijn. U hoeft niet in de FIPS-modus uit te voeren speciale niets te doen. Als u **maken** of **importeren** sleutels als beveiligd met HSM, gegarandeerd ze worden verwerkt in HSM's die worden gevalideerd door de FIPS 140-2 Level 2 of hoger. Als u **maken** of **importeren** sleutels als softwarebeveiliging en ze worden verwerkt binnen cryptografiemodules gevalideerd door de FIPS 140-2-niveau 1 of hoger. Zie voor meer informatie [sleutels en sleuteltypen](about-keys-secrets-and-certificates.md#BKMK_KeyTypes).

#### <a name="wrapunwrap-encryptdecrypt"></a>TEKSTTERUGLOOP/UITPAKKEN, VERSLEUTELEN/ONTSLEUTELEN

-   **RSA1_5** -sleutelcodering RSAES PKCS1 V1_5 [RFC3447].  
-   **RSA OAEP** - RSAES met optimale asymmetrische versleuteling opvulling (OAEP) [RFC3447] met de standaardparameters die is opgegeven door RFC 3447 in de sectie A.2.1. Deze standaardparameters gebruikmaakt van een hash-functie van SHA-1 en een functie van de generatie masker van MGF1 met SHA-1.  

 #### <a name="signverify"></a>AANMELDING/CONTROLEREN  

-   **RS256** - RSASSA-PKCS-v1_5 met SHA-256. De opgegeven toepassing digest-waarde moet worden berekend met SHA-256 en moet 32 bytes lang.  
-   **RS384** - RSASSA-PKCS-v1_5 met SHA-384. De opgegeven toepassing digest-waarde moet worden berekend met behulp van SHA-384 en moet 48 bytes lang.  
-   **RS512** - RSASSA-PKCS-v1_5 met SHA-512. De opgegeven toepassing digest-waarde moet worden berekend met behulp van SHA-512 en moet 64 bytes lang.  
-   RSNULL: Zie [RFC2437] een gespecialiseerde gebruiksvoorbeeld bepaalde TLS-scenario's te maken.  

###  <a name="BKMK_KeyOperations"></a> Sleutelbewerkingen

Azure Sleutelkluis ondersteunt de volgende bewerkingen voor sleutels objecten:  

-   **Maak**: kunt u een client een sleutel te maken in Azure Sleutelkluis. De waarde van de sleutel wordt gegenereerd door Azure Sleutelkluis en opgeslagen en wordt niet vrijgegeven voor de client. Asymmetrische (en in de toekomst elliptische en Symmetric) sleutels kunnen worden gemaakt in Azure Sleutelkluis.  
-   **Importeren**: kunt u een client voor het importeren van een bestaande sleutel naar Azure Sleutelkluis. Asymmetrische (en in de toekomst elliptische en Symmetric) sleutels voor Azure Sleutelkluis een aantal methoden die andere pakketten in een constructie JWK kunnen worden geïmporteerd.  
-   **Update**: kunt u een client met voldoende machtigingen voor het wijzigen van de metagegevens (sleutelkenmerken) gekoppeld aan een sleutel die eerder zijn opgeslagen in Azure Sleutelkluis.  
-   **Verwijderen**: kunt u een client met voldoende machtigingen voor het verwijderen van een sleutel uit Azure Sleutelkluis.  
-   **Lijst**: kunt u een client voor een lijst met alle sleutels in een bepaalde Azure Sleutelkluis.  
-   **Versies**: kunt u een client voor een lijst met alle versies van een opgegeven sleutel in een bepaalde Azure Sleutelkluis.  
-   **Ophalen van**: kunt u een client voor het ophalen van de openbare onderdelen van een opgegeven sleutel in een Azure Sleutelkluis.  
-   **Back-up**: exporteert een sleutel in een beveiligde vorm.  
-   **Herstellen**: een back-up sleutel geïmporteerd.  

Zie voor meer informatie [operations sleutel](/rest/api/keyvault/key-operations.md)  

Wanneer een sleutel is gemaakt in Azure Sleutelkluis, kunnen de volgende cryptografische bewerkingen worden uitgevoerd met behulp van de sleutel:  

-   **Aanmelding en controleer of**: strikt, deze bewerking wordt 'aanmelding hash' of 'controle hash' zoals Azure Key Vault biedt geen ondersteuning voor hashing van inhoud als onderdeel van het maken van de handtekening. De gegevens die lokaal zijn ondertekend en vervolgens aanvragen dat Azure Sleutelkluis-teken de hash moeten hash-toepassingen. Verificatie van de ondertekende hashes wordt ondersteund als een bewerking gemak voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; We raden voor optimale toepassingsprestaties, controleert u of bewerkingen lokaal worden uitgevoerd.  
-   **Codering met een sleutel / Wrapping**: een sleutel die is opgeslagen in Azure Key Vault kan worden gebruikt om een andere sleutel, meestal een symmetrische inhoud versleutelingssleutel (CEK) te beveiligen. Wanneer de sleutel in Azure Key Vault asymmetrische, sleutelcodering wordt gebruikt, bijvoorbeeld RSA OAEP en de bewerkingen WRAPKEY/UNWRAPKEY gelijk zijn aan VERSLEUTELEN/ONTSLEUTELEN. Wanneer de sleutel in Azure Key Vault symmetrische is, wordt de sleutel wrapping gebruikt; bijvoorbeeld AES KW. De bewerking WRAPKEY wordt ondersteund voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; gemakkelijker te maken het wordt aanbevolen dat voor de beste toepassingsprestaties, WRAPKEY bewerkingen worden uitgevoerd lokaal.  
-   **Versleutelen en ontsleutelen van**: een sleutel die wordt opgeslagen in Azure Key Vault kan worden gebruikt voor het versleutelen of ontsleutelen van gegevens, de grootte van die wordt bepaald door het type en sleutel geselecteerde versleutelingsalgoritme één blok. De bewerking versleutelen is voor uw gemak geleverd voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; Het verdient aanbeveling dat voor de beste toepassingsprestaties, versleutelen operations lokaal worden uitgevoerd.  

Terwijl WRAPKEY/UNWRAPKEY met asymmetrische sleutels lijkt overbodig zijn als de bewerking gelijk aan het VERSLEUTELEN/ONTSLEUTELEN is, het gebruik van verschillende bewerkingen wordt beschouwd als belangrijk om te zorgen semantische en autorisatie scheiding van deze bewerkingen en consistentie Wanneer andere typen sleutels worden ondersteund door de service.  

Bewerkingen EXPORT biedt geen ondersteuning voor Azure Sleutelkluis: zodra een sleutel is geconfigureerd in het systeem deze kan niet worden opgehaald wordt geplaatst of de sleutelmateriaal gewijzigd.  Echter gebruikers van Azure Sleutelkluis waarvoor de sleutel voor andere gebruiksvoorbeelden of nadat deze is verwijderd in Azure Key Vault hebben, kunnen de BACKUP and RESTORE operations gebruiken om te exporteren/importeren van de sleutel in een beveiligde vorm. Sleutels die zijn gemaakt door de back-up zijn niet kan worden gebruikt buiten Azure Sleutelkluis. De importbewerking kan ook worden gebruikt met meerdere exemplaren van Azure Sleutelkluis.  

Gebruikers kunnen een van de cryptografische bewerkingen die ondersteuning biedt voor Azure Sleutelkluis op basis van per sleutel met de eigenschap key_ops van het object JWK beperken.  

Zie voor meer informatie over JWK objecten [JSON Web sleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Belangrijke kenmerken

Naast de sleutelmateriaal opslaat, kunnen de volgende kenmerken worden opgegeven. In een JSON-aanvraag zijn de kenmerken sleutelwoord en de accolades, ' {' '}', zijn vereist, zelfs als er geen kenmerken opgegeven zijn.  

- *ingeschakeld*: boolean, optioneel, de standaardwaarde is **true**. Hiermee geeft u op of de sleutel ingeschakeld en bruikbaar voor cryptografische bewerkingen is. De *ingeschakeld* kenmerk wordt gebruikt in combinatie met *nbf* en *exp*. Wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, wordt alleen worden toegestaan als *ingeschakeld* is ingesteld op **true**. Bewerkingen buiten de *nbf* / *exp* venster worden automatisch niet toegestaan, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops).
- *NBF*: IntDate, optioneel, standaard is nu. De *nbf* (niet voor) kenmerk identificeert de tijd dat moet de sleutel niet worden gebruikt voor cryptografische bewerkingen, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). De verwerking van de *nbf* kenmerk is vereist dat de huidige datum en tijd moeten na of gelijk aan het niet-vóór de datum/tijd die worden vermeld in de *nbf* kenmerk. Azure Sleutelkluis kan bieden voor een aantal kleine eenheidsprofiel meestal niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *EXP*: IntDate, optioneel, standaard 'permanent' is. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na waarvoor de sleutel moet niet worden gebruikt voor cryptografische bewerking, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). De verwerking van de *exp* kenmerk is vereist dat de huidige datum en tijd zijn moet voordat de vervaldatum van de datum/tijd die worden vermeld in de *exp* kenmerk. Azure Sleutelkluis kan bieden voor een aantal kleine eenheidsprofiel meestal niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  

Er zijn extra kenmerken voor alleen-lezen die zijn opgenomen in een antwoord met de belangrijkste kenmerken:  

- *gemaakt*: IntDate, optioneel. De *gemaakt* kenmerk geeft aan wanneer deze versie van de sleutel is gemaakt. Deze waarde is null voor sleutels die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De *bijgewerkt* kenmerk geeft aan wanneer deze versie van de sleutel is bijgewerkt. Deze waarde is null voor sleutels die het laatst zijn bijgewerkt voor het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  

Zie voor meer informatie over IntDate en andere gegevenstypen [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Datum / tijd-bewerkingen worden beheerd

Niet nog geldig zijn en verlopen sleutels, die buiten de *nbf* / *exp* venster werkt voor **ontsleutelen**, **uitpakken** en **controleren** operations (403, niet als resultaat verboden). De logica voor het gebruik van de status niet nog geldig is om een sleutel moet worden getest voordat gebruik in productieomgevingen. De logica voor het gebruik van de verlopen staat is om herstelbewerkingen op gegevens die zijn gemaakt wanneer de sleutel ongeldig is. U kunt ook toegang tot een sleutel met behulp van Sleutelkluis beleid of door bij te werken uitschakelen de *ingeschakeld* sleutelkenmerk naar **false**.

Zie voor meer informatie over gegevens typen, [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes).

Zie voor meer informatie over andere mogelijke kenmerken de [JSON Web sleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Sleutellabels

U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van labels. Azure Sleutelkluis ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde van 256 tekens zijn.  

>[!Note]
>Labels kunnen worden gelezen door de aanroeper als ze hebben de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

###  <a name="BKMK_KeyAccessControl"></a> Beheer van toegang tot de sleutel

Toegangsbeheer voor sleutels die worden beheerd door de Sleutelkluis is opgegeven op het niveau van een Sleutelkluis die als de container van sleutels fungeert. Er is een beleid voor toegangsbeheer voor sleutels die verschilt van het beleid voor toegangsbeheer voor geheimen in de kluis met dezelfde sleutel. Gebruikers kunnen een of meer kluizen voor het opslaan van sleutels maken en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van sleutels. Toegangsbeheer voor sleutels is onafhankelijk van toegangsbeheer voor geheimen.  

De volgende machtigingen kunnen worden verleend, op een per gebruiker / service-principal basis, in de access control entry sleutels op een kluis. Deze machtigingen nauw het spiegelen van de bewerkingen die zijn toegestaan op een hoofdobject:  

-   *Maak*: nieuwe sleutels maken
-   *ophalen van*: het openbare deel van een sleutel, plus de kenmerken lezen
-   *lijst*: lijst van de sleutels of versies van een sleutel die wordt opgeslagen in een sleutelkluis
-   *bijwerken*: de kenmerken voor een sleutel bijwerken
-   *verwijderen*: het sleutel-object verwijderen
-   *Meld u*: Gebruik de sleutel voor het ondertekenen van verwerkingen
-   *Controleer of*: de sleutel gebruiken om te controleren of verwerkingen
-   *wrapKey*: de sleutel gebruiken om een symmetrische sleutel te beveiligen
-   *unwrapKey*: de sleutel gebruiken om op te heffen ingepakt symmetrische sleutels
-   *versleutelen*: de sleutel gebruiken om te beveiligen van een willekeurige volgorde van bytes
-   *ontsleutelen*: de sleutel gebruiken om op te heffen van een reeks bytes
-   *importeren*: een sleutel importeren naar een sleutelkluis
-   *back-*: back-up van een sleutel in een sleutelkluis
-   *herstellen*: herstel een reservekopie van sleutel naar een sleutelkluis
-   *alle*: alle machtigingen

## <a name="key-vault-secrets"></a>Sleutelkluis geheimen 

###  <a name="BKMK_WorkingWithSecrets"></a> Werken met geheimen

Geheimen in Azure Key Vault zijn octet reeksen met een maximale grootte van 25 k bytes. De Azure Sleutelkluis-service biedt geen een semantiek voor geheimen; deze alleen de gegevens accepteert, versleutelt en slaat het retourneren van een geheime 'id', die kan worden gebruikt voor het ophalen van het geheim op een later tijdstip-id.  

Voor zeer gevoelige gegevens clients moeten rekening houden met extra beveiligingslagen voor gegevens die zijn opgeslagen in Azure Key Vault; bijvoorbeeld vooraf door gegevens te coderen met behulp van een afzonderlijke beveiliging-sleutel.  

Azure Sleutelkluis ondersteunt ook het veld contentType voor geheimen. Clients kunnen het inhoudstype, 'contentType' van een geheim om te helpen bij het interpreteren van de geheime gegevens wanneer deze wordt opgehaald opgeven. De maximumlengte van dit veld is 255 tekens. Er zijn geen vooraf gedefinieerde waarden. Het voorgestelde gebruik is als een hint op voor het interpreteren van de geheime gegevens. Een implementatie kan bijvoorbeeld zowel wachtwoorden en certificaten opslaan als geheimen vervolgens dit veld gebruiken om aan te geven die. Er zijn geen vooraf gedefinieerde waarden.  

###  <a name="BKMK_SecretAttrs"></a> Geheime kenmerken

Naast de geheime gegevens kunnen de volgende kenmerken worden opgegeven:  

- *EXP*: IntDate, optioneel, standaard is **permanent**. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na die de geheime gegevens mogen niet worden opgehaald, behalve in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). De verwerking van de *exp* kenmerk is vereist dat de huidige datum en tijd zijn moet voordat de vervaldatum van de datum/tijd die worden vermeld in de *exp* kenmerk. Azure Sleutelkluis kan bieden voor een aantal kleine eenheidsprofiel meestal niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *NBF*: IntDate, optioneel, standaard is **nu**. De *nbf* (niet voor) kenmerk identificeert de tijd die de geheime gegevens moet niet worden opgehaald, behalve in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). De verwerking van de *nbf* kenmerk is vereist dat de huidige datum en tijd moeten na of gelijk aan het niet-vóór de datum/tijd die worden vermeld in de *nbf* kenmerk. Azure Sleutelkluis kan bieden voor een aantal kleine eenheidsprofiel meestal niet meer dan een paar minuten, ter compensatie van klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *ingeschakeld*: boolean, optioneel, de standaardwaarde is **true**. Dit kenmerk geeft aan of de geheime gegevens kunnen worden opgehaald. Het kenmerk enabled wordt gebruikt in combinatie met en *exp* wanneer een bewerking plaatsvindt tussen exp, deze alleen worden toegestaan als ingeschakeld is ingesteld op **true**. Bewerkingen buiten de *nbf* en *exp* venster automatisch zijn niet toegestaan, behalve in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops).  

Er zijn extra kenmerken voor alleen-lezen die zijn opgenomen in een antwoord dat geheime kenmerken bevat:  

- *gemaakt*: IntDate, optioneel. De gemaakte kenmerk geeft aan wanneer deze versie van het geheim is gemaakt. Deze waarde is null voor geheimen die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De bijgewerkte kenmerk geeft aan wanneer deze versie van het geheim is bijgewerkt. Deze waarde is null voor geheimen die vóór het toevoegen van dit kenmerk laatst zijn bijgewerkt. De waarde moet een getal met een IntDate-waarde.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Datum / tijd-bewerkingen worden beheerd

Een geheim **ophalen** -bewerking werkt voor verlopen en niet nog geldig geheimen, buiten de *nbf* / *exp* venster. Het aanroepen van een geheim **ophalen** bewerking, voor een niet nog geldig geheim kan worden gebruikt voor testdoeleinden. Bij het ophalen van (**ophalen**ing) een verlopen geheim kunnen worden gebruikt voor herstelbewerkingen.

Zie voor meer informatie over gegevens typen, [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Geheime toegangsbeheer

Toegangsbeheer voor geheimen in Azure Key Vault beheerd is op het niveau van een Sleutelkluis die als de container van deze geheimen fungeert opgegeven. Er is een beleid voor toegangsbeheer voor geheimen die verschilt van het beleid voor toegangsbeheer voor sleutels in de kluis met dezelfde sleutel. Gebruikers kunnen maken van een of meer kluizen geheimen kan bevatten en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van geheimen. Toegangsbeheer voor geheimen zijn onafhankelijk van toegangsbeheer voor sleutels.  

De volgende machtigingen kunnen worden gebruikt, op basis van per principal, in de access control entry geheimen in een kluis en nauw het spiegelen van de bewerkingen die zijn toegestaan op een geheim object:  

-   *Stel*: nieuwe geheimen maken  
-   *ophalen van*: een geheim lezen  
-   *lijst*: de geheimen of versies van een geheim dat is opgeslagen in een Sleutelkluis  
-   *verwijderen*: het geheim verwijderen  
-   *alle*: alle machtigingen  

Zie voor meer informatie over het werken met geheimen [geheime bewerkingen](/rest/api/keyvault/secret-operations.md).  

###  <a name="BKMK_SecretTags"></a> Geheime codes  
U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van labels. Azure Sleutelkluis ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde van 256 tekens zijn.  

>[!Note]
>Labels kunnen worden gelezen door de aanroeper als ze hebben de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

## <a name="key-vault-certificates"></a>Sleutelkluis-certificaten

Sleutelkluis certificaten ondersteuning biedt voor het beheren van uw x509 certificaten en de volgende problemen:  

-   Kan de eigenaar van een certificaat om een certificaat via een proces voor het maken van Sleutelkluis of het importeren van een bestaand certificaat te maken. Dit omvat zowel zelfondertekend en de certificeringsinstantie die certificaten worden gegenereerd.
-   Hiermee kunt u de eigenaar van een Sleutelkluis-certificaat voor het implementeren van veilige opslag en beheer van X509 certificaten zonder interactie met privésleutelmateriaal.  
-   Kan de eigenaar van een certificaat om een beleid waarin wordt verwezen Sleutelkluis voor het beheren van de levenscyclus van een certificaat te maken.  
-   Kan certificaat eigenaren van contactgegevens voor melding over de levenscyclus van gebeurtenissen van verlopen en verlenging van het certificaat opgeven.  
-   Ondersteunt automatische vernieuwing met geselecteerde verleners - Sleutelkluis partner X509 providers van het certificaat / certificeringsinstanties.

>[!Note]
>Providers/instanties niet samenwerking ook zijn toegestaan, maar geen ondersteuning voor de functie voor automatisch vernieuwen.

###  <a name="BKMK_CompositionOfCertificate"></a> Samenstelling van een certificaat

Wanneer een Sleutelkluis-certificaat is gemaakt, worden ook een adresseerbare sleutel en geheime gemaakt met dezelfde naam. De Sleutelkluis-sleutel kan bewerkingen met sleutels en het geheim Sleutelkluis kan als een geheim ophalen van de waarde van het certificaat. Een Sleutelkluis-certificaat bevat ook openbare x509 metagegevens van het certificaat.  

De id en versie van certificaten is vergelijkbaar met die van sleutels en geheimen. Een specifieke versie van een adresseerbare sleutel en een geheim dat is gemaakt met de versie van de certificaat Sleutelkluis is beschikbaar in het antwoord Sleutelkluis-certificaat.
 
![Cetificates zijn complexe objecten](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Sleutel exporteerbaar of niet exporteerbaar

Wanneer een Sleutelkluis-certificaat is gemaakt, kan het worden opgehaald uit het adresseerbare geheim met de persoonlijke sleutel in PFX of PEM-indeling als het beleid dat wordt gebruikt voor het maken van het certificaat wordt aangegeven dat de sleutel exporteerbaar is. Het beleid dat wordt gebruikt voor het maken van het certificaat van de Sleutelkluis aangegeven voor de sleutel worden niet geëxporteerd, klikt u vervolgens is de persoonlijke sleutel niet als een deel van de waarde moet worden opgehaald als een geheim.  

De adresseerbare sleutel wordt meer relevante met niet-exporteerbare KV certificaten. Bewerkingen van de adresseerbare KV sleutel vanuit zijn toegewezen *keyusage* veld van het certificaatbeleid KV is gebruikt voor het maken van het certificaat KV.  

Twee soorten sleutel worden ondersteund: *RSA* of *RSA HSM* met certificaten. Exporteerbaar is alleen toegestaan met RSA, niet wordt ondersteund door RSA HSM.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Certificaat-kenmerken en labels

Naast de certificaatmetagegevens van het, een adresseerbare sleutel en een adresseerbare geheim bevat een Sleutelkluis certificaat ook kenmerken en labels.  

#### <a name="attributes"></a>Kenmerken

De kenmerken van het certificaat wordt gespiegeld in kenmerken van de adresseerbare sleutel en geheime gemaakt wanneer KV certificaat wordt gemaakt.  

Een Sleutelkluis-certificaat heeft de volgende kenmerken:  

-   *ingeschakeld*: boolean, optioneel, de standaardwaarde is **true**. Dit kenmerk kan worden opgegeven om aan te geven als de gegevens van het certificaat kunnen worden opgehaald als geheime of bediend als sleutel. Dit wordt gebruikt in combinatie met *nbf* en*exp*wanneer een bewerking plaatsvindt tussen *nbf* exp, deze alleen worden toegestaan als ingeschakeld is ingesteld op true. Bewerkingen buiten de *nbf* en*exp*venster automatisch zijn niet toegestaan.  

Er zijn extra kenmerken voor alleen-lezen die zijn opgenomen in het antwoord:

-   *gemaakt*: IntDate: geeft aan wanneer deze versie van het certificaat is gemaakt.  
-   *bijgewerkt*: IntDate: Hiermee wordt aangegeven wanneer deze versie van het certificaat is bijgewerkt.  
-   *EXP*: IntDate: bevat de waarde van de vervaldatum van de x509 certificaat.  
-   *NBF*: IntDate: bevat de waarde van de datum van de x509 certificaat.  

> [!Note] 
> Als een Sleutelkluis-certificaat is verlopen, is het adresseerbare sleutel en geheime niet meer worden gebruikt.  

#### <a name="tags"></a>Tags

 Client opgegeven woordenlijst van sleutelwaarde-paren, vergelijkbaar met de labels in de sleutels en geheimen.  

 > [!Note]
> Labels kunnen worden gelezen door de aanroeper als ze hebben de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

###  <a name="BKMK_CertificatePolicy"></a> Certificaatbeleid

Een certificaatbeleid bevat informatie over het maken en beheren van de levenscyclus van een certificaat KV. Wanneer een certificaat met persoonlijke sleutel worden geïmporteerd in de sleutelkluis, een standaardbeleid gemaakt door te lezen van de x509 certificaat.  

Wanneer een certificaat KV vanaf het begin is gemaakt, moet een beleid voor Sleutelkluis deze informeren over het maken van deze versie van het certificaat KV of de volgende versie van KV certificaat worden opgegeven. Wanneer een beleid is ingesteld, is het niet vereist met opeenvolgende bewerkingen voor het maken van de volgende versies van KV certificaat maken.  

Er is slechts één exemplaar van een beleid voor alle versies van een certificaat KV.  

Een certificaatbeleid bevat op een hoog niveau het volgende:  

-   X509-eigenschappen voor certificaat: bevat de onderwerpnaam van het, alternatieve onderwerpnamen enzovoort. Hiermee maakt u een x509 certificaataanvraag.  
-   Sleuteleigenschappen: bevat belangrijke type sleutellengte, geëxporteerd en velden opnieuw te gebruiken. Deze velden instrueren sleutelkluis over het maken van een sleutel.  
-   Eigenschappen van de geheime: bevat geheime eigenschappen zoals het inhoudstype van adresseerbare geheim voor het genereren van de geheime waarde, voor het certificaat als een geheim ophalen.  
-   Levensduur acties: bevat de levensduur van acties voor het certificaat KV. Elke actie levensduur bevat:  

     - Trigger: opgegeven via dagen vóór het verlopen of de levensduur span percentage  

     - Actie: geven actietype – *emailContacts* of *autoRenew*  

-   Uitgever: De Parameters van over de uitgever van het certificaat moet worden gebruikt om certificaten te verlenen x509.  
-   Beleid kenmerken: bevat kenmerken die zijn gekoppeld aan het beleid  

#### <a name="x509-to-key-vault-usage-mapping"></a>X509 toewijzing van de Sleutelkluis-gebruik

De volgende tabel geeft de toewijzing van x509 sleutelgebruik beleid effectieve belangrijkste activiteiten van een sleutel die is gemaakt als onderdeel van een Sleutelkluis certificaat maken.

|**X509 sleutelgebruik vlaggen**|**Sleutelkluis sleutels ops**|**Standaardgedrag**|
|----------|--------|--------|
|DataEncipherment|versleutelen, ontsleutelen| N/A |
|DecipherOnly|Ontsleutelen| N/A  |
|DigitalSignature|Meld u aan, controleert u of| Sleutelkluis standaard zonder een specificatie voor gebruik tijdens de aanmaak van certificaat | 
|EncipherOnly|encrypt| N/A |
|KeyCertSign|Meld u aan, controleert u of|N/A|
|KeyEncipherment|wrapKey, unwrapKey| Sleutelkluis standaard zonder een specificatie voor gebruik tijdens de aanmaak van certificaat | 
|Niet-afwijzing|Meld u aan, controleert u of| N/A |
|crlsign|Meld u aan, controleert u of| N/A |

###  <a name="BKMK_CertificateIssuer"></a> Certificaatverlener

Een Sleutelkluis certificaat-object bevat een configuratie die wordt gebruikt om te communiceren met een provider van de verlener geselecteerd certificaat volgorde x509 certificaten.  

-   Sleutelkluis partners met de volgende certificaatproviders uitgever voor SSL-certificaten

|**Providernaam**|**Locaties**|
|----------|--------|
|DigiCert|Ondersteund op alle locaties van sleutelkluis-service in de openbare cloud en Azure Government|
|GlobalSign|Ondersteund op alle locaties van sleutelkluis-service in de openbare cloud en Azure Government|

Voordat de certificaatuitgever van een kan worden gemaakt in een Sleutelkluis, moeten, worden volgende vereiste stappen 1 en 2 uitgevoerd.  

1. Onboarding van Certificaatproviders (certificeringsinstantie)  

    -   Een beheerder van de organisatie moet ingebouwde hun bedrijf (ex. Contoso) met ten minste één CA-provider.  

2. De beheerder maakt een aanvrager referenties voor Sleutelkluis te registreren (en verlengen) SSL-certificaten  

    -   Voorziet in de configuratie moet worden gebruikt voor het maken van een object van de verlener van de provider in de sleutelkluis  

Zie voor meer informatie over het maken van objecten van de certificaatverlener uit de Certificates portal de [Sleutelkluis certificaten blog](http://aka.ms/kvcertsblog)  

Sleutelkluis kunt voor het maken van meerdere verlener objecten met verschillende verlener provider configureren. Zodra een verlener-object is gemaakt, kan de naam ervan in een of meerdere certificaatbeleid worden verwezen. Verwijzen naar het object verlener Hiermee geeft u de Sleutelkluis configuratie zoals opgegeven in het object verlener gebruiken bij het aanvragen van de x509 certificaat van de CA-provider tijdens het maken van het certificaat en verlenging.  

Certificaatverlener objecten worden gemaakt in de kluis en kunnen alleen worden gebruikt met KV certificaten in dezelfde kluis.  

###  <a name="BKMK_CertificateContacts"></a> Certificaat contactpersonen

Certificaat contactpersonen bevatten informatie over het verkrijgen voor het verzenden van meldingen die worden geactiveerd door gebeurtenissen van de levensduur van certificaat. De informatie contactpersonen wordt gedeeld door alle certificaten in de sleutelkluis. Er wordt een melding verzonden naar alle contactpersonen die opgegeven is voor een gebeurtenis voor alle certificaten in de sleutelkluis.  

Als een certificaat beleid is ingesteld op automatisch verlengen, wordt op de volgende gebeurtenissen een melding verzonden.  

-   Voordat een certificaat vernieuwen
-   Nadat de vernieuwing van certificaten, wordt weergegeven als het certificaat is vernieuwd of als er een fout opgetreden is, handmatige verlenging van het certificaat vereisen.  

 Als een certificaat beleid is ingesteld op handmatig worden wordt vernieuwd (alleen e-mail) en vervolgens een melding verzonden wanneer het tijd om het certificaat te vernieuwen.  

###  <a name="BKMK_CertificateAccessControl"></a> Certificaat-toegangsbeheer

 Toegangsbeheer voor certificaten wordt beheerd door de Sleutelkluis en op het niveau van een Sleutelkluis die als de container van die certificaten fungeert is opgegeven. Er is een beleid voor toegangsbeheer voor certificaten die verschilt van het beleid voor toegangsbeheer voor sleutels en geheimen in de kluis met dezelfde sleutel. Gebruikers kunnen een of meer kluizen voor het opslaan van certificaten maken en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van certificaten.  

 De volgende machtigingen kunnen worden gebruikt op een basis per principal in de access control entry geheimen op een sleutelkluis en nauw mirrors de bewerkingen die zijn toegestaan op een geheim object:  

-   *ophalen van*: ophalen van de huidige certificaatversie of een willekeurige versie van een certificaat kunt 
-   *lijst*: kan de lijst met de huidige certificaten of versies van een certificaat  
-   *verwijderen*: kan het verwijderen van een certificaat, het beleid en alle versies  
-   *Maak*: kunt maken van een certificaat van de Sleutelkluis.  
-   *importeren*: kunt u het importeren van certificaat materiaal in een kluis sleutelcertificaat.  
-   *bijwerken*: kunnen updates van een certificaat.  
-   *manageconnects*: beheer van Sleutelkluis certificaat contacten toegestaan  
-   *getissuers*: kunt ophalen van een certificaat uitgevers van certificaten  
-   *listissuers*: kan de lijst met certificaatverleners van  
-   *setissuers*: kunt maken of bijwerken van certificaatverleners Sleutelkluis  
-   *deleteissuers*: kan het verwijderen van de Sleutelkluis certificaatverleners  
-   *alle*: verleent alle machtigingen  

## <a name="additional-information-for-certificates"></a>Aanvullende informatie voor certificaten

- [Certificaten en het beleid](/rest/api/keyvault/certificates-and-policies.md)
- [Uitgevers van certificaten](/rest/api/keyvault/certificate-issuers.md)
- [Certificaat contactpersonen](/rest/api/keyvault/certificate-contacts.md)

## <a name="see-also"></a>Zie ook

- [Verificatie, aanvragen en antwoorden](authentication-requests-and-responses.md)
- [Sleutelkluis-versies](key-vault-versions.md)
- [Kluis Ontwikkelaarshandleiding voor sleutel](/azure/key-vault/key-vault-developers-guide)
