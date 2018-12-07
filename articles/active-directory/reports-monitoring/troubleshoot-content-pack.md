---
title: Het oplossen van Azure Active Directory-activiteitenlogboek registreert inhoudspakketfouten | Microsoft Docs
description: Biedt u een lijst met foutberichten van het inhoudspakket van Azure Active Directory-activiteit en werk deze kunt oplossen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 65747da92a3cad770cd9d474d27645782f7306b9
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998740"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Het oplossen van Azure Active Directory-activiteitenlogboek registreert inhoudspakketfouten 

|  |
|--|
|Op dit moment maakt het inhoudspakket voor Microsoft Azure AD Power BI gebruik van de Azure AD Graph-API's om gegevens op te halen uit uw Azure AD-tenant. Hierdoor ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met de [Microsoft Graph-API's voor rapportage](concept-reporting-api.md). |
|  |

Als u werkt met het Power BI-inhoudspakket voor Azure Active Directory (Azure AD), is het mogelijk dat u in de volgende fouten uitvoert: 

- [Vernieuwen is mislukt](troubleshoot-content-pack.md#refresh-failed) 
- [Gegevensbronreferenties bijwerken is mislukt](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Het importeren van gegevens is te lang duurt](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 

In dit artikel vindt u informatie over de mogelijke oorzaken en over het oplossen van deze fouten.
 
## <a name="refresh-failed"></a>Vernieuwen is mislukt 
 
**Hoe deze fout is opgehaald**: e-mailbericht van Power BI of mislukte status in de geschiedenis vernieuwen. 


| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Fout bij fouten kunnen worden veroorzaakt wanneer de referenties van de gebruikers die verbinding maken met het inhoudspakket is opnieuw ingesteld, maar niet bijgewerkt in de instellingen van het inhoudspakket vernieuwen. | In Power BI, zoek de gegevensset die overeenkomt met het dashboard activiteitenlogboeken van Azure AD (**Azure Active Directory-activiteitenlogboek registreert**), kies planning vernieuwen en voer vervolgens uw Azure AD-referenties. |
| Vernieuwen kan mislukken vanwege problemen met de gegevens in het onderliggende inhoudspakket. | [Een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Gegevensbronreferenties bijwerken is mislukt 
 
**Hoe deze fout is opgehaald**: inhoudspakket In Power BI, wanneer u verbinding met de Azure AD-activiteit maakt-Logboeken. 

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Gebruiker die de verbinding is niet een globale beheerder of beveiligingslezer of een beveiligingsbeheerder. | Gebruik een account dat een globale beheerder of beveiligingslezer of een beveiligingsbeheerder voor toegang tot de inhoudspakketten. |
| Uw tenant is niet een Premium-tenant of beschikt niet over ten minste één gebruiker met Premium-licentie bestand. | [Een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>Het importeren van gegevens is te langzaam verlopen 
 
**Hoe deze fout is opgehaald**: In Power BI, nadat u verbinding maken met uw inhoudspakket het importeren van gegevens wordt gestart op uw dashboard voorbereiden voor Azure AD-activiteit Logboeken. U ziet het bericht: **importeren van gegevens...**  zonder eventuele verdere wordt uitgevoerd.  

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Afhankelijk van de grootte van uw tenant, kan deze stap tussen een paar minuten duren tot 30 minuten. | Als het bericht wordt niet gewijzigd in het dashboard worden weergegeven binnen een uur [een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Volgende stappen

* [Installeer Power BI contect pack voor Azure AD-rapporten](quickstart-install-power-bi-content-pack.md).
* [Gebruik Power BI content pack voor Azure AD-rapporten om uw gegevens te visualiseren](howto-power-bi-content-pack.md)
* [Ondersteuning voor Azure Active Directory verkrijgen](../fundamentals/active-directory-troubleshooting-support-howto.md)
