---
title: Waarschuwingsexemplaren beheren
description: Waarschuwing-exemplaren beheren in Azure
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: cb93f38c05156d7ab5acb89ffff810949583e507
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089400"
---
# <a name="manage-alert-instances"></a>Waarschuwingsexemplaren beheren
Met de [ervaring voor waarschuwingen van geïntegreerde](https://aka.ms/azure-alerts-overview) in Azure Monitor kunt u nu alle de verschillende typen waarschuwingen zien in Azure, meerdere abonnementen, in één enkel glazen spanning. Dit artikel begeleidt u hoe u de waarschuwing-instanties kunt bekijken en hoe u uitgebreide op de portal voor specifieke waarschuwingen exemplaren vinden voor het oplossen van problemen.

1. Er zijn drie manieren op de pagina met waarschuwingen

   + In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie Monitor - **waarschuwingen**.  
     ![Controle](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + U kunt navigeren naar de waarschuwingen van binnen de context van een specifieke **resource**. Wanneer een resource is geopend, gaat u door de inhoudsopgave aan de sectie bewaking en kies **waarschuwingen**, met de landingspagina vooraf voor waarschuwingen voor die specifieke bron wordt gefilterd.
   
     ![Bewaking](media/alerts-managing-alert-instances/alert-resource.JPG)
    
   + U kunt navigeren naar de waarschuwingen van binnen de context van een specifieke **resourcegroep**. Wanneer een resourcegroep is geopend, gaat u door de inhoudsopgave aan de sectie bewaking en kies **waarschuwingen**, met de landingspagina vooraf voor waarschuwingen over die specifieke resourcegroep wordt gefilterd.    
   
     ![Bewaking](media/alerts-managing-alert-instances/alert-rg.JPG)

1. U komt terecht op de **waarschuwingsoverzicht** pagina, waardoor u een overzicht van alle exemplaren van uw waarschuwingen binnen Azure. U kunt de weergave Samenvatting wijzigen door het selecteren van **meerdere abonnementen** (maximaal 5) of door te filteren op **resourcegroepen**, specifieke **resources**, of **tijd bereiken**. Klik op het totaal aantal waarschuwingen of een van de ernst stroken om naar de lijstweergave voor uw waarschuwingen te gaan.     
   ![Samenvatting van waarschuwingen](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
1. U komt terecht op de **alle waarschuwingen** pagina, u alle exemplaren van de waarschuwing in Azure die worden vermeld ziet uit. Als u eerder naar de portal van een waarschuwingsmelding, kunt u de filters die beschikbaar zijn om te beperken op dat specifieke waarschuwingsexemplaar. (**Opmerking**: als u op de pagina door te klikken op een van de ernst-banden, de lijst is vooraf gefilterde voor die incidenten met ernst is wanneer u komt terecht). Naast de filters die beschikbaar zijn op de vorige pagina, kunt u nu ook filteren op basis van de monitor-service (bijvoorbeeld Platform voor metrische gegevens), bewakingsvoorwaarde (geactiveerd of opgelost), ernst, waarschuwingsstatus (nieuwe/bevestigd/gesloten) of de smart-ID.

   ![Alle waarschuwingen](media/alerts-managing-alert-instances/all-alerts.jpg)

   > [!NOTE]
   >  Als u op de pagina door te klikken op een van de ernst-banden, wordt de lijst vooraf gefilterde voor die incidenten met ernst niet wanneer u komt op deze pagina terecht.
 
1. Op een waarschuwing geval opent u de **Waarschuwingsdetails** pagina, zodat u kunt uitgebreide informatie over dat specifieke waarschuwingsexemplaar.   
   ![Waarschuwingsdetails](media/alerts-managing-alert-instances/alert-details.jpg)  

