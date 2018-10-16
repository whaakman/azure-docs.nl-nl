---
title: VM bijwerken en beheren met Azure Stack | Microsoft Docs
description: Informatie over het gebruik van de Update Management, wijzigingen bijhouden en inventaris-oplossingen in Azure Automation voor het beheren van Windows-VM's die zijn geïmplementeerd in Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: rtiberiu
ms.openlocfilehash: 1ef20dc35b069c5f12c2f31d0979949be27271e0
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324162"
---
# <a name="azure-stack-vm-update-and-management"></a>Azure Stack-VM bijwerken en beheren
U kunt de volgende functies van Azure Automation-oplossing voor het beheren van Windows-VM's die zijn geïmplementeerd met behulp van Azure Stack:

- **[Updatebeheer](https://docs.microsoft.com/azure/automation/automation-update-management)**. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en beheren van het installatieproces van vereiste updates voor deze Windows-VM's met de oplossing Update Management.

- **[Wijzigingen bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking)**. Wijzigingen in de geïnstalleerde software, services voor Windows, Windows-register en bestanden op de bewaakte servers worden verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens. Met behulp van de informatie op het dashboard voor wijzigingen bijhouden, kunt u gemakkelijk zien van de wijzigingen die zijn aangebracht in uw serverinfrastructuur.

- **[Inventaris](https://docs.microsoft.com/azure/automation/automation-vm-inventory)**. De inventaris bijhouden voor een virtuele machine van Azure Stack Windows biedt een gebruikersinterface op basis van een browser voor het instellen en configureren van de inventarisverzameling. 

> [!IMPORTANT]
> Deze oplossingen zijn hetzelfde als de die worden gebruikt voor het beheren van virtuele Azure-machines. Zowel Azure als Azure Stack Windows-VM's worden beheerd op dezelfde manier, vanuit de dezelfde interface, met dezelfde hulpmiddelen. De Azure Stack-VM's gelden dezelfde zijn als virtuele Azure-machines ook bij het gebruik van de Update Management, wijzigingen bijhouden en inventaris oplossingen met Azure Stack.

## <a name="prerequisites"></a>Vereisten
Verschillende vereisten moeten worden voldaan voordat u deze functies voor het bijwerken en beheren van Azure Stack Windows-VM's. Deze omvatten stappen die moeten worden genomen in de Azure-portal, evenals de Azure Stack-beheerportal.

### <a name="in-the-azure-portal"></a>In de Azure-portal
Voor het gebruik van de voorraad, wijzigingen bijhouden en Update Management Azure automation-functies voor Azure Stack Windows VM's, moet u eerst om in te schakelen van deze oplossingen in Azure.

> [!TIP]
> Als u al deze functies ingeschakeld voor Azure-VM's hebt, kunt u de referenties van uw bestaande LogAnalytics werkruimte. Als u al hebt een LogAnalytics WorkspaceID en primaire sleutel die u wilt gebruiken, gaat u verder met [de volgende sectie](.\vm-update-management.md#in-the-azure-stack-administration-portal). Anders gaat u door in deze sectie om een nieuwe LogAnalytics en een automation-account te maken.

De eerste stap bij het inschakelen van deze oplossingen is het [maken van een werkruimte LogAnalytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) in uw Azure-abonnement. Een Log Analytics-werkruimte is een unieke Log Analytics-omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. Als u een werkruimte hebt gemaakt, noteert u de werkruimte-id en de sleutel. Als u wilt deze informatie te bekijken, gaat u naar de blade van de werkruimte, klikt u op **geavanceerde instellingen**, en bekijk de **werkruimte-ID** en **primaire sleutel** waarden. 

Vervolgens moet u [maken van een Automation-Account](https://docs.microsoft.com/azure/automation/automation-create-standalone-account). Een Automation-Account is een container voor uw Azure Automation-resources. Het biedt een manier om te scheiden van uw omgevingen of verder organiseren uw Automation-werkstromen en resources. Nadat het automation-account is gemaakt, moet u de inventaris inschakelen bijhouden van wijzigingen en beheerfuncties bijwerken. U doet dit door deze stappen om in te schakelen van elke functie uit te voeren:

1. In de Azure-portal, gaat u naar de Automation-Account dat u wilt gebruiken.

2. Selecteer de oplossing om in te schakelen (ofwel **voorraad**, **wijzigingen bijhouden**, of **updatebeheer**).

3. Gebruik de **werkruimte selecteren...**  vervolgkeuzelijst te selecteren van de Log Analytics-werkruimte te gebruiken.

4. Controleer of alle overige informatie juist is en klik vervolgens op **inschakelen** de oplossing in te schakelen.

5. Herhaal stappen 2-4 om in te schakelen van alle drie oplossingen. 

   [![](media/vm-update-management/1-sm.PNG "Automation-account-functies inschakelen")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="in-the-azure-stack-administration-portal"></a>In de beheerportal van Azure Stack
Na het inschakelen van de Azure Automation-oplossingen in Azure portal, moet u naast het aanmelden bij de Azure Stack-beheerportal als een cloudbeheerder en downloaden van de **Azure Update- en Configuratiebeheer** extensie voor Azure Stack marketplace-item. 

   ![Azure-update en configuration management-extensie marketplace-item](media/vm-update-management/2.PNG) 

## <a name="enable-update-management-for-azure-stack-virtual-machines"></a>Inschakelen van updatebeheer voor virtuele machines van Azure Stack
Volg deze stappen voor het inschakelen van updatebeheer voor Azure Stack Windows VM's.

1. Meld u aan bij de gebruikersportal van Azure Stack.

2. In de Azure Stack-gebruikersportal, gaat u naar de blade extensies van de Windows virtuele machines waarvoor u wilt deze oplossingen inschakelen, klikt u op **+ toevoegen**, selecteer de **Azure Update- en Configuratiebeheer**-extensie en klik op **maken**:

   [![](media/vm-update-management/3-sm.PNG "Windows-VM-extensie-blade")](media/vm-update-management/3-lg.PNG#lightbox)

3. Geeft u de eerder gemaakte werkruimte-id en primaire sleutel voor het koppelen van de agent met de werkruimte LogAnalytics en op **OK** implementeren van de extensie.

   [![](media/vm-update-management/4-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/4-lg.PNG#lightbox) 

4. Zoals beschreven in de [documentatie over automation update management](https://docs.microsoft.com/azure/automation/automation-update-management), moet u de oplossing updatebeheer inschakelen voor elke virtuele machine die u wilt beheren. Als u de oplossing voor alle virtuele machines rapporteren aan de werkruimte, schakelt **updatebeheer**, klikt u op **beheren van virtuele machines**, en selecteer vervolgens de **inschakelen op alle beschikbare en toekomstige computers** optie.

   [![](media/vm-update-management/5-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Herhaal deze stap voor het inschakelen van elke oplossing voor de Azure Stack Windows VM's die rapporteren aan de werkruimte. 
  
Nadat de extensie Azure Update- en Configuratiebeheer is ingeschakeld, wordt twee keer per dag een scan uitgevoerd voor elke beheerde Windows-VM. De Windows-API wordt aangeroepen om de 15 minuten aan de query voor de laatste updatetijd om te bepalen of de status is gewijzigd. Als de status is gewijzigd, wordt er een nalevingsscan gestart.

Nadat de virtuele machines worden gescand, worden deze weergegeven in de Azure Automation-account in de oplossing Update Management: 

   [![](media/vm-update-management/6-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> Duurt tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers worden weergegeven.

De Azure Stack Windows VM's kunnen nu worden opgenomen in de geplande update-implementaties, samen met de Azure-VM's.

## <a name="enable-update-management-using-a-resource-manager-template"></a>Update-beheer inschakelen met Resource Manager-sjabloon
Als u een groot aantal Azure Stack Windows-VM's hebt, kunt u [deze Azure Resource Manager-sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) eenvoudiger de oplossing te implementeren op Windows-VM's. De sjabloon implementeert u de Microsoft Monitoring Agent-extensie aan een bestaande Windows-VM en deze toevoegen aan een bestaande werkruimte van Azure LogAnalytics.
 
## <a name="next-steps"></a>Volgende stappen
[SQL Server-prestaties optimaliseren](azure-stack-sql-server-vm-considerations.md)
