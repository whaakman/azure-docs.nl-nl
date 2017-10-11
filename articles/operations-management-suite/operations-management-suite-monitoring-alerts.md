---
title: Waarschuwing management in Microsoft monitoring producten | Microsoft Docs
description: Een waarschuwing geeft een probleem aan dat de aandacht van een beheerder vereist.  Dit artikel worden de verschillen in hoe waarschuwingen worden gemaakt en beheerd in System Center Operations Manager (SCOM) en Log Analytics beschreven en vindt u aanbevolen procedures in het gebruik van de twee producten voor een strategie voor een hybride beheer van waarschuwingen.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 7df2fd7ef838465a60e3b0ce2f889127b7487684
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-alerts-with-microsoft-monitoring"></a>Het beheren van waarschuwingen met het bewaken van Microsoft
Een waarschuwing geeft een probleem aan dat de aandacht van een beheerder vereist.  Er toch duidelijke verschillen tussen System Center Operations Manager (SCOM) en Log Analytics in Operations Management Suite (OMS) in termen van hoe waarschuwingen worden gemaakt, hoe ze worden beheerd en geanalyseerd, en hoe u wordt gewaarschuwd dat een kritiek probleem is gedetecteerd.

## <a name="alerts-in-operations-manager"></a>Waarschuwingen in Operations Manager
Waarschuwingen in SCOM worden gegenereerd door afzonderlijke regels of monitors om aan te geven van een specifiek probleem.  Een monitor kan een waarschuwing genereren wanneer deze een foutstatus krijgt, terwijl een regel genereert een waarschuwing om aan te geven van een kritiek probleem dat is niet direct gerelateerd aan de status van een beheerd object.  Management packs bevatten allerlei werkstromen die waarschuwingen genereren voor de toepassing of service die ze beheren.  Onderdeel van het proces van het configureren van een nieuw management pack is afstemmen om ervoor te zorgen dat u geen overmatige waarschuwingen voor problemen die u niet van essentieel belang.

