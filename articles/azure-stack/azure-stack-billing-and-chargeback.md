---
title: Klant-facturering en terugstorting In Azure-Stack | Microsoft Docs
description: Informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: alfredop
ms.openlocfilehash: ea7510c239ee07a9a27f3e682e61a6b08eb5694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="usage-and-billing-in-azure-stack"></a>Informatie over het gebruik en facturering in Azure-Stack

Gebruik vertegenwoordigt de hoeveelheid resources verbruikt door een gebruiker. Azure Stack verzamelt informatie over het gebruik voor elke gebruiker en gebruikt deze om ze in rekening. Dit artikel wordt beschreven hoe gebruikers Azure Stack wordt gefactureerd voor het gebruik van bronnen en hoe de factureringsgegevens wordt geopend voor analyses, terugstorting, enzovoort.

Azure Stack bevat de infrastructuur te verzamelen en samenvoegen van gegevens over het gebruik voor alle resources en voor het doorsturen van deze gegevens naar Azure commerce. U kunt toegang tot deze gegevens exporteren naar een factureringssysteem met behulp van een adapter facturering en exporteren naar een business intelligence-hulpprogramma zoals Microsoft Power BI. Nadat u hebt geëxporteerd, is deze factureringsgegevens gebruikt voor analyses of overgedragen naar een terugstorting systeem.

![Conceptuele model van Azure-Stack verbinden met een facturering facturering netwerkadapter toepassing](media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>Gebruik-pipeline

Elke resourceprovider in Azure-Stack verzendt gebruiksgegevens aan de hand van bronnen beter worden benut. De Service Usage regelmatig (elk uur of dagelijks) aggregeert deze gebruiksgegevens en slaat ze op in de Gebruiksdatabase. De opgeslagen gebruiksgegevens toegankelijk zijn voor Azure-Stack operators en gebruikers lokaal met behulp van API's voor gebruik. 

Als u hebt [uw Azure-Stack-instantie geregistreerd bij Azure](azure-stack-register.md), gebruik Bridge is geconfigureerd voor de gebruiksgegevens verzenden naar Azure commerce. Nadat de gegevens in Azure beschikbaar is, kunt u deze openen via de facturering portal of met behulp van Azure usage-API's. Raadpleeg de [gegevens gebruiksrapportage](azure-stack-usage-reporting.md) onderwerp leest u meer gegevens over het gebruik van welke is gerapporteerd aan Azure. 

De volgende afbeelding toont de belangrijke onderdelen in de pijplijn gebruik:

![Gebruik-pipeline](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welke informatie over het gebruik vind ik, en hoe?

Azure Resource Stack-providers, zoals de berekenings-, opslag- en netwerk, genereren de gebruiksgegevens elk uur voor elk abonnement. De gebruiksgegevens bevat informatie over de resource die wordt gebruikt zoals resourcenaam, abonnement dat u gebruikt, de hoeveelheid die wordt gebruikt, enzovoort. Raadpleeg voor meer informatie over de resources meters-ID, de [gebruik API Veelgestelde vragen over](azure-stack-usage-related-faq.md) artikel. 

Nadat de gebruiksgegevens zijn verzameld, is het [gerapporteerd aan Azure](azure-stack-usage-reporting.md) voor het genereren van een factuur, kan worden weergegeven in de Azure-facturering portal. 

> [!NOTE]
> Gebruiksrapportage van gegevens is niet vereist voor Azure Stack Development Kit en voor Azure-Stack geïntegreerd systeemgebruikers die een licentie onder het capaciteitsmodel. Zie voor meer informatie over licentieverlening in Azure-Stack, de [Inpakken en prijzen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) blad.

De Azure-facturering portal ziet u de gebruiksgegevens alleen voor de toerekenbare resources. Naast de toerekenbare resources bevat Azure Stack gebruiksgegevens voor een uitgebreidere set resources die u in uw omgeving Azure Stack via REST-API's of PowerShell openen kunt. Azure Stack-operators kunnen de gebruiksgegevens voor alle gebruikersabonnementen ophalen dat een gebruiker de details van hun gebruik krijgt.

## <a name="retrieve-usage-information"></a>Ophalen van gebruiksgegevens

Als de gebruiksgegevens worden gegenereerd, moet u de resources die worden uitgevoerd en dat actief met behulp van het systeem, bijvoorbeeld, een actieve virtuele machine of een opslagaccount met bepaalde gegevens enzovoort hebben. Als u niet zeker weet of u bronnen die worden uitgevoerd in Azure Stack Marketplace, een virtuele machine (VM) te implementeren en controleren van de virtuele machine wordt bewaking blade om te controleren of deze uitgevoerd. De volgende PowerShell-cmdlets gebruiken om de gebruiksgegevens weer te geven:

1. [Installeer PowerShell voor Azure-Stack.](azure-stack-powershell-install.md)
2. [Configureren van de gebruiker van de Azure-Stack](user/azure-stack-powershell-configure-user.md) of de [Azure Stack-operator](azure-stack-powershell-configure-admin.md) PowerShell-omgeving 

3. Voor het ophalen van de gebruiksgegevens, gebruiken de [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell-cmdlet:

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>Volgende stappen

[Azure Stack gebruiksgegevens rapporteren naar Azure](azure-stack-usage-reporting.md)

[Provider Resourcegebruik API](azure-stack-provider-resource-api.md)

[Tenant-Resourcegebruik API](azure-stack-tenant-resource-usage-api.md)

[Veelgestelde vragen over het gebruik van gerelateerde](azure-stack-usage-related-faq.md)

