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
ms.openlocfilehash: 3ffd783ec41b1b0c4a11ee426648c1e36fbbbf75
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883697"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Health service-gegevens is niet up-to-date waarschuwing

## <a name="overview"></a>Overzicht

De agents op de on-premises computers die Azure AD Connect Health regelmatig controleert uploaden gegevens naar de Azure AD Connect Health-Service. Als de service geen gegevens van een agent ontvangt, is de informatie het portal geeft is verlopen. Het probleem wilt markeren, de service verhoogt de **Health service-gegevens is niet up-to-date** waarschuwing. Deze waarschuwing wordt gegenereerd wanneer de service niet volledige gegevens in de afgelopen twee uur ontvangen heeft.  

- De **waarschuwing** status waarschuwing wordt geactiveerd als de Health-Service alleen heeft ontvangen **gedeeltelijke** gegevenstypen die zijn verzonden vanaf de server in de afgelopen twee uur. De status waarschuwing wordt niet geactiveerd voor e-mailmeldingen naar geconfigureerde ontvangers. 
- De **fout** status waarschuwing wordt geactiveerd als de Health-Service niet alle gegevenstypen van de server in de afgelopen twee uur ontvangen is. De status fout waarschuwing krijgt e-mailmeldingen naar geconfigureerde ontvangers.

De gegevens van de service worden opgehaald van agents die worden uitgevoerd op de on-premises computers, afhankelijk van het type van de service. De volgende tabel bevat de agents die worden uitgevoerd op de computer, wat ze doen en de gegevenstypen die de service wordt gegenereerd. In sommige gevallen zijn er meerdere services die betrokken zijn bij het proces, zodat ze van het overmatig zijn kan. 

## <a name="understanding-the-alert"></a>Informatie over de waarschuwing

De **Waarschuwingsdetails** blade ziet u wanneer de waarschuwing is opgetreden en de laatste is gedetecteerd. Een achtergrondproces waarmee elke twee uur wordt uitgevoerd, genereert en opnieuw evalueert de waarschuwing. In het volgende voorbeeld is de eerste waarschuwing opgetreden op 03/10 om 9:59 uur. De waarschuwing wordt nog steeds aanwezig op 03/12 om 10:00 wanneer de waarschuwing opnieuw is geëvalueerd. De blade bevat ook de tijd waarop die de Health-Service laatst een bepaald type ontvangen. 
 
 ![Azure AD Connect Health Waarschuwingsdetails](./media/how-to-connect-health-data-freshness/data-freshness-details.png)
 
De volgende tabel wordt servicetypen toegewezen aan de overeenkomende typen van de vereiste gegevens:

| Servicetype | Agent (naam van de Windows-Service) | Doel | Type van de gegevens die zijn gegenereerd  |
| --- | --- | --- | --- |  
| Azure AD Connect (Sync) | Azure AD Connect Health Sync Insights-service | Verzamelen van AAD Connect-specifieke informatie (connectors, synchronisatieregels, enz.) | AadSyncService-SynchronizationRules <br />  AadSyncService-Connectors <br /> - AadSyncService-GlobalConfigurations  <br />  AadSyncService-RunProfileResults <br /> AadSyncService-ServiceConfigurations <br /> AadSyncService-ServiceStatus   |
|  | Azure AD Connect Health Sync Monitoring-service | AAD Connect-specifieke prestatiemeteritems, ETW-traceringen, bestanden verzamelen | Prestatiemeteritem |
| AD DS | Azure AD Connect Health AD DS Insights-service | Synthetische uit te voeren, informatie over netwerktopologie, replicatie-metagegevens verzamelen |  - Adds-TopologyInfo-Json <br /> Algemene-TestData-Json (Hiermee maakt u de resultaten van de)   | 
|  | Azure AD Connect Health AD DS Monitoring-service | ADDS-specifieke prestatiemeteritems, ETW-traceringen, bestanden verzamelen | -Prestatiemeteritem  <br /> Algemene-TestData-Json (uploadt de resultaten van de)  |
| AD FS | Azure AD Connect Health AD FS Diagnostics-service | Synthetische uit te voeren | TestResult (Hiermee maakt u de resultaten van de) | 
| | Azure AD Connect Health AD FS Insights-service  | Verzamelen van metrische gegevens over AD FS-gebruik | Adfs-UsageMetrics |
| | Azure AD Connect Health AD FS Monitoring-service | AD FS-specifieke prestatiemeteritems, ETW-traceringen, bestanden verzamelen | TestResult (uploadt de resultaten van de) |

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing 

Hieronder volgt de stappen die nodig zijn om het probleem vast te stellen. De eerste is een set van basic controles die gemeenschappelijk voor alle typen van de Service zijn. De onderstaande tabel met een lijst met specifieke stappen voor elk Type Service en het gegevenstype. 

> [!IMPORTANT] 
> Deze waarschuwing volgt Connect Health [bewaarbeleid voor gegevens](reference-connect-health-user-privacy.md#data-retention-policy)

* Zorg ervoor dat de meest recente versies van de agents zijn geïnstalleerd. Weergave [versiegeschiedenis](reference-connect-health-version-history.md). 
* Zorg ervoor dat de services van Azure AD Connect Health-Agents zijn **met** op de machine. Connect Health voor AD FS moet bijvoorbeeld drie services hebben.
  ![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install5.png)

* Zorg ervoor dat u via en voldoen aan de [gedeelte vereisten](how-to-connect-health-agent-install.md#requirements).
* Gebruik [test connectiviteitshulpprogramma](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) voor het detecteren van problemen met de netwerkverbinding.
* Als u een HTTP-Proxy hebt, volgt u deze [configuratiestappen](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 


## <a name="next-steps"></a>Volgende stappen
Als een van de bovenstaande stappen het probleem hebt geïdentificeerd, oplossen en wachten op de waarschuwing op te lossen. De waarschuwing achtergrondproces uitgevoerd elke 2 uur, zodat het duurt maximaal twee uur de waarschuwing te verhelpen. 

* [Bewaarbeleid voor gegevens met Azure AD Connect Health](reference-connect-health-user-privacy.md#data-retention-policy)
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
