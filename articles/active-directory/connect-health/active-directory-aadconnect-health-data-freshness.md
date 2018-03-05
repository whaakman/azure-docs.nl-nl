---
title: Azure AD Connect Health - Health-servicegegevens zijn niet tot datum waarschuwing | Microsoft Docs
description: Dit document beschrijft de oorzaak van de waarschuwing 'Health service-gegevens is niet up-to-date' en het oplossen van dit probleem.
services: active-directory
documentationcenter: 
author: zhiweiw
manager: maheshu
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 45e88320a64770239c8f322212e12ca7826cd0da
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Statusgegevens van de service is niet up-to-date waarschuwing

## <a name="overview"></a>Overzicht
<li>Azure AD Connect Health genereert gegevens nieuwe waarschuwing als er geen de gegevenspunten van de server gedurende 2 uur ontvangt. Titel van de waarschuwing is **Health service-gegevens is niet up-to-date**. </li>
<li>De **waarschuwing** status waarschuwing wordt geactiveerd als Connect Health niet gegevens gedeeltelijk elementen van de server verzonden voor twee uur ontvangt. Status van waarschuwing automatisch niet e-mailmeldingen naar de beheerder van de tenant. </li>
<li>De **fout** status waarschuwing wordt geactiveerd als Connect Health niet gegevenselementen verzonden vanaf de server voor twee uur ontvangt. Fout status waarschuwing krijgt e-mailmeldingen naar de beheerder van de tenant. </li>

>[!IMPORTANT] 
> Deze waarschuwing volgt Connect Health [bewaarbeleid voor gegevens](active-directory-aadconnect-health-gdpr.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing 
* Zorg ervoor dat u via en voldoen aan de [gedeelte vereisten](active-directory-aadconnect-health-agent-install.md#requirements).
* Gebruik [test connectiviteitshulpprogramma](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) voor het detecteren van problemen met de netwerkverbinding.


## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
