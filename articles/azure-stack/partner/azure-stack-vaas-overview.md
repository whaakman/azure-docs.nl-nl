---
title: Een overzicht van de validatie uit als een Service voor Azure Stack | Microsoft Docs
description: Een overzicht van Azure Stack-validatie als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 12/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: c0694a3839ae6b843abd1ddc8b67d1f3e3322e0e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246038"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Wat is gevalideerd als een Service voor Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validatie als een Service (VaaS) is een systeemeigen Azure-service die is ontworpen voor de voor oplossingspartners die zijn naast elkaar engineering-aanbiedingen met Microsoft Azure Stack. Oplossingspartners kunnen gebruikmaken van de service om te controleren dat hun oplossingen voldoen aan de vereisten van Microsoft en werkt zoals verwacht met Azure Stack.

De primaire gebruikstoepassingen voor VaaS zijn:

- Valideren van nieuwe oplossingen voor Azure Stack
- Valideren van wijzigingen in de Azure Stack-software
- Partner oplossingspakketten gebruikt tijdens de implementatie digitaal te ondertekenen
- Een VaaS testen kunnen

## <a name="validate-a-new-azure-stack-solution"></a>Een nieuwe Azure Stack-oplossing te valideren

Partners gebruiken het **oplossing validatie** werkstroom om nieuwe Azure Stack-oplossing te valideren. De oplossing moet de vereiste Hardware Lab Kit (HLK) Azure Stack-onderdeel tests slagen. Als u wilt certificeren een scala aan hardwareconfiguraties, de werkstroom moet worden uitgevoerd twee keer voor elke nieuwe oplossing: eenmaal voor elk voor de minimale en maximale configuraties.

Zie voor meer informatie, [een nieuwe Azure Stack-oplossing te valideren](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Valideren van wijzigingen in de Azure Stack-software

Partners gebruiken het **oplossing validatie** werkstroom om te controleren dat de oplossing met de meest recente software-updates van Azure Stack werkt. De werkstroom voor validatie van de oplossing moet worden uitgevoerd op een Microsoft-aanbevolen hardware-omgeving waar voor patches en updates (P & U) is gebruikt voor het toepassen van de update. Het verdient ook de werkstroom uitvoeren op de basislijn-build.

Zie voor meer informatie, [valideren van software-updates van Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Digitaal ondertekend solution partner pakketten ophalen

Naast het valideren van updates voor Azure Stack-partners gebruiken het **oplossing validatie** werkstroom voor het valideren van updates voor OEM customization pakketten, waaronder Azure Stack-partner-specifieke stuurprogramma's, firmware en andere software tijdens de implementatie van de Azure Stack-software wordt gebruikt. Implementeer het pakket dat u voor wordt gevalideerd op de huidige versie van de Azure Stack-software met behulp van ten minste de minimale grootte oplossing die worden ondersteund. Het pakket wordt verzonden naar VaaS voordat u tests uitvoert. Als de tests zijn voltooid, op de hoogte stellen [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) dat het pakket testen is voltooid en moet digitaal worden ondertekend met de digitale handtekening van de Azure Stack. Microsoft ondertekent het pakket en de Azure Stack-partner meldt dat het pakket gedownload in de portal VaaS is.

Zie voor meer informatie, [valideren OEM pakketten](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Preview-versie VaaS testen kunnen

Microsoft maakt regelmatig nieuwe functies beschikbaar zijn in Azure Stack. Als onderdeel van het ontwikkelingsproces voor het leveren van deze functies op de markt, nieuwe test zekerheid wordt beschikbaar gesteld in de **testronde** werkstroom. De werkstroom testronde bevat kunnen testen van de andere werkstromen om toe te staan voor de testuitvoering van niet-officiële. Gebruik de werkstroom testronde geen resultaten voor goedkeuring verzenden. Gebruik de validatie van de oplossing en de validatie van de oplossing werkstromen om officieel erkenning voor uw oplossing.

Zie voor meer informatie [Snelstart: De validatie gebruiken als een serviceportal voor het plannen van uw eerste test](azure-stack-vaas-schedule-test-pass.md).

## <a name="validation-workflow-tests-summary"></a>Werkstroom voor groepsvalidatie test samenvatting

| Werkstroom voor groepsvalidatie | Vereiste tests |
|----|------------|
| [Nieuwe oplossing validatie](azure-stack-vaas-validate-solution-new.md) | Engine voor cloud-simulatie<br>COMPUTE operationele SDK-pakket<br>Test voor schijf-ID<br>KeyVault Extension SDK Operational Suite<br>KeyVault SDK Operational Suite<br>Network SDK Operational Suite<br>Storage-Account SDK operationele Suite<br> |
| [Validatie van de OEM-pakket](azure-stack-vaas-validate-oem-package.md) | OEM-extensie pakket verificatie<br>Engine voor cloud-simulatie |
| [Validatie van de maandelijkse update](azure-stack-vaas-validate-microsoft-updates.md) | Maandelijkse AzureStack Update-verificatie<br>Engine voor cloud-simulatie<br> |

## <a name="next-steps"></a>Volgende stappen

- [De validatie als een serviceresources instellen](azure-stack-vaas-set-up-resources.md)
- Meer informatie over [validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md)
