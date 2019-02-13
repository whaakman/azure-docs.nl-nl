---
title: Azure AD Connect Health - Health-servicegegevens zijn niet tot de datum-waarschuwing | Microsoft Docs
description: Dit document beschrijft de oorzaak van de waarschuwing 'Health service-gegevens is niet up-to-date' en het oplossen van deze.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35586de180b1193e9886677ce4112eaa051395ae
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196753"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Health service-gegevens is niet up-to-date waarschuwing

## <a name="overview"></a>Overzicht
<li>Azure AD Connect Health genereert nieuwe gegevensmelding wanneer deze niet alle gegevenspunten van de server gedurende twee uur ontvangen worden. De titel van de waarschuwing is **Health service-gegevens is niet up-to-date**. </li>
<li>De **waarschuwing** status waarschuwing wordt geactiveerd als Connect Health geen gedeeltelijke gegevenselementen verzonden vanaf de server voor twee uur ontvangt. De status waarschuwing wordt niet geactiveerd voor e-mailmeldingen naar de tenant-beheerder. </li>
<li>De **fout** status waarschuwing wordt geactiveerd als Connect Health worden verzonden vanaf de server voor twee uur gegevenselementen niet ontvangt. Fout bij de status waarschuwing triggers e-mailmeldingen naar de tenant-beheerder. </li>

>[!IMPORTANT] 
> Deze waarschuwing volgt Connect Health [bewaarbeleid voor gegevens](reference-connect-health-user-privacy.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing 
* Zorg ervoor dat u via en voldoen aan de [gedeelte vereisten](how-to-connect-health-agent-install.md#requirements).
* Gebruik [test connectiviteitshulpprogramma](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) voor het detecteren van problemen met de netwerkverbinding.
* Als u HTTP-Proxy hebt, volgt u [configuratiestappen hier](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

### <a name="connect-health-for-sync"></a>Connect Health for Sync

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [De opgegeven websites toestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
| AadSyncService-SynchronizationRules <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | -De uitgaande connectiviteit op basis van IP-adressen, verwijzen naar [IP-adresbereiken voor Azure](https://www.microsoft.com/download/details.aspx?id=41653) <br /> - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health voor AD FS

Extra stappen uitvoeren om te valideren voor AD FS en volgt u de werkstroom in [Help bij AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter, TestResult | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [De opgegeven websites toestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
|  Adfs-UsageMetrics | Uitgaande verbindingen op basis van IP-adressen, verwijzen naar [IP-adresbereiken voor Azure](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>Connect Health voor ADDS

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter, voegt-TopologyInfo-Json, algemene TestData Json | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> - [SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [De opgegeven websites toestaan als verbeterde beveiliging van Internet Explorer is ingeschakeld](https://technet.microsoft.com/windows/ms537180(v=vs.60)) <br />  -De uitgaande connectiviteit op basis van IP-adressen, verwijzen naar [IP-adresbereiken voor Azure](https://www.microsoft.com/download/details.aspx?id=41653)  |


## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
