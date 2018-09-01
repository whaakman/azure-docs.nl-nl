---
title: Het gebruik en facturering voor Azure Stack als een Cloudserviceprovider beheren | Microsoft Docs
description: Beknopt overzicht van Azure Stack registreren als een Cloud-Provider (CSP) en het toevoegen van klanten voor facturering.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: 9bb4a4ea81f2dc0fb11e2f7cae1b9d02b0edfdde
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341432"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Het gebruik en facturering voor Azure Stack als een Cloudserviceprovider beheren 

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel helpt u bij het registreren van Azure Stack als een Cloud-Provider (CSP) en het toevoegen van klanten.

Als een CSP werken u met diverse klanten met behulp van uw Azure Stack. Elke klant heeft een CSP-abonnement in Azure. U moet rechtstreeks gebruik vanuit uw Azure-Stack aan elk gebruikersabonnement.

Het volgende diagram toont de stappen die u moet uw gedeelde services-account kiezen en het registreren van het Azure-account met Azure Stack-account. Geregistreerd, kunt u Onboarding van uw eindgebruikers merken.

**Stappen voor het gebruik bijhouden als een CSP toevoegen**

![Het proces voor het inschakelen van gebruik en beheer als een Cloudserviceprovider.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>Een CSP of CSPSS abonnement maken

### <a name="cloud-service-provider-subscription-types"></a>Cloud Service Provider-abonnementstypen

U moet het type gedeelde services-account dat u voor Azure Stack gebruikt kiezen. De typen die kunnen worden gebruikt voor de registratie van een multitenant Azure Stack-abonnementen zijn:

 - Cloudserviceprovider 
 - Partner Services gedeeld abonnement 

#### <a name="csp-shared-services"></a>CSP gedeelde Services

Cloud Service Provider gedeelde Services (CSPSS)-abonnementen zijn de beste keuze voor registratie wanneer een directe CSP of een CSP-Distributor werkt Azure Stack.

CSPSS abonnementen worden gekoppeld aan een tenant gedeelde services. Wanneer u zich registreert voor Azure Stack, moet u referenties opgeven voor een account dat is eigenaar van het abonnement. Het account dat u gebruikt voor het registreren van Azure Stack kan afwijken van de administrator-account dat u voor de implementatie gebruikt van het netwerk. Bovendien, de twee accounts doen *niet* moet deel uitmaken van hetzelfde domein bevinden. Met andere woorden, kunt u implementeren met behulp van de tenant die u al gebruikt. U kunt bijvoorbeeld ContosoCSP.onmicrosoft.com gebruiken en vervolgens registreren met behulp van een andere tenant, bijvoorbeeld IURContosoCSP.onmicrosoft.com. U moet Houd er rekening mee dat u zich met behulp van ContosoCSP.onmicrosoft.com aanmelden wanneer u dit dag to do Azure Stack-beheer doet. Wanneer u zich aanmeldt bij Azure met behulp van IURContosoCSP.onmicrosoft.com wanneer u nodig hebt voor van registratiebewerkingen.

Raadpleeg de volgende voor een beschrijving van CSPSS abonnementen en instructies voor het maken van abonnement [toevoegen Azure Partner gedeelde Services](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP-abonnementen

Cloud Service Provider (CSP)-abonnementen zijn de beste keuze voor registratie wanneer een CSP-reseller of een eindklant werkt Azure Stack.

## <a name="register-azure-stack"></a>Azure Stack registreren

Gebruik het CSPSS abonnement gemaakt na de informatie in de vorige sectie om te registreren Azure Stack met Azure. Zie voor meer informatie, [Azure Stack registreren met uw Azure-abonnement](azure-stack-registration.md).

## <a name="add-end-customer"></a>Eindklant toevoegen

Zie configureren van Azure Stack zodat wanneer een nieuwe tenant maakt gebruik van resources hun gebruik gerapporteerd aan hun Cloud Service Provider (CSP)-abonnement [toevoegen van tenant voor gebruik en facturering met Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Kosten in rekening gebracht in de juiste abonnementen

Azure Stack maakt gebruik van een functie met de naam van de registratie. Een registratie is een object dat is opgeslagen in Azure. De registratie-object documenten welke Azure-abonnementen te gebruiken in rekening brengen van een bepaald Azure-Stack. Deze sectie heeft betrekking op het belang van de inschrijving.

Inschrijving met een Azure Stack kunt doen:
 - Azure Stack-gebruiksgegevens doorsturen naar Azure Commerce en factureren van een Azure-abonnement.
 - Gebruik van elke klant een rapport over een ander abonnement met een multitenant Azure Stack-implementatie. Multitenancy kan Azure Stack voor de ondersteuning van verschillende organisaties op hetzelfde exemplaar van Azure Stack.

Er is een standaardabonnement voor elke Azure Stack, en veel tenant-abonnementen. Het standaardabonnement is een Azure-abonnement dat in rekening gebracht wordt als er geen een tenant-specifieke abonnement. Het moet abonnement geregistreerd als eerste zijn. Voor meerdere tenants voor gebruiksrapporten te werken, moet het abonnement een CSP of CSPSS-abonnement.

De registratie wordt vervolgens bijgewerkt met een Azure-abonnement voor elke tenant die u wilt gebruiken van Azure Stack. Tenant-abonnementen moeten van het CSP-type en moeten getotaliseerd naar de partner die eigenaar is van het standaardabonnement. Met andere woorden, registreren u iemand anders van klanten niet.

Wanneer informatie over het gebruik van Azure Stack wordt doorgestuurd naar globale Azure, wordt een service in Azure raadpleegt de registratie en gebruik van elke tenant wordt toegewezen aan de juiste tenantabonnement. Als een tenant is niet geregistreerd, wordt dat verbruik gerouteerd naar de standaardabonnement voor de Azure Stack-instantie waaruit deze afkomstig is.

Aangezien tenant abonnementen CSP-abonnementen, de factuur is verzonden naar de CSP-partner en informatie over het gebruik is niet zichtbaar voor de eindklant.

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het CSP-programma, [programma Cloud Solution Provider](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).
