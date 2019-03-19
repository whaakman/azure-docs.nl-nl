---
title: Cryptografie - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 608792d8389a87bad3521d3a48947b20dd036d67
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57887099"
---
# <a name="security-frame-cryptography--mitigations"></a>Beveiliging-Frame: Cryptografie | Oplossingen 

| Product/Service | Artikel |
| --------------- | ------- |
| **Web-App** | <ul><li>[Gebruik alleen goedgekeurde symmetrische blok coderingen toe en sleutellengten](#cipher-length)</li><li>[Gebruik goedgekeurde blok cipher modi en initialisatie vectoren voor symmetrische codering](#vector-ciphers)</li><li>[Gebruik goedgekeurde asymmetrische algoritmen, sleutellengten en opvulling](#padding)</li><li>[Goedgekeurde willekeurig getal generatoren gebruiken](#numgen)</li><li>[Gebruik geen symmetrische stream-codering](#stream-ciphers)</li><li>[Goedgekeurde MAC/HMAC/opgegeven hash-algoritmen gebruiken](#mac-hash)</li><li>[Alleen goedgekeurde cryptografische hash-functies gebruiken](#hash-functions)</li></ul> |
| **Database** | <ul><li>[Algoritmen voor sterke versleuteling gebruiken voor het versleutelen van gegevens in de database](#strong-db)</li><li>[SSIS-pakketten moeten worden versleuteld en digitaal zijn ondertekend](#ssis-signed)</li><li>[Digitale handtekening toevoegen aan essentiële database beveiligbare items](#securables-db)</li><li>[SQL server EKM gebruiken om te beveiligen van versleutelingssleutels](#ekm-keys)</li><li>[AlwaysEncrypted functie te gebruiken als versleutelingssleutels moeten niet worden weergegeven met Database-engine](#keys-engine)</li></ul> |
| **IoT-apparaat** | <ul><li>[Store cryptografische sleutels veilig op IoT-apparaat](#keys-iot)</li></ul> | 
| **IoT Cloud Gateway** | <ul><li>[Genereren van een willekeurige symmetrische sleutel lang genoeg zijn voor verificatie van IoT Hub](#random-hub)</li></ul> | 
| **Dynamics CRM Mobile Client** | <ul><li>[Zorg ervoor dat een apparaat management-beleid is dat een gebruik PINCODE is vereist en kan extern wissen](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook Client** | <ul><li>[Zorg ervoor dat een apparaat management-beleid is ingesteld dat een PINCODE/wachtwoord/automatische vergrendeling is vereist en codeert alle gegevens (bijvoorbeeld BitLocker)](#bitlocker)</li></ul> | 
| **Identiteitsserver** | <ul><li>[Zorg ervoor dat ondersteuningssleutels worden meegenomen bij het gebruik van Identiteitsserver](#rolled-server)</li><li>[Zorg ervoor dat cryptografisch sterke client-ID, client secret zijn gebruikt in Identiteitsserver](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Gebruik alleen goedgekeurde symmetrische blok coderingen toe en sleutellengten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Producten moeten gebruiken alleen de symmetrische blok coderingen toe en de bijbehorende sleutellengte die expliciet zijn goedgekeurd door de Crypto-Advisor in uw organisatie. Goedgekeurde symmetrische algoritmen van Microsoft zijn onder andere het volgende blok coderingen toe:</p><ul><li>Voor nieuwe code AES-128, AES-192 en AES-256 worden geaccepteerd</li><li>Voor achterwaartse compatibiliteit met bestaande code is drie-sleutel 3DES acceptabel</li><li>Voor producten die met behulp van de symmetrische blok coderingen toe:<ul><li>Geavanceerde Encryption Standard (AES) is vereist voor de nieuwe code</li><li>Drie-sleutel triple Data Encryption Standard (3DES) is toegestaan in de bestaande code voor achterwaartse compatibiliteit</li><li>Alle andere blok coderingen toe, met inbegrip van RC2, DES, 2 sleutel 3DES, DESX en gestreepte, kunnen alleen worden gebruikt voor het ontsleutelen van oude gegevens en moeten worden vervangen als gebruikt voor versleuteling</li></ul></li><li>Voor blok symmetrische versleutelingsalgoritmen is een minimum sleutellengte van 128 bits vereist. Het alleen blok-versleutelingsalgoritme aanbevolen voor nieuwe code AES is (AES-128, AES-192 en AES-256 zijn alle aanvaardbaar)</li><li>Drie-sleutel 3DES is momenteel acceptabel als deze al in gebruik in de bestaande code te wijzigen; overgang naar AES wordt aanbevolen. DES, DESX RC2 en gestreepte worden niet meer als veilig beschouwd. Deze algoritmen kunnen alleen worden gebruikt voor het ontsleutelen van bestaande gegevens ten behoeve van achterwaartse compatibiliteit en gegevens opnieuw moeten worden versleuteld met behulp van een aanbevolen blokcodering</li></ul><p>Houd er rekening mee dat alle symmetrische blokversleutelingsmethoden moeten worden gebruikt met een goedgekeurde cipher-modus, waarbij gebruik van een juiste initialisatievector (IV) is vereist. Een juiste IV worden gebruikt, is doorgaans een willekeurig getal en nooit een constante waarde</p><p>Het gebruik van verouderde of anderszins niet-goedgekeurde cryptografische algoritmen en sleutellengten voor kleinere voor het lezen van bestaande gegevens (in plaats van nieuwe gegevens schrijven) mogen nadat uw organisatie Crypto-bord. U moet echter het bestand voor een uitzondering op basis van deze vereiste. Bovendien moeten producten in enterprise-implementaties overwegen van waarschuwing beheerders wanneer zwakke crypto wordt gebruikt om gegevens te lezen. Deze waarschuwingen moeten verklarende en bruikbare. In sommige gevallen kan het geschikt is voor het beheren van het gebruik van zwakke crypto Groepsbeleid zijn</p><p>.NET-algoritmen voor beheerde crypto flexibiliteit toegestaan (in volgorde van voorkeur)</p><ul><li>AesCng (FIPS-compatibel)</li><li>AuthenticatedAesCng (FIPS-compatibel)</li><li>AESCryptoServiceProvider (FIPS-compatibel)</li><li>AESManaged (niet-FIPS-compatibel)</li></ul><p>Houd er rekening mee dat geen van deze algoritmen kan worden opgegeven de `SymmetricAlgorithm.Create` of `CryptoConfig.CreateFromName` methoden zonder wijzigingen aanbrengen in het machine.config-bestand. Merk ook op AES in versies van .NET voordat u .NET 3.5 heet `RijndaelManaged`, en `AesCng` en `AuthenticatedAesCng` zijn > beschikbaar via CodePlex en CNG in het onderliggende besturingssysteem vereisen</p>

## <a id="vector-ciphers"></a>Gebruik goedgekeurde blok cipher modi en initialisatie vectoren voor symmetrische codering

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Alle symmetrische blokversleutelingsmethoden moeten worden gebruikt met een modus voor goedgekeurde symmetrische codering. De enige goedgekeurde modi zijn CBC en CTS. In het bijzonder moet de elektronische code book (ECB)-modus van de bewerking worden vermeden; gebruik van ECB vereist Crypto-bord-beoordeling van uw organisatie. Extra gebruik van de OFB, CFB, CTR, CCM, en GCM of andere versleutelingsmodus moet worden gecontroleerd door de Crypto-bord van uw organisatie. Opnieuw gebruiken van de dezelfde initialisatievector (IV) met blok coderingen toe in de 'modi voor codering, streaming' zoals CTR, kan ertoe leiden dat versleutelde gegevens om te worden aan het licht komt. Alle symmetrische blokversleutelingsmethoden moeten ook worden gebruikt met een juiste initialisatievector (IV). Een juiste IV is een cryptografisch sterk, willekeurig getal en nooit een constante waarde. |

## <a id="padding"></a>Gebruik goedgekeurde asymmetrische algoritmen, sleutellengten en opvulling

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Het gebruik van cryptografische algoritmen uitgesloten introduceert aanzienlijke risico's voor beveiliging van het product en moet worden vermeden. Producten moeten gebruiken alleen de cryptografische algoritmen en bijbehorende sleutellengten en opvulling die expliciet zijn goedgekeurd door uw organisatie Crypto-bord.</p><ul><li>**RSA -** kunnen worden gebruikt voor versleuteling, sleuteluitwisseling en handtekening. RSA-versleuteling moet alleen de OAEP of RSA-KEM opvulling modi gebruiken. Bestaande code kan PKCS #1 v1.5 opvulling van de modus alleen voor compatibiliteit gebruiken. Gebruik van null opvulling is uitdrukkelijk verboden. Sleutels > = 2048 bits is vereist voor de nieuwe code. Bestaande code kan ondersteunen sleutels < 2048 bits alleen voor achterwaartse compatibiliteit na een overzicht van uw organisatie Crypto-bord. Sleutels < 1024 bits kan alleen worden gebruikt voor de oude gegevens ontsleutelen/controleren, en moet worden vervangen als gebruikt voor versleuteling of ondertekening</li><li>**ECDSA -** voor handtekening alleen kan worden gebruikt. ECDSA met > = 256-bits sleutels voor de nieuwe code is vereist. Handtekeningen op basis van ECDSA moeten een van de drie NIST goedgekeurd curven gebruiken (p-256, P-384 of P521). Curven die grondig is geanalyseerd kunnen alleen na een overzicht met Crypto-bord van uw organisatie worden gebruikt.</li><li>**ECDH -** voor sleuteluitwisseling alleen kan worden gebruikt. ECDH met > = 256-bits sleutels voor de nieuwe code is vereist. Op basis van een ECDH sleuteluitwisseling moet een van de drie NIST goedgekeurd curven gebruiken (p-256, P-384 of P521). Curven die grondig is geanalyseerd kunnen alleen na een overzicht met Crypto-bord van uw organisatie worden gebruikt.</li><li>**DSA -** na beoordeling en goedkeuring van uw organisatie Crypto-bord acceptabel zijn. Neem contact op met uw security advisor om te plannen van uw organisatie Crypto-bord controleren. Als uw gebruik van DSA is goedgekeurd, houd er rekening mee dat u moet minder dan 2048 bits lang verbiedt het gebruik van sleutels. CNG ondersteunt 2048-bits en groter sleutellengten vanaf Windows 8.</li><li>**Diffie-Hellman -** voor sessie Sleutelbeheer alleen kan worden gebruikt. Sleutellengte > = 2048 bits is vereist voor de nieuwe code. Bestaande code kan ondersteunen sleutellengten < 2048 bits alleen voor achterwaartse compatibiliteit na een overzicht van uw organisatie Crypto-bord. Sleutels < 1024 bits niet kan worden gebruikt.</li><ul>

## <a id="numgen"></a>Goedgekeurde willekeurig getal generatoren gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Producten moeten goedgekeurde willekeurig getal generatoren gebruiken. Pseudo-willekeurige functies, zoals de C-runtime functie rand, .NET Framework-klasse System.Random of systeemfuncties zoals GetTickCount moeten daarom nooit worden gebruikt in deze code. Gebruik van het dubbele elliptische algoritme willekeurige getallen generator (DUAL_EC_DRBG) is niet toegestaan</p><ul><li>**CNG -** BCryptGenRandom (gebruik van de aanbevolen, tenzij de oproepende functie mogelijk worden uitgevoerd op een groter dan 0 [dat wil zeggen, PASSIVE_LEVEL] IRLQ BCRYPT_USE_SYSTEM_PREFERRED_RNG-markering)</li><li>**CAPI -** cryptGenRandom</li><li>**Win32/64-** RtlGenRandom (nieuwe implementaties moeten gebruik BCryptGenRandom of CryptGenRandom) * rand_s * SystemPrng (voor de kernelmodus)</li><li>**. NET -** RNGCryptoServiceProvider of RNGCng</li><li>**Windows Store-Apps -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom of. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (willekeurig, SecRandomRef size_t count, uint8_t \*bytes)</li><li>**Apple OS X (< 10.7)-** gebruik/dev/willekeurige voor het ophalen van willekeurige getallen</li><li>**Java(including Google Android Java code) -** java.security.SecureRandom klasse. Let erop dat voor Android 4.3 (geleiproducten Bean) ontwikkelaars moeten gaat u als de aanbevolen oplossing voor Android volgt en hun toepassingen expliciet initialiseren de PRNG met entropie van /dev/urandom of /dev/random bijwerken</li></ul>|

## <a id="stream-ciphers"></a>Gebruik geen symmetrische stream-codering

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Symmetrische stream coderingen toe, zoals RC4, mogen niet worden gebruikt. In plaats van de symmetrische stream coderingen toe, moeten de producten een cipher block, specifiek AES met een sleutellengte van ten minste 128 bits gebruiken. |

## <a id="mac-hash"></a>Goedgekeurde MAC/HMAC/opgegeven hash-algoritmen gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Producten moeten alleen goedgekeurde code message authentication code (MAC) of hash-gebaseerde message authentication code (HMAC) algoritmen gebruiken.</p><p>Een message authentication code (MAC) is een stukje informatie die is gekoppeld aan een bericht waarmee de ontvanger om te controleren of zowel de authenticiteit van de afzender en de integriteit van het bericht met behulp van een geheime sleutel. Het gebruik van een van beide een hash-gebaseerde MAC ([HMAC](https://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) of [MAC cipher-op basis van blokken](https://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) is toegestaan als lange als alle onderliggende hash of symmetrische codering algoritmen ook zijn goedgekeurd voor gebruik; momenteel dit omvat de functies van de HMAC-SHA2 (HMAC-SHA256, HMAC-SHA384 en SHA512 HMAC-gebruikt) en de vereist/OMAC1 OMAC2 blokkeren op basis van cipher MACs (deze zijn gebaseerd op AES).</p><p>Gebruik van de HMAC-SHA1 mogelijk toelaatbaar voor compatibiliteit tussen platforms, maar kunt u zich een uitzondering op deze procedure het bestand en ondergaan cryptografische controle van uw organisatie. Het afkappen van HMAC's naar minder dan 128 bits is niet toegestaan. Met behulp van methoden op hash een sleutel en de gegevens niet is goedgekeurd en van uw organisatie Crypto-bord moet ondergaan voordat gebruik doorlezen van de klant.</p>|

## <a id="hash-functions"></a>Alleen goedgekeurde cryptografische hash-functies gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Producten moeten de SHA-2-familie van hash-algoritmen (SHA256, SHA384 en SHA512) gebruiken. Als een kortere hash is vereist, zoals een lengte van 128-bits-uitvoer als u wilt aanpassen aan een gegevensstructuur die is ontworpen met de kortere MD5-hash Denk eraan dat u mogelijk een van de hashes SHA2 (meestal SHA256) productteams afkappen. Houd er rekening mee dat SHA384 een afgekapte versie van SHA512 gebruikt is. Het afkappen van cryptografische hashes om veiligheidsredenen naar minder dan 128 bits is niet toegestaan. Nieuwe code gebruik niet de MD2, MD4, MD5, SHA-0, SHA-1 of RIPEMD hash-algoritmen. Hash-conflicten zijn rekenkundig uitvoerbaar zijn voor deze algoritmen die ze effectief wordt verbroken.</p><p>.NET-hash-algoritmen voor beheerde crypto flexibiliteit toegestaan (in volgorde van voorkeur):</p><ul><li>SHA512Cng (FIPS-compatibel)</li><li>SHA384Cng (FIPS-compatibel)</li><li>SHA256Cng (FIPS-compatibel)</li><li>SHA512Managed (niet-FIPS-compatibel) (gebruik SHA512 gebruikt als naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA384Managed (niet-FIPS-compatibel) (gebruik SHA384 als naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA256Managed (niet-FIPS-compatibel) (gebruik SHA256 als naam van algoritme in aanroepen naar HashAlgorithm.Create of CryptoConfig.CreateFromName)</li><li>SHA512CryptoServiceProvider (FIPS-compatibel)</li><li>SHA256CryptoServiceProvider (FIPS-compatibel)</li><li>SHA384CryptoServiceProvider (FIPS-compatibel)</li></ul>| 

## <a id="strong-db"></a>Algoritmen voor sterke versleuteling gebruiken voor het versleutelen van gegevens in de database

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Een coderingsalgoritme kiezen](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Stappen** | Versleutelingsalgoritmen definiëren gegevenstransformaties die gemakkelijk kunnen niet worden teruggedraaid door onbevoegde gebruikers. SQL Server kunnen beheerders en ontwikkelaars kiezen uit verschillende algoritmen, waaronder DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, 128-bits RC4, DESX, AES 128-bits, 192-bits AES en AES 256-bits |

## <a id="ssis-signed"></a>SSIS-pakketten moeten worden versleuteld en digitaal zijn ondertekend

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Identificeer de bron van de pakketten met digitale handtekeningen](https://msdn.microsoft.com/library/ms141174.aspx), [bedreiging en beveiligingsproblemen risicobeperking (integratieservices)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Stappen** | De bron van een pakket is de persoon of organisatie die het pakket hebt gemaakt. Uitvoeren van een pakket van een onbekende of niet-vertrouwde bron mogelijk riskant. Om te voorkomen dat onbevoegde knoeien met de SSIS-pakketten, moeten digitale handtekeningen worden gebruikt. Ook om te controleren of de vertrouwelijkheid van de pakketten tijdens opslag/doorvoer, SSIS-pakketten moeten worden versleuteld |

## <a id="securables-db"></a>Digitale handtekening toevoegen aan essentiële database beveiligbare items

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [HANDTEKENING (Transact-SQL) toevoegen](https://msdn.microsoft.com/library/ms181700) |
| **Stappen** | In gevallen waarin de integriteit van een beveiligbaar kritieke database heeft om te worden geverifieerd, moeten digitale handtekeningen worden gebruikt. Database beveiligbare items zoals een opgeslagen procedure, functie, assembly of trigger kunnen digitaal worden ondertekend. Hieronder volgt een voorbeeld van wanneer dit kan nuttig zijn: Laat het ons dat u dat een ISV (Independent Software Vendor) ondersteuning voor een software die worden geleverd aan een van hun klanten heeft geleverd. De ISV wilt voordat er ondersteuning wordt geboden, om ervoor te zorgen dat een database kunnen worden beveiligd in de software niet is geknoeid, per ongeluk of door een kwaadwillende poging. Als de beveiligbare digitaal is ondertekend, kan de ISV controleren of de digitale handtekening en de integriteit ervan valideren.| 

## <a id="ekm-keys"></a>SQL server EKM gebruiken om te beveiligen van versleutelingssleutels

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [SQL Server Extensible Key Management (EKM)](https://msdn.microsoft.com/library/bb895340), [uitbreidbaar Sleutelbeheer met Azure Key Vault (SQL Server)](https://msdn.microsoft.com/library/dn198405) |
| **Stappen** | SQL Server Extensible Key Management kunt de versleutelingssleutels waarmee de databasebestanden worden opgeslagen in een uit box-apparaat zoals een smartcard, USB-apparaat of EKM/HSM-module worden beveiligd. Hierdoor kunnen ook gegevensbescherming van databasebeheerders (met uitzondering van leden van de sysadmin-groep). Gegevens kunnen worden versleuteld met behulp van coderingssleutels dat alleen de databasegebruiker toegang tot op de externe EKM/HSM-module heeft. |

## <a id="keys-engine"></a>AlwaysEncrypted functie te gebruiken als versleutelingssleutels moeten niet worden weergegeven met Database-engine

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure, OnPrem |
| **Kenmerken**              | SQL-versie - V12 MsSQL2016 |
| **Verwijzingen**              | [Altijd versleuteld (Database-Engine)](https://msdn.microsoft.com/library/mt163865) |
| **Stappen** | Altijd is versleuteld een functie die is ontworpen voor het beveiligen van gevoelige gegevens, zoals creditcardnummers of nationale identificatienummers (bijvoorbeeld VS burgerservicenummers), die zijn opgeslagen in Azure SQL Database of SQL Server-databases. Altijd kunt versleutelde clients voor het versleutelen van gevoelige gegevens binnen clienttoepassingen en nooit onthullen de versleutelingssleutels voor de Database-Engine (SQL-Database of SQL Server). Als gevolg hiervan Always Encrypted biedt een scheiding tussen personen die eigenaar zijn van de gegevens (en het kunnen bekijken) en degenen die de gegevens beheren (maar geen toegang mogen hebben) |

## <a id="keys-iot"></a>Store cryptografische sleutels veilig op IoT-apparaat

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Besturingssysteem van het apparaat - Windows IoT Core, verbindingsmogelijkheden voor apparaten - Azure IoT device-SDK 's |
| **Verwijzingen**              | [TPM op Windows IoT Core](https://developer.microsoft.com/windows/iot/docs/tpm), [instellen van de TPM op Windows IoT Core](https://docs.microsoft.com/windows/iot-core/secure-your-device/setuptpm), [Azure IoT Device SDK TPM](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Stappen** | Symmetric of persoonlijke sleutels veilig op in een hardware-beveiligde opslag zoals TPM of een smartcard-chips. Windows 10 IoT Core biedt ondersteuning voor de gebruiker van een TPM en er zijn verschillende compatibele TPM's die kunnen worden gebruikt: https://docs.microsoft.com/windows/iot-core/secure-your-device/tpm#discrete-tpm-dtpm. Het verdient aanbeveling om een Firmware of Discrete TPM te gebruiken. Een TPM Software moet alleen worden gebruikt voor ontwikkeling en tests. Zodra een TPM beschikbaar is en de sleutels in deze zijn ingericht, moet de code waarmee het token wordt gegenereerd zonder code gevoelige informatie in deze worden geschreven. | 

### <a name="example"></a>Voorbeeld
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on the TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Zoals u kunt zien, is primaire sleutel voor het apparaat niet aanwezig in de code. In plaats daarvan wordt deze opgeslagen in de TPM in sleuf 0. TPM-apparaat genereert een eenvoudige SAS-token dat vervolgens gebruikt wordt om verbinding maken met de IoT-Hub. 

## <a id="random-hub"></a>Genereren van een willekeurige symmetrische sleutel lang genoeg zijn voor verificatie van IoT Hub

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Keuze van de gateway - Azure IoT Hub |
| **Verwijzingen**              | N/A  |
| **Stappen** | IoT Hub bevat een apparaat-id-register en tijdens het inrichten van een apparaat, genereert automatisch een willekeurige symmetrische sleutel. Het verdient aanbeveling om deze functie van de Azure IoT Hub-Identiteitsregister te genereren van de sleutel die wordt gebruikt voor verificatie. IoT Hub kan ook een sleutel worden opgegeven tijdens het maken van het apparaat. Als tijdens het inrichten van apparaten buiten de IoT Hub een sleutel wordt gegenereerd, is het aanbevolen om een willekeurige symmetrische sleutel of ten minste 256-bits te maken. |

## <a id="pin-remote"></a>Zorg ervoor dat een apparaat management-beleid is dat een gebruik PINCODE is vereist en kan extern wissen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM Mobile Client | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat een apparaat management-beleid is dat een gebruik PINCODE is vereist en kan extern wissen |

## <a id="bitlocker"></a>Zorg ervoor dat een apparaat management-beleid is ingesteld dat een PINCODE/wachtwoord/automatische vergrendeling is vereist en codeert alle gegevens (bijvoorbeeld BitLocker)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM Outlook Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Zorg ervoor dat een apparaat management-beleid is ingesteld dat een PINCODE/wachtwoord/automatische vergrendeling is vereist en codeert alle gegevens (bijvoorbeeld BitLocker) |

## <a id="rolled-server"></a>Zorg ervoor dat ondersteuningssleutels worden meegenomen bij het gebruik van Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Identiteitsserver - sleutels, handtekeningen en cryptografische](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Stappen** | Zorg ervoor dat ondersteuningssleutels worden meegenomen bij het gebruik van identiteit-Server. De koppeling in de sectie verwijzingen wordt uitgelegd hoe dit moet worden gepland zonder onderbrekingen voor toepassingen die afhankelijk zijn van identiteit-Server. |

## <a id="client-server"></a>Zorg ervoor dat cryptografisch sterke client-ID, client secret zijn gebruikt in Identiteitsserver

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Zorg ervoor dat cryptografisch sterke client-ID, client secret in Identiteitsserver gebruikt. De volgende richtlijnen moeten worden gebruikt tijdens het genereren van een client-ID en -geheim:</p><ul><li>Een willekeurige GUID genereren als de client-ID</li><li>Een cryptografisch willekeurige 256-bits sleutel als het geheim genereren</li></ul>|
