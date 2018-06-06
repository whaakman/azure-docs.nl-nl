---
title: Het gebruik en facturering voor Azure-Stack als een Cloudserviceprovider beheren | Microsoft Docs
description: Beknopt overzicht van Azure-Stack geregistreerd als Cloudprovider en het toevoegen van klanten.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 21a52af4943004789b0a9bdbe4695ab1a603c046
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796696"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Het gebruik en facturering voor Azure-Stack als een Cloud Service Provider beheren 

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel begeleidt u bij het registreren van Azure-Stack als een Cloud-Provider (CSP) en het toevoegen van klanten.

Als een CSP bent u waarschijnlijk veel verschillende klanten met behulp van uw Azure-Stack. Elke klant heeft een CSP-abonnement in Azure en moet u informatie over het gebruik van uw Azure-Stack aan elke gebruiker abonnement wordt omgeleid.

Het volgende diagram toont de stappen die u moet uw account gedeelde services kiezen en registreren van azure-account aan het account. Als dit is gedaan, kunt u vrijgeven uw klanten end.

**Stappen voor het gebruik bijhouden als een CSP toevoegen**

![Proces voor het inschakelen van gebruiks- en management als een Cloud-serviceprovider.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>Een CSP of CSPSS abonnement maken

### <a name="cloud-service-provider-subscription-types"></a>Cloudtypen serviceprovider-abonnement

U moet het type van gedeelde services-account dat u voor Azure-Stack gebruikt kiezen. De abonnementen die kunnen worden gebruikt voor de registratie van een multitenant Azure Stack-typen zijn:

 - Cloudserviceprovider 
 - Abonnement van de gedeelde Services partner 

#### <a name="csp-shared-services"></a>CSP gedeelde Services

Cloudabonnementen Service Provider gedeelde Services (CSPSS) zijn de voorkeur keuze voor registratie bij een directe CSP of CSP distributeur werkt Azure-Stack.

CSPSS abonnementen worden gekoppeld aan een tenant gedeelde services. Wanneer u Azure-Stack registreert, moet u referenties opgeven voor een account dat is eigenaar van het abonnement. Het account waarmee u Azure-Stack registreren kan afwijken van de administrator-account die u voor de implementatie van gebruikt; de twee doen *niet* moet deel uitmaken van hetzelfde domein. Met andere woorden, u mogelijk implementeren met behulp van de tenant die u al gebruikt. U kunt bijvoorbeeld ContosoCSP.onmicrosoft.com gebruiken en vervolgens registreren met een andere tenant, bijvoorbeeld IURContosoCSP.onmicrosoft.com. U moet onthouden dat u zich met behulp van ContosoCSP.onmicrosoft.com aanmelden wanneer u beheer van de Azure-Stack dag to do doet. Wanneer u zich aanmeldt bij Azure met behulp van IURContosoCSP.onmicrosoft.com wanneer u wilt registreren.

Raadpleeg de volgende voor een beschrijving van CSPSS abonnementen en instructies voor het maken van abonnement [Azure Partner gedeelde Services toevoegen](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP-abonnementen

Cloudabonnementen Service Provider (CSP) zijn de voorkeur keuze voor registratie bij een wederverkoper CSP of een end-klant werkt Azure-Stack.

## <a name="register-azure-stack"></a>Azure Stack registreren

Als u wilt registreren met Azure-Stack, Zie [Azure Stack registreren met uw Azure-abonnement](azure-stack-registration.md).

## <a name="add-end-customer"></a>Einde klant toevoegen

Zie het Azure-Stack configureren zodat wanneer een nieuwe tenant resources gebruikt hun gebruik wordt gemeld aan hun Cloud Service Provider (CSP) abonnement [toevoegen tenant voor informatie over het gebruik en facturering naar Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>De juiste abonnementen kosten in rekening gebracht

Azure Stack maakt gebruik van een functie voor registratie. Een registratie is een object, opgeslagen in Azure, welke Azure-abonnementen te gebruiken om een bepaald Azure-Stack kosten documenten. Deze sectie wordt het belang van inschrijving.

Registratie met een Azure-Stack kunt doen:
 - Gebruiksgegevens van de Azure-Stack door te sturen naar Azure handel en een Azure-abonnement in rekening brengen.
 - Informatie over het gebruik van elke klant een rapport over een ander abonnement met een Azure-Stack multitenant-implementatie. Multitenancy kan Azure-Stack ondersteuning bieden voor verschillende organisaties op hetzelfde exemplaar van Azure-Stack.

Voor elke Azure-Stack is één standaardabonnement en veel abonnementen zo nodig de tenantsleutel. Het standaardabonnement is een Azure-abonnement in rekening gebracht wordt als er geen tenant-specifieke-abonnement bestaat. Het moet de eerste worden geregistreerd. Voor gebruik van meerdere tenants rapporten te laten werken, moet het abonnement een CSP of CSPSS abonnement.

De registratie wordt vervolgens bijgewerkt met een Azure-abonnement voor iedere tenant die er gebruik van Azure-Stack. Tenant-abonnementen moeten van het type van de CSP en moeten getotaliseerd naar de partner die eigenaar is van het standaardabonnement. Met andere woorden, registreren u klanten van iemand anders niet.

Wanneer informatie over het gebruik Azure Stack naar globale Azure doorstuurt, wordt een service in Azure raadpleegt de registratie en informatie over het gebruik van elke tenant toegewezen aan de juiste tenantabonnement. Als een tenant niet is geregistreerd, wordt die informatie over het gebruik gaat naar het standaardabonnement voor het Azure-Stack-exemplaar van waaruit deze afkomstig is.

Aangezien tenant abonnementen CSP abonnementen, hun factuur wordt verzonden naar de CSP-partner en informatie over het gebruik is niet zichtbaar voor de klant.



## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het programma CSP, [programma Cloud Solution Provider](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack, [gebruiks- en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).
