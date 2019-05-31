---
title: Beveiligingskenmerken voor Azure Cosmos DB
description: Een controlelijst met beveiligingsinstellingen voor het evalueren van Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: 891a8389d7e34c48a00dda8fdebd251a698e5937
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420646"
---
# <a name="security-attributes-azure-cosmos-db"></a>Beveiligingskenmerken Azure Cosmos DB

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Cosmos DB.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | Ja | Alle Cosmos DB-databases en de back-ups worden versleuteld standaard; Zie [versleuteling van gegevens in Azure Cosmos DB](database-encryption-at-rest.md). Versleuteling op de server met de klant beheerde sleutels wordt niet ondersteund. |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in Vnet-versleuteling en versleuteling van de VNet-VNet)| Ja | Alle Azure Cosmos DB-gegevens in transit versleuteld. |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom (Azure-gegevensservices)| Ja | Alleen in de tabellen API Premium. Niet alle API's ondersteunen deze functie. Zie [Inleiding tot Azure Cosmos DB: Table-API](table-introduction.md). |
| API-aanroepen die zijn versleuteld| Ja | Alle verbindingen met Azure Cosmos DB bieden ondersteuning voor HTTPS. Azure Cosmos DB biedt ook ondersteuning voor TLS 1.2-verbindingen, maar dit nog niet wordt afgedwongen. Als klanten lager niveau TLS op hun eindgebruikers te zorgen uitschakelen voor het verbinding maken met Cosmos DB.  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| ondersteuning voor vNET-injectie| Ja | U kunt een Azure Cosmos DB-account om toe te staan alleen toegankelijk is vanaf een specifiek subnet van een virtueel netwerk (VNet) configureren met VNet-service-eindpunt. U kunt ook VNet toegang combineren met firewall-regels.  Zie [toegang tot Azure Cosmos DB van virtuele netwerken](vnet-service-endpoint.md). |
| Ondersteuning voor netwerkisolatie en Firewalling| Ja | Met firewallondersteuning voor de, kunt u uw Azure Cosmos-account zodat toegang alleen vanaf een goedgekeurde set IP-adressen, een bereik van IP-adressen en/of cloud services configureren. Zie [configureren IP-firewall in Azure Cosmos DB](how-to-configure-firewall.md).|
| Ondersteuning voor geforceerde tunneling | Ja | Kan worden geconfigureerd op de client op de VNET waar de virtuele machines zich bevinden.   |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Alle aanvragen die worden verzonden naar Azure Cosmos DB worden geregistreerd. [Azure Monitoring](../azure-monitor/overview.md), metrische gegevens van Azure, Azure-controlelogboeken worden ondersteund.  U kunt informatie over gegevens vlak aanvragen, query-runtime-statistieken, querytekst zich, MongoDB-aanvragen. U kunt ook waarschuwingen instellen. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Ja op het niveau van de Database-Account; Cosmos DB gebruikt op het niveau van de gegevens vlak, brontokens en toegang tot de sleutel. |
| Autorisatie| Ja | Ondersteund op de Azure Cosmos-account met hoofdsleutels (primaire en secundaire) en de Resource-tokens. U kunt opvragen voor lezen/schrijven of alleen-lezentoegang tot gegevens met hoofdsleutels. Resource-tokens kunnen beperkte tijd toegang tot resources, zoals documenten en containers. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle| Ja | Azure-activiteitenlogboek voor account niveau bewerkingen, zoals Firewalls, VNets, toegang tot sleutels en IAM. |
| Gegevens vlak logboekregistratie en controle | Ja | Diagnostische logboekregistratie voor container niveau bewerkingen, zoals bewaking maken-container, inrichten, doorvoer, beleidsregels en CRUD-bewerkingen op documenten te indexeren. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Nee  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Kenmerken voor extra beveiliging voor Cosmos DB

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Cross-Origin Resource Sharing (CORS) | Ja | Zie [Cross-Origin Resource Sharing (CORS) configureren](how-to-configure-cross-origin-resource-sharing.md). |
