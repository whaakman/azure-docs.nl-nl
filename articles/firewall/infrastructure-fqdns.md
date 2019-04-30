---
title: FQDN-naam voor de Firewall van Azure-infrastructuur
description: Meer informatie over de infrastructuur FQDN-namen in de Azure-Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 34201a0eb4139de64261f77f285096a2aa2dd3aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61066318"
---
# <a name="infrastructure-fqdns"></a>FQDN's voor infrastructuur

Azure Firewall bevat een ingebouwde regelverzameling voor infrastructuur-FQDN’s die standaard zijn toegestaan. Deze FQDN’s zijn specifiek voor het platform en kunnen niet voor andere doeleinden worden gebruikt. 

De volgende services zijn opgenomen in de ingebouwde verzameling:

- Toegang tot opslag Platform Image Repository (PIR) COMPUTE
- Beheerde schijven voor toegang tot opslag status
- Azure Diagnostics en logboekregistratie (MDS)
- Azure Active Directory

## <a name="overriding"></a>Overschrijven 

U kunt deze ingebouwde infrastructuur regelverzameling overschrijven door het maken van een weigeren alle verzameling van de toepassing-regel die als laatste is verwerkt. Deze wordt altijd vóór de infrastructuurregelverzameling verwerkt. Alles wat niet in de infrastructuurregelverzameling staat, wordt standaard geweigerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md).