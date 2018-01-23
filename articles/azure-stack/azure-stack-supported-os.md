---
title: Ondersteunde gastbesturingssystemen voor Azure-Stack | Microsoft Docs
description: Deze gastbesturingssystemen kan worden gebruikt op Azure-Stack.
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: c9f5bee38772623fb79fa081be8eaece981cc8ab
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Gastbesturingssystemen die worden ondersteund op Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="windows"></a>Windows
Azure Stack ondersteunt de volgende Windows-gastbesturingssystemen. Afbeeldingen in de Marketplace zijn beschikbaar voor het downloaden van Azure-Stack. Windows client-installatiekopieën zijn niet beschikbaar in de Marketplace.

Azure-Stack zorgt ervoor dat een geschikte versie van de guest-agent is opgenomen in de installatiekopie tijdens de implementatie.

| Besturingssysteem | Beschrijving | Uitgever | Het type besturingssysteem | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64-bits | Microsoft | Windows | Datacenter |
| Windows Server 2012 | 64-bits | Microsoft | Windows | Datacenter |
| Windows Server 2012 R2 | 64-bits | Microsoft | Windows | Datacenter |
| Windows Server 2016 | 64-bits | Microsoft | Windows | Datacenter, Datacenter-Core, Datacenter met Containers |
| Windows 7 | 64-bits, Pro en Enterprise | Microsoft | Windows | Nee |
| Windows 8.1 | 64-bits, Pro en Enterprise | Microsoft | Windows | Nee |
| Windows 10 *(Zie Opmerking 1)* | 64-bits, Pro en Enterprise | Microsoft | Windows | Nee |

***Opmerking 1:****Windows 10-client om besturingssystemen te implementeren op Azure-Stack, hebt u [Windows per gebruiker-licentieverlening](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) of aanschaffen via een gekwalificeerde Multitenant Hoster ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

Linux-distributies hier weergegeven, bevatten de benodigde Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Installatiekopieën die zijn gemaakt met WALA versies ouder dan 2.2.3 *niet* ondersteund en waarschijnlijk niet te implementeren. Sommige versies van de agent WALA bekend zijn bij de not-functie op virtuele Azure-Stack-machines, inclusief versies 2.2.12 en 2.2.13.


| Distributie | Beschrijving | Uitgever | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Container Linux |  64-bits | CoreOS | Stabiel |
| Op basis van centOS 6,9 | 64-bits | Rogue Wave | Ja |
| 7.3 op basis van CentOS | 64-bits | Rogue Wave | Ja |
| 7.4 op basis van centOS | 64-bits | Rogue Wave | Ja |
| Debian 8 'Jessie' | 64-bits | credativ |  Ja |
| Debian 9 'Stretch' | 64-bits | credativ | Ja |
| Oracle Linux | 64-bits | Oracle | Nee |
| Red Hat Enterprise Linux 7.x | 64-bits | Red Hat | Nee |
| SLES 11SP4 | 64-bits | SUSE | Ja |
| SLES 12SP3 | 64-bits | SUSE | Ja |
| Ubuntu 14.04-LTS | 64-bits | Canonical | Ja |
| Ubuntu 16.04-LTS | 64-bits | Canonical | Ja |

In de toekomst mogelijk andere Linux-distributies worden ondersteund.
