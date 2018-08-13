---
title: Azure Stack-fysiek apparaat controleren
description: Meer informatie over het integreren met het fysieke apparaat toegang controleren in Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/01/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: 459cdf4e1a70ee02d818dd6abe101e4fc3475b68
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "40034743"
---
# <a name="azure-stack-datacenter-integration---physical-device-auditing"></a>Integratie van datacenter voor Azure Stack - fysiek apparaat controleren

Alle fysieke apparaten in Azure Stack, zoals de baseboard management controllers (bmc's) en de netwerkswitches, verzenden de logboeken voor controle en ze moeten worden geïntegreerd in de algehele oplossing voor controle. Aangezien de apparaten tussen de verschillende Azure Stack OEM-hardwareleveranciers verschillen, neem dan contact op met de leverancier van uw documentatie over de integratie van controle. De onderstaande secties bevatten enkele algemene informatie voor het controleren van fysiek apparaat in Azure Stack.  

## <a name="physical-device-access-auditing"></a>Fysiek apparaat toegang controleren

Alle fysieke apparaten in Azure Stack ondersteuning voor het gebruik van TACACS of RADIUS. Dit omvat toegang tot de BMC (baseboard management controller) en de netwerkswitches.

Azure Stack-oplossingen niet geleverd met RADIUS of TACACS ingebouwd. De oplossingen zijn echter gevalideerd ter ondersteuning van het gebruik van bestaande RADIUS of TACACS oplossingen beschikbaar in de markt.

Voor RADIUS, is MSCHAPv2 gevalideerd. Hiermee wordt de veiligste implementatie met behulp van RADIUS.
Neem contact op met uw hardwareleverancier OEM TACAS of RADIUS inschakelen in de apparaten die zijn opgenomen in uw Azure Stack-oplossing.

## <a name="syslog-forwarding-for-network-devices"></a>Syslog doorsturen voor netwerkapparaten

Alle fysieke netwerkapparaten in Azure Stack ondersteuning voor syslog-berichten. Azure Stack-oplossingen niet geleverd met een syslog-server. De apparaten zijn echter gevalideerd ter ondersteuning van berichten verzenden naar bestaande syslog oplossingen beschikbaar in de markt.

Het doeladres syslog is een optionele parameter voor de implementatie kan worden verzameld, maar deze kan ook worden toegevoegd na de implementatie. Neem contact op met uw hardwareleverancier OEM syslog doorsturen op uw VPN-apparaten configureren.

## <a name="next-steps"></a>Volgende stappen

[Servicebeleid](azure-stack-servicing-policy.md)
