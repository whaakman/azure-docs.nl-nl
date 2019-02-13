---
title: Belangrijke opmerkingen bij de Release van de kluis .NET 2.x API | Microsoft Docs
description: .NET-ontwikkelaars wordt deze API in code gebruiken voor Azure Key Vault
services: key-vault
author: bryanla
manager: barbkess
editor: bryanla
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bryanla
ms.openlocfilehash: 486f31e79210aa67c8545d6bd56f6bedd9297e9c
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105930"
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault .NET 2.0 - Release-opmerkingen en Migratiehandleiding
De volgende informatie helpt bij het migreren naar versie 2.0 van de Azure Key Vault-bibliotheek voor C# en .NET.  Apps die zijn geschreven voor oudere versies moeten worden bijgewerkt ter ondersteuning van de meest recente versie.  Deze wijzigingen nodig zijn voor een volledige nieuwe en verbeterde functies, zoals ondersteuning **Key Vault-certificaten**.

## <a name="key-vault-certificates"></a>Key Vault-certificaten

Key Vault-certificaten beheren x509 certificaten en biedt ondersteuning voor het volgende gedrag:  

* Certificaten via een proces voor het maken van Key Vault maken of bestaande certificaat importeren. Dit omvat zowel zelfondertekend en certificeringsinstantie (CA) die certificaten worden gegenereerd.
* Veilig opslaan en beheren van x509 opslag zonder tussenkomst van de privésleutelmateriaal met behulp van het certificaat.  
* Zelfs beleidsregels definiëren die Key Vault directe voor het beheren van de levensduur van het certificaat.  
* Geef de contactgegevens van de van levenscyclusgebeurtenissen, zoals een vervaldatum waarschuwingen en meldingen voor het vernieuwen.  
* Automatisch vernieuwen van certificaten met geselecteerde verleners (Key Vault partner X509 certificaatproviders en -CA's). * ondersteuning certificaat uit andere (niet-partners) biedt en certificeringsinstanties (geen ondersteuning voor automatische verlenging).  

## <a name="net-support"></a>.NET-ondersteuning

* **.NET 4.0** wordt niet ondersteund door versie 2.0 van de Azure Key Vault .NET-bibliotheek
* **.NET framework 4.5.2** wordt ondersteund door versie 2.0 van de Azure Key Vault .NET-bibliotheek
* **.NET standard 1.4** wordt ondersteund door versie 2.0 van de Azure Key Vault .NET-bibliotheek

## <a name="namespaces"></a>Naamruimten

* De naamruimte voor **modellen** is gewijzigd van **Microsoft.Azure.KeyVault** naar **Microsoft.Azure.KeyVault.Models**.
* De **Microsoft.Azure.KeyVault.Internal** naamruimte is verwijderd.
* De volgende naamruimten van de Azure SDK-afhankelijkheden 

    - **Hyak.Common** is nu **Microsoft.Rest**.
    - **Hyak.Common.Internals** is nu **Microsoft.Rest.Serialization**.

## <a name="type-changes"></a>Wijzigingen van het type

* *Geheim* gewijzigd in *SecretBundle*
* *Woordenlijst* gewijzigd in *IDictionary*
* *Lijst met<T>, string []* gewijzigd in *IList<T>*
* *NextList* gewijzigd in *NextPageLink*

## <a name="return-types"></a>Typen retourneren

* **KeyList** en **SecretList** retourneert nu *IPage<T>*  in plaats van *ListKeysResponseMessage*
* Het gegenereerde **BackupKeyAsync** retourneert nu *BackupKeyResult*, die bevat *waarde* (back-up maken van blob). De methode is eerder, verpakt en alleen de waarde geretourneerd.

## <a name="exceptions"></a>Uitzonderingen

* *KeyVaultClientException* wordt gewijzigd naar *KeyVaultErrorException*
* De servicefout gewijzigd van *uitzondering. Fout* naar *uitzondering. Body.Error.Message*.
* Aanvullende informatie verwijderd uit het foutbericht voor **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors

* In plaats van het accepteren van een *HttpClient* als een constructorargument accepteert de constructor alleen *HttpClientHandler* of *DelegatingHandler []*.

## <a name="downloaded-packages"></a>Gedownloade pakketten

Wanneer een client een Key Vault-afhankelijkheid verwerkt, worden de volgende pakketten gedownload:

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

## <a name="class-changes"></a>Wijzigingen van de klasse

* **UnixEpoch** klasse is verwijderd.
* **Base64UrlConverter** klasse is gewijzigd in **Base64UrlJsonConverter**.

## <a name="other-changes"></a>Andere wijzigingen

* Ondersteuning voor de configuratie van beleid voor KV bewerking opnieuw proberen op tijdelijke fouten toegevoegd aan deze versie van de API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Voor de bewerkingen die geretourneerd een *kluis*, het retourtype is een klasse die een **kluis** eigenschap. Het retourtype is nu *kluis*.
* *PermissionsToKeys* en *PermissionsToSecrets* zijn nu *Permissions.Keys* en *Permissions.Secrets*
* De wijzigingen van bepaalde typen retourneren die van toepassing op het besturingselement vlak ook.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Het pakket is verbroken tot **Microsoft.Azure.KeyVault.Extensions** en **Microsoft.Azure.KeyVault.Cryptography** voor de cryptografische bewerkingen.

