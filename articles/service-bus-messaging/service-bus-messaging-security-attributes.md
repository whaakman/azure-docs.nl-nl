---
title: Algemene beveiligingskenmerken voor Azure Service Bus-berichten
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Service Bus-berichten
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: d68ffe6561da6a23c288dfabd1d3eb6b34099bb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66003116"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Beveiligingskenmerken voor Azure Service Bus-berichten

In dit artikel worden de beveiligingskenmerken die is ingebouwd in Azure Service Bus Messaging.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  Ja voor serverzijde versleuteling-at-rest standaard. | Beheerd door de klant sleutels en BYOK zijn nog niet ondersteund. Versleuteling aan de clientzijde is de verantwoordelijkheid van de client |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Biedt ondersteuning voor standaard HTTPS/TLS-mechanisme. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee |   |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | API-aanroepen worden gedaan via [Azure Resource Manager](../azure-resource-manager/index.yml) - en HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja (alleen Premium-laag) | VNet-service-eindpunten worden ondersteund voor [Service Bus Premium-laag](service-bus-premium-messaging.md) alleen. |
| Ondersteuning voor VNet-injectie| Nee | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja (alleen Premium-laag) |  |
| Geforceerde tunneling ondersteuning| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Ondersteund via [Azure Monitor en waarschuwingen](service-bus-metrics-azure-monitor.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Beheerd via [Azure Active Directory beheerde Service-identiteit](service-bus-managed-service-identity.md); Zie [Service Bus-verificatie en autorisatie](service-bus-authentication-and-authorization.md).|
| Autorisatie| Ja | Biedt ondersteuning voor autorisatie via [RBAC](service-bus-role-based-access-control.md) (Preview) en SAS-token; Zie [Service Bus-verificatie en autorisatie](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Operations-logboeken zijn beschikbaar. Zie [diagnostische logboeken van Service Bus](service-bus-diagnostic-logs.md).  |
| Gegevens vlak logboekregistratie en controle| Nee |  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Biedt ondersteuning voor resource provider versiebeheer via de [Azure Resource Manager API](/rest/api/resources/).|
