---
title: Problemen oplossen van Azure Active Directory-activiteit inhoudspakket fouten-Logboeken | Microsoft Docs
description: Biedt u een lijst met foutberichten van het inhoudspakket van Azure Active Directory-activiteit en werk deze kunt oplossen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c880e9eb6d48bd1e38075fbd867d3906ec67b547
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Het oplossen van Azure Active Directory-activiteit registreert inhoudspakket fouten 


Als u werkt met het Power BI-inhoudspakket voor Azure Active Directory-Preview, is het mogelijk dat u in de volgende fouten uitvoert: 

- [Vernieuwen is mislukt](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [Gegevensbronreferenties bijwerken is mislukt](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Importeren van gegevens duurt te lang is](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
In dit onderwerp vindt u informatie over de mogelijke oorzaken en hoe deze fouten te herstellen.
 
## <a name="refresh-failed"></a>Vernieuwen is mislukt 
 
**Hoe deze fout wordt opgehaald**: e-mailbericht met Power BI- of mislukte status in de geschiedenis vernieuwen. 


| Oorzaak | Op te lossen |
| ---   | ---        |
| Vernieuwen van de fout fouten kunnen worden veroorzaakt wanneer de referenties van de gebruikers die verbinding maken met het inhoudspakket is opnieuw ingesteld, maar niet bijgewerkt in de verbindingsinstellingen van de van het inhoudspakket. | Zoek de gegevensset die overeenkomt met het Azure Active Directory-activiteit logboeken dashboard (Azure Active Directory-activiteit Logboeken), kies schema vernieuwen en voer uw Azure AD-referenties in Power BI. |
| Vernieuwen kan mislukken vanwege gegevensproblemen met de in het onderliggende inhoudspakket. | Een ondersteuningsticket-bestand. Zie voor meer informatie [ondersteuning voor Azure Active Directory krijgen](active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Gegevensbronreferenties bijwerken is mislukt 
 
**Hoe deze fout wordt opgehaald**: In Power BI, wanneer u een verbinding met het inhoudspakket van Azure Active Directory-activiteit Logboeken (preview). 

| Oorzaak | Op te lossen |
| ---   | ---        |
| Gebruiker die de verbinding is een globale beheerder noch een lezer beveiliging of een beheerder beveiliging. | Gebruik een account dat een globale beheerder of een lezer beveiliging of een beheerder van de beveiliging voor toegang tot de inhoudspakketten. |
| Uw tenant is niet een Premium-tenant of beschikt niet over ten minste één gebruiker met Premium-licentie bestand. | Een ondersteuningsticket-bestand. Zie voor meer informatie [ondersteuning voor Azure Active Directory krijgen](active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Importeren van gegevens duurt te lang is 
 
**Hoe deze fout wordt opgehaald**: In Power BI zodra u verbinding hebt gemaakt met uw inhoudspakket het importproces gestart uw dashboard voorbereiden voor Azure Active Directory-logboek. Het bericht: '*importeren van gegevens...* '  

| Oorzaak | Op te lossen |
| ---   | ---        |
| Afhankelijk van de grootte van uw tenant, kan deze stap enkele minuten tot duren 30 minuten. | NET worden geduld. Als het bericht wordt niet gewijzigd in uw dashboard binnen een uur wordt weergegeven, neem een ondersteuningsticket-bestand. Zie voor meer informatie [ondersteuning voor Azure Active Directory krijgen](active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Volgende stappen

Klik op om de Power BI-inhoudspakket voor Azure Active Directory-Preview [hier](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).


