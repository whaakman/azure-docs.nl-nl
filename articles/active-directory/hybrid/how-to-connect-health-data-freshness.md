---
title: Azure AD Connect Health - Health-servicegegevens zijn niet tot de datum-waarschuwing | Microsoft Docs
description: Dit document beschrijft de oorzaak van de waarschuwing 'Health service-gegevens is niet up-to-date' en het oplossen van deze.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ad829b976d8b712ee8027c89fb618c6c07de1bc
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429007"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Health service-gegevens is niet up-to-date waarschuwing

## <a name="overview"></a>Overzicht
De Agents op de on-premises machines die Azure AD Connect Health periodiek controleert worden gegevens geüpload naar Azure AD Connect Health-Service. Als de service geen gegevens van een agent ontvangt, is de informatie in de portal is verlopen. Het probleem wilt markeren, de service wordt er **Health service-gegevens is niet up-to-date** waarschuwing. Dit wordt gegenereerd wanneer de service geen gegevens in de afgelopen twee uur ontvangen heeft.  

* De **waarschuwing** status waarschuwing wordt geactiveerd als Connect Health geen gedeeltelijke gegevenselementen verzonden vanaf de server voor twee uur ontvangt. De status waarschuwing wordt niet geactiveerd voor e-mailmeldingen naar de tenant-beheerder.
* De **fout** status waarschuwing wordt geactiveerd als Connect Health worden verzonden vanaf de server voor twee uur gegevenselementen niet ontvangt. Fout bij de status waarschuwing triggers e-mailmeldingen naar de tenant-beheerder.


## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing 

> [!IMPORTANT] 
> Deze waarschuwing volgt Connect Health [bewaarbeleid voor gegevens](reference-connect-health-user-privacy.md#data-retention-policy)

* Zorg ervoor dat de services van Azure AD Connect Health-Agents worden uitgevoerd op de machine. Connect Health voor AD FS moet bijvoorbeeld drie services hebben.  
  ![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install5.png)

* Zorg ervoor dat u via en voldoen aan de [gedeelte vereisten](how-to-connect-health-agent-install.md#requirements).
* Gebruik [test connectiviteitshulpprogramma](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) voor het detecteren van problemen met de netwerkverbinding.
* Als u een HTTP-Proxy hebt, volgt u deze [configuratiestappen](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

De Waarschuwingsdetails-blade een lijst met ontbrekende gegevens element(en) van een server. De volgende tabel wordt Verfijn het probleem verder helpen. 
### <a name="connect-health-for-sync"></a>Connect Health for Sync

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| AadSyncService-SynchronizationRules <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health voor AD FS

Extra stappen uitvoeren om te valideren voor AD FS en volgt u de werkstroom in [Help bij AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter, TestResult | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
|  Adfs-UsageMetrics | Uitgaande verbindingen op basis van IP-adressen, verwijzen naar [IP-adresbereiken voor Azure](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>Connect Health voor ADDS

| Gegevenselementen | Stappen voor probleemoplossing |
| --- | --- | 
| PerfCounter, voegt-TopologyInfo-Json, algemene TestData Json | - [Uitgaande connectiviteit met het Azure-service-eindpunt](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Firewall-poorten op de server waarop de agent wordt uitgevoerd](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |


## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
