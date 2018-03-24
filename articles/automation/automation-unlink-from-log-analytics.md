---
title: Een Azure Automation-account loskoppelen van Log Analytics
description: Dit artikel bevat een overzicht van hoe u uw Azure Automation-account van een werkruimte voor logboekanalyse ontkoppelen.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 2e6f887a7684ae995e9b7da84dace523843ff555
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Het ontkoppelen van uw Automation-account van een werkruimte voor logboekanalyse

Azure Automation worden geïntegreerd met logboekanalyse te bieden niet alleen ondersteuning voor bewaking van uw runbooktaken voor alle Automation-accounts, maar is ook vereist als u de volgende oplossingen die afhankelijk van logboekanalyse zijn importeren:

* [Updatebeheer](../operations-management-suite/oms-solution-update-management.md)
* [Tracering wijzigen](../log-analytics/log-analytics-change-tracking.md)
* [Virtuele machines starten/stoppen buiten kantooruren](automation-solution-vm-management.md)
 
Als u dat u niet langer wilt integreren van uw Automation-account met Log Analytics besluit, kunt u uw account rechtstreeks vanuit de Azure portal kunt ontkoppelen.  Voordat u doorgaat, moet u eerst te verwijderen van de oplossingen die eerder vermeld, anders dit proces zal worden voorkomen dat u doorgaat.  Bekijk het onderwerp voor de specifieke oplossing die u hebt geïmporteerd voor informatie over de stappen die nodig zijn om deze te verwijderen.  

U kunt de volgende stappen voor het ontkoppelen van uw Automation-account uitvoeren na het verwijderen van deze oplossingen.

## <a name="unlink-workspace"></a>Ontkoppelen van de werkruimte

1. Open uw Automation-account in de Azure-portal en op de Automation-account selecteren pagina **ontkoppelen werkruimte** onder de sectie **verwante Resources** aan de linkerkant.<br><br> ![Werkruimte optie ontkoppelen](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Klik op de pagina van de werkruimte ontkoppelen **ontkoppelen werkruimte**.<br><br> ![Ontkoppelen van de pagina voor werkruimte](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.<br><br>
3. Terwijl Azure Automation probeert te ontkoppelen van het account werkruimte voor logboekanalyse, u kunt de voortgang volgen onder **meldingen** in het menu.

Als u de Update-beheeroplossing gebruikt, eventueel u mogelijk wilt verwijderen van de volgende items die niet langer nodig zijn na het verwijderen van de oplossing.

* Schema's worden bijgewerkt.  Elk moeten namen die overeenkomen met de update-implementaties die u hebt gemaakt)

* Hybrid worker-groepen is gemaakt voor de oplossing.  Elke naam op dezelfde manier naar machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Als u de starten/stoppen virtuele machines tijdens rustige uren oplossing gebruikt, eventueel u mogelijk wilt verwijderen van de volgende items die niet langer nodig zijn na het verwijderen van de oplossing.

* Starten en stoppen van runbook-planningen VM 
* VM-runbooks starten en stoppen
* Variabelen   

## <a name="next-steps"></a>Volgende stappen

Als u wilt uw Automation-account om te integreren met logboekanalyse OMS configureren, Zie [doorsturen taakstatus en taak stromen van Automation voor logboekanalyse (OMS)](automation-manage-send-joblogs-log-analytics.md). 