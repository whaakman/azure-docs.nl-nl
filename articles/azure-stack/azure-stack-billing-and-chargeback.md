---
title: Klant-facturering en terugstorting In Azure-Stack | Microsoft Docs
description: Informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: eca335797f48b7c44351655f17c8b6499f3d5999
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="usage-and-billing-in-azure-stack"></a>Informatie over het gebruik en facturering in Azure-Stack

Dit artikel wordt beschreven hoe gebruikers Azure Stack wordt gefactureerd voor het gebruik van bronnen. U leert u hoe de factureringsgegevens voor analytics en kosten-back wordt geopend.

Azure Stack verzamelt en groepen van gebruiksgegevens voor alle resources die worden gebruikt en verzendt deze gegevens naar Azure Commerce. Azure Commerce stuklijsten u voor de Azure-Stack-gebruik op dezelfde manier als voor het gebruik van Azure kosten zou.

U kunt ook gebruiksgegevens en exporteren naar uw eigen facturering of kosten back-systeem met behulp van een facturering adapter of exporteren naar een business intelligence-hulpprogramma zoals Microsoft Power BI en deze gebruiken voor analytics ophalen.


## <a name="usage-pipeline"></a>Gebruik-pipeline

Elke resourceprovider in Azure-Stack verzendt gebruiksgegevens per Resourcegebruik. De Service Usage periodiek (elk uur en per dag) maakt een aggregatie van gebruiksgegevens en opgeslagen in de Gebruiksdatabase. Azure Stack operators en gebruikers toegang tot de opgeslagen gebruiksgegevens via de API's van Azure Stack van Resource-gebruik. 

Als u hebt [uw Azure-Stack-instantie geregistreerd bij Azure](azure-stack-register.md), Azure-Stack is geconfigureerd voor de gebruiksgegevens verzenden naar Azure Commerce. Nadat de gegevens is geüpload naar Azure, kunt u deze openen via de facturering portal of met behulp van API's voor informatie over het gebruik van Azure-Resource. Raadpleeg de [gegevens gebruiksrapportage](azure-stack-usage-reporting.md) onderwerp leest u meer gegevens over het gebruik van welke is gerapporteerd aan Azure.  

De volgende afbeelding toont de belangrijke onderdelen in de pijplijn gebruik: 

![Gebruik-pipeline](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welke informatie over het gebruik vind ik, en hoe?

Azure Resource Stack-providers, zoals de berekenings-, opslag- en netwerk, genereren de gebruiksgegevens elk uur voor elk abonnement. De gebruiksgegevens bevat informatie over de resource die wordt gebruikt zoals resourcenaam abonnement dat u gebruikt en gebruikte hoeveelheid. Raadpleeg voor meer informatie over de resources meters-ID, de [Veelgestelde vragen over het gebruik van API](azure-stack-usage-related-faq.md) artikel.

Nadat de gebruiksgegevens zijn verzameld, is het [gerapporteerd aan Azure](azure-stack-usage-reporting.md) voor het genereren van een factuur, kan worden weergegeven in de Azure-facturering portal. 


> [!NOTE]
> Gebruiksrapportage van gegevens is niet vereist voor Azure Stack Development Kit en voor Azure-Stack geïntegreerd systeemgebruikers die een licentie onder het capaciteitsmodel. Zie voor meer informatie over licentieverlening in Azure-Stack, de [Inpakken en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) blad.

De Azure-facturering portal ziet u gebruiksgegevens voor de toerekenbare bronnen. Naast de toerekenbare resources bevat Azure Stack gebruiksgegevens voor een uitgebreidere set resources die u in uw omgeving Azure Stack via REST-API's of PowerShell openen kunt. Azure Stack-operators krijgt de gebruiksgegevens voor alle gebruikersabonnementen. Afzonderlijke gebruikers kunnen alleen informatie over hun gebruik ophalen. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Gebruik reporting voor multitenant Cloudserviceproviders

Een multitenant-Cloud Service Provider (CSP) die veel klanten die gebruikmaken van Azure-Stack heeft mogelijk wilt rapporteren gebruik van de klant afzonderlijk, zodat de provider kosten in rekening gebruik van verschillende Azure-abonnementen gebracht kunt. 

Elke klant gaat om hun identiteit die wordt vertegenwoordigd door een andere tenant van Azure Active Directory (Azure AD). Azure Stack ondersteunt toewijzen één CSP-abonnement aan elke Azure AD-tenant. U kunt tenants en hun abonnementen toevoegen aan de basis Azure Stack-registratie. De registratie van de basis wordt voor alle Azure-Stacks gedaan. Als een abonnement is niet geregistreerd voor een tenant, Azure Stack kunt nog steeds gebruiken door de gebruiker en hun gebruik wordt verzonden naar het abonnement dat u gebruikt voor de registratie van de basis. 


## <a name="next-steps"></a>Volgende stappen

[Registreren bij Azure Stack](azure-stack-registration.md)

[Azure Stack gebruiksgegevens rapporteren naar Azure](azure-stack-usage-reporting.md)

[Provider Resourcegebruik API](azure-stack-provider-resource-api.md)

[Tenant-Resourcegebruik API](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)