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
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 863c1ec562cd71af0df69ccc0547e16d02c7ee82
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="what-is-azure-stack"></a>Wat is Azure Stack?

Microsoft Azure-Stack is een hybride cloud-platform waarmee u Azure-services leveren van uw organisatie datacenter.  Azure-Stack is ontworpen om in te schakelen nieuwe scenario's voor uw moderne toepassingen in de belangrijkste scenario's, zoals rand en niet-verbonden omgevingen of vergadering specifieke beveiliging en naleving vereisten.  Azure-Stack is verkrijgbaar in twee implementatieopties om te voldoen aan uw behoeften.

## <a name="azure-stack-integrated-systems"></a>Azure Stack-geïntegreerde systemen
Geïntegreerde systemen worden aangeboden via een verbinding van Microsoft Azure-Stack en [hardwarepartners](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), het maken van een oplossing voor eigen tempo van cloud innovatie in evenwicht zijn met eenvoud in beheer.  Omdat Azure Stack wordt aangeboden als een geïntegreerd systeem van hardware en software, krijgt u de juiste hoeveelheid flexibiliteit en beheer, terwijl u nog steeds overstap innovatie vanuit de cloud.  Azure Stack geïntegreerd systemen in grootte variëren van 4-12-knooppunten en gezamenlijk worden ondersteund door de partner van de hardware- en Microsoft.  Gebruik Azure Stack geïntegreerd systemen nieuwe scenario's voor uw productie-workloads te maken.    

## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit
Microsoft [Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) is een implementatie met één knooppunt van de Azure-Stack, dat u gebruiken kunt om te evalueren en meer informatie over Azure-Stack.  U kunt de ASDK ook gebruiken als een ontwikkelomgeving waarin u kunt ontwikkelen met API's en tooling consistent zijn met Azure. De ASDK is niet bedoeld als een productie-omgeving moet worden gebruikt.

De ASDK heeft de volgende beperkingen:
* ASDK is gekoppeld aan één Azure Active Directory (Azure AD) of een id-provider voor Active Directory Federation Services (AD FS). U kunt meerdere gebruikers maken in deze map en abonnementen toewijzen aan elke gebruiker.
* Met alle onderdelen zijn geïmplementeerd op één computer, zijn beperkt fysieke resources beschikbaar voor tenantbronnen. Deze configuratie is niet bedoeld voor evaluatie van scale- of Prestatieweergave.
* Scenario's voor netwerken zijn beperkt omdat de vereiste één host/NIC.  

## <a name="next-steps"></a>Volgende stappen
[Belangrijke functies en concepten](azure-stack-key-features.md)

[Azure-Stack: Een uitbreiding van Azure (pdf)](https://azure.microsoft.com/en-us/resources/azure-stack-an-extension-of-azure/)

