---
title: Belangrijke opmerkingen bij de Release van de kluis .NET 2.x API | Microsoft Docs
description: .NET-ontwikkelaars wilt deze API code voor Azure Sleutelkluis gebruiken
services: key-vault
author: lleonard-msft
manager: mbaldwin
editor: alleonar
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: alleonar
ms.openlocfilehash: a7735f8c1c4332bf2472bc83c0c37baf49019004
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Sleutelkluis .NET 2.0 - Release-opmerkingen en Migratiehandleiding
De volgende informatie kunt migreren naar versie 2.0 van de Azure Sleutelkluis-bibliotheek voor C# en .NET.  Apps die zijn geschreven voor oudere versies moeten worden bijgewerkt ter ondersteuning van de meest recente versie.  Deze wijzigingen nodig zijn voor een volledige nieuwe en verbeterde functies, zoals ondersteuning **Sleutelkluis certificaten**.

## <a name="key-vault-certificates"></a>Sleutelkluis-certificaten

Sleutelkluis certificaten beheren x509 certificaten en ondersteunt de volgende problemen:  

* Certificaten via een proces voor het maken van een Sleutelkluis maken of bestaande certificaat importeren. Dit omvat zowel zelfondertekend en certificeringsinstantie (CA) certificaten gegenereerd.
* Veilig opslaan en beheren van x509 opslag zonder tussenkomst van de privésleutelmateriaal met behulp van het certificaat.  
* Definieer de beleidsregels die Sleutelkluis directe voor het beheren van de levensduur van het certificaat.  
* Contactgegevens voor de levenscyclus van gebeurtenissen, zoals een vervaldatum waarschuwingen en meldingen voor verlenging bevatten.  
* Automatisch vernieuwen van certificaten met de geselecteerde verleners (Sleutelkluis partner X509 certificaatproviders en CA's). * certificaat voor ondersteuning van alternatieve (niet-partner) biedt en certificeringsinstanties (geen ondersteuning voor automatisch verlengen).  

## <a name="net-support"></a>Ondersteuning voor .NET

* **.NET 4.0** wordt niet ondersteund door de versie 2.0 van de Azure Key Vault .NET-bibliotheek
* **.NET framework 4.5.2** wordt ondersteund door de versie 2.0 van de Azure Key Vault .NET-bibliotheek
* **Standard 1.4 .NET** wordt ondersteund door de versie 2.0 van de Azure Key Vault .NET-bibliotheek

## <a name="namespaces"></a>Naamruimten

* De naamruimte voor **modellen** wordt gewijzigd van **Microsoft.Azure.KeyVault** naar **Microsoft.Azure.KeyVault.Models**.
* De **Microsoft.Azure.KeyVault.Internal** naamruimte is verwijderd.
* De volgende naamruimten van de Azure SDK-afhankelijkheden hebben 

    - **Hyak.Common** is nu **Microsoft.Rest**.
    - **Hyak.Common.Internals** is nu **Microsoft.Rest.Serialization**.

## <a name="type-changes"></a>Wijzigingen van het type

* *Geheim* gewijzigd in *SecretBundle*
* *Woordenlijst* gewijzigd in *IDictionary*
* *Lijst<T>, string []* gewijzigd in *IList<T>*
* *NextList* gewijzigd in *NextPageLink*

## <a name="return-types"></a>Retourtypen

* **KeyList** en **SecretList** nu retourneert *IPage<T>*  in plaats van *ListKeysResponseMessage*
* De gegenereerde **BackupKeyAsync** nu retourneert *BackupKeyResult*, die bevat *waarde* (back-up van blob). De methode is eerder ingepakt en alleen de waarde geretourneerd.

## <a name="exceptions"></a>Uitzonderingen

* *KeyVaultClientException* wordt gewijzigd naar *KeyVaultErrorException*
* De servicefout gewijzigd van *uitzondering. Fout* naar *uitzondering. Body.Error.Message*.
* Aanvullende informatie verwijderd uit het foutbericht voor **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors

* In plaats van het accepteren van een *HttpClient* als constructor-argument accepteert de constructor alleen *HttpClientHandler* of *DelegatingHandler []*.

## <a name="downloaded-packages"></a>Gedownloade pakketten

Wanneer een client een afhankelijkheid Sleutelkluis verwerkt, worden de volgende pakketten gedownload:

### <a name="previous-package-list"></a>Vorige pakketlijst

* `package id="Hyak.Common" version="1.0.2" targetFramework="net45"`
* `package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"`
* `package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"`
* `package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"`
* `package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"`
* `package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"`

### <a name="current-package-list"></a>Huidige pakketlijst

* `package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"`

## <a name="class-changes"></a>Wijzigingen voor klasse

* **UnixEpoch** klasse is verwijderd.
* **Base64UrlConverter** klasse is gewijzigd in **Base64UrlJsonConverter**.

## <a name="other-changes"></a>Andere wijzigingen

* Ondersteuning voor de configuratie van beleid voor KV bewerking opnieuw proberen op tijdelijke fouten is toegevoegd in deze versie van de API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Voor de bewerkingen die geretourneerd een *kluis*, het retourtype is een klasse die een **kluis** eigenschap. Het retourtype is nu *kluis*.
* *PermissionsToKeys* en *PermissionsToSecrets* zijn nu *Permissions.Keys* en *Permissions.Secrets*
* Bepaalde retourtypen wijzigingen aan het besturingselement-ook van toepassing.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Het pakket is verbroken tot **Microsoft.Azure.KeyVault.Extensions** en **Microsoft.Azure.KeyVault.Cryptography** voor de cryptografie-bewerkingen.

