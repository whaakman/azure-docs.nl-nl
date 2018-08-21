---
title: Een overzicht van de validatie uit als een service voor Azure Stack | Microsoft Docs
description: Een overzicht van Azure Stack-validatie als een service die bekende problemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c96e7385356354d398108ad69492603d38667e46
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234914"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Wat is gevalideerd als een service voor Azure Stack?

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validatie als een service (VaaS) is een systeemeigen Azure-service die is ontworpen voor de voor oplossingspartners die zijn naast elkaar engineering-aanbiedingen met Microsoft Azure Stack. Oplossingspartners kunnen gebruikmaken van de service om te controleren dat hun oplossingen voldoen aan de vereisten van Microsoft en werkt zoals verwacht met Azure Stack.

Het primaire gebruik voor VaaS is:

- Valideren van de nieuwe Azure Stack-oplossingen
- Valideren van wijzigingen in de Azure Stack-software
- Digitaal ondertekend oplossingspartner pakketten die worden gebruikt tijdens de implementatie ophalen
- Voorbeeld van Azure Stack-validatie kunnen

## <a name="validate-new-azure-stack-solution"></a>Nieuwe Azure Stack-oplossing te valideren

De werkstroom van de oplossing validatie partners gebruiken om te controleren op nieuwe oplossingen voor Azure Stack. De oplossing moet slagen voor de vereiste Hardware Lab Kit (HKL) Azure Stack tests onderdeel tests. U hoeft alleen het uitvoeren van de werkstroom twee keer voor elke nieuwe oplossing: eenmaal voor de minimale en maximale configuratie.

Zie voor meer informatie, [een nieuwe Azure Stack-oplossing te valideren](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Valideren van wijzigingen in de Azure Stack-software

De pakket-werkstroom voor groepsvalidatie partners gebruiken om te controleren dat de oplossing met de recente update van de Azure Stack-software werkt. Ten minste moet de werkstroom van de validatie pakket worden uitgevoerd op de hardware-omgeving door Microsoft wordt aanbevolen, en aan een oplossing waarin de update (P & U) en patch zijn gebruikt voor het toepassen van de update. De validatie van het pakket moet worden uitgevoerd op de basislijn-build.

Zie voor meer informatie, [valideren van software-updates van Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Digitaal ondertekend solution partner pakketten ophalen

Naast Azure Stack-updates wilt valideren, kunt u de werkstroom van de validatie pakket gebruiken om te controleren op updates voor OEM customization pakketten, waaronder Azure Stack-partner-specifieke stuurprogramma's, firmware en andere software die is gebruikt tijdens de implementatie van de Azure Stack software. Implementeer het pakket dat u op de huidige versie controleert van het Azure Stack software op een minimaal de minimale grootte oplossing die worden ondersteund. Het bijgewerkte pakket moet worden geüpload naar de service voordat u de test te beginnen. Als de tests zijn voltooid, op de hoogte stellen vaashelp@microsoft.com. De Azure Stack-partner zien dat het pakket testen is voltooid en digitaal worden ondertekend met de digitale handtekening van de Azure Stack. Microsoft ondertekent het pakket en de Azure Stack-partner meldt dat het pakket gedownload in de portal is.

Zie voor meer informatie, [valideren OEM pakketten](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Voorbeeld van Azure Stack-validatie kunnen

Microsoft maakt regelmatig nieuwe functies beschikbaar zijn in Azure Stack. Als onderdeel van het ontwikkelingsproces voor het leveren van deze functies op de markt, is nieuwe test kunnen beschikbaar in de werkstroom testronde. De werkstroom test-pass bevat kunnen testen van de andere werkstromen om toe te staan voor de testuitvoering van niet-officiële. Gebruik de werkstroom test-pass geen resultaten voor goedkeuring verzenden. Gebruik de validatie en pakket validaties werkstroom van de oplossing om officieel erkenning voor uw oplossing.

## <a name="next-steps"></a>Volgende stappen

- Aan de slag, en [uw validatie als een service-account instellen](azure-stack-vaas-validate-solution-new.md)
