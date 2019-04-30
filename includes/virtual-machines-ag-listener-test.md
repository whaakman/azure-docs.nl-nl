---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: d579e7a4fd83c1a0ce335e0b2357dcbafb217398
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097672"
---
In deze stap test u de beschikbaarheidsgroep-listener met behulp van een clienttoepassing die wordt uitgevoerd op hetzelfde netwerk.

-Clientconnectiviteit gelden de volgende vereisten:

* Clientverbindingen met de listener moeten afkomstig zijn van de machines die zich in een andere cloudservice dan de versie die als host fungeert voor de Always On availability replica's bevinden.
* Als wordt altijd op replica's in verschillende subnetten, clients moeten opgeven *MultisubnetFailover = True* in de verbindingsreeks. Deze voorwaarde resulteert in parallelle verbindingspogingen met replica's in verschillende subnetten. Dit scenario omvat een regio-overschrijdende altijd op beschikbaarheid van groep implementatie.

Een voorbeeld is het verbinding maken met de listener van een van de virtuele machines in dezelfde Azure virtual network (maar niet een die als host fungeert voor een replica). Er is een eenvoudige manier om te voltooien van deze test om te proberen verbinding maken met SQL Server Management Studio de beschikbaarheidsgroep-listener. Een andere eenvoudige manier is om uit te voeren [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx), als volgt:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Als de waarde EndpointPort *1433*, u niet moet opgeven in de aanroep. De vorige aanroep zijn ook van uitgegaan dat de client-computer is gekoppeld aan hetzelfde domein en dat de oproepende functie heeft gekregen machtigingen voor de database met behulp van Windows-verificatie.
> 
> 

Wanneer u de listener test, moet u de beschikbaarheidsgroep om ervoor te zorgen dat clients verbinding met de listener voor failovers maken kunnen failover.

<!-- Update_Description: update meta properties -->