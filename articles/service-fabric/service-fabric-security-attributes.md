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
ms.openlocfilehash: f12d11cecbf682ae82f9c432804b1d611ee3e39f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719397"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Algemene beveiligingskenmerken voor Azure Service Fabric

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Service Fabric. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | De klant is eigenaar van het cluster en de virtuele-machineschaalset op dat het cluster is gebaseerd. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja |  |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | De klant is eigenaar van het cluster en de virtuele-machineschaalset op dat het cluster is gebaseerd. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Service Fabric-API-aanroepen worden gemaakt via Azure Resource Manager. Een geldig JSON webtoken (JWT) is vereist. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| Ondersteuning voor VNet-injectie| Ja |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's). |
| Geforceerde tunneling ondersteuning| Ja | Azure-netwerk bieden geforceerde tunneling. |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Met behulp van Azure-ondersteuning en ondersteuning voor externe bewaking. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Identiteits- en toegangsbeheer management (IAM) voor aanroepen via SFRP. Aanroepen rechtstreeks naar het eindpunt cluster ondersteunt twee rollen: Gebruiker en beheerder. De klant kan de API's worden toegewezen aan een van beide rollen. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle bewerkingen voor de controlelaag uitvoeren door processen voor controle en -goedkeuringen. |
| Gegevens vlak logboekregistratie en controle| N/A | De klant is eigenaar van het cluster.  |

## <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | |