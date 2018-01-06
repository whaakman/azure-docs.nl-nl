---
title: Beheren van gegevens in Azure Automation | Microsoft Docs
description: In dit artikel bevat meerdere onderwerpen voor het beheren van een Azure Automation-omgeving.  Momenteel bevat bewaren van gegevens en back-ups van Azure Automation-noodherstel in Azure Automation.
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: 2896f129-82e3-43ce-b9ee-a3860be0423a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/201
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 2afcf918ffa104bd0e13048c152e04992f55ffe1
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="managing-azure-automation-data"></a>Azure Automation-gegevens beheren
In dit artikel bevat meerdere onderwerpen voor het beheren van een Azure Automation-omgeving.

## <a name="data-retention"></a>Bewaartijd van gegevens
Wanneer u een resource in Azure Automation verwijdert, wordt wel worden bewaard gedurende 90 dagen voor controledoeleinden voordat het wordt permanent verwijderd.  U kan niet zien of de resource gebruiken tijdens deze periode.  Dit beleid geldt ook voor resources die horen bij een automation-account wordt verwijderd.

Azure Automation wordt automatisch verwijderd en taken die ouder zijn dan 90 dagen permanent verwijderd.

De volgende tabel geeft een overzicht van het bewaarbeleid voor verschillende bronnen.

| Gegevens | Beleid |
|:--- |:--- |
| Accounts |Nadat het account is verwijderd door een gebruiker de 90 dagen permanent verwijderd. |
| Assets |Negentig dagen na de asset door een gebruiker wordt verwijderd of 90 dagen na de rekening met dat de asset is verwijderd door een gebruiker permanent verwijderd. |
| Modules |Negentig dagen na de module wordt verwijderd door een gebruiker of 90 dagen na de rekening met dat de module wordt verwijderd door een gebruiker permanent verwijderd. |
| Runbooks |Negentig dagen na de resource is verwijderd door een gebruiker of 90 dagen na de rekening met dat de resource is verwijderd door een gebruiker permanent verwijderd. |
| Taken |Verwijderde en permanent verwijderd 90 dagen na de laatste wordt gewijzigd. Dit kan zijn nadat de taak is voltooid, is gestopt of is onderbroken. |
| Knooppunt configuraties/MOF-bestanden |Oude knooppuntconfiguratie wordt definitief verwijderd negentig dagen na de configuratie van een nieuw knooppunt wordt gegenereerd. |
| DSC-knooppunten |Negentig dagen na het knooppunt niet is geregistreerd van Automation-Account Azure-portal met permanent te verwijderen of de [Unregister AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) cmdlet in Windows PowerShell. Knooppunten zijn 90 dagen na de rekening met dat het knooppunt is verwijderd door een gebruiker ook permanent verwijderd. |
| Knooppunt-rapporten |Nadat een nieuw rapport is gegenereerd voor dat knooppunt de 90 dagen definitief verwijderd |

Het bewaarbeleid geldt voor alle gebruikers en kan momenteel niet worden aangepast.

Echter, als u nodig hebt om gegevens te behouden voor een langere periode, kunt u doorsturen runbook de taaklogboeken voor logboekanalyse.  Bekijk voor meer informatie [doorsturen van gegevens van Azure Automation-taak met OMS Log Analytics](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Back-ups maken met Azure Automation
Wanneer u een automation-account in Microsoft Azure verwijdert, worden alle objecten in het account verwijderd waaronder runbooks, modules, configuraties, instellingen, taken en activa. De objecten kunnen niet worden hersteld nadat het account is verwijderd.  De volgende informatie kunt u back-up van de inhoud van uw automation-account voordat u het verwijdert. 

### <a name="runbooks"></a>Runbooks
U kunt uw runbooks exporteren naar scriptbestanden met de Azure portal of de [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet in Windows PowerShell.  Deze scriptbestanden kunnen worden geïmporteerd in een andere automation-account, zoals beschreven in [een Runbook maken of importeren](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integratiemodules
U kunt integratiemodules niet exporteren uit Azure Automation.  U moet ervoor zorgen dat ze beschikbaar is buiten het automation-account zijn.

### <a name="assets"></a>Assets
Kan niet worden geëxporteerd [activa](https://msdn.microsoft.com/library/dn939988.aspx) van Azure Automation.  Met de Azure portal, moet u de details van variabelen, referenties, certificaten, verbindingen en planningen noteren.  Vervolgens moet u handmatig maken elementen die worden gebruikt door runbooks die u in een andere automation importeren.

U kunt [Azure-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) details van niet-versleutelde activa vallen of opslaan voor toekomstig gebruik ophalen of gelijkwaardige activa in een andere automation-account maken.

U kunt de waarde voor gecodeerde variabelen of het wachtwoordveld van referenties met behulp van cmdlets niet ophalen.  Als u deze waarden niet weet, moet u ze vanuit een runbook met ophalen kunt de [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) en [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) activiteiten.

U kunt certificaten niet exporteren uit Azure Automation.  U moet ervoor zorgen dat alle certificaten buiten Azure beschikbaar zijn.

### <a name="dsc-configurations"></a>DSC-configuraties
U kunt uw configuraties exporteren naar scriptbestanden met de Azure portal of de [Export AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) cmdlet in Windows PowerShell. Deze configuraties kunnen worden geïmporteerd en gebruikt in een andere automation-account.

## <a name="geo-replication-in-azure-automation"></a>Geo-replicatie in Azure Automation
Geo-replicatie, standaard in Azure Automation-accounts, back-ups van gegevens naar een ander geografische regio voor de redundantie. U kunt een primaire regio kiezen bij het instellen van uw account en vervolgens een secundaire regio is toegewezen aan deze automatisch. De secundaire gegevens gekopieerd van de primaire regio wordt voortdurend bijgewerkt in geval van gegevensverlies.  

De volgende tabel bevat de beschikbare primaire en secundaire regio-koppelingen.

| Primair | Secundair |
| --- | --- |
| Zuid-centraal VS |Noord-centraal VS |
| Verenigde Staten (oost 2) |VS - midden |
| West-Europa |Noord-Europa |
| Zuidoost-Azië |Oost-Azië |
| Japan - oost |Japan - west |

In het onwaarschijnlijke geval dat een primaire regiogegevens niet verloren zijn, probeert Microsoft om deze te herstellen. Als de primaire gegevens kunnen niet worden hersteld, klikt u vervolgens geo-failover wordt uitgevoerd en de betrokken klanten ontvangt een melding over deze via hun abonnement.

