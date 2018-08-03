---
title: Een Azure Automation-account loskoppelen van Log Analytics
description: Dit artikel bevat een overzicht van hoe u uw Azure Automation-account loskoppelen van Log Analytics-werkruimte.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1328ce8c306188c32bce5385f58f118a63c08deb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426530"
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Hoe u uw Automation-account loskoppelen van Log Analytics-werkruimte

Azure Automation kan worden geïntegreerd met Log Analytics kunt u niet alleen ondersteuning voor bewaking van de runbooktaken voor al uw Automation-accounts, maar is ook vereist wanneer u de volgende oplossingen die afhankelijk van Log Analytics zijn importeren:

* [Updatebeheer](../operations-management-suite/oms-solution-update-management.md)
* [Tracering wijzigen](../log-analytics/log-analytics-change-tracking.md)
* [VM's starten/stoppen buiten kantooruren](automation-solution-vm-management.md)

Als u besluit dat u niet meer wilt integreren van uw Automation-account met Log Analytics, kunt u uw account rechtstreeks vanuit de Azure portal kunt ontkoppelen.  Voordat u doorgaat, moet u eerst te verwijderen van de oplossingen die eerder vermeld, anders wordt dit proces zal worden voorkomen dat u doorgaat. Lees het artikel voor de desbetreffende oplossing die u hebt geïmporteerd voor meer informatie over de stappen die nodig zijn om deze te verwijderen.

Nadat u deze oplossingen hebt verwijderd, kunt u de volgende stappen uit als u wilt loskoppelen van uw Automation-account kunt uitvoeren.

> [!NOTE]
> Sommige oplossingen met inbegrip van eerdere versies van de Azure SQL-oplossing voor controle mogelijk gemaakt automatiseringsactiva en moet mogelijk ook worden verwijderd voordat het ontkoppelen van de werkruimte.

## <a name="unlink-workspace"></a>Werkruimte ontkoppelen

1. Open uw Automation-account vanuit Azure portal, en op de Automation-account selecteren pagina **gekoppelde werkruimte** onder de sectie **gerelateerde Resources** aan de linkerkant.

1. Klik op de pagina van de werkruimte ontkoppelen **werkruimte ontkoppelen**.

   ![Pagina voor werkruimte ontkoppelen](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).

   U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.

1. Terwijl Azure Automation probeert te ontkoppelen van het account uw Log Analytics-werkruimte, u kunt de voortgang volgen onder **meldingen** in het menu.

Als u de oplossing Update Management gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Plant u update - elk hebben namen die overeenkomen met de update-implementaties die u hebt gemaakt)

* Hybrid worker-groepen gemaakt voor de oplossing - elke naam op dezelfde manier naar machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Als u de VM's starten/stoppen buiten kantooruren oplossing gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Starten en stoppen van VM runbook schema 's
* VM-runbooks starten en stoppen
* Variabelen

## <a name="next-steps"></a>Volgende stappen

Als u wilt uw Automation-account om te integreren in Log Analytics configureren, Zie [taakstatus en taakstromen van Automation doorsturen naar Log Analytics](automation-manage-send-joblogs-log-analytics.md).