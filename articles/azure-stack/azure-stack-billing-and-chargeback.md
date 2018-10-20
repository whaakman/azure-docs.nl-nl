---
title: De facturering van klanten en terugstorting in Azure Stack | Microsoft Docs
description: Informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: a5f3b206b83beb15ee3b29d5d5b9e389e85a91fb
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466984"
---
# <a name="usage-and-billing-in-azure-stack"></a>Gebruik en facturering in Azure Stack

Dit artikel wordt beschreven hoe Azure Stack-gebruikers worden in rekening gebracht voor gebruik van bronnen. U kunt informatie over hoe de factureringsgegevens voor analyse en kosten weer wordt geopend.

Azure Stack verzamelt en groepen van gebruiksgegevens voor resources die worden gebruikt. Azure Stack stuurt vervolgens deze gegevens naar Azure Commerce. Azure Commerce kosten in rekening gebracht voor gebruik met Azure Stack op dezelfde manier als die deze kosten in rekening voor gebruik van Azure gebracht.

U krijgt ook gebruiksgegevens en exporteren naar uw eigen facturering- of terugstortingscodes back-systeem met behulp van een adapter facturering of exporteren naar een business intelligence-hulpprogramma, zoals Microsoft Power BI.

## <a name="usage-pipeline"></a>Gebruik pijplijn

Elke resourceprovider in Azure Stack berichten gebruiksgegevens per gebruik. De usage-service periodiek (per uur en dagelijks) verzamelt gegevens over gebruik en slaat ze op in de Gebruiksdatabase. Azure Stack-operators en gebruikers kunnen de opgeslagen gebruiksgegevens toegang tot een via het Azure Stack-Resourcegebruik API's. 

Als u hebt [uw Azure Stack-instantie geregistreerd bij Azure](azure-stack-register.md), Azure Stack is geconfigureerd voor de gebruiksgegevens verzenden naar Azure Commerce. Nadat de gegevens wordt geüpload naar Azure, kunt u deze openen via de portal voor facturering of met behulp van Azure-Resourcegebruik API's. Zie voor meer informatie meer gegevens over welke informatie wordt gerapporteerd aan Azure, [gegevens gebruiksrapportage](azure-stack-usage-reporting.md).  

De volgende afbeelding toont de belangrijke onderdelen in de pijplijn gebruik: 

![Gebruik pijplijn](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welke informatie over het gebruik kan ik vinden, en hoe?

Azure Stack resourceproviders (zoals rekentijd, opslag en netwerk) genereren van gebruiksgegevens elk uur voor elk abonnement. Gegevens over gebruik bevat informatie over de resource die wordt gebruikt, zoals de resourcenaam van de, abonnement dat u gebruikt en hoeveelheid die wordt gebruikt. Zie voor meer informatie over de resources van meter-ID, de [Veelgestelde vragen over het gebruik van API](azure-stack-usage-related-faq.md).

Nadat de gebruiksgegevens zijn verzameld, is het [gerapporteerd aan Azure](azure-stack-usage-reporting.md) voor het genereren van een factuur, dat kan worden weergegeven via de Azure-factureringsportal. 

> [!NOTE]  
> Rapportage van gegevens over het gebruik is niet vereist voor de Azure Stack Development Kit (ASDK) en voor gebruikers van de geïntegreerde Azure Stack-systeem die onder het capaciteitsmodel van licentie. Zie voor meer informatie over licentieverlening in Azure Stack, de [-verpakking en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) gegevensblad.

De Azure-factureringsportal bevat gegevens over gebruik voor de toerekenbare resources. Naast de toerekenbare resources legt Azure Stack gebruiksgegevens voor een bredere set van resources die u in uw Azure Stack-omgeving via REST-API's of PowerShell-cmdlets openen kunt. Azure Stack-operators krijgt gegevens over gebruik voor alle gebruikersabonnementen. Afzonderlijke gebruikers krijgt alleen hun eigen informatie over het gebruik. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Gebruiksrapportage voor multitenant Cloud-serviceproviders

Een multitenant Cloud Service Provider (CSP) die veel klanten met behulp van Azure Stack kunt voor het rapporteren van het gebruik van elke klant afzonderlijk, zodat de provider kosten in rekening gebruik tot verschillende Azure-abonnementen gebracht kan. 

Elke klant beschikt over de identiteit die wordt vertegenwoordigd door een andere Azure Active Directory (Azure AD)-tenant. Azure Stack biedt ondersteuning voor toewijzen een CSP-abonnement aan elke Azure AD-tenant. U kunt tenants en hun abonnementen toevoegen aan de basis Azure Stack-registratie. De registratie van de basis is uitgevoerd voor alle Azure-Stacks. Als een abonnement is niet geregistreerd voor een tenant, Azure Stack kunt nog steeds gebruiken door de gebruiker en hun gebruik wordt verzonden naar het abonnement dat is gebruikt voor de registratie van de basis. 

## <a name="next-steps"></a>Volgende stappen

- [Registreren bij Azure Stack](azure-stack-registration.md)
- [Rapport van Azure Stack-gebruiksgegevens naar Azure](azure-stack-usage-reporting.md)
- [Resourcegebruik-provider API](azure-stack-provider-resource-api.md)
- [Resourcegebruik-API voor de tenantsleutel](azure-stack-tenant-resource-usage-api.md)
- [Veelgestelde vragen over het gebruik gerelateerde](azure-stack-usage-related-faq.md)