---
title: Het gebruik en facturering voor Azure Stack als een Cloudserviceprovider beheren | Microsoft Docs
description: Beknopt overzicht van Azure Stack registreren als een Cloud-Provider (CSP) en het toevoegen van klanten voor facturering.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 1ba3697b5c683ed2e892396db71328e0ed41fdce
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266914"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Het gebruik en facturering voor Azure Stack als een Cloudserviceprovider beheren

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel helpt u bij het registreren van Azure Stack als een Cloud-Provider (CSP) en het toevoegen van klanten.

Als een CSP werken u met diverse klanten met behulp van uw Azure Stack. Elke klant heeft een CSP-abonnement in Azure. U moet gebruik rechtstreeks afkomstig zijn van uw Azure-Stack aan elk gebruikersabonnement.

De volgende afbeelding ziet u de vereiste stappen voor het kiezen van uw gedeelde services-account en het Azure-account registreren bij Azure Stack-account. Zodra geregistreerd, kunt u Onboarding van uw eindgebruikers merken:

[![Proces voor het inschakelen van gebruik en beheer als een Cloudserviceprovider](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png "proces voor het inschakelen van gebruik en beheer als een Cloudserviceprovider")](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>Een CSP of APSS abonnement maken

### <a name="cloud-service-provider-subscription-types"></a>Cloud Service Provider-abonnementstypen

Kies het type gedeelde services-account dat u voor Azure Stack gebruikt. De typen die kunnen worden gebruikt voor de registratie van een multitenant Azure Stack-abonnementen zijn:

- Cloud Service Provider
- Partner Services gedeeld abonnement

#### <a name="azure-partner-shared-services"></a>Gedeelde Services van Azure-Partner

Azure Partner gedeelde Services (APSS)-abonnementen worden de beste keuze voor registratie bij een directe CSP of een CSP-distributor werkt Azure Stack.

APSS abonnementen worden gekoppeld aan een tenant gedeelde services. Als u Azure Stack registreert, kunt u referenties opgeven voor een account dat is eigenaar van het abonnement. Het account dat u gebruikt voor het registreren van Azure Stack kan afwijken van de administrator-account dat u voor de implementatie gebruikt van het netwerk. Bovendien, de twee accounts hoeft te behoren tot hetzelfde domein; u kunt implementeren met behulp van de tenant die u al gebruikt. Bijvoorbeeld, kunt u `ContosoCSP.onmicrosoft.com`, klikt u vervolgens registreren met behulp van een andere tenant; bijvoorbeeld `IURContosoCSP.onmicrosoft.com`. U moet zich aanmelden met `ContosoCSP.onmicrosoft.com` wanneer u dagelijkse beheer van Azure Stack uitvoert. U aanmelden bij Azure met `IURContosoCSP.onmicrosoft.com` wanneer u moet registratiebewerkingen uitvoeren.

Zie voor een beschrijving van de APSS inschrijvingen en hoe ze worden gemaakt, [toevoegen Azure Partner gedeelde Services](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP-abonnementen

Cloud Service Provider (CSP)-abonnementen zijn de beste keuze voor registratie wanneer een CSP-reseller of een eindklant werkt Azure Stack.

## <a name="register-azure-stack"></a>Azure Stack registreren

Gebruik de APSS abonnement gemaakt met behulp van de informatie in de voorgaande sectie voor het registreren van Azure Stack met Azure. Zie voor meer informatie, [Azure Stack registreren met uw Azure-abonnement](azure-stack-registration.md).

## <a name="add-end-customer"></a>Eindklant toevoegen

Zie configureren van Azure Stack zodat wanneer een nieuwe tenant resources gebruikt, wordt hun gebruik wordt gerapporteerd aan hun Cloud Service Provider (CSP)-abonnement [toevoegen van tenant voor gebruik en facturering met Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Kosten in rekening gebracht in de juiste abonnementen

Azure Stack maakt gebruik van een functie met de naam van de registratie. Een registratie is een object dat is opgeslagen in Azure. De registratie-object documenten welke Azure-abonnementen te gebruiken in rekening brengen van een bepaald Azure-Stack. Deze sectie heeft betrekking op het belang van de inschrijving.

Inschrijving met een Azure Stack kunt doen:

- Azure Stack-gebruiksgegevens doorsturen naar Azure Commerce en factureren van een Azure-abonnement.
- Rapport van elke klant '' s gebruik op een ander abonnement met een multitenant Azure Stack-implementatie. Multitenancy kan Azure Stack voor de ondersteuning van verschillende organisaties op hetzelfde exemplaar van Azure Stack.

Er is een standaardabonnement voor elke Azure Stack, en veel tenant-abonnementen. Het standaardabonnement is een Azure-abonnement dat in rekening gebracht wordt als er geen tenant-specifieke abonnement bestaat. Het moet het eerste abonnement te registreren. Voor meerdere tenants voor gebruiksrapporten te werken, moet het abonnement een CSP of APSS-abonnement.

De registratie wordt vervolgens bijgewerkt met een Azure-abonnement voor elke tenant die gebruikmaakt van Azure Stack. Tenant-abonnementen moeten van het type van de CSP en moeten getotaliseerd naar de partner die eigenaar is van het standaardabonnement. U kunt de klanten van iemand anders niet registreren.

Wanneer informatie over het gebruik van Azure Stack wordt doorgestuurd naar globale Azure, wordt een service in Azure raadpleegt de registratie en gebruik van elke tenant wordt toegewezen aan de juiste tenantabonnement. Als een tenant is niet geregistreerd, wordt dat verbruik gerouteerd naar de standaardabonnement voor de Azure Stack-instantie waaruit deze afkomstig is.

Aangezien tenant abonnementen CSP-abonnementen, de factuur is verzonden naar de CSP-partner en informatie over het gebruik is niet zichtbaar voor de eindklant.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het CSP-programma, [programma Cloud Solution Provider](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).
