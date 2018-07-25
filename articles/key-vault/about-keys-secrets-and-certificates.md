---
title: Over sleutels, geheimen en certificaten
description: Overzicht van de REST-interface en informatie over KV developer
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
ms.openlocfilehash: 77675b3c0b2ed9fcdb923c92638384d215bddc40
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972397"
---
# <a name="about-keys-secrets-and-certificates"></a>Over sleutels, geheimen en certificaten
Azure Key Vault kunnen gebruikers voor het opslaan en gebruiken van cryptografische sleutels binnen de Microsoft Azure-omgeving. Key Vault ondersteunt meerdere sleuteltypen en algoritmen, en maakt het gebruik van Hardware Security Modules (HSM) voor hoge waarde sleutels. Key Vault kunnen bovendien gebruikers geheimen veilig opslaan. Geheimen zijn geen specifieke semantiek beperkte grootte octet objecten. Key Vault biedt ook ondersteuning voor certificaten die zijn gebaseerd op de sleutels en geheimen en toevoegen van een functie voor automatische verlenging.

Zie voor meer algemene informatie over Azure Key Vault [wat is Azure Key Vault?](/azure/key-vault/key-vault-whatis)

**Algemene informatie van Key Vault**

-   [Ondersteuning van standaarden](about-keys-secrets-and-certificates.md#BKMK_Standards)
-   [Gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  
-   [Objecten,-id's en versiebeheer](about-keys-secrets-and-certificates.md#BKMK_ObjId)  

**Over sleutels**

-   [Sleutels en sleuteltypen](about-keys-secrets-and-certificates.md#BKMK_KeyTypes)  
-   [RSA-algoritmen](about-keys-secrets-and-certificates.md#BKMK_RSAAlgorithms)  
-   [RSA-HSM-algoritmen](about-keys-secrets-and-certificates.md#BKMK_RSA-HSMAlgorithms)  
-   [Cryptografische beveiliging](about-keys-secrets-and-certificates.md#BKMK_Cryptographic)
-   [Sleutelbewerkingen](about-keys-secrets-and-certificates.md#BKMK_KeyOperations)  
-   [Belangrijke kenmerken](about-keys-secrets-and-certificates.md#BKMK_KeyAttributes)  
-   [Sleutel-tags](about-keys-secrets-and-certificates.md#BKMK_Keytags)  

**Over geheimen** 

-   [Werken met geheimen](about-keys-secrets-and-certificates.md#BKMK_WorkingWithSecrets)  
-   [Geheime kenmerken](about-keys-secrets-and-certificates.md#BKMK_SecretAttrs)  
-   [De geheime codes](about-keys-secrets-and-certificates.md#BKMK_SecretTags)  
-   [Toegangsbeheer voor geheimen](about-keys-secrets-and-certificates.md#BKMK_SecretAccessControl)  

**Over certificaten**

-   [Samenstelling van een certificaat](#BKMK_CompositionOfCertificate)  
-   [Certificaatkenmerken en -Tags](#BKMK_CertificateAttributesAndTags)  
-   [Certificaatbeleid](#BKMK_CertificatePolicy)  
-   [Certificaatverlener](#BKMK_CertificateIssuer)  
-   [Certificaatcontactpersonen](#BKMK_CertificateContacts)  
-   [Certificaat-toegangsbeheer](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Algemene informatie van Key Vault

De volgende secties bieden algemene informatie die van toepassing zijn voor de implementatie van de Azure Key Vault-service.

###  <a name="BKMK_Standards"></a> Ondersteuning van standaarden

De JavaScript Object Notation (JSON) en de specificaties voor JavaScript Object ondertekening en versleuteling (JOSE) zijn belangrijke achtergrondinformatie.  

-   [JSON-Websleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web codering (met JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmen (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web handtekening (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> gegevenstypen

Raadpleeg de [JOSE specificaties](#BKMK_Standards) voor relevante gegevenstypen voor sleutels, versleuteling en ondertekening.  

-   **algoritme** -een ondersteunde algoritme voor een sleutel is, bijvoorbeeld RSA1_5  
-   **ciphertext-waarde** -tekst octetten, gecodeerd met Base64URL cipher  
-   **Digest-waarde** -de uitvoer van een hash-algoritme, gecodeerd met Base64URL  
-   **sleutel-type** -een van de ondersteunde typen voor sleutels, bijvoorbeeld RSA.  
-   **niet-gecodeerde tekst / waarde-** -als tekst zonder opmaak octetten, gecodeerd met Base64URL  
-   **waarde handtekening** - uitvoer van een algoritme voor handtekening, gecodeerd met Base64URL  
-   **base64URL** -een Base64URL [RFC4648] gecodeerde binaire waarde  
-   **Booleaanse** -true of false  
-   **Identiteit** : een identiteit van Azure Active Directory (AAD).  
-   **IntDate** - een decimale waarde die aangeeft van het aantal seconden tussen 1970 van JSON-01-01T0:0:0Z UTC tot en met de opgegeven UTC-datum/tijd. Zie [RFC3339] voor meer informatie over datum/tijden in het algemeen en UTC met name.  

###  <a name="BKMK_ObjId"></a> Objecten, -id's en versiebeheer

Objecten die zijn opgeslagen in Azure Key Vault behouden versies wanneer een nieuw exemplaar van een object wordt gemaakt, en elke versie een unieke id en een URL heeft. Wanneer een object wordt gemaakt, krijgt een unieke id en is gemarkeerd als de huidige versie van het object. Het maken van een nieuw exemplaar met dezelfde naam van het object een unieke id van het nieuwe object biedt en zorgt ervoor dat de huidige versie.  

Objecten in Azure Key Vault kunnen worden aangepakt met behulp van de huidige id of een specifiek voor een versie-id. Bijvoorbeeld, uitgaande van een sleutel met de naam "Hoofdsleutel", uitvoeren van bewerkingen met de huidige id zorgt ervoor dat het systeem moet de meest recente beschikbare versie. Uitvoeren van bewerkingen met de id van de specifieke versies zorgt ervoor dat het systeem die specifieke versie van het object te gebruiken.  

Objecten kunnen worden onderscheiden in Azure Key Vault met behulp van een URL, zodat er geen twee objecten in het systeem, ongeacht de geografische locatie, dezelfde URL hebben. De volledige URL naar een object met de naam de Object-id en bestaat uit een voorvoegsel dat de Key Vault, het objecttype identificeert, een gebruiker opgegeven naam van Object en de versie van een Object. Naam van het Object is niet hoofdlettergevoelig en onveranderbaar. Id's die niet de Object-versie omvatten worden aangeduid als Base-id.  

Zie voor meer informatie, [verificatie, vragen en antwoorden](authentication-requests-and-responses.md)

Een object-id heeft de volgende algemene indeling:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Waar:  

|||  
|-|-|  
|`keyvault-name`|De naam voor een key vault in de Microsoft Azure Key Vault-service.<br /><br /> Key Vault-namen worden geselecteerd door de gebruiker en wereldwijd uniek zijn.<br /><br /> Key Vault-naam moet een tekenreeks 3 tot 24 tekens lang dat alleen (0-9, a-z, A-Z, en -).|  
|`object-type`|Het type van het object, 'sleutels' of 'geheimen'.|  
|`object-name`|Een `object-name` is een door de gebruiker opgegeven naam voor en moet uniek zijn binnen een Key Vault. De naam moet een tekenreeks van 1-127 tekens lang dat alleen 0-9, a-z, A-Z, en -.|  
|`object-version`|Een `object-version` is een door het systeem gegenereerde, tekenreeks-id van 32 tekens die eventueel wordt gebruikt om een unieke versie van een object.|  

## <a name="key-vault-keys"></a>Key Vault sleutels

###  <a name="BKMK_KeyTypes"></a> Sleutels en sleuteltypen

Cryptografische sleutels in Azure Key Vault worden weergegeven als JSON-Websleutel [JWK]-objecten. De base JWK/JWA specificaties zijn ook uitgebreid zodat sleuteltypen uniek is voor de implementatie van Azure Key Vault, bijvoorbeeld het importeren van de sleutels naar Azure Key Vault met behulp van de specifieke verpakking voor HSM-leverancier (Thales) op het inschakelen van beveiligde vervoer van dergelijke sleutels die ze kunnen alleen worden gebruikt in de Azure Key Vault HSM's.  

De eerste release van Azure Key Vault ondersteunt RSA-sleutels. toekomstige versies kunnen andere sleuteltypen zoals symmetrische en elliptic curve ondersteunen.  

-   **RSA**: een 2048-bits RSA-sleutel. Dit is een 'soft' sleutel, die in de software wordt verwerkt door Key Vault, maar die gegevens worden versleuteld met behulp van een systeemsleutel die in een HSM is opgeslagen. Clients kunnen importeren van een bestaande RSA-sleutel of vraag Azure Key Vault één genereert.  
-   **RSA-HSM**: een RSA-sleutel die wordt verwerkt in een HSM. RSA-HSM-sleutels worden beveiligd in een van de Azure Key Vault HSM-Beveiligingswerelden (Er is een Beveiligingswereld per Geografie isolatie onderhouden). Clients kunnen een RSA-sleutel in zachte formulier of door het exporteren van een compatibel apparaat van de HSM importeren of Azure Key Vault genereren een aanvraag. Dit sleuteltype voegt de T-kenmerk toe aan de JWK ophalen voor het uitvoeren van de HSM-sleutelmateriaal.  

     Zie voor meer informatie over geografische grenzen [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/)  

###  <a name="BKMK_RSAAlgorithms"></a> RSA-algoritmen  
 De volgende algoritme-id's worden ondersteund met de RSA-sleutels in Azure Key Vault.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>SLEUTEL INPAKKEN/SLEUTEL UITPAKKEN, VERSLEUTELEN/ONTSLEUTELEN

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] key-versleuteling  
-   **RSA-OAEP** - RSAES met optimale asymmetrische codering opvulling (OAEP) [RFC3447] met de standaardparameters die is opgegeven door RFC 3447 in sectie A.2.1. Deze standaardparameters worden met behulp van een hash-functie van SHA-1 en een functie voor het genereren van masker van MGF1 met SHA-1.  

#### <a name="signverify"></a>AANMELDING/CONTROLEREN

-   **RS256** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met SHA-256. De opgegeven toepassing digest-waarde moet worden berekend op basis van SHA-256 en moet 32 bytes lang zijn.  
-   **RS384** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-384. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-384 en moet 48 bytes lang zijn.  
-   **RS512** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-512. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-512 en moet 64 bytes lang zijn.  
-   **RSNULL** -Zie [RFC2437] een gespecialiseerde use-case waarmee bepaalde TLS-scenario's.  

###  <a name="BKMK_RSA-HSMAlgorithms"></a> RSA-HSM-algoritmen  
De volgende algoritme-id's worden ondersteund met de RSA-HSM-sleutels in Azure Key Vault.  

### <a name="BKMK_Cryptographic"></a> Cryptografische beveiliging

De cryptografische modules die gebruikmaakt van Azure Key Vault, of HSM of software zijn FIPS-gevalideerde modules. U hoeft te doen niets om uit te voeren in de FIPS-modus. Als u **maken** of **importeren** sleutels als HSM-beveiliging, ze moeten worden verwerkt in HSM's die zijn gevalideerd voor FIPS 140-2 Level 2 of hoger worden gegarandeerd. Als u **maken** of **importeren** sleutels als softwarebeveiliging en ze worden verwerkt in de cryptografische modules die zijn gevalideerd voor FIPS 140-2 niveau 1 of hoger. Zie voor meer informatie, [sleutels en sleuteltypen](about-keys-secrets-and-certificates.md#BKMK_KeyTypes).

#### <a name="wrapunwrap-encryptdecrypt"></a>VERPAKKEN/UITPAKKEN, VERSLEUTELEN/ONTSLEUTELEN

-   **RSA1_5** -RSAES PKCS1 V1_5 [RFC3447] key-versleuteling.  
-   **RSA-OAEP** - RSAES met optimale asymmetrische codering opvulling (OAEP) [RFC3447] met de standaardparameters die is opgegeven door RFC 3447 in sectie A.2.1. Deze standaardparameters worden met behulp van een hash-functie van SHA-1 en een functie voor het genereren van masker van MGF1 met SHA-1.  

 #### <a name="signverify"></a>AANMELDING/CONTROLEREN  

-   **RS256** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met SHA-256. De opgegeven toepassing digest-waarde moet worden berekend op basis van SHA-256 en moet 32 bytes lang zijn.  
-   **RS384** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-384. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-384 en moet 48 bytes lang zijn.  
-   **RS512** - ONDERTEKENINGSMETHODE-PKCS-v1_5 met behulp van SHA-512. De toepassing opgegeven digest-waarde moet worden berekend op basis van SHA-512 en moet 64 bytes lang zijn.  
-   RSNULL: Zie [RFC2437] een gespecialiseerde use-case waarmee bepaalde TLS-scenario's.  

###  <a name="BKMK_KeyOperations"></a> Sleutelbewerkingen

Azure Key Vault ondersteunt de volgende bewerkingen voor sleutels objecten:  

-   **Maak**: Hiermee kan een client een sleutel te maken in Azure Key Vault. De waarde van de sleutel wordt gegenereerd door Azure Key Vault en opgeslagen en niet naar de client is vrijgegeven. Asymmetrische (en in de toekomst, Elliptic Curve en Symmetric) sleutels kunnen worden gemaakt in Azure Key Vault.  
-   **Importeren**: Hiermee kan een client voor het importeren van een bestaande sleutel naar Azure Key Vault. Asymmetrische (en in de toekomst, Elliptic Curve en Symmetric) sleutels kunnen worden geïmporteerd in Azure Key Vault met behulp van een aantal methoden die andere pakketten in een JWK-constructie.  
-   **Update**: Hiermee kan een client met voldoende machtigingen voor het wijzigen van de metagegevens (belangrijke kenmerken) die zijn gekoppeld aan een sleutel die eerder zijn opgeslagen in Azure Key Vault.  
-   **Verwijder**: Hiermee kan een client met voldoende machtigingen voor het verwijderen van een sleutel uit Azure Key Vault.  
-   **Lijst met**: kan een lijst van alle sleutels in een bepaalde Azure Key Vault-client.  
-   **Versies weergeven**: kan een lijst van alle versies van een opgegeven sleutel in een bepaalde Azure Key Vault-client.  
-   **Ophalen**: Hiermee kan een client om op te halen van de openbare gedeelten van een opgegeven sleutel in een Azure Key Vault.  
-   **Back-up**: Hiermee exporteert u een sleutel in een beveiligde vorm.  
-   **Herstellen**: een back-up sleutel geïmporteerd.  

Zie voor meer informatie, [operations sleutel](/rest/api/keyvault/key-operations).  

Zodra een sleutel is gemaakt in Azure Key Vault, kunnen de volgende cryptografische bewerkingen worden uitgevoerd met behulp van de sleutel:  

-   **Aanmelden en controleer of**: strikt deze bewerking is 'aanmelding hash' of 'verifiëren hash' als Azure Key Vault biedt geen ondersteuning voor hashing van inhoud als onderdeel van het maken van de handtekening. Toepassingen moeten de gegevens die lokaal zijn ondertekend en vervolgens aanvragen dat Azure Key Vault-teken de hash-hash. Verificatie van de ondertekende hashes wordt ondersteund als een bewerking gemak voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; We raden aan dat, voor de beste toepassingsprestaties, controleert u of bewerkingen lokaal worden uitgevoerd.  
-   **Versleuteling op sleutel / Wrapping**: een sleutel is opgeslagen in Azure Key Vault kan worden gebruikt om een andere sleutel, meestal de sleutel van een symmetrische versleuteling van de inhoud (CEK) te beveiligen. Wanneer de sleutel in Azure Key Vault is asymmetrische, key-versleuteling wordt gebruikt, bijvoorbeeld RSA-OAEP en de sleutel INPAKKEN/UNWRAPKEY-bewerkingen zijn gelijk aan het coderen/decoderen. Wanneer de sleutel in Azure Key Vault symmetrische is, wordt wrapping sleutel gebruikt; bijvoorbeeld AES-KW. De sleutel INPAKKEN-bewerking wordt ondersteund als nuttig voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; Het verdient aanbeveling dat voor de beste toepassingsprestaties, sleutel INPAKKEN bewerkingen worden uitgevoerd lokaal.  
-   **Versleutelen en ontsleutelen van**: een sleutel is opgeslagen in Azure Key Vault kan worden gebruikt om te versleutelen of ontsleutelen van één blok van gegevens, de grootte hiervan wordt bepaald door het type sleutel en de geselecteerde versleutelingsalgoritme. De bewerking versleutelen is opgegeven voor het gemak voor toepassingen die mogelijk geen toegang tot [public] sleutelmateriaal; Het verdient aanbeveling dat voor de beste toepassingsprestaties, versleutelen bewerkingen lokaal worden uitgevoerd.  

Tijdens het gebruik van asymmetrische sleutels sleutel uitpakken en WRAPKEY lijkt overbodige als de bewerking gelijk aan het coderen/decoderen is, het gebruik van verschillende bewerkingen is als belangrijk worden beschouwd voor semantische en autorisatie scheiding van deze bewerkingen en consistentie Als andere typen sleutels worden ondersteund door de service.  

Azure Key Vault biedt geen ondersteuning voor exportbewerkingen: zodra een sleutel is ingericht in het systeem deze niet kan worden geëxtraheerd of het sleutelmateriaal gewijzigd.  Echter gebruikers van Azure Key Vault die mogelijk de sleutel voor andere use-cases of nadat deze is verwijderd in Azure Key Vault, kunnen de back-up en herstel-bewerkingen gebruiken voor het exporteren/importeren van de sleutel in een beveiligde vorm. Sleutels die zijn gemaakt door de back-up zijn niet kan worden gebruikt buiten Azure Key Vault. De importbewerking kan ook worden gebruikt op basis van meerdere exemplaren van Azure Key Vault.  

Gebruikers kunnen een van de cryptografische bewerkingen die ondersteuning biedt voor Azure Key Vault op basis van de key_ops-eigenschap van het object JWK per sleutel beperken.  

Zie voor meer informatie over JWK objecten [JSON Web sleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Belangrijke kenmerken

Naast het sleutelmateriaal, kunnen de volgende kenmerken worden opgegeven. In een JSON-aanvraag, de kenmerken sleutelwoord en accolades, ' {' '}', zijn vereist, zelfs als er geen kenmerken opgegeven zijn.  

- *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Hiermee geeft u op of de sleutel ingeschakeld en bruikbaar voor cryptografische bewerkingen is. De *ingeschakeld* kenmerk wordt gebruikt in combinatie met *nbf* en *exp*. Wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, wordt alleen worden toegestaan als *ingeschakeld* is ingesteld op **waar**. Bewerkingen buiten de *nbf* / *exp* venster zijn automatisch toegestaan, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops).
- *NBF*: IntDate, optioneel, standaard is nu. De *nbf* (niet voor) kenmerk wordt de tijd waarbinnen moet de sleutel niet worden gebruikt voor cryptografische bewerkingen, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). De verwerking van de *nbf* kenmerk vereist dat de huidige datum en tijd moeten na of gelijk aan de niet-vóór de datum/tijd die worden vermeld in de *nbf* kenmerk. Azure Key Vault kan bieden voor enkele kleine eenheidsprofiel meestal niet meer dan een paar minuten, voor het account voor de klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *EXP*: IntDate, optioneel, standaard 'altijd' is. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na die de sleutel mag niet worden gebruikt voor cryptografische bewerking, met uitzondering van bepaalde bewerkingstypen onder [bepaalde voorwaarden](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). De verwerking van de *exp* kenmerk is vereist dat de huidige datum en tijd zijn moet voordat de vervaldatum van de datum/tijd die worden vermeld in de *exp* kenmerk. Azure Key Vault kan bieden voor enkele kleine eenheidsprofiel meestal niet meer dan een paar minuten, voor het account voor de klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in elke reactie met belangrijke kenmerken:  

- *gemaakt*: IntDate, optioneel. De *gemaakt* kenmerk geeft aan wanneer deze versie van de sleutel is gemaakt. Deze waarde is null voor sleutels die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De *bijgewerkt* kenmerk geeft aan wanneer deze versie van de sleutel is bijgewerkt. Deze waarde is null voor sleutels die voor het laatst vóór het toevoegen van dit kenmerk bijgewerkt zijn. De waarde moet een getal met een IntDate-waarde.  

Zie voor meer informatie over IntDate en andere gegevenstypen [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Beheerde bewerkingen voor datum / tijd

Niet nog geldig is en verlopen sleutels, die buiten de *nbf* / *exp* venster werkt voor **ontsleutelen**, **uitpakken** en **controleren** bewerkingen (403, niet als resultaat verboden). De logica voor het gebruik van de status niet nog geldig is om een sleutel moet worden getest voordat u gebruik in productieomgevingen. De logica voor het gebruik van de verlopen staat is om toe te staan van de recovery-bewerkingen op gegevens die is gemaakt tijdens de sleutel ongeldig is. Bovendien kunt u toegang tot een sleutel met behulp van Key Vault-beleid of door bij te werken uitschakelen de *ingeschakeld* sleutelkenmerk naar **false**.

Zie voor meer informatie over gegevens gegevenstypen, [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes).

Zie voor meer informatie over andere kenmerken, de [JSON Web sleutel (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Sleutel-tags

U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van tags. Azure Key Vault ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde 256 tekens hebben.  

>[!Note]
>Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

###  <a name="BKMK_KeyAccessControl"></a> Toegangsbeheer voor sleutels

Toegangsbeheer voor sleutels die worden beheerd door Key Vault is opgegeven op het niveau van een Key Vault die als de container van sleutels fungeert. Er is een beleid voor toegangsbeheer voor sleutels die verschilt van het beleid voor toegangsbeheer voor geheimen in de dezelfde Key Vault. Gebruikers kunnen maken van een of meer kluizen voor het opslaan van sleutels en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van sleutels. Toegangsbeheer voor sleutels is onafhankelijk van toegangsbeheer voor geheimen.  

De volgende machtigingen kunnen worden verleend, op een per gebruiker / service-principal uit te voeren, in de access control entry sleutels in een kluis. Deze machtigingen spiegelen nauw de bewerkingen die zijn toegestaan op een sleutel-object:  

-   *Maak*: nieuwe sleutels maken
-   *ophalen*: het openbare deel van een sleutel, plus de kenmerken lezen
-   *lijst met*: de sleutels of versies van een sleutel die zijn opgeslagen in een key vault
-   *Update*: de kenmerken voor een sleutel bijwerken
-   *Verwijder*: de sleutel object verwijderen
-   *Meld u*: Gebruik de sleutel aan te melden verwerkingen
-   *Controleer of*: de sleutel gebruiken om te controleren of verwerkingen
-   *sleutel inpakken*: de sleutel gebruiken om een symmetrische sleutel te beveiligen
-   *sleutel uitpakken*: de sleutel gebruiken om op te heffen verpakt symmetrische sleutels
-   *versleutelen*: de sleutel gebruiken om te beveiligen van een willekeurige volgorde van de bytes
-   *ontsleutelen*: de sleutel gebruiken om op te heffen van een reeks bytes
-   *importeren*: importeren van een sleutel in een key vault
-   *back-up*: back-up van een sleutel in een key vault
-   *herstellen*: een back-ups herstellen van de sleutel tot een key vault
-   *alle*: alle machtigingen

## <a name="key-vault-secrets"></a>Key Vault-geheimen 

###  <a name="BKMK_WorkingWithSecrets"></a> Werken met geheimen

Geheimen in Azure Key Vault zijn octet reeksen met een maximale grootte van 25 k bytes. De Azure Key Vault-service biedt geen eventuele semantiek voor geheimen; het gewoon accepteert de gegevens, versleutelt en slaat het retourneren van een geheim 'id', die kan worden gebruikt om op te halen van het geheim op een later tijdstip-id.  

Voor zeer gevoelige gegevens, clients moeten rekening houden met extra beveiligingslagen voor gegevens die zijn opgeslagen in Azure Key Vault; bijvoorbeeld vooraf door gegevens te coderen met behulp van een afzonderlijke protection-tenantsleutel.  

Azure Key Vault ondersteunt ook het veld contentType voor geheimen. Clients kunnen het type inhoud, 'contentType' van een geheim om te helpen bij het interpreteren van de geheime gegevens wanneer deze wordt opgehaald opgeven. De maximale lengte van dit veld is 255 tekens. Er zijn geen vooraf gedefinieerde waarden. Het voorgestelde gebruik is als een hint op voor het interpreteren van de geheime gegevens. Een van de implementatie kan bijvoorbeeld zowel wachtwoorden en certificaten opgeslagen als geheimen vervolgens dit veld gebruiken om aan te geven die. Er zijn geen vooraf gedefinieerde waarden.  

###  <a name="BKMK_SecretAttrs"></a> Geheime kenmerken

Naast de geheime gegevens kunnen de volgende kenmerken worden opgegeven:  

- *EXP*: IntDate, optioneel, standaard is **altijd**. De *exp* (verlooptijd) kenmerk identificeert de verlooptijd op of na die de geheime gegevens mag niet worden opgehaald, met uitzondering van in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). De verwerking van de *exp* kenmerk is vereist dat de huidige datum en tijd zijn moet voordat de vervaldatum van de datum/tijd die worden vermeld in de *exp* kenmerk. Azure Key Vault kan bieden voor enkele kleine eenheidsprofiel meestal niet meer dan een paar minuten, voor het account voor de klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *NBF*: IntDate, optioneel, standaard is **nu**. De *nbf* (niet voor) kenmerk wordt de tijd waarbinnen moet de geheime gegevens niet worden opgehaald, met uitzondering van in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). De verwerking van de *nbf* kenmerk vereist dat de huidige datum en tijd moeten na of gelijk aan de niet-vóór de datum/tijd die worden vermeld in de *nbf* kenmerk. Azure Key Vault kan bieden voor enkele kleine eenheidsprofiel meestal niet meer dan een paar minuten, voor het account voor de klok scheeftrekken. De waarde moet een getal met een IntDate-waarde.  
- *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Dit kenmerk wordt opgegeven of de geheime gegevens kunnen worden opgehaald. De ingeschakelde kenmerk wordt gebruikt in combinatie met en *exp* wanneer een bewerking plaatsvindt tussen exp, deze alleen worden toegestaan als ingeschakeld is ingesteld op **waar**. Bewerkingen buiten de *nbf* en *exp* venster worden automatisch niet-toegestane, behalve in [bepaalde situaties](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops).  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in elke reactie met geheime kenmerken:  

- *gemaakt*: IntDate, optioneel. De gemaakte kenmerk geeft aan wanneer deze versie van het geheim is gemaakt. Deze waarde is null voor geheimen die zijn gemaakt vóór het toevoegen van dit kenmerk. De waarde moet een getal met een IntDate-waarde.  
- *bijgewerkt*: IntDate, optioneel. De bijgewerkte kenmerk geeft aan wanneer deze versie van het geheim is bijgewerkt. Deze waarde is null voor geheimen die voor het laatst vóór het toevoegen van dit kenmerk bijgewerkt zijn. De waarde moet een getal met een IntDate-waarde.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Beheerde bewerkingen voor datum / tijd

Van een geheim **ophalen** bewerking werkt voor geen nog geldig is en verlopen geheimen, buiten de *nbf* / *exp* venster. Aanroepen van een geheim **ophalen** bewerking, voor een geheim niet nog geldig kan worden gebruikt voor testdoeleinden. Bij het ophalen van (**ophalen**ing) een geheim verlopen, kan worden gebruikt voor herstelbewerkingen.

Zie voor meer informatie over gegevens gegevenstypen, [gegevenstypen](about-keys-secrets-and-certificates.md#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Toegangsbeheer voor geheimen

Toegangsbeheer voor geheimen die worden beheerd in Azure Key Vault is opgegeven op het niveau van een Key Vault die als de container van deze geheimen fungeert. Er is een beleid voor toegangsbeheer voor geheimen die verschilt van het beleid voor toegangsbeheer voor sleutels in de dezelfde Key Vault. Gebruikers kunnen maken van een of meer kluizen voor het opslaan van geheimen en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van geheimen. Toegangsbeheer voor geheimen zijn onafhankelijk van toegangsbeheer voor sleutels.  

De volgende machtigingen kunnen worden gebruikt, op basis van per-principal, in de access control entry geheimen in een kluis en nauw mirror van de bewerkingen die zijn toegestaan op een geheim object:  

-   *Stel*: maken van nieuwe geheimen  
-   *ophalen*: een geheim lezen  
-   *lijst met*: de geheimen of versies van een geheim opgeslagen in een Key Vault  
-   *Verwijder*: het geheim verwijderen  
-   *alle*: alle machtigingen  

Zie voor meer informatie over het werken met geheimen [geheime bewerkingen](/rest/api/keyvault/secret-operations).  

###  <a name="BKMK_SecretTags"></a> De geheime codes  
U kunt aanvullende toepassingsspecifieke metagegevens opgeven in de vorm van tags. Azure Key Vault ondersteunt maximaal 15 tags, die elk een naam van 256 tekens en een waarde 256 tekens hebben.  

>[!Note]
>Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

## <a name="key-vault-certificates"></a>Key Vault-certificaten

Key Vault-certificaten-ondersteuning biedt voor het beheer van uw x509 certificaten en het volgende gedrag:  

-   Kan de eigenaar van een certificaat om een certificaat via een proces voor het maken van Key Vault of via het importeren van een bestaand certificaat te maken. Dit omvat zowel zelfondertekend en de certificeringsinstantie die certificaten worden gegenereerd.
-   Kan de eigenaar van een Key Vault-certificaat voor het implementeren van veilige opslag en het beheer van X509 certificaten zonder interactie met privésleutelmateriaal.  
-   Kan de eigenaar van een certificaat te maken van een beleid dat zorgt ervoor dat de Key Vault voor het beheren van de levenscyclus van een certificaat.  
-   Kan certificaat eigenaren contact op met informatie voor melding over gebeurtenissen van de levenscyclus van de vervaldatum en -verlenging van het certificaat opgeven.  
-   Automatische vernieuwing ondersteunt met geselecteerde verleners - Key Vault partner X509 providers van het certificaat / certificeringsinstanties.

>[!Note]
>Providers/instanties niet gelieerde zijn ook toegestaan, maar wordt geen ondersteuning voor de functie voor automatisch verlengen.

###  <a name="BKMK_CompositionOfCertificate"></a> Samenstelling van een certificaat

Wanneer een Key Vault-certificaat is gemaakt, worden ook een adresseerbare sleutel en -geheim gemaakt met dezelfde naam. De Key Vault-sleutel kan sleutelbewerkingen en de Key Vault-geheim kan ophalen van de waarde van het certificaat als een geheim. Een Key Vault-certificaat bevat ook openbare x509 metagegevens van het certificaat.  

De id en de versie van de certificaten is vergelijkbaar met dat van sleutels en geheimen. Een specifieke versie van een adresseerbare sleutel en -geheim dat is gemaakt met de versie van Key Vault-certificaat is beschikbaar in het antwoord van Key Vault-certificaat.
 
![Cetificates zijn complexe objecten](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> De sleutel exporteerbaar of niet kan worden geëxporteerd

Wanneer een Key Vault-certificaat is gemaakt, kan het worden opgehaald uit de adresseerbare geheim met de persoonlijke sleutel in pfx- of PEM-indeling als het beleid dat wordt gebruikt voor het maken van het certificaat wordt aangegeven dat de sleutel exporteerbaar is. Het beleid dat wordt gebruikt voor het maken van de Key Vault-certificaat de sleutel niet exporteerbaar worden vermeld, klikt u vervolgens is de persoonlijke sleutel niet als onderdeel van de waarde als opgehaald als een geheim.  

De adresseerbare sleutel wordt meer relevante met niet-exporteerbare KV certificaten. Bewerkingen van de adresseerbare KV sleutel vanuit zijn toegewezen *keyusage* veld van het beleid voor KV certificaat gebruikt voor het maken van het certificaat KV.  

Twee typen sleutel worden ondersteund: *RSA* of *RSA HSM* met certificaten. Exporteerbaar is alleen toegestaan met RSA, niet wordt ondersteund door RSA HSM.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Certificaatkenmerken en -Tags

Naast het certificaatmetagegevens, een adresseerbare-sleutel en een adresseerbare geheim bevat een Key Vault-certificaat ook kenmerken en tags.  

#### <a name="attributes"></a>Kenmerken

De certificaatkenmerken worden gespiegeld op kenmerken van de adresseerbare sleutel en -geheim gemaakt wanneer KV certificaat wordt gemaakt.  

Een Key Vault-certificaat heeft de volgende kenmerken:  

-   *ingeschakeld*: Booleaans, optioneel, de standaardwaarde is **waar**. Dit kenmerk kan worden opgegeven om aan te geven als de gegevens van het certificaat als geheim of bediend als een sleutel kan worden opgehaald. Dit wordt gebruikt in combinatie met *nbf* en *exp* wanneer een bewerking plaatsvindt tussen *nbf* en *exp*, deze alleen worden toegestaan als ingeschakeld is ingesteld op true. Bewerkingen buiten de *nbf* en *exp* venster automatisch zijn niet toegestaan.  

Er zijn aanvullende kenmerken voor alleen-lezen die zijn opgenomen in het antwoord:

-   *gemaakt*: IntDate: geeft aan wanneer deze versie van het certificaat is gemaakt.  
-   *bijgewerkt*: IntDate: geeft aan wanneer deze versie van het certificaat is bijgewerkt.  
-   *EXP*: IntDate: bevat de waarde van de vervaldatum van de x509 certificaat.  
-   *NBF*: IntDate: bevat de waarde van de datum van de x509 certificaat.  

> [!Note] 
> Als een Key Vault-certificaat is verlopen, is het beschikbaar maken zonder sleutel en geheim niet meer worden gebruikt.  

#### <a name="tags"></a>Tags

 Client opgegeven woordenlijst met sleutel-waardeparen, vergelijkbaar met de labels in sleutels en geheimen.  

 > [!Note]
> Labels kunnen worden gelezen door een aanroeper als ze beschikken over de *lijst* of *ophalen* machtiging voor het objecttype;-sleutels, geheimen of certificaten.

###  <a name="BKMK_CertificatePolicy"></a> Certificaatbeleid

Een certificaatbeleid bevat informatie over het maken en beheren van de levenscyclus van een certificaat KV. Wanneer een certificaat met persoonlijke sleutel wordt geïmporteerd in de key vault, een standaardbeleid is gemaakt met het lezen van de x509 certificaat.  

Wanneer een certificaat KV helemaal is gemaakt, moet een beleid voor Key Vault voor kennis over het maken van deze KV certificaat of de volgende versie van de KV-certificaat worden opgegeven. Wanneer een beleid is ingesteld, is het niet vereist met opeenvolgende bewerkingen voor het maken van de volgende versies van KV certificaat maken.  

Er is slechts één exemplaar van een beleid voor alle versies van een certificaat KV.  

Op hoog niveau bevat een certificaatbeleid het volgende:  

-   Eigenschappen van het certificaat X509: bevat onderwerpnaam, alternatieve namen voor onderwerpen enzovoort. gebruikt voor het maken van een x509 certificaataanvraag.  
-   Sleuteleigenschappen: bevat belangrijke type sleutellengte, kan worden geëxporteerd en velden voor sleutels opnieuw te gebruiken. Deze velden vertelt u key vault voor het genereren van een sleutel.  
-   Eigenschappen van geheim: eigenschappen van geheim, zoals het inhoudstype van adresseerbare geheim voor het genereren van de geheime waarde voor het ophalen van certificaat als een geheim bevat.  
-   De levensduur van acties: bevat de levensduur van acties voor het certificaat KV. Elke actie voor de levensduur bevat:  

     - Trigger: opgegeven via dagen vóór de vervaldatum of de levensduur span percentage  

     - Actie: op te geven actietype – *emailContacts* of *automatisch verlengen*  

-   Uitgever: De Parameters van over de uitgever van het certificaat te gebruiken om certificaten te verlenen x509.  
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
|Keyencipherment-bit|sleutel inpakken, sleutel uitpakken| Key Vault standaard zonder een specificatie voor gebruik tijdens de aanmaak van certificaat | 
|Niet-afwijzing|Meld u aan, controleert u of| N/A |
|crlsign|Meld u aan, controleert u of| N/A |

###  <a name="BKMK_CertificateIssuer"></a> Certificaatverlener

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

Zie voor meer informatie over het maken van objecten van certificaatverlener uit de Certificates-portal, de [Key Vault-certificaten-blog](http://aka.ms/kvcertsblog)  

Key Vault kunt u het maken van meerdere verlener objecten met andere verlener-providerconfiguratie. Zodra een verlener-object is gemaakt, kan de naam ervan in een of meerdere beleidsregels van het certificaat worden verwezen. Key Vault configuratie zoals opgegeven in het object verlener gebruiken bij het aanvragen van de x509 verwijzen naar het object verlener geeft certificaten van CA-provider tijdens het maken van het certificaat en de verlenging.  

Objecten van certificaatverlener worden gemaakt in de kluis en kunnen alleen worden gebruikt met KV certificaten in dezelfde kluis.  

###  <a name="BKMK_CertificateContacts"></a> Certificaatcontactpersonen

Certificaatcontactpersonen bevatten informatie voor het verzenden van meldingen die worden geactiveerd door gebeurtenissen voor certificaat-levensduur. De gegevens van contactpersonen wordt gedeeld door alle certificaten in de key vault. Een melding wordt verzonden naar de opgegeven contactpersonen voor een gebeurtenis voor alle certificaten in de key vault.  

Als het beleid van het certificaat is ingesteld op automatische verlenging, wordt op de volgende gebeurtenissen een melding verzonden.  

-   Voordat u certificaatvernieuwing
-   Na het vernieuwen van een certificaat, waarin wordt vermeld als het certificaat is verlengd of als er een fout optreedt, waarvoor handmatige verlenging van het certificaat.  

 Als een certificaat van beleid is ingesteld op handmatig worden wordt vernieuwd (alleen e-mail) en vervolgens een melding verzonden wanneer het is nu tijd om het certificaat te vernieuwen.  

###  <a name="BKMK_CertificateAccessControl"></a> Certificaat-toegangsbeheer

 Toegangsbeheer voor certificaten wordt beheerd door Key Vault en op het niveau van een Key Vault die als de container van die certificaten fungeert wordt aangeboden. Er is een beleid voor toegangsbeheer voor certificaten die verschilt van het beleid voor toegangsbeheer voor sleutels en geheimen in de dezelfde Key Vault. Gebruikers kunnen een of meer kluizen voor het opslaan van certificaten maken en zijn vereist voor het onderhouden van de juiste segmentering scenario en het beheer van certificaten.  

 De volgende machtigingen kunnen worden gebruikt, op basis van per-principal, in de access control entry geheimen in een key vault en nauw mirrors de bewerkingen die zijn toegestaan op een geheim object:  

-   *ophalen*: ophalen van de huidige certificaatversie van het of een willekeurige versie van een certificaat kunt 
-   *lijst met*: kan de lijst met de huidige certificaten of versies van een certificaat  
-   *Verwijder*: verwijderen van een certificaat, het beleid en alle versies ervan kunt  
-   *Maak*: kan het maken van een Key Vault-certificaat.  
-   *importeren*: kunt u het importeren van certificaten materiaal in een Key Vault-certificaat.  
-   *Update*: kan de update van een certificaat.  
-   *manageconnects*: beheer van Key Vault certificaatcontactpersonen toegestaan  
-   *getissuers*: ophalen van uitgevers van certificaten van een certificaat kunt  
-   *listissuers*: kan de lijst van certificaatverleners  
-   *setissuers*: kunt maken of bijwerken van certificaatverleners Key Vault  
-   *deleteissuers*: kan het verwijderen van certificaatverleners Key Vault  
-   *alle*: verleent alle machtigingen  

## <a name="additional-information-for-certificates"></a>Aanvullende informatie voor certificaten

- [Certificaten en het beleid](/rest/api/keyvault/certificates-and-policies)
- [Uitgevers van certificaten](/rest/api/keyvault/certificate-issuers)
- [Certificaatcontactpersonen](/rest/api/keyvault/certificate-contacts)

## <a name="see-also"></a>Zie ook

- [Verificatie, vragen en antwoorden](authentication-requests-and-responses.md)
- [Key Vault-versies](key-vault-versions.md)
- [Key Vault-ontwikkelaars-handleiding](/azure/key-vault/key-vault-developers-guide)
