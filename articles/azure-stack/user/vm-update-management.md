---
title: VM bijwerken en beheren met Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van de Azure-Monitor voor virtuele machines, updatebeheer, wijzigingen bijhouden en inventaris oplossingen in Azure Automation voor het beheren van Windows en Linux-machines die zijn geïmplementeerd in Azure Stack.
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
ms.date: 03/20/2019
ms.author: jeffgilb
ms.reviewer: rtiberiu
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: cb8258c0f837d0e70ba87a26246f055b0efe5c00
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316140"
---
# <a name="azure-stack-vm-update-and-management"></a>Azure Stack-VM bijwerken en beheren
U kunt de volgende functies van Azure Automation-oplossing voor het beheren van Windows en Linux-machines die zijn geïmplementeerd met behulp van Azure Stack:

- **[Updatebeheer](https://docs.microsoft.com/azure/automation/automation-update-management)**. U kunt snel de status van de beschikbare updates op alle agentcomputers beoordelen en beheren van het installatieproces van vereiste updates voor deze Windows en Linux-VM's met de oplossing Update Management.

- **[Wijzigingen bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking)**. Wijzigingen in de geïnstalleerde software, services voor Windows, Windows-register en bestanden en Linux-daemons op de bewaakte servers worden verzonden naar de service Azure Monitor in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens. Met behulp van de informatie op het dashboard voor wijzigingen bijhouden, kunt u gemakkelijk zien van de wijzigingen die zijn aangebracht in uw serverinfrastructuur.

- **[Inventaris](https://docs.microsoft.com/azure/automation/automation-vm-inventory)**. De inventaris bijhouden voor een virtuele machine van Azure Stack biedt een gebruikersinterface op basis van een browser voor het instellen en configureren van de inventarisverzameling.

- **[Azure Monitor voor virtuele machines](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**. Azure Monitor voor virtuele machines bewaakt uw Azure en Azure Stack virtuele machines (VM) en virtuele-machineschaalsets op schaal. Deze analyseert de prestaties en status van uw Windows en Linux-VM's en hun processen en afhankelijkheden van andere bronnen en de externe processen bewaakt. 

> [!IMPORTANT]
> Deze oplossingen zijn hetzelfde als de die worden gebruikt voor het beheren van virtuele Azure-machines. Zowel Azure als Azure Stack-VM's worden beheerd op dezelfde manier, vanuit de dezelfde interface, met dezelfde hulpmiddelen. De Azure Stack-VM's gelden dezelfde zijn als virtuele Azure-machines ook bij het gebruik van de oplossingen updatebeheer, bijhouden, inventaris en virtuele Machines van Azure bewaken met Azure Stack.

## <a name="prerequisites"></a>Vereisten
Verschillende vereisten moeten worden voldaan voordat u deze functies bijwerken en beheren van virtuele machines van Azure Stack. Deze omvatten stappen die moeten worden genomen in de Azure-portal, evenals de Azure Stack-beheerportal.

### <a name="in-the-azure-portal"></a>In de Azure Portal
Voor het gebruik van de Azure-Monitor voor virtuele machines, voorraad, wijzigingen bijhouden en Update Management Azure automation-functies voor Azure Stack-VM's, moet u eerst om in te schakelen van deze oplossingen in Azure.

> [!TIP]
> Als u al deze functies ingeschakeld voor Azure-VM's hebt, kunt u de referenties van uw bestaande LogAnalytics werkruimte. Als u al hebt een LogAnalytics WorkspaceID en primaire sleutel die u wilt gebruiken, gaat u verder met [de volgende sectie](./vm-update-management.md#in-the-azure-stack-administration-portal). Anders gaat u door in deze sectie om een nieuwe LogAnalytics en een automation-account te maken.

De eerste stap bij het inschakelen van deze oplossingen is het [maken van een werkruimte LogAnalytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) in uw Azure-abonnement. Een Log Analytics-werkruimte is een unieke omgeving van de logboeken voor Azure Monitor met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. Als u een werkruimte hebt gemaakt, noteert u de werkruimte-id en de sleutel. Als u wilt deze informatie te bekijken, gaat u naar de blade van de werkruimte, klikt u op **geavanceerde instellingen**, en bekijk de **werkruimte-ID** en **primaire sleutel** waarden. 

Vervolgens moet u [maken van een Automation-Account](https://docs.microsoft.com/azure/automation/automation-create-standalone-account). Een Automation-Account is een container voor uw Azure Automation-resources. Het biedt een manier om te scheiden van uw omgevingen of verder organiseren uw Automation-werkstromen en resources. Nadat het automation-account is gemaakt, moet u de inventaris inschakelen bijhouden van wijzigingen en beheerfuncties bijwerken. U doet dit door deze stappen om in te schakelen van elke functie uit te voeren:

1. In de Azure-portal, gaat u naar de Automation-Account dat u wilt gebruiken.

2. Selecteer de oplossing om in te schakelen (ofwel **voorraad**, **wijzigingen bijhouden**, of **updatebeheer**).

3. Gebruik de **werkruimte selecteren...**  vervolgkeuzelijst te selecteren van de Log Analytics-werkruimte te gebruiken.

4. Controleer of alle overige informatie juist is en klik vervolgens op **inschakelen** de oplossing in te schakelen.

5. Herhaal stappen 2-4 om in te schakelen van alle drie oplossingen. 

   [![](media/vm-update-management/1-sm.PNG "Automation-account-functies inschakelen")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="enable-azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines inschakelen

Azure Monitor voor virtuele machines bewaakt uw Azure virtual machines (VM) en virtuele-machineschaalsets op schaal. Deze analyseert de prestaties en status van uw Windows en Linux-VM's en hun processen en afhankelijkheden van andere bronnen en de externe processen bewaakt.

Azure Monitor voor virtuele machines bevat als een oplossing, ondersteuning voor het bewaken van prestatie- en afhankelijkheden voor virtuele machines die zich on-premises gehost of in een andere cloudprovider. Drie belangrijke functies bieden een diepgaande informatie:

1. Logische onderdelen van Azure-VM's met Windows en Linux: Ten opzichte van de van vooraf geconfigureerde gezondheidscriteria worden gemeten en ze een melding wanneer de geëvalueerde voorwaarde wordt voldaan. 

2. Vooraf gedefinieerde trending worden prestatiegrafieken weergegeven: Core maatstaven voor prestaties van het gastbesturingssysteem van de virtuele machine weergegeven.

3. Kaart van afhankelijkheden: Geeft de onderling verbonden onderdelen met de virtuele machine vanuit de verschillende resourcegroepen en abonnementen.

Nadat de Log Analytics-werkruimte is gemaakt, moet u inschakelen van prestatiemeteritems in de werkruimte voor de verzameling op Linux- en Windows-machines, evenals installeren en inschakelen van de oplossing ServiceMap en InfrastructureInsights in uw werkruimte. Het proces wordt beschreven in de [implementeert Azure Monitor voor virtuele machines](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#deploy-azure-monitor-for-vms) handleiding.

### <a name="in-the-azure-stack-administration-portal"></a>In de beheerportal van Azure Stack
Na het inschakelen van de Azure Automation-oplossingen in Azure portal, moet u naast het aanmelden bij de Azure Stack-beheerportal als een cloudbeheerder en downloaden van de **Azure Monitor-, Update- en Configuratiebeheer** en de **Azure Monitor-, Update- en Configuratiebeheer voor Linux** extensie Azure Stack marketplace-items. 

   ![Azure Monitor-, update- en configuration management-extensie marketplace-item](media/vm-update-management/2.PNG) 

Voor het inschakelen van de Azure-Monitor voor virtuele machines kaart oplossing en krijg inzicht in de netwerken afhankelijkheden, moet u ook downloaden de **Azure Monitor-Agent voor afhankelijkheden**:

   ![Azure Monitor-Agent voor afhankelijkheden](media/vm-update-management/2-dependency.PNG) 

## <a name="enable-update-management-for-azure-stack-virtual-machines"></a>Inschakelen van updatebeheer voor virtuele machines van Azure Stack
Volg deze stappen voor het inschakelen van updatebeheer voor virtuele machines van Azure Stack.

1. Meld u aan bij de gebruikersportal van Azure Stack.

2. In de Azure Stack-gebruikersportal, gaat u naar de blade van de extensies van de virtuele machines waarvoor u wilt deze oplossingen inschakelen, klikt u op **+ toevoegen**, selecteer de **Azure Update- en Configuratiebeheer** extensie en klik op **maken**:

   [![](media/vm-update-management/3-sm.PNG "Blade VM-extensie")](media/vm-update-management/3-lg.PNG#lightbox)

3. Geeft u de eerder gemaakte werkruimte-id en primaire sleutel voor het koppelen van de agent met de werkruimte LogAnalytics en op **OK** implementeren van de extensie.

   [![](media/vm-update-management/4-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/4-lg.PNG#lightbox) 

4. Zoals beschreven in de [documentatie over automation update management](https://docs.microsoft.com/azure/automation/automation-update-management), moet u de oplossing updatebeheer inschakelen voor elke virtuele machine die u wilt beheren. Als u de oplossing voor alle virtuele machines rapporteren aan de werkruimte, schakelt **updatebeheer**, klikt u op **beheren van virtuele machines**, en selecteer vervolgens de **inschakelen op alle beschikbare en toekomstige computers** optie.

   [![](media/vm-update-management/5-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Herhaal deze stap voor het inschakelen van elke oplossing voor de Azure Stack VM's die rapporteren aan de werkruimte. 
  
Nadat de extensie Azure Update- en Configuratiebeheer is ingeschakeld, wordt twee keer per dag een scan uitgevoerd voor elke beheerde virtuele machine. De API wordt aangeroepen om de 15 minuten aan de query voor de laatste updatetijd om te bepalen of de status is gewijzigd. Als de status is gewijzigd, wordt er een nalevingsscan gestart.

Nadat de virtuele machines worden gescand, worden deze weergegeven in de Azure Automation-account in de oplossing Update Management: 

   [![](media/vm-update-management/6-sm.PNG "Hiervoor de werkruimte-id en sleutel")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> Duurt tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers worden weergegeven.

De Azure Stack-VM's kunnen nu worden opgenomen in de geplande update-implementaties, samen met de Azure-VM's.

## <a name="enable-azure-monitor-for-vms-running-on-azure-stack"></a>Azure Monitor inschakelen voor virtuele machines die worden uitgevoerd op Azure Stack
Zodra de virtuele machine heeft de **Azure Monitor-, Update- en Configuratiebeheer** en de **Azure Monitor-Agent voor afhankelijkheden** extensies geïnstalleerd, start het melden van gegevens in de [Azure Monitor voor virtuele machines](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) oplossing. 

> [!TIP]
> De **Azure Monitor-Agent voor afhankelijkheden** extensie geen parameters vereist. De Azure-Monitor voor agent voor afhankelijkheden van virtuele machines toewijzen niet de gegevens zelf worden verzonden en er is geen wijzigingen in de firewalls en poorten vereist. De kaartgegevens worden altijd verzonden door de Log Analytics-agent naar de service Azure Monitor, hetzij rechtstreeks of via de [OMS-Gateway](https://docs.microsoft.com/azure/azure-monitor/platform/gateway) als uw IT-beveiligingsbeleid niet toestaat dat computers in het netwerk verbinding maken met internet.

Azure Monitor voor virtuele machines bevat een set van prestatiegrafieken weergegeven die zijn gericht verschillende prestatie-indicatoren (KPI's) om te bepalen hoe goed een virtuele machine wordt uitgevoerd. De grafieken weergegeven Resourcegebruik gedurende een bepaalde periode, zodat u kunt identificeren van knelpunten, afwijkingen, of schakel over naar een perspectief aanbieding elke machine om gebruik van resources op basis van de geselecteerde meetwaarde weer te geven. Er zijn talrijke elementen bij het omgaan met prestaties, Azure-Monitor voor virtuele machines monitors besturingssysteem key performance indicators die betrekking hebben op processor, geheugen, netwerkadapter en schijfgebruik. Prestaties is een aanvulling op de functie voor health monitoring en helpt u problemen die wijzen op een onderdeel systeemfouten, ondersteuning voor afstemmen en optimalisatie voor het bereiken van efficiëntie blootstellen of ondersteuning voor capaciteitsplanning.

   ![Tabblad met Azure Monitor-prestaties](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-performance/vminsights-performance-aggview-01.png)

Weergave van de gedetecteerde toepassingsonderdelen op Windows en Linux virtuele machines die worden uitgevoerd in Azure Stack kan worden waargenomen op twee manieren met Azure Monitor voor virtuele machines, van een virtuele machine rechtstreeks of via groepen virtuele machines van Azure Monitor.
De [met behulp van Azure Monitor voor virtuele machines (preview) de kaart om te begrijpen van toepassingsonderdelen](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-maps) artikel krijgt u inzicht in de ervaring tussen de twee perspectieven en hoe u de functie van de kaart.

   ![Tabblad met Azure Monitor-prestaties](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-maps/map-multivm-azure-monitor-01.png)


## <a name="enable-update-management-using-a-resource-manager-template"></a>Update-beheer inschakelen met Resource Manager-sjabloon
Als u een groot aantal virtuele machines van Azure Stack hebt, kunt u [deze Azure Resource Manager-sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) eenvoudiger de oplossing te implementeren op virtuele machines. De sjabloon implementeert de Microsoft Monitoring Agent-extensie op een bestaande VM in Azure Stack en deze toevoegen aan een bestaande werkruimte van Azure LogAnalytics.
 
## <a name="next-steps"></a>Volgende stappen
[SQL Server-VM-prestaties optimaliseren](azure-stack-sql-server-vm-considerations.md)




