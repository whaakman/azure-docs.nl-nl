---
title: Een Azure-logboekanalyse-werkruimte verwijderen | Microsoft Docs
description: Informatie over het verwijderen van de werkruimte voor logboekanalyse als u deze in een persoonlijke abonnement gemaakt of het werkruimtemodel van uw herstructureren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 3d99f9869dfdfbe18f82e873bd367cc85f9414f1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Verwijderen van een Azure-logboekanalyse-werkruimte met de Azure-portal
Dit onderwerp leest hoe de Azure portal gebruiken om te verwijderen van een Log Analytics workpace die u mogelijk niet meer nodig hebt. 

## <a name="to-delete-a-workspace"></a>Een werkruimte verwijderen 
Wanneer u een werkruimte voor logboekanalyse verwijdert, wordt alle gegevens met betrekking tot uw werkruimte verwijderd van de service binnen 30 dagen.  Wilt u voorzichtig wanneer u een werkruimte verwijderen omdat er mogelijk belangrijke gegevens en configuratie die mogelijk negatieve invloed hebben op uw servicebewerkingen.  
 
Als u een beheerder bent en er aan de werkruimte meerdere gebruikers zijn gekoppeld, wordt de koppeling tussen gebruikers en de werkruimte verbroken. Als de gebruikers gekoppeld aan andere werkruimtes zijn, kunnen ze vervolgens blijven Log Analytics gebruiken met die andere werkruimtes. Als ze niet gekoppeld aan andere werkruimtes zijn vervolgens moeten zij om een werkruimte voor logboekanalyse gebruik te maken. 

1. Meld u aan bij de [Azure Portal](http://portal.azure.com). 
2. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
3. Selecteer een werkruimte in het deelvenster abonnementen van logboekanalyse en klik vervolgens op **verwijderen** vanaf de bovenkant van het middelste deelvenster.<br><br> ![Optie verwijderen uit het deelvenster met eigenschappen](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Wanneer het bevestigingsvenster bericht wordt weergegeven waarin u de verwijdering van de werkruimte bevestigen, klikt u op **Ja**.<br><br> ![Verwijderen van de werkruimte bevestigen](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

