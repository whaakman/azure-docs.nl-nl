---
title: Meld u Analytics-functies voor serviceproviders | Microsoft Docs
description: Log Analytics kunt beheerd serviceproviders (MSPs) grote ondernemingen Independent Software Vendors (ISV's) en hosting serviceproviders beheren en bewaken van servers in een van de klant on-premises of cloudinfrastructuur.
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 3910038e788352df45ab00c0f697d9a5426b3498
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="log-analytics-features-for-service-providers"></a>Log Analytics-functies voor serviceproviders
Log Analytics kunt beheerde serviceproviders (MSPs), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hosting serviceproviders beheren en bewaken van servers in een van de klant on-premises of cloudinfrastructuur. 

Grote ondernemingen delen veel overeenkomsten met serviceproviders, vooral wanneer u een centrale IT-team dat verantwoordelijk is voor het beheren van IT voor veel verschillende bedrijfseenheden. Voor het gemak, dit document wordt de term *serviceprovider* maar dezelfde functionaliteit is ook beschikbaar voor ondernemingen en andere klanten.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider
Voor partners en leveranciers van een service die deel uitmaken van de [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programma, Log Analytics is een van de Azure-services beschikbaar zijn in [CSP Azure-abonnement](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

Log Analytics, worden de volgende mogelijkheden ingeschakeld in *Cloud Solution Provider* abonnementen.

Als een *Cloud Solution Provider* kunt u:

* Log Analytics-werkruimten maken in een tenant (de klant) abonnement.
* Access-werkruimten die zijn gemaakt door tenants. 
* Toevoegen en verwijderen van de gebruikerstoegang tot de werkruimte met behulp van Azure Gebruikersbeheer. Wanneer in een tenant-werkruimte in de OMS-portal de Gebruikersbeheer pagina onder instellingen is niet beschikbaar
  * Log Analytics biedt geen ondersteuning voor op rollen gebaseerde toegang nog -zodat een gebruiker `reader` machtiging in de Azure portal kan ze configuratiewijzigingen aanbrengen in de OMS-portal

Voor het aanmelden bij een tenant-abonnement, moet u de tenant-id opgeven. De tenant-id is vaak laatste deel van het e-mailadres gebruikt om aan te melden.

* Voeg in de OMS-portal `?tenant=contoso.com` in de URL voor de portal. Bijvoorbeeld: `mms.microsoft.com/?tenant=contoso.com`
* Gebruik in PowerShell de `-Tenant contoso.com` parameter wanneer u `Add-AzureRmAccount` cmdlet
* De tenant-id wordt automatisch toegevoegd wanneer u de `OMS portal` koppeling van de Azure-portal te openen en aanmelden bij de OMS-portal voor de geselecteerde werkruimte

Als een *klant* van een Cloud Solution Provider die u kunt:

* Log analytics-werkruimten maken in een CSP-abonnement
* Toegang tot werkruimten die zijn gemaakt door de CSP
  * Gebruik de `OMS portal` koppeling van de Azure-portal te openen en aanmelden bij de OMS-portal voor de geselecteerde werkruimte
* Weergeven en gebruiken van de pagina voor gebruikersbeheer onder de instellingen in de OMS-portal

> [!NOTE]
> De opgenomen back-up en herstel van de Site-oplossingen voor logboekanalyse kunnen geen verbinding maken met een Recovery Services-kluis en kunnen niet worden geconfigureerd in een CSP-abonnement. 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>Het beheren van meerdere klanten met Log Analytics
Het wordt aanbevolen dat u een werkruimte voor logboekanalyse maken voor elke klant die u beheert. Een werkruimte voor logboekanalyse biedt:

* Een geografische locatie voor de gegevens worden opgeslagen. 
* Details voor facturering 
* Gegevensisolatie 
* Unieke configuratie

Als u een werkruimte per klant maakt, bent u kunnen de gegevens van elke klant gescheiden te houden en ook het gebruik van elke klant bijhouden.

Meer informatie over wanneer en waarom meerdere werkruimten maken wordt beschreven in [beheer toegang tot analytics Meld](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Maken en de configuratie van de klant werkruimten kunnen worden geautomatiseerd met [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md), of met behulp van de [REST-API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Het gebruik van Resource Manager-sjablonen voor de configuratie van de werkruimte kunt u een master-configuratie die kunnen worden gebruikt voor het maken en configureren van werkruimten. U kunt er zeker van te zijn als werkruimten zijn gemaakt voor klanten automatisch geconfigureerd zijn voor uw vereisten zijn. Wanneer u de vereisten van uw bijwerkt, wordt de sjabloon wordt bijgewerkt en vervolgens de bestaande werkruimten toegepast. Dit proces zorgt ervoor dat uw nieuwe standaarden voldoen aan de zelfs bestaande werkruimten.    

Wanneer het beheer van meerdere Log Analytics-werkruimten, raden we aan elke werkruimte integreren met uw bestaande ticketsysteem gebruikt / operations-console met de [waarschuwingen](log-analytics-alerts.md) functionaliteit. Door te integreren met uw bestaande systemen, kunnen ondersteuningsmedewerkers blijven hun bekend procedures volgen. Log Analytics regelmatig gecontroleerd elke werkruimte tegen de waarschuwingscriteria die u opgeeft en genereert een waarschuwing wanneer de actie te ondernemen.

Voor aangepaste weergaven van gegevens, gebruikt u de [dashboard](../azure-portal/azure-portal-dashboards.md) mogelijkheden in de Azure-portal.  

Voor executive niveau rapporten die gegevens worden samengevat in werkruimten kunt u de integratie tussen Log Analytics en [PowerBI](log-analytics-powerbi.md). Als u integreren met een andere rapportagesysteem wilt, kunt u de Search-API (via PowerShell of [REST](log-analytics-log-search-api.md)) voor het uitvoeren van query's en zoekresultaten exporteren.

## <a name="next-steps"></a>Volgende stappen
* Het maken en de configuratie van werkruimten met automatiseren [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md)
* Het maken van werkruimten met automatiseren [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Gebruik [waarschuwingen](log-analytics-alerts.md) integreren met bestaande systemen
* Samenvatting rapporten genereren met de [Power BI](log-analytics-powerbi.md)

