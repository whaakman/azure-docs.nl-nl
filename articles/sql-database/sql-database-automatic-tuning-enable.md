---
title: Schakel automatische afstemming voor Azure SQL Database | Microsoft Docs
description: U kunt inschakelen automatische afstemming op uw Azure SQL Database eenvoudig.
services: sql-database
documentationcenter: 
author: veljko-msft
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/19/2016
ms.author: veljko-msft
ms.openlocfilehash: 82db5996c1ba1f224593e4eaa5b3b0067755db49
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen

Azure SQL-Database is een automatisch beheerd gegevens-service die voortdurend bewaakt uw query's en geeft de actie die u uitvoeren kunt om de prestaties van uw workload te verbeteren. U kunt bekijken van aanbevelingen en handmatig toepassen of Azure SQL Database automatisch toegepast corrigerende maatregelen kunt: dit staat bekend als **automatische afstemmen modus**. Automatische afstemming kan worden ingeschakeld op de server of de database.

## <a name="enable-automatic-tuning-on-server"></a>Automatische afstemming op server inschakelen

Om automatische afstemming op Azure SQL Database-server, gaat u naar de server in Azure-portal en selecteer vervolgens **automatische afstemming** in het menu. Selecteer de automatische afstemmen opties die u wilt inschakelen en selecteer **toepassen**:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Opties voor automatische afstemmen op de server worden toegepast op alle databases op de server. Standaard alle databases de configuratie van hun bovenliggende server overnemen, maar dit kan worden genegeerd en afzonderlijk opgegeven voor elke database.

## <a name="configure-automatic-tuning-on-database"></a>Configureer Automatische afstemming op database

De Azure-portal kunt u de automatische configuratie afstemmen afzonderlijk op elke database opgeven.

> [!NOTE]
> Er is een algemene aanbevelingen voor het beheren van de automatische afstemmen configuratie op serverniveau zodat dezelfde configuratie-instellingen kunnen worden toegepast op elke database automatisch. Configureer Automatische afstemming op een individuele database als de database andere die door anderen op dezelfde server.
>

Navigeer naar de database in de Azure portal om automatische afstemming op een individuele database, en vervolgens selecteert **automatische afstemming**. U kunt een individuele database om de instellingen overnemen van de server door de optie configureren of u kunt de configuratie voor een database afzonderlijk opgeven.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Nadat u de juiste configuratie hebt geselecteerd, klikt u op **toepassen**.

## <a name="next-steps"></a>Volgende stappen
* Lees de [automatische afstemmen artikel](sql-database-automatic-tuning.md) voor meer informatie over automatische afstemming en hoe deze kan u helpen de prestaties worden verbeterd.
* Zie [prestaties aanbevelingen](sql-database-advisor.md) voor een overzicht van Azure SQL Database prestaties aanbevelingen.
* Zie [inzichten voor queryprestaties](sql-database-query-performance.md) voor meer informatie over het weergeven van de prestatie-invloed van de meest gebruikte query's.
