---
title: Een Azure Log Analytics-werkruimte verwijderen | Microsoft Docs
description: Meer informatie over het verwijderen van uw Log Analytics-werkruimte als u dat hebt gemaakt in een persoonlijke abonnement of het werkruimtemodel van uw herstructureren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.openlocfilehash: a6542838acba3143123dc90d96746179a2b4469b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55989131"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Een Azure Log Analytics-werkruimte met de Azure-portal verwijderen
Dit artikel leest hoe u de Azure portal gebruiken voor het verwijderen van een Log Analytics-werkruimte die u mogelijk niet meer nodig hebt. 

## <a name="to-delete-a-workspace"></a>Een werkruimte verwijderen 
Wanneer u een Log Analytics-werkruimte verwijdert, wordt alle gegevens die betrekking hebben op uw werkruimte binnen 30 dagen verwijderd uit de service.  Wilt u Wees voorzichtig wanneer u een werkruimte verwijderen omdat er zijn mogelijk belangrijke gegevens en configuratie die mogelijk negatieve invloed hebben op uw servicebewerkingen. Houd rekening met de andere Azure-services en bronnen die de gegevens opslaan in Log Analytics, zoals:

* Application Insights
* Azure Security Center
* Azure Automation
* Agents die worden uitgevoerd op Windows en Linux-machines
* Agents die worden uitgevoerd op Windows en Linux computers in uw omgeving
* System Center Operations Manager
* Beheeroplossingen 

Alle agents en System Center Operations Manager-beheergroepen die is geconfigureerd om te rapporteren aan de werkruimte blijven in een zwevende status.  Inventariseren welke agents, oplossingen, en andere Azure-services kunnen worden geïntegreerd in de werkruimte voordat u doorgaat.   
 
Als u een beheerder bent en er aan de werkruimte meerdere gebruikers zijn gekoppeld, wordt de koppeling tussen gebruikers en de werkruimte verbroken. Als de gebruikers zijn gekoppeld aan andere werkruimten, kunnen ze Log Analytics blijven gebruiken met die andere werkruimten. Echter, als ze niet gekoppeld aan andere werkruimten zijn ze moeten een werkruimte maken voor het gebruik van Log Analytics. 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com). 
2. Klik in Azure Portal op **Meer services** in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics-werkruimten**.
3. In het deelvenster voor abonnementen van Log Analytics een werkruimte selecteren en klik vervolgens op **verwijderen** vanaf de bovenkant van het middelste deelvenster.<br><br> ![De optie verwijderen uit het deelvenster met eigenschappen werkruimte](media/delete-workspace/log-analytics-delete-workspace.png)<br>  
4. Wanneer de bevestiging berichtvenster wordt weergegeven waarin wordt gevraagd te bevestigen van de werkruimte, klikt u op **Ja**.<br><br> ![Bevestig de verwijdering van werkruimte](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

