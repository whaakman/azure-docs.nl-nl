---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 760bb5b62e9bba9b7a83f99760f7fe5d8c399dfb
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226712"
---
1. Vouw in Failoverclusterbeheer **rollen**, en selecteer vervolgens de beschikbaarheidsgroep.  

2. Op de **Resources** tabblad en klik vervolgens op met de rechtermuisknop op de listenernaam van de **eigenschappen**.

3. Klik op de **afhankelijkheden** tabblad. Als er meerdere resources worden weergegeven, Controleer of de IP-adressen hebt of niet en afhankelijkheden.  

4. Klik op **OK**.

5. Met de rechtermuisknop op de listenernaam van de, en klik vervolgens op **Online brengen**.

6. Nadat de listener is online op het **Resources** tabblad en klik vervolgens op met de rechtermuisknop op de beschikbaarheidsgroep **eigenschappen**.
   
    ![De beschikbaarheidsgroepresource configureren](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Maken van een afhankelijkheid op de bron van listener (niet de IP-adres resources naam) en klik vervolgens op **OK**.
   
    ![Afhankelijkheid toevoegen op de listenernaam van de](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Start SQL Server Management Studio en maak verbinding met de primaire replica.

9. Ga naar **AlwaysOn hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **\<AvailabilityGroupName\>**   >  **Beschikbaarheidsgroep Listeners**.  
    De listenernaam van de die u hebt gemaakt in Failoverclusterbeheer weergegeven.

10. Met de rechtermuisknop op de listenernaam van de, en klik vervolgens op **eigenschappen**.

11. In de **poort** vak, geef het poortnummer voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort die u eerder hebt gebruikt (in deze zelfstudie is 1433 de standaardinstelling), en klik vervolgens op **OK**.

