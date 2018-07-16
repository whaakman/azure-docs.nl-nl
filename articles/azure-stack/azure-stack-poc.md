---
title: Wat is Azure Stack? | Microsoft Docs
description: Stack van Azure kunt u Azure-services uitvoeren in uw datacenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/04/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 848df59b01cc0c03b9a5f3dae2644d469c8651bb
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234883"
---
# <a name="what-is-azure-stack"></a>Wat is Azure Stack?

Microsoft Azure-Stack is een hybride cloud-platform waarmee u Azure-services leveren in uw datacenter. Dit platform is ontworpen ter ondersteuning van uw groeiende zakelijke vereisten. Azure Stack kunt nieuwe scenario's voor uw moderne toepassingen, zoals rand en niet-verbonden omgevingen of voldoen aan specifieke vereisten voor beveiliging en naleving.

Azure-Stack is verkrijgbaar in twee implementatieopties om te voldoen aan uw behoeften.

## <a name="azure-stack-integrated-systems"></a>Azure Stack-geïntegreerde systemen
Geïntegreerde systemen worden aangeboden via een verbinding van Microsoft Azure-Stack en [hardwarepartners](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), het maken van een oplossing die eigen tempo van cloud innovatie biedt en computing management eenvoud. Omdat Azure Stack wordt aangeboden als een geïntegreerde hardware en software, hebt u de flexibiliteit en controle die u nodig hebt, naast de mogelijkheid om innoveren vanuit de cloud. Azure Stack geïntegreerd systemen in grootte variëren van 4-12-knooppunten en gezamenlijk worden ondersteund door de partner van de hardware- en Microsoft.  Gebruik Azure Stack geïntegreerd systemen nieuwe scenario's maken en implementeren van nieuwe oplossingen voor uw productie-workloads.

## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit

Microsoft [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) is een implementatie met één knooppunt van Azure-protocolstack die u gebruiken kunt om te evalueren en meer informatie over Azure-Stack.  U kunt ook ASDK als een ontwikkelomgeving voor het bouwen van apps met de API's en tooling die consistent zijn met Azure.

>[!Note]
>De ASDK is niet bedoeld om te worden gebruikt als een productie-omgeving.

De ASDK heeft de volgende beperkingen:

* ASDK is gekoppeld aan één Azure Active Directory (Azure AD) of een id-provider voor Active Directory Federation Services (AD FS). U kunt meerdere gebruikers maken in deze map en abonnementen toewijzen aan elke gebruiker.
* Omdat Azure Stack-onderdelen zijn geïmplementeerd op een hostcomputer, zijn er beperkt fysieke resources beschikbaar voor tenantbronnen. Deze configuratie is niet bedoeld voor evaluatie van de scale- of Prestatieweergave.
* Scenario's voor netwerken zijn beperkt vanwege de vereisten voor NIC-implementatie en één host.

## <a name="next-steps"></a>Volgende stappen

- [Belangrijke functies en concepten](azure-stack-key-features.md)
- [Azure-Stack: Een uitbreiding van Azure (pdf)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
