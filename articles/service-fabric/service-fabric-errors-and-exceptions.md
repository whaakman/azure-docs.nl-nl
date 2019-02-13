---
title: Veelvoorkomende FabricClient-uitzonderingen | Microsoft Docs
description: Beschrijving van de algemene uitzonderingen en fouten die door de APIs FabricClient kunnen worden gegenereerd tijdens het uitvoeren van de toepassing en cluster-beheerbewerkingen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/20/2018
ms.author: ryanwi
ms.openlocfilehash: fae345530a28bc7fdb875570546b125b13a79310
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104029"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Algemene uitzonderingen en fouten bij het werken met de APIs FabricClient
De [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API's stellen beheerders in staat cluster en de toepassing administratieve taken uitvoeren op een Service Fabric-toepassing, service of -cluster. Bijvoorbeeld, toepassingsimplementatie, bijwerken en verwijderen, de status van een cluster controleren, of het testen van een service. Ontwikkelaars van toepassingen en clusterbeheerders kunnen de APIs FabricClient gebruiken voor het ontwikkelen van hulpprogramma's voor het beheren van de Service Fabric-cluster en toepassingen.

Er zijn veel verschillende soorten bewerkingen die kunnen worden uitgevoerd met behulp van FabricClient.  Elke methode kunt uitzonderingen voor fouten als gevolg van onjuiste invoer, runtime-fouten of problemen met tijdelijke infrastructuur genereert.  Zie de documentatie van de API-verwijzing om te zoeken welke uitzonderingen worden veroorzaakt door een specifieke methode. Er zijn enkele uitzonderingen, maar kunnen worden gegenereerd door veel verschillende [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API's. De volgende tabel bevat de uitzonderingen die door de APIs FabricClient gedeeld worden.

| Uitzondering | Gegenereerd wanneer |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception) |De [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) object bevindt zich in een status gesloten. Buitengebruikstelling van het [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) object u gebruikt en een nieuwe [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) object. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception) |Er is een time-out opgetreden voor de bewerking. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) wordt geretourneerd wanneer de bewerking duurt langer dan MaxOperationTimeout om te voltooien. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception) |De toegangscontrole voor de bewerking is mislukt. E_ACCESSDENIED wordt geretourneerd. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) |Er is een runtime-fout opgetreden tijdens het uitvoeren van de bewerking. Een van de methoden FabricClient mogelijk kunt throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception), wordt de [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception.ErrorCode) eigenschap geeft de exacte oorzaak van de uitzondering. Foutcodes zijn gedefinieerd in de [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) opsomming. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception) |De bewerking is mislukt vanwege een tijdelijke fout van een of andere. Een bewerking kan bijvoorbeeld mislukken omdat een quorum van replica's tijdelijk niet bereikbaar is. Tijdelijke uitzonderingen komen overeen met mislukte bewerkingen die opnieuw kunnen worden verstuurd. |

Sommige algemene [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) fouten die kunnen worden geretourneerd in een [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception):

| Fout | Voorwaarde |
| --- |:--- |
| CommunicationError |De bewerking is mislukt, probeer het opnieuw, vanwege een communicatiefout. |
| InvalidCredentialType |Het referentietype is ongeldig. |
| InvalidX509FindType |De X509FindType is ongeldig. |
| InvalidX509StoreLocation |De X509 opslaglocatie is ongeldig. |
| InvalidX509StoreName |De X509 Opslagnaam is ongeldig. |
| InvalidX509Thumbprint |De X509 vingerafdruk van certificaat is ongeldig. |
| InvalidProtectionLevel |Het beveiligingsniveau is ongeldig. |
| InvalidX509Store |De X509 certificaatarchief kan niet worden geopend. |
| InvalidSubjectName |Naam van de certificaathouder is ongeldig. |
| InvalidAllowedCommonNameList |De indeling van tekenreeks van de lijst met algemene naam is ongeldig. Dit moet een door komma's gescheiden lijst. |

