---
title: Evalueren van de Azure-Stack Development Kit | Microsoft Docs
description: Informatie over het implementeren van de Azure-Stack Development Kit voor evaluatiedoeleinden.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 4ad2e0a91e2fd5023417722fc0a1a6fae93960d0
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Snelstartgids: De Azure-Stack Development Kit evalueren

*Van toepassing op: Azure stapelen Development Kit*

De [Azure Stack Development Kit](azure-stack-poc.md) is een test- en -omgeving die u implementeren kunt om te evalueren en de Azure-Stack-functies en services demonstreren. Als u deze moet actief is, u de hardware van de omgeving voorbereiden en enkele (dit duurt enkele uren) scripts uitvoeren. Daarna kunt u zich aanmeldt bij de portals beheerder en gebruiker voor het beheren van Azure-Stack en aanbiedingen te testen. 

1. [**Plannen van uw hardware, software en netwerk**](azure-stack-deploy.md). De computer die als host fungeert voor de development kit (de host development kit) moet voldoen aan de eisen aan hardware, software en netwerk. U moet er ook voor kiezen tussen het gebruik van Azure Active Directory of Active Directory Federation Services. Zorg ervoor dat deze vereisten voldoen voordat u uw implementatie start, zodat het installatieproces soepel wordt uitgevoerd. 

2. [**Downloaden en uitpakken van het implementatiepakket**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). U kunt het implementatiepakket downloaden naar de development kit host of naar een andere computer. De implementatie van de uitgepakte bestanden duren 60 GB aan vrije schijfruimte, zodat een andere computer kan helpen verminderen de hardwarevereisten voor de host van development kit.

3. [**Voorbereiden van de host van development kit** ](azure-stack-run-powershell-script.md) met behulp van het installatieprogramma. Na deze stap wordt de development kit host opgestart naar de Cloudbuilder.vhdx (een virtuele harde schijf met een besturingssysteem en de Azure-Stack installeren bestanden).

4. [**Implementeer de development kit** ](azure-stack-run-powershell-script.md) op de host van development kit.

5. Als uw Azure-Stack-implementatie gebruikmaakt van Azure Active Directory, moet u [Azure Stack registreren bij Azure](azure-stack-register.md) zodat u kunt [Azure marketplace-items downloaden](azure-stack-download-azure-marketplace-item.md) naar Azure-Stack.

Wanneer u deze stappen uitvoert, beschikt u over een ontwikkelingsomgeving kit met portals van zowel de beheerder als de gebruiker. Vervolgens kunt u [verbinding en meld u aan](azure-stack-connect-azure-stack.md) naar de portal. Vervolgens kunt u starten resourceproviders implementeren, maken [biedt](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions), en de Azure-Stack in te vullen [marketplace](azure-stack-marketplace.md).
