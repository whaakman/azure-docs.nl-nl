---
title: Cryptografie - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die worden weergegeven in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 96e74371fe51a8050a91c86215e3eefab07bbed8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="security-frame-cryptography--mitigations"></a>Beveiliging Frame: Cryptografie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Webtoepassing** | <ul><li>[Gebruik alleen goedgekeurde symmetrische blokcodering en sleutellengten](#cipher-length)</li><li>[Gebruik goedgekeurde blok cipher modi en initialisatie vectoren voor symmetrische codering](#vector-ciphers)</li><li>[Goedgekeurde asymmetrische algoritmen, sleutellengten en opvulling gebruiken](#padding)</li><li>[Gebruik goedgekeurde genereren van willekeurige nummer](#numgen)</li><li>[Gebruik geen symmetrische stroom coderingen](#stream-ciphers)</li><li>[Goedgekeurde MAC, HMAC/sleutelhash hash-algoritmen gebruiken](#mac-hash)</li><li>[Gebruik alleen goedgekeurde cryptografische hash-functies](#hash-functions)</li></ul> |
| **Database** | <ul><li>[Algoritmen voor sterke versleuteling gebruiken voor het versleutelen van gegevens in de database](#strong-db)</li><li>[SSIS-pakketten moeten worden versleuteld en digitaal zijn ondertekend](#ssis-signed)</li><li>[Digitale handtekening toevoegen aan database kritieke securables](#securables-db)</li><li>[SQL server EKM gebruiken om te beschermen versleutelingssleutels](#ekm-keys)</li><li>[AlwaysEncrypted-functie te gebruiken als de versleutelingssleutels niet moeten worden getoond met Database-engine](#keys-engine)</li></ul> |
| **IoT-apparaat** | <ul><li>[Cryptografische sleutels veilig opslaan op IoT-apparaat](#keys-iot)</li></ul> | 
| **IoT-Cloudgateway** | <ul><li>[Genereren van een willekeurige symmetrische sleutel lang genoeg zijn voor verificatie met IoT Hub](#random-hub)</li></ul> | 
| **Mobiele Dynamics CRM-Client** | <ul><li>[Zorg ervoor dat een apparaat management-beleid is geïmplementeerd waarmee een gebruik PINCODE vereist is en extern wissen](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook-Client** | <ul><li>[Zorg ervoor dat een apparaat management-beleid is geïmplementeerd die vereist dat een PINCODE of wachtwoord/automatisch vergrendelen en versleutelt alle gegevens (bijvoorbeeld Bitlocker)](#bitlocker)</li></ul> | 
| **Identiteitsserver** | <ul><li>[Zorg ervoor dat de handtekeningsleutels zijn overgeschakeld bij gebruik van Identiteitsserver](#rolled-server)</li><li>[Zorg ervoor dat cryptografisch sterke client-ID clientgeheim zijn gebruikt in Identiteitsserver](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Gebruik alleen goedgekeurde symmetrische blokcodering en sleutellengten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Producten moeten alleen de symmetrische blokcodering en de bijbehorende sleutellengte die expliciet zijn goedgekeurd door de Crypto-Advisor in uw organisatie gebruiken. Goedgekeurde symmetrische algoritmen van Microsoft zijn onder andere de volgende blokcodering:</p><ul><li>Voor nieuwe code AES-128, AES-192 en AES-256 worden geaccepteerd</li><li>Voor achterwaartse compatibiliteit met bestaande code wordt 3DES drie-sleutel geaccepteerd</li><li>Voor producten met symmetrische blokcodering:<ul><li>Geavanceerde Encryption Standard (AES) is vereist voor nieuwe code</li><li>Drie sleutel triple Data Encryption Standard (3DES) is toegestaan in de bestaande code voor achterwaartse compatibiliteit</li><li>Alle andere blokcodering, met inbegrip van RC2, DES, 2 sleutel 3DES, DESX en gestreepte, kunnen alleen worden gebruikt voor het ontsleutelen van oude gegevens en moeten worden vervangen als gebruikt voor versleuteling</li></ul></li><li>Voor symmetrische blok versleutelingsalgoritmen is een minimum sleutellengte van 128 bits vereist. Het enige blok versleutelingsalgoritme aanbevolen voor nieuwe code AES is (AES-128, AES-192 en AES-256 zijn allemaal geaccepteerd)</li><li>Drie sleutel 3DES is momenteel acceptabel als deze al in gebruik in de bestaande code. overgang naar AES wordt aanbevolen. DES, DESX RC2 en gestreepte worden niet langer als veilig beschouwd. Deze algoritmen kunnen alleen worden gebruikt voor het ontsleutelen van bestaande gegevens ten behoeve van achterwaartse compatibiliteit en gegevens opnieuw moet worden versleuteld met een aanbevolen blokcodering</li></ul><p>Houd er rekening mee dat alle symmetrische blokcodering moeten worden gebruikt met een goedgekeurde versleutelingsmodus waarvoor gebruik van een juiste initialisatievector (IV). Een juiste IV is doorgaans een willekeurig getal en nooit een constante waarde</p><p>Het gebruik van verouderde of anderszins niet-goedgekeurde cryptografische algoritmen en sleutellengten kleinere voor het lezen van bestaande gegevens (in plaats van nieuwe gegevens worden geschreven) kan worden toegestaan nadat Crypto het mededelingenbord van uw organisatie. U moet echter bestand voor een uitzondering op deze vereiste. Bovendien in bedrijfsimplementaties overwegen producten waarschuwing beheerders wanneer zwakke crypto wordt gebruikt om gegevens te lezen. Deze waarschuwingen moeten verklarende en actie worden uitgevoerd. In sommige gevallen kan het geschikt is voor het beheren van het gebruik van zwakke crypto Groepsbeleid zijn</p><p>.NET-algoritmen voor beheerde crypto flexibiliteit toegestaan (in volgorde van voorkeur)</p><ul><li>AesCng (FIPS-compatibel)</li><li>AuthenticatedAesCng (FIPS-compatibel)</li><li>AESCryptoServiceProvider (FIPS-compatibel)</li><li>AESManaged (niet-FIPS-compatibel)</li></ul><p>Houd er rekening mee dat geen van deze algoritmen kan worden opgegeven de `SymmetricAlgorithm.Create` of `CryptoConfig.CreateFromName` methoden zonder wijzigingen aanbrengt in het machine.config-bestand. Let ook op AES in versies van .NET vóór .NET 3.5 heet `RijndaelManaged`, en `AesCng` en `AuthenticatedAesCng` zijn > beschikbaar via CodePlex en vereisen CNG in het onderliggende besturingssysteem</p>

## <a id="vector-ciphers"></a>Gebruik goedgekeurde blok cipher modi en initialisatie vectoren voor symmetrische codering

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Alle symmetrische blokcodering moeten worden gebruikt met een goedgekeurde symmetrische versleutelingsmodus. De enige goedgekeurde modi zijn CBC en CTS. In het bijzonder moet de elektronische code adresboek (ECB) modus worden vermeden; gebruik van ECB moet uw organisatie Crypto mededelingenbord. Alle informatie over het gebruik van de OFB, CFB, CTR, CCM, en GCM of andere versleuteling-modus moet worden gecontroleerd door de Crypto-mededelingenbord van uw organisatie. De dezelfde initialisatievector (IV) met blokcodering in 'coderingen modi, streaming' zoals CTR, hergebruiken, kan dit ertoe leiden dat versleutelde gegevens om te worden getoond. Alle symmetrische blokcodering moeten ook worden gebruikt met een juiste initialisatievector (IV). Een juiste IV is een cryptografisch sterk, willekeurig getal en nooit een constante waarde. |

## <a id="padding"></a>Goedgekeurde asymmetrische algoritmen, sleutellengten en opvulling gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Het gebruik van cryptografische algoritmen verboden aanzienlijke risico introduceert aan beveiliging van het product en moet worden vermeden. Producten moeten alleen de cryptografische algoritmen en sleutellengten en opvulling die expliciet zijn goedgekeurd door uw organisatie Crypto Board die is gekoppeld.</p><ul><li>**RSA -** kunnen worden gebruikt voor versleuteling, sleuteluitwisseling en handtekening. RSA-versleuteling moet alleen de OAEP of RSA KEM opvulling modi gebruiken. Bestaande code kunt PKCS #1 v1.5 opvulling van de modus alleen voor compatibiliteit. Gebruik van null opvulling is expliciet verboden. Sleutels > = 2048 bits voor nieuwe code is vereist. Bestaande code ondersteunen mogelijk sleutels < 2048 bits zijn alleen voor achterwaartse compatibiliteit na een controle door de Crypto-mededelingenbord van uw organisatie. Sleutels < 1024 bits mag alleen worden gebruikt voor de oude gegevens decoderen/controleren, en moet worden vervangen als gebruikt voor versleuteling of ondertekening</li><li>**ECDSA -** voor handtekening alleen kan worden gebruikt. ECDSA met > = 256 bitssleutels is vereist voor nieuwe code. Handtekeningen op basis van ECDSA moeten een van de drie NIST goedgekeurd curven gebruiken (P-256, p-384 of P521). Curven die grondig is geanalyseerd, kunnen alleen na een overzicht met Crypto het mededelingenbord van uw organisatie worden gebruikt.</li><li>**ECDH -** voor sleuteluitwisseling alleen kan worden gebruikt. ECDH met > = 256 bitssleutels is vereist voor nieuwe code. Op basis van ECDH sleuteluitwisseling moet een van de drie NIST goedgekeurd curven gebruiken (P-256, p-384 of P521). Curven die grondig is geanalyseerd, kunnen alleen na een overzicht met Crypto het mededelingenbord van uw organisatie worden gebruikt.</li><li>**DSA -** mogelijk acceptabele na beoordeling en goedkeuring van uw organisatie Crypto mededelingenbord. Neem contact op met uw advisor beveiliging plannen van uw organisatie Crypto mededelingenbord controleren. Als uw gebruik van DSA wordt goedgekeurd, houd er rekening mee dat u wel wilt verbieden gebruik van sleutels minder dan 2048 bits lang. CNG ondersteunt 2048-bits en groter sleutellengten vanaf Windows 8.</li><li>**Diffie-Hellman -** voor sessie Sleutelbeheer alleen kan worden gebruikt. Sleutellengte > = 2048 bits voor nieuwe code is vereist. Bestaande code kan ondersteunen sleutellengten < 2048 bits zijn alleen voor achterwaartse compatibiliteit na een controle door de Crypto-mededelingenbord van uw organisatie. Sleutels < 1024 bits kan niet worden gebruikt.</li><ul>

## <a id="numgen"></a>Gebruik goedgekeurde genereren van willekeurige nummer

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Producten moeten goedgekeurde genereren van willekeurige nummer gebruiken. Pseudo-willekeurige functies zoals de rand C runtime-functie, de .NET Framework-klasse System.Random of systeemfuncties zoals GetTickCount moeten daarom nooit worden gebruikt in deze code toebehoort. Gebruik van het dubbele elliptische algoritme willekeurige getallen generator (DUAL_EC_DRBG) is niet toegestaan</p><ul><li>**CNG -** BCryptGenRandom (gebruik van de aanbevolen, tenzij de aanroeper mogelijk worden uitgevoerd op een groter dan 0 [dat wil zeggen, PASSIVE_LEVEL] IRQL BCRYPT_USE_SYSTEM_PREFERRED_RNG-vlag)</li><li>**CAPI -** cryptGenRandom</li><li>**Win32/64-** RtlGenRandom (nieuwe implementaties moeten gebruiken BCryptGenRandom of CryptGenRandom) * rand_s * SystemPrng (voor kernelmodus)</li><li>**. NET -** RNGCryptoServiceProvider of RNGCng</li><li>**Windows Store-Apps -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom of. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (willekeurige SecRandomRef size_t telling, uint8_t *bytes)</li><li>**Apple OS X (< 10.7)-** gebruik/dev/willekeurige voor het ophalen van willekeurige getallen</li><li>**Java(including Google Android Java code) -** java.security.SecureRandom-klasse. Voor Android 4.3 (geleiproducten Bean) ontwikkelaars moeten Volg de aanbevolen oplossing Android en hun toepassingen initialiseren expliciet de PRNG met entropie van /dev/urandom of /dev/random bijwerken</li></ul>|

## <a id="stream-ciphers"></a>Gebruik geen symmetrische stroom coderingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Symmetrische stroom coderingen toe, zoals RC4, mogen niet worden gebruikt. Producten moeten een blokcodering, speciaal AES met een sleutellengte van ten minste 128 bits gebruiken in plaats van de symmetrische stream-codering. |

## <a id="mac-hash"></a>Goedgekeurde MAC, HMAC/sleutelhash hash-algoritmen gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Producten moeten alleen goedgekeurde message authentication code (MAC) of op basis van het hash-code (HMAC) algoritmen voor berichtauthenticatie gebruiken.</p><p>Een message authentication code (MAC) is een stukje informatie toegevoegd aan een bericht waarmee de ontvanger om te controleren of zowel de echtheid van de afzender en de integriteit van het bericht met behulp van een geheime sleutel. Het gebruik van beide een hash op basis van een MAC ([HMAC](http://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) of [cipher-op basis van blokken MAC](http://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) is toegestaan zolang alle onderliggende hash of symmetrische versleutelingsalgoritmen zijn ook goedgekeurd voor gebruik; momenteel dit bevat de HMAC SHA2-functies (HMAC SHA256, HMAC SHA384 en HMAC SHA512) en de CMAC/OMAC1 en OMAC2 blokkeren op basis van cipher MACs (deze zijn gebaseerd op AES).</p><p>Gebruik van HMAC-SHA1 mogelijk voor compatibiliteit tussen platforms toegestane, maar is vereist voor het bestand een uitzondering op deze procedure en ondergaan Crypto revisie van uw organisatie. Het afkappen van HMAC's naar minder dan 128 bits is niet toegestaan. Met behulp van methoden met hash een sleutel en de gegevens niet is goedgekeurd en Crypto het mededelingenbord van uw organisatie moet ondergaan voordat gebruik doorlezen van de klant.</p>|

## <a id="hash-functions"></a>Gebruik alleen goedgekeurde cryptografische hash-functies

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Producten, moeten de SHA-2-familie van hash-algoritmen (SHA256, SHA384 en SHA512) gebruiken. Als een kortere hash is vereist, zoals een lengte van 128-bits uitvoer om aan te passen een gegevensstructuur die is ontworpen met een kortere MD5-hash in acht nemen, kunnen een van de hashes SHA2 (meestal SHA256) door productteams afkappen. Houd er rekening mee dat SHA384 een afgekapte versie van SHA512 gebruikt is. Het afkappen van cryptografische hashes om beveiligingsredenen naar minder dan 128 bits is niet toegestaan. Nieuwe code gebruik niet de MD2, MD4, MD5, SHA-0, SHA-1 of RIPEMD hash-algoritmen. Hash-conflicten rekenkundig uitvoerbaar zijn worden voor deze algoritmen die ze effectief wordt verbroken.</p><p>.NET-hash-algoritmen voor beheerde crypto flexibiliteit toegestaan (in volgorde van voorkeur):</p><ul><li>SHA512Cng (FIPS-compatibel)</li><li>SHA384Cng (FIPS-compatibel)</li><li>SHA256Cng (FIPS-compatibel)</li><li>SHA512Managed (niet-FIPS-compatibele) (gebruik SHA512 gebruikt als de naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA384Managed (niet-FIPS-compatibele) (gebruik SHA384 als naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA256Managed (niet-FIPS-compatibele) (gebruik SHA256 als naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA512CryptoServiceProvider (FIPS-compatibel)</li><li>SHA256CryptoServiceProvider (FIPS-compatibel)</li><li>SHA384CryptoServiceProvider (FIPS-compatibel)</li></ul>| 

## <a id="strong-db"></a>Algoritmen voor sterke versleuteling gebruiken voor het versleutelen van gegevens in de database

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Een algoritme kiezen](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Stappen** | Versleutelingsalgoritmen definiëren gegevenstransformaties die gemakkelijk kunnen niet worden teruggedraaid door onbevoegde gebruikers. SQL Server kunnen beheerders en ontwikkelaars kiezen uit verschillende algoritmen, zoals DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, 128-bits RC4, DESX, AES 128-bits, 192 bits AES en AES 256-bits |

## <a id="ssis-signed"></a>SSIS-pakketten moeten worden versleuteld en digitaal zijn ondertekend

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [De bron van de pakketten met digitale handtekeningen identificeren](https://msdn.microsoft.com/library/ms141174.aspx), [dreiging en beveiligingslek risicobeperking (integratieservices)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Stappen** | De bron van een pakket is de persoon of organisatie die het pakket hebt gemaakt. Een pakket uitgevoerd van een onbekende of niet-vertrouwde bron mogelijk riskant. Om te voorkomen dat onbevoegde knoeien met SSIS-pakketten, moeten digitale handtekeningen worden gebruikt. Om te controleren of de vertrouwelijkheid van de pakketten tijdens opslag/doorvoer, moeten SSIS-pakketten ook worden versleuteld |

## <a id="securables-db"></a>Digitale handtekening toevoegen aan database kritieke securables

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [HANDTEKENING toevoegen (Transact-SQL)](https://msdn.microsoft.com/library/ms181700) |
| **Stappen** | In gevallen waarin de integriteit van een beveiligbaar kritieke database heeft worden gecontroleerd, moeten digitale handtekeningen worden gebruikt. Database-securables zoals een opgeslagen procedure, functie, assembly of trigger kunnen digitaal worden ondertekend. Hieronder volgt een voorbeeld van wanneer dit kan handig zijn: we spreken ondersteuning aan een software die is geleverd aan een van hun klanten is opgegeven door een ISV (onafhankelijke softwareleverancier). De ISV zou voordat het bieden van ondersteuning kunt om ervoor te zorgen dat een database kunnen worden beveiligd in de software niet is geknoeid per ongeluk of door een kwaadwillende poging. Als de beveiligbare digitaal is ondertekend, kan de ISV Controleer of de digitale handtekening en de integriteit te valideren.| 

## <a id="ekm-keys"></a>SQL server EKM gebruiken om te beschermen versleutelingssleutels

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [SQL Server Extensible Key Management (EKM)](https://msdn.microsoft.com/library/bb895340), [Extensible Key Management met behulp van Azure Sleutelkluis (SQL Server)](https://msdn.microsoft.com/library/dn198405) |
| **Stappen** | SQL Server Extensible Key Management kunnen de versleutelingssleutels die de databasebestanden worden opgeslagen in een uitgeschakeld selectievakje apparaat zoals een smartcard, USB-apparaat of EKM/HSM-module te beveiligen. Hierdoor kunnen ook gegevensbeveiliging van databasebeheerders (met uitzondering van de leden van de sysadmin-groep). Gegevens kunnen worden versleuteld met behulp van coderingssleutels dat alleen de databasegebruiker toegang tot op de externe EKM/HSM-module heeft. |

## <a id="keys-engine"></a>AlwaysEncrypted-functie te gebruiken als de versleutelingssleutels niet moeten worden getoond met Database-engine

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12, MsSQL2016 |
| **Verwijzingen**              | [Altijd versleuteld (Database-Engine)](https://msdn.microsoft.com/library/mt163865) |
| **Stappen** | Versleutelde is altijd een functie ter bescherming van gevoelige gegevens, zoals creditcardnummers of nationale ID-nummers (bijvoorbeeld Amerikaans sofi-nummer), opgeslagen in Azure SQL Database of SQL Server-databases. Altijd toegestaan versleutelde clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de Database-Engine (SQL-Database of SQL Server). Als gevolg hiervan altijd versleuteld biedt een scheiding tussen degenen die eigenaar zijn van de gegevens (en het kunnen bekijken) en degenen die de gegevens te beheren (maar moet hebben geen toegang) |

## <a id="keys-iot"></a>Cryptografische sleutels veilig opslaan op IoT-apparaat

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Besturingssysteem van het apparaat - Windows-IoT-Core, connectiviteit van apparaten - SDK's Azure IoT-apparaat |
| **Verwijzingen**              | [TPM op Windows IoT Core](https://developer.microsoft.com/windows/iot/docs/tpm), [TPM op Windows IoT Core instellen](https://developer.microsoft.com/windows/iot/win10/setuptpm), [TPM Azure IoT-apparaat-SDK](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Stappen** | Symmetric of persoonlijke sleutels veilig in een hardware beveiligde opslag zoals chips TPM of een smartcard. Windows 10 IoT Core ondersteunt de gebruiker van een TPM en er zijn verschillende compatibele TPM's die kunnen worden gebruikt: https://developer.microsoft.com/windows/iot/win10/tpm. Het is raadzaam een Firmware of Discrete TPM te gebruiken. Een TPM Software moet alleen worden gebruikt voor ontwikkeling en voor testdoeleinden. Zodra een TPM beschikbaar is en de sleutels in deze zijn ingericht, moet de code die het token wordt gegenereerd zonder vaste codering gevoelige informatie in deze worden geschreven. | 

### <a name="example"></a>Voorbeeld
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on the TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Zoals u kunt zien, is de primaire sleutel voor het apparaat niet aanwezig in de code. In plaats daarvan wordt opgeslagen in de TPM op sleuf 0. TPM-apparaat genereert een tijdelijke SAS-token vervolgens gebruikt wordt voor het verbinden met de IoT-Hub. 

## <a id="random-hub"></a>Genereren van een willekeurige symmetrische sleutel lang genoeg zijn voor verificatie met IoT Hub

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloudgateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | IoT Hub bevat een apparaat-id-register en tijdens het inrichten van een apparaat genereert automatisch een willekeurige symmetrische sleutel. Het is raadzaam deze functie van de id-register van Azure IoT Hub gebruiken voor het genereren van de sleutel die wordt gebruikt voor verificatie. IoT Hub kan voor een sleutel die wordt opgegeven tijdens het maken van het apparaat. Als tijdens het inrichten buiten IoT Hub een sleutel wordt gegenereerd, wordt het aanbevolen voor het maken van een willekeurige symmetrische sleutel of ten minste 256 bits. |

## <a id="pin-remote"></a>Zorg ervoor dat een apparaat management-beleid is geïmplementeerd waarmee een gebruik PINCODE vereist is en extern wissen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele Dynamics CRM-Client | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Zorg ervoor dat een apparaat management-beleid is geïmplementeerd waarmee een gebruik PINCODE vereist is en extern wissen |

## <a id="bitlocker"></a>Zorg ervoor dat een apparaat management-beleid is geïmplementeerd die vereist dat een PINCODE of wachtwoord/automatisch vergrendelen en versleutelt alle gegevens (bijvoorbeeld Bitlocker)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM Outlook-Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Zorg ervoor dat een apparaat management-beleid is geïmplementeerd die vereist dat een PINCODE of wachtwoord/automatisch vergrendelen en versleutelt alle gegevens (bijvoorbeeld Bitlocker) |

## <a id="rolled-server"></a>Zorg ervoor dat de handtekeningsleutels zijn overgeschakeld bij gebruik van Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Van Identiteitsserver - sleutels, handtekeningen en cryptografie](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Stappen** | Zorg ervoor dat ondertekeningssleutels zijn overgeschakeld wanneer Identity-Server. De koppeling in de sectie verwijzingen wordt uitgelegd hoe dit moet worden gepland zonder dat de uitval vertrouwen op de Server van de identiteit van toepassingen. |

## <a id="client-server"></a>Zorg ervoor dat cryptografisch sterke client-ID clientgeheim zijn gebruikt in Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Zorg ervoor dat cryptografisch sterke client-ID clientgeheim in Identiteitsserver gebruikt. De volgende richtlijnen moeten worden gebruikt tijdens het genereren van een client-ID en geheim:</p><ul><li>Een willekeurige GUID genereren als de client-ID</li><li>Een cryptografisch willekeurige 256-bits sleutel genereren als het geheim</li></ul>|