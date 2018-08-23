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
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: eafbe25a5a0fa9182030304e9142a6013c9fb29b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054102"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Het oplossen van Azure Active Directory-activiteitenlogboek registreert inhoudspakketfouten 


Als u werkt met het Power BI-inhoudspakket voor Azure Active Directory-Preview, is het mogelijk dat u in de volgende fouten uitvoert: 

- [Vernieuwen is mislukt](troubleshoot-content-pack.md#refresh-failed) 
- [Gegevensbronreferenties bijwerken is mislukt](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Het importeren van gegevens is te lang duurt](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
In dit artikel vindt u informatie over de mogelijke oorzaken en over het oplossen van deze fouten.
 
## <a name="refresh-failed"></a>Vernieuwen is mislukt 
 
**Hoe deze fout is opgehaald**: e-mailbericht van Power BI of mislukte status in de geschiedenis vernieuwen. 


| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Fout bij fouten kunnen worden veroorzaakt wanneer de referenties van de gebruikers die verbinding maken met het inhoudspakket is opnieuw ingesteld, maar niet bijgewerkt in de instellingen van het inhoudspakket vernieuwen. | In Power BI, zoek de gegevensset die overeenkomt met het dashboard in Azure Active Directory-activiteit Logboeken (Azure Active Directory activiteitenlogboeken), kies planning vernieuwen en voer vervolgens uw Azure AD-referenties. |
| Vernieuwen kan mislukken vanwege problemen met de gegevens in het onderliggende inhoudspakket. | Een ondersteuningsticket. Zie voor meer informatie, [over het verkrijgen van ondersteuning voor Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Gegevensbronreferenties bijwerken is mislukt 
 
**Hoe deze fout is opgehaald**: In Power BI, wanneer u verbinding met het inhoudspakket van Azure Active Directory-activiteit Logboeken (preview maakt). 

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Gebruiker die de verbinding is niet een globale beheerder of beveiligingslezer of een beheerder beveiliging. | Gebruik een account dat een globale beheerder of beveiligingslezer of een beveiligingsbeheerder voor toegang tot de inhoudspakketten. |
| Uw tenant is niet een Premium-tenant of beschikt niet over ten minste één gebruiker met Premium-licentie bestand. | Een ondersteuningsticket. Zie voor meer informatie, [over het verkrijgen van ondersteuning voor Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Het importeren van gegevens is te lang duurt 
 
**Hoe deze fout is opgehaald**: In Power BI, nadat u verbinding hebt gemaakt met uw inhoudspakket het importeren van gegevens wordt uw dashboard voorbereiden voor Azure Active Directory-activiteitenlogboek. U ziet het bericht: "*importeren van gegevens...* "  

| Oorzaak | Voor het oplossen van |
| ---   | ---        |
| Afhankelijk van de grootte van uw tenant, kan deze stap duren vanaf een paar minuten tot 30 minuten. | Gedraagt zich nu even geduld. Als het bericht wordt niet gewijzigd in uw dashboard binnen een uur worden weergegeven, kunt u een ondersteuningsticket indienen. Zie voor meer informatie, [over het verkrijgen van ondersteuning voor Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Volgende stappen

Als u wilt de Power BI-inhoudspakket voor Azure Active Directory-Preview hebt geïnstalleerd, klikt u op [hier](https://powerbi.microsoft.com/blog/azure-active-directory-meets-power-bi/).


