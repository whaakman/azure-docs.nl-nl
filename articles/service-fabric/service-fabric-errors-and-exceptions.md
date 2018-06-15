---
title: Algemene FabricClient uitzonderingen | Microsoft Docs
description: Beschrijft de algemene uitzonderingen en fouten die door de FabricClient APIs kunnen worden gegenereerd tijdens het uitvoeren van de toepassing en clusterbeheerbewerkingen.
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
ms.date: 12/18/2017
ms.author: ryanwi
ms.openlocfilehash: 31defde289643b2adfd6e15bb8417b5498ba65d1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212497"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Algemene uitzonderingen en fouten bij het werken met de FabricClient APIs
De [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) -API's kunnen beheerders cluster en de toepassing administratieve taken uitvoeren op een Service Fabric-toepassing, een service of een cluster. Bijvoorbeeld: implementatie van de toepassing, upgrade en verwijderen van een service te testen of controleren van de status van een cluster. Ontwikkelaars en clusterbeheerders kunnen u de FabricClient APIs gebruiken voor het ontwikkelen van hulpprogramma's voor het beheren van de Service Fabric-cluster en toepassingen.

Er zijn veel verschillende soorten bewerkingen waarvoor kunnen worden uitgevoerd met behulp van FabricClient.  Elke methode kunt uitzonderingen op fouten als gevolg van onjuiste invoer, runtime-fouten of problemen van voorbijgaande aard infrastructuur genereert.  Zie de documentatie van de API-verwijzing om te zoeken welke uitzonderingen worden veroorzaakt door een specifieke methode. Er zijn enkele uitzonderingen, maar die kunnen worden gegenereerd door veel verschillende [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API's. De volgende tabel bevat de uitzonderingen die door de FabricClient APIs gedeeld worden.

| Uitzondering | Gegenereerd wanneer |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |De [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) bevindt zich in een gesloten. Buitengebruikstelling van het [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) object u gebruikt en een nieuwe [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) object. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |Er is een time-out opgetreden voor de bewerking. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) wordt geretourneerd wanneer de bewerking langer dan MaxOperationTimeout duurt te voltooien. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |De toegangscontrole voor de bewerking is mislukt. E_ACCESSDENIED wordt geretourneerd. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |Er is een runtime-fout opgetreden tijdens het uitvoeren van de bewerking. Een van de methoden FabricClient kunt mogelijk de throw [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), wordt de [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) eigenschap geeft u de exacte oorzaak van de uitzondering. Foutcodes zijn gedefinieerd in de [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) opsomming. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |De bewerking is mislukt wegens een tijdelijke fout type. Bijvoorbeeld: een bewerking mislukken omdat een quorum van replica's tijdelijk niet bereikbaar is. Tijdelijke uitzonderingen overeenkomen met de mislukte bewerkingen die kunnen opnieuw worden uitgevoerd. |

Sommige algemene [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) fouten die kunnen worden geretourneerd in een [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Fout | Voorwaarde |
| --- |:--- |
| CommunicationError |Een communicatiefout is de bewerking is mislukt, probeer het opnieuw. |
| InvalidCredentialType |Het referentietype is ongeldig. |
| InvalidX509FindType |De X509FindType is ongeldig. |
| InvalidX509StoreLocation |De X509 opslaglocatie is ongeldig. |
| InvalidX509StoreName |De X509 Opslagnaam is ongeldig. |
| InvalidX509Thumbprint |De X509 tekenreeks voor de vingerafdruk van certificaat is ongeldig. |
| InvalidProtectionLevel |Het beveiligingsniveau is ongeldig. |
| InvalidX509Store |De X509 certificaatarchief kan niet worden geopend. |
| InvalidSubjectName |Naam van de certificaathouder is ongeldig. |
| InvalidAllowedCommonNameList |De indeling van tekenreeks van de lijst met algemene naam is ongeldig. Deze moet een door komma's gescheiden lijst. |

