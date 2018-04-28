---
title: Ondersteunde gastbesturingssystemen voor Azure-Stack | Microsoft Docs
description: Deze gastbesturingssystemen kan worden gebruikt op Azure-Stack.
services: azure-stack
documentationcenter: ''
author: Brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: ff3aea4e449e3d489b0c0f01345ecd9773c7d885
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Gastbesturingssystemen die worden ondersteund op Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="windows"></a>Windows
De Windows-gastbesturingssystemen die worden vermeld in de volgende tabel biedt ondersteuning voor Azure Stack: afbeeldingen in de Marketplace zijn beschikbaar voor het downloaden van Azure-Stack. Windows client-installatiekopieën zijn niet beschikbaar in de Marketplace.

Tijdens de implementatie van injects Azure-Stack een geschikte versie van de gastagent in de afbeelding.

| Besturingssysteem | Beschrijving | Beschikbaar in de Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server, versie 1709 | 64-bits | Core met Containers |
| Windows Server 2016 | 64-bits |  Datacenter, Datacenter-Core, Datacenter met Containers |
| Windows Server 2012 R2 | 64-bits |  Datacenter |
| Windows Server 2012 | 64-bits |  Datacenter |
| Windows Server 2008 R2 SP1 | 64-bits |  Datacenter |
| Windows Server 2008 SP2 | 64-bits |  Breng uw eigen installatiekopie |
| Windows 10 *(Zie Opmerking 1)* | 64-bits, Pro en Enterprise | Breng uw eigen installatiekopie |

***Opmerking 1:****Windows 10-client om besturingssystemen te implementeren op Azure-Stack, hebt u [Windows per gebruiker-licentieverlening](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) of aanschaffen via een gekwalificeerde Multitenant Hoster ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).*

Marketplace-installatiekopieën zijn beschikbaar voor betaalde-als-gebruik of BYOL (EA/SPLA)-licentieverlening. Gebruik van beide op één Azure-Stack-exemplaar wordt niet ondersteund. 

Datacenter-edities zijn beschikbaar in de marketplace; klanten kunnen hun eigen serverinstallatiekopieën, met inbegrip van andere edities brengen.

## <a name="linux"></a>Linux

Linux-distributies hier weergegeven, bevatten de benodigde Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Aangepaste installatiekopieën moeten worden gebouwd met de meest recente openbare WALA-versie. Versies ouder dan 2.2.18 mogelijk niet goed op Azure-Stack.  
>
> [cloud-init](https://cloud-init.io/) wordt niet ondersteund op Azure-Stack op dit moment.

| Distributie | Beschrijving | Uitgever | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Container Linux |  64-bits | CoreOS | Stabiel |
| Op basis van centOS 6,9 | 64-bits | Rogue Wave | Ja |
| 7.4 op basis van centOS | 64-bits | Rogue Wave | Ja |
| ClearLinux | 64-bits | ClearLinux.org | Ja |
| Debian 8 'Jessie' | 64-bits | credativ |  Ja |
| Debian 9 'Stretch' | 64-bits | credativ | Ja |
| Red Hat Enterprise Linux 7.x (in behandeling) | 64-bits | Red Hat |Breng uw eigen installatiekopie |
| SLES 11SP4 | 64-bits | SUSE | Ja |
| SLES 12SP3 | 64-bits | SUSE | Ja |
| Ubuntu 14.04-TNS | 64-bits | Canonical | Ja |
| Ubuntu 16.04-TNS | 64-bits | Canonical | Ja |

In de toekomst mogelijk andere Linux-distributies worden ondersteund.
