---
title: Azure AD Connect Health - Health-servicegegevens zijn niet tot de datum-waarschuwing | Microsoft Docs
description: Dit document beschrijft de oorzaak van de waarschuwing 'Health service-gegevens is niet up-to-date' en het oplossen van deze.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 430ea5f0a6f737d7632a4352c24d893368b80558
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315096"
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


## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