![Weergave van de SCOM-waarschuwing](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM biedt volledige waarschuwingenbeheer waarschuwingen met een status die door beheerders kan worden gewijzigd als ze werken als het probleem wilt oplossen.  Als het probleem is opgelost, stelt de beheerder de waarschuwing gesloten op dat moment wordt deze niet meer weergegeven in weergaven worden actieve waarschuwingen weergegeven.  Waarschuwingen die zijn gegenereerd uit monitors kunnen automatisch worden opgelost wanneer de monitor een goede status retourneren.

![Waarschuwingsstatus](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Waarschuwingen in Log Analytics
Een waarschuwing in Log Analytics wordt gemaakt van een logboek-query die automatisch wordt uitgevoerd met regelmatige tussenpozen.  U kunt geen waarschuwingsregel maken van een logboek-query.  Als de query resultaten die overeenkomen met de criteria die u opgeeft retourneert, wordt een waarschuwing gemaakt.  Dit kan een specifieke query die u een waarschuwing maakt als er een bepaalde gebeurtenis wordt gedetecteerd, of u kunt een meer algemene query zoekt naar een foutgebeurtenis met betrekking tot een bepaalde toepassing.

Log Analytics waarschuwingen zijn geschreven in de OMS-opslagplaats als een gebeurtenis en kunnen worden opgehaald met een logboek-query.  Ze hoeft geen status zoals SCOM gebeurtenissen zodat u aangeven kunt wanneer het probleem is opgelost.

![OMS-waarschuwing](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Wanneer SCOM wordt gebruikt als gegevensbron voor logboekanalyse, worden SCOM waarschuwingen in de OMS-opslagplaats geschreven als ze zijn gemaakt en gewijzigd.  

![SCOM-waarschuwing](media/operations-management-suite-monitoring-alerts/scom-alert.png)

De [waarschuwing beheeroplossing](http://technet.microsoft.com/library/mt484092.aspx) biedt een overzicht van actieve waarschuwingen en enkele algemene query's voor het ophalen van verschillende sets van waarschuwingen.  Dit biedt u effectiever analyse van uw waarschuwingen dan een rapport in SCOM.  U kunt inzoomen op uit de samenvattingen naar gedetailleerde gegevens en maken van de ad-hocquery's voor het ophalen van verschillende sets van waarschuwingen.

![Oplossing voor beheer van waarschuwingen](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Meldingen
Meldingen in SCOM verzendt u een e-mail of tekst in reactie op waarschuwingen die aan bepaalde criteria voldoen.  U kunt verschillende meldingsabonnementen die andere mensen gewaarschuwd, afhankelijk van criteria zoals het object dat wordt bewaakt, de ernst van de waarschuwing, het soort probleem dat gedetecteerd hebben of van de dag maken.

Enkele abonnementen kunnen worden gebruikt voor het implementeren van een strategie voor een volledige aanmelding voor een groot aantal management packs.

![SCOM-waarschuwingen](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Log Analytics kunt u een melding via e-mail dat er een waarschuwing is gemaakt door in te stellen van een e-meldingsactie op elke [waarschuwingsregel](http://technet.microsoft.com/library/mt614775.aspx).  Er geen de mogelijkheid van SCOM de abonneren op meerdere waarschuwingen met een enkele regel.  U moet ook uw eigen regels voor waarschuwingen maken omdat OMS geen vooraf geconfigureerd biedt.

![Waarschuwingen in Log Analytics](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

U kunt geen SCOM-waarschuwingen in Log Analytics volledig echter beheren omdat u alleen in de Operations-Console wijzigen kunt.  Log Analytics is nuttig als onderdeel van een waarschuwing management verwerken echter voor analyseprogramma's bieden die alleen SCOM beschikt niet over.

## <a name="alert-remediation"></a>Waarschuwing herstel
[Herstel](http://technet.microsoft.com/library/mt614775.aspx) verwijst naar een poging tot automatisch los het probleem dat wordt geïdentificeerd door een waarschuwing.

SCOM kunt u diagnoses en herstelbewerkingen uitvoeren als reactie op een monitor een slechte status invoeren.  Dit gebeurt gelijktijdige naar de monitor maken van de waarschuwing.  Diagnoses en herstelbewerkingen worden doorgaans geïmplementeerd als een script dat wordt uitgevoerd op de agent.  Een diagnose probeert meer informatie over het gedetecteerde probleem verzamelen tijdens een herstelpoging gedaan om het probleem te verhelpen.

Log Analytics kunt u starten een [Azure Automation-runbook](https://azure.microsoft.com/documentation/services/automation/) of een webhook aanroepen in reactie op een waarschuwing voor logboekanalyse.  Runbooks kunnen complexe logica geïmplementeerd in PowerShell bevatten.  Het script wordt uitgevoerd in Azure en toegang tot alle Azure-resources of externe bronnen beschikbaar vanuit de cloud.  Azure Automation heeft de mogelijkheid voor het uitvoeren van runbooks op een server in uw lokale datacentrum, maar deze functie is momenteel niet beschikbaar wanneer het runbook wordt gestart als reactie op waarschuwingen logboekanalyse.

Zowel herstelbewerkingen in SCOM en runbooks in OMS kan PowerShell-scripts bevatten, maar er zijn herstelbewerkingen moeilijker te maken en beheren omdat ze moeten zijn opgenomen in een management pack.  Runbooks worden opgeslagen in Azure Automation dat voorziet in functies voor schrijven, testen en het beheer van runbooks.

Als u SCOM als gegevensbron voor Log Analytics gebruiken, kunt u een waarschuwing voor logboekanalyse met een logboek-query voor het ophalen van de SCOM-waarschuwingen die zijn opgeslagen in de OMS-opslagplaats maken.  Deze manier kunt u een Azure Automation-runbook uitvoeren in reactie op een SCOM-waarschuwing.  Omdat het runbook wordt uitgevoerd in Azure, zou dit natuurlijk niet een geschikte strategie voor herstelbewerkingen van lokale problemen zijn.

## <a name="next-steps"></a>Volgende stappen
* Lees meer over [waarschuwingen in System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).

