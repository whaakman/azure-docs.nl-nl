---
title: Een Azure-logboekanalyse-werkruimte verwijderen | Microsoft Docs
description: Informatie over het verwijderen van de werkruimte voor logboekanalyse als u deze in een persoonlijke abonnement gemaakt of het werkruimtemodel van uw herstructureren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 54f2af60751ed0d9c64e71efad6fa9aa3ef06589
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129112"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Verwijderen van een Azure-logboekanalyse-werkruimte met de Azure-portal
In dit artikel laat zien hoe de Azure portal gebruiken om te verwijderen van een werkruimte voor logboekanalyse die u mogelijk niet meer nodig hebt. 

## <a name="to-delete-a-workspace"></a>Een werkruimte verwijderen 
Wanneer u een werkruimte voor logboekanalyse verwijdert, wordt alle gegevens met betrekking tot uw werkruimte verwijderd van de service binnen 30 dagen.  Wilt u voorzichtig wanneer u een werkruimte verwijderen omdat er mogelijk belangrijke gegevens en configuratie die mogelijk negatieve invloed hebben op uw servicebewerkingen. Houd rekening met het Azure-services en andere bronnen die de gegevens opslaan in Log Analytics, zoals:

* Application Insights
* Azure Security Center
* Azure Automation
* Agents die worden uitgevoerd op Windows- en Linux virtuele machines
* Agents die worden uitgevoerd op Windows- en Linux computers in uw omgeving
* System Center Operations Manager
* Beheeroplossingen 

De agents en System Center Operations Manager-beheergroepen zijn geconfigureerd voor het rapport naar de werkruimte blijven zwevende status.  Welke agents, oplossingen, inventariseren en andere Azure-services zijn geïntegreerd met de werkruimte voordat u doorgaat.   
 
Als u een beheerder bent en er aan de werkruimte meerdere gebruikers zijn gekoppeld, wordt de koppeling tussen gebruikers en de werkruimte verbroken. Als de gebruikers zijn gekoppeld aan andere werkruimten, kunnen ze Log Analytics blijven gebruiken met die andere werkruimten. Als ze niet gekoppeld aan andere werkruimtes zijn vervolgens moeten zij om een werkruimte voor logboekanalyse gebruik te maken. 

1. Meld u aan bij de [Azure Portal](http://portal.azure.com). 
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. Selecteer een werkruimte in het deelvenster abonnementen van logboekanalyse en klik vervolgens op **verwijderen** vanaf de bovenkant van het middelste deelvenster.<br><br> ![Optie verwijderen uit het deelvenster met eigenschappen](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Wanneer het bevestigingsvenster bericht wordt weergegeven waarin u de verwijdering van de werkruimte bevestigen, klikt u op **Ja**.<br><br> ![Verwijderen van de werkruimte bevestigen](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

