---
title: Algemene beveiligingskenmerken voor Azure Service Fabric
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: f6db40a35c289fa0870ac4c9e5e55b773c84f4f4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679224"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Algemene beveiligingskenmerken voor Azure Service Fabric

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Service Fabric. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | De klant is eigenaar van het cluster en de virtuele-machineschaalset op dat het cluster is gebaseerd. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja |  |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Ja | De klant is eigenaar van het cluster en de virtuele-machineschaalset op dat het cluster is gebaseerd. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling op kolom (Azure-gegevensservices)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Service Fabric-API-aanroepen worden gemaakt via Azure Resource Manager. Een geldig JSON webtoken (JWT) is vereist. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| ondersteuning voor vNET-injectie| Ja |  |
| Netwerkisolatie / netwerkfunctie ondersteuning| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's). |
| Ondersteuning voor geforceerde tunneling | Ja | Azure-netwerk bieden geforceerde tunneling. |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Met behulp van Azure-ondersteuning en ondersteuning voor externe bewaking. |

## <a name="iam-support"></a>IAM-ondersteuning

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangsbeheer - verificatie| Ja | Verificatie is via Azure Active Directory. |
| Toegangsbeheer - autorisatie| Ja | Identiteits- en toegangsbeheer management (IAM) voor aanroepen via SFRP. Aanroepen rechtstreeks naar het eindpunt cluster ondersteunt twee rollen: Gebruiker en beheerder. De klant kan de API's worden toegewezen aan een van beide rollen. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle| Ja | Alle bewerkingen voor de controlelaag uitvoeren door processen voor controle en -goedkeuringen. |
| Gegevens vlak logboekregistratie en controle| N/A | De klant is eigenaar van het cluster.  |

## <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Configuratie van de service is samengesteld en geïmplementeerd met behulp van Azure implementeren. De code (toepassings- en runtime) is samengesteld met behulp van Azure bouwen.