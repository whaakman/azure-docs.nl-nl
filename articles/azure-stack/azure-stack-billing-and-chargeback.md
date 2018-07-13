---
title: Klant-facturering en toerekenen In Azure Stack | Microsoft Docs
description: Informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: f2449176f96b18a374ff6d54fbf7e09c8f5e21cc
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003574"
---
# <a name="usage-and-billing-in-azure-stack"></a>Gebruik en facturering in Azure Stack

Dit artikel wordt beschreven hoe Azure Stack-gebruikers worden in rekening gebracht voor gebruik van bronnen. U kunt informatie over hoe de factureringsgegevens voor analyse en kosten weer wordt geopend.

Azure Stack verzamelt en gebruiksgegevens voor gebruikte resources worden gegroepeerd. Azure Stack stuurt vervolgens deze gegevens naar Azure Commerce. Azure Commerce kosten in rekening gebracht voor gebruik met Azure Stack op dezelfde manier als u voor gebruik van Azure factureren zou.

U krijgt ook gebruiksgegevens en exporteren naar uw eigen facturering- of terugstortingscodes back-systeem met behulp van een adapter facturering of exporteren naar een business intelligence-hulpprogramma, zoals Microsoft Power BI.


## <a name="usage-pipeline"></a>Gebruik pijplijn

Elke resourceprovider in Azure Stack berichten gebruiksgegevens per gebruik. De Usage-Service periodiek (per uur en dagelijks) verzamelt gegevens over gebruik en slaat ze op in de Gebruiksdatabase. Azure Stack-operators en gebruikers kunnen de opgeslagen gebruiksgegevens toegang tot een via de API's van Azure Stack Resource gebruik. 

Als u hebt [uw Azure Stack-instantie geregistreerd bij Azure](azure-stack-register.md), Azure Stack is geconfigureerd voor de gebruiksgegevens verzenden naar Azure Commerce. Nadat de gegevens wordt geüpload naar Azure, kunt u deze openen via de portal voor facturering of met behulp van API's voor gebruik van Azure-Resource. Raadpleeg de [gegevens gebruiksrapportage](azure-stack-usage-reporting.md) artikel voor meer informatie meer gegevens over welke informatie wordt gerapporteerd aan Azure.  

De volgende afbeelding toont de belangrijke onderdelen in de pijplijn gebruik: 

![Gebruik pijplijn](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welke informatie over het gebruik kan ik vinden, en hoe?

Azure Stack Resource providers (zoals rekentijd, opslag en netwerk) genereren van gebruiksgegevens elk uur voor elk abonnement. Gegevens over gebruik bevat informatie over de resource die wordt gebruikt, zoals de resourcenaam van de, abonnement dat u gebruikt en hoeveelheid die wordt gebruikt. Raadpleeg voor meer informatie over de resources van meter-ID, de [Veelgestelde vragen over het gebruik van API](azure-stack-usage-related-faq.md) artikel.

Nadat de gebruiksgegevens zijn verzameld, is het [gerapporteerd aan Azure](azure-stack-usage-reporting.md) voor het genereren van een factuur, dat kan worden weergegeven via de Azure-factureringsportal. 

> [!NOTE]  
> Rapportage van gegevens over het gebruik is niet vereist voor Azure Stack Development Kit en voor gebruikers van de geïntegreerde Azure Stack-systeem die onder het capaciteitsmodel van licentie. Zie voor meer informatie over licentieverlening in Azure Stack, de [-verpakking en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) gegevensblad.

De Azure-factureringsportal bevat gegevens over gebruik voor de toerekenbare resources. Naast de toerekenbare resources legt Azure Stack gebruiksgegevens voor een bredere set van resources die u in uw Azure Stack-omgeving via REST-API's of PowerShell openen kunt. Azure Stack-operators krijgt gegevens over gebruik voor alle gebruikersabonnementen. Afzonderlijke gebruikers krijgt alleen informatie over hun gebruik. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Gebruiksrapportage voor multitenant Cloud-serviceproviders

Een multitenant Cloud Service Provider (CSP) die veel klanten met behulp van Azure Stack kunt voor het rapporteren van het gebruik van elke klant afzonderlijk, zodat de provider kosten in rekening gebruik tot verschillende Azure-abonnementen gebracht kan. 

Elke klant u hun identiteit die wordt vertegenwoordigd door een andere Azure Active Directory (Azure AD)-tenant. Azure Stack biedt ondersteuning voor toewijzen een CSP-abonnement aan elke Azure AD-tenant. U kunt tenants en hun abonnementen toevoegen aan de basis Azure Stack-registratie. De registratie van de basis is uitgevoerd voor alle Azure-Stacks. Als een abonnement is niet geregistreerd voor een tenant, Azure Stack kunt nog steeds gebruiken door de gebruiker en hun gebruik wordt verzonden naar het abonnement dat is gebruikt voor de registratie van de basis. 


## <a name="next-steps"></a>Volgende stappen

[Registreren bij Azure Stack](azure-stack-registration.md)

[Rapport van Azure Stack-gebruiksgegevens naar Azure](azure-stack-usage-reporting.md)

[Resourcegebruik-provider API](azure-stack-provider-resource-api.md)

[Resourcegebruik-API voor de tenantsleutel](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik gerelateerde](azure-stack-usage-related-faq.md)