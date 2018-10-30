---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 394b242ab46da7821f77e8d008836753f4e358e2
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227104"
---
In deze stap maakt maken u handmatig de beschikbaarheidsgroep-listener in Failoverclusterbeheer en SQL Server Management Studio.

1. Open Failoverclusterbeheer vanuit het knooppunt dat als host fungeert voor de primaire replica.

2. Selecteer de **netwerken** knooppunt en de clusternetwerknaam opmerking. Deze naam wordt gebruikt in de variabele $ClusterNetworkName in het PowerShell-script.

3. Vouw de naam van het cluster en klik vervolgens op **rollen**.

4. In de **rollen** in het deelvenster met de rechtermuisknop op de naam van de beschikbaarheidsgroep en selecteer vervolgens **Resource toevoegen** > **Client Access Point**.
   
    ![Client Access Point voor de beschikbaarheidsgroep toevoegen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. In de **naam** vak, een naam op voor deze nieuwe listener maken, klikt u op **volgende** twee keer, en klik vervolgens op **voltooien**.  
    Worden niet weergegeven in de listener of resource online op dit moment.

6. Klik op de **Resources** tabblad uit en vouw vervolgens het clienttoegangspunt dat u zojuist hebt gemaakt. 
    De IP-adresresource voor elk clusternetwerk in uw cluster wordt weergegeven. Als dit een Azure-only-oplossing, wordt slechts één IP-adresresource wordt weergegeven.

7. Voer een van de volgende bewerkingen uit:
   
   * Een hybride oplossing configureren:
     
        a. Met de rechtermuisknop op de resource van het IP-adres dat overeenkomt met uw on-premises-subnet, en selecteer vervolgens **eigenschappen**. Noteer de naam van de IP-adres en de netwerknaam.
   
        b. Selecteer **statisch IP-adres**, een niet-gebruikte IP-adres toewijzen en klik vervolgens op **OK**.
 
   * Het configureren van een Azure-only-oplossing:

        a. Met de rechtermuisknop op de resource van het IP-adres dat overeenkomt met uw Azure-subnet, en selecteer vervolgens **eigenschappen**.
       
       > [!NOTE]
       > Als de listener niet later online is gekomen vanwege een conflicterende IP-adres dat is geselecteerd door DHCP, kunt u een statisch IP-adres configureren in dit eigenschappenvenster.
       > 
       > 

       b. In dezelfde **IP-adres** wijzigen van het eigenschappenvenster de **naam IP-adres**.  
        Deze naam wordt gebruikt in de variabele $IPResourceName van het PowerShell-script. Als uw oplossing meerdere Azure-netwerken omvat, herhaalt u deze stap voor elke IP-resource.

