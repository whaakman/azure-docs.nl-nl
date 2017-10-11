---
title: Belangrijke opmerkingen bij de Release van de kluis .NET 2.x API | Microsoft Docs
description: .NET-ontwikkelaars wilt deze API code voor Azure Sleutelkluis gebruiken
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: c5b5fd7f16faf17d16ecc82269fb1264adf4dd06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Sleutelkluis .NET 2.0 - Release-opmerkingen en Migratiehandleiding
De volgende opmerkingen en richtlijnen zijn voor ontwikkelaars die werken met Azure Key Vault .NET / C#-bibliotheek. In de overgang van versie 1.0 naar versie 2.0, een aantal updates zijn aangebracht die wordt vereist migratie werk in uw code in om te kunnen profiteren van de functionele verbeteringen en toevoegingen, zoals functies **Sleutelkluis-certificaten**  ondersteunen.

## <a name="key-vault-certificates"></a>Sleutelkluis-certificaten

Sleutelkluis certificaten ondersteuning biedt voor het beheren van uw x509 certificaten en de volgende problemen:  

* Kan de eigenaar van een certificaat om een certificaat via een proces voor het maken van Sleutelkluis of het importeren van een bestaand certificaat te maken. Dit omvat zowel zelfondertekend en de certificeringsinstantie die certificaten worden gegenereerd.
* Hiermee kunt u de eigenaar van een Sleutelkluis-certificaat voor het implementeren van veilige opslag en beheer van X509 certificaten zonder interactie met privésleutelmateriaal.  
* Kan de eigenaar van een certificaat om een beleid waarin wordt verwezen Sleutelkluis voor het beheren van de levenscyclus van een certificaat te maken.  
* Kan certificaat eigenaren van contactgegevens voor melding over de levenscyclus van gebeurtenissen van verlopen en verlenging van het certificaat opgeven.  
* Ondersteunt automatische vernieuwing met geselecteerde verleners - Sleutelkluis partner X509 providers van het certificaat / certificeringsinstanties.
  
  * Opmerking: providers/instanties niet in de samenwerking ook zijn toegestaan, maar geen ondersteuning voor de functie voor automatisch vernieuwen.

## <a name="net-support"></a>Ondersteuning voor .NET

* **.NET 4.0** wordt niet ondersteund door de versie 2.0 van .NET Azure Key Vault / C#-bibliotheek
* **.NET core** wordt ondersteund door de versie 2.0 van .NET Azure Key Vault / C#-bibliotheek

## <a name="namespaces"></a>Naamruimten

* De naamruimte voor **modellen** wordt gewijzigd van **Microsoft.Azure.KeyVault** naar **Microsoft.Azure.KeyVault.Models**.
* De **Microsoft.Azure.KeyVault.Internal** naamruimte is verwijderd.
* De naamruimte Azure SDK-afhankelijkheden zijn gewijzigd van **Hyak.Common** en **Hyak.Common.Internals** naar **Microsoft.Rest** en  **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Wijzigingen van het type

* *Geheim* gewijzigd in *SecretBundle*
* *Woordenlijst* gewijzigd in *IDictionary*
* *Lijst<T>, string []* gewijzigd in *IList<T>*
* *NextList* gewijzigd in *NextPageLink*

## <a name="return-types"></a>Retourtypen

* **KeyList** en **SecretList** retourneert *IPage<T>*  in plaats van *ListKeysResponseMessage*
* De gegenereerde **BackupKeyAsync** retourneert *BackupKeyResult* die bevat *waarde* (back-up van blob). Voordat de methode is ingepakt en alleen de waarde te retourneren.

## <a name="exceptions"></a>Uitzonderingen

* *KeyVaultClientException* wordt gewijzigd naar *KeyVaultErrorException*
* De servicefout is gewijzigd van *uitzondering. Fout* naar *uitzondering. Body.Error.Message*.
* Aanvullende informatie verwijderd uit het foutbericht voor **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors

* In plaats van het accepteren van een *HttpClient* als constructor-argument accepteert de constructor alleen *HttpClientHandler* of *DelegatingHandler []*.

## <a name="downloaded-packages"></a>Gedownloade pakketten

Wanneer een client een afhankelijkheid van Sleutelkluis verwerkt zijn de volgende gedownload

### <a name="previous-package-list"></a>Vorige pakketlijst

* versie van het pakket id="Hyak.Common' = '1.0.2' targetFramework ="net45"
* versie van het pakket id="Microsoft.Azure.Common' = '2.0.4' targetFramework ="net45"
* versie van het pakket id="Microsoft.Azure.Common.Dependencies' = '1.0.0' targetFramework ="net45"
* versie van het pakket id="Microsoft.Azure.KeyVault' = '1.0.0' targetFramework ="net45"
* versie van het pakket id="Microsoft.Bcl' = '1.1.9' targetFramework ="net45"
* versie van het pakket id="Microsoft.Bcl.Async' = '1.0.168' targetFramework ="net45"
* versie van het pakket id="Microsoft.Bcl.Build' = '1.0.14' targetFramework ="net45"
* versie van het pakket id="Microsoft.Net.Http' = '2.2.22' targetFramework ="net45"

### <a name="current-package-list"></a>Huidige pakketlijst

* versie van het pakket id="Microsoft.Azure.KeyVault' = '2.0.0-preview' targetFramework ="net45"
* versie van het pakket id="Microsoft.Rest.ClientRuntime' = '2.2.0' targetFramework ="net45"
* versie van het pakket id="Microsoft.Rest.ClientRuntime.Azure' = '3.2.0' targetFramework ="net45"

## <a name="class-changes"></a>Wijzigingen voor klasse

* **UnixEpoch** klasse is verwijderd
* **Base64UrlConverter** klasse is gewijzigd in **Base64UrlJsonConverter**

## <a name="other-changes"></a>Andere wijzigingen

* Ondersteuning voor de configuratie van beleid voor KV bewerking opnieuw proberen op tijdelijke fouten is toegevoegd in deze versie van de API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Voor de bewerkingen die geretourneerd een *kluis*, het retourtype is een klasse die deel uitmaakt van een eigenschap van de kluis. Het retourtype is nu *kluis*.
* *PermissionsToKeys* en *PermissionsToSecrets* zijn nu *Permissions.Keys* en *Permissions.Secrets*
* Enkele wijzigingen retourtypen aan het besturingselement-ook van toepassing.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Het pakket is verbroken tot **Microsoft.Azure.KeyVault.Extensions** en **Microsoft.Azure.KeyVault.Cryptography** voor de cryptografie-bewerkingen.

