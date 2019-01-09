---
title: Bijwerken van Windows Defender Antivirus op Azure Stack
description: Meer informatie over hoe antivirus actueel blijven in Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/8/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: d4eb0cbf6c55f3d8da460370ec9209638e3e1d62
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118214"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Bijwerken van Windows Defender Antivirus op Azure Stack

[Windows Defender Antivirus](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) is een anti-malware-oplossing waarmee beveiliging en virusbeveiliging biedt. Elke Azure Stack-infrastructuur-onderdeel (Hyper-V-hosts en virtuele machines) wordt beschermd met Windows Defender Antivirus. Vereisen dat periodieke updates voor beveiliging, Windows Defender Antivirus-definities, -engine en -platform. Hoe updates worden toegepast, is afhankelijk van uw configuratie.

## <a name="connected-scenario"></a>Verbonden scenario

Voor anti-malware-definitie en engine-updates, de Azure Stack [resourceprovider voor update](azure-stack-updates.md#the-update-resource-provider) anti-malwaredefinities en engine-updates meerdere keren per dag worden gedownload. Elk onderdeel van de Azure Stack-infrastructuur wordt opgehaald van de update van de resourceprovider voor update en de update automatisch toegepast.

Voor updates voor anti-malware-platform, gelden de [maandelijkse update van Azure Stack](azure-stack-apply-updates.md). De maandelijkse update van Azure Stack bevat updates voor Windows Defender Antivirus-platform voor de maand.

## <a name="disconnected-scenario"></a>Niet-verbonden scenario

 Van toepassing de [maandelijkse update van Azure Stack](azure-stack-apply-updates.md). De maandelijkse update van Azure Stack omvat Windows Defender Antivirus-definities, -engine en platform-updates voor de maand.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure Stack-beveiliging](azure-stack-security-foundations.md)
