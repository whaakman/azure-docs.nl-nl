---
title: Een virtuele Azure-machine beheren met inventarisverzameling | Microsoft Docs
description: Een virtuele machine beheren met inventarisverzameling
services: automation
keywords: inventaris, automation, wijziging, bijhouden
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: article
manager: carmonm
ms.openlocfilehash: 7b0e39e98a81231b68414f36ac5c1fc0897304a1
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Een virtuele Azure-machine met inventarisverzameling beheren

U kunt Inventaris bijhouden voor een virtuele Azure-machine inschakelen vanaf de resourcepagina van de computer. Deze methode biedt een gebruikersinterface op basis van een browser voor het instellen en configureren van een inventarisverzameling.

## <a name="before-you-begin"></a>Voordat u begint
Als u nog geen abonnement op Azure hebt, [maak dan een gratis account](https://azure.microsoft.com/free/).
Als u nog geen virtuele Azure-machine hebt, [maak dan een virtuele machine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Verzamelen van inventaris inschakelen op de resourcepagina van de virtuele machine

1. Selecteer in het linkerdeelvenster van de Azure-portal de optie **Virtuele machines**.
2. Selecteer een virtuele machine in de lijst met virtuele machine.
3. Selecteer in het menu **Resource** onder **Bewerkingen** de optie **Inventaris (preview)**.  
    Bovenaan het venster wordt een banner weergegeven met het bericht dat de oplossing niet is ingeschakeld. 
4. Selecteer de banner om de oplossing in te schakelen.
5. Selecteer een Log Analytics-werkruimte om de gegevenslogboeken op te slaan.  
    Als er geen werkruimte beschikbaar is voor deze regio, wordt u gevraagd om een standaardwerkruimte en een Automation-account te maken. 
6. Selecteer **Inschakelen** om de onboarding voor uw computer te starten.

   ![Opties voor onboarding weergeven](./media/automation-vm-inventory/inventory-onboarding-options.png)  
    In een statusbalk ziet u de melding dat de oplossing wordt ingeschakeld. Dit proces duurt maximaal 15 minuten. Gedurende deze tijd kunt u het venster sluiten. Als u het geopend laat, verschijnt er een melding wanneer de oplossing is ingeschakeld. U kunt de implementatiestatus controleren vanuit het deelvenster met meldingen.

   ![De inventarisoplossing onmiddellijk na de onboarding weergeven](./media/automation-vm-inventory/inventory-onboarded.png)

Wanneer de implementatie is voltooid, verdwijnt de statusbalk. Het systeem is nog bezig met het verzamelen van inventarisgegevens en de gegevens zijn daarom mogelijk nog niet zichtbaar. Het duurt maximaal 24 uur voordat een volledige verzameling van de gegevens beschikbaar is.

## <a name="configure-your-inventory-settings"></a>De inventarisinstellingen configureren

Standaard worden software-, Windows Services- en Linux-daemons geconfigureerd voor verzameling. Als u Windows-register- en bestandsinventarisgegevens wilt verzamelen, configureer dan de instellingen voor de inventarisverzameling.

1. Selecteer in de weergave **Inventaris (preview)** de knop **Instellingen bewerken** bovenaan het venster.
2. Als u een nieuwe instelling voor de verzameling wilt toevoegen, selecteert u de tabbladen **Windows-register**, **Windows-bestanden** en **Linux-bestanden** om naar de instellingencategorie te gaan die u wilt toevoegen. 
3. Selecteer bovenaan het venster de optie **Toevoegen**.
4. Ga naar de [documentatiepagina voor de inventaris](https://aka.ms/configinventorydocs) om de details en beschrijvingen voor elke instelling te bekijken.

## <a name="disconnect-your-virtual-machine-from-management"></a>De virtuele machine loskoppelen van beheer

Ga als volgt te werk om uw virtuele machine te verwijderen uit inventarisbeheer:

1. Selecteer in het linkermenu van de Azure-portal de optie **Log Analytics** en selecteer de werkruimte die u hebt gebruikt tijdens de onboarding van de virtuele machine.
2. Selecteer in het venster **Log Analytics** in het menu **Resource** de optie **Virtuele machines** in de categorie **Gegevensresources voor werkruimte**. 
3. Selecteer in de lijst de virtuele machine die u wilt loskoppelen. De virtuele machine heeft een groen vinkje naast de tekst **Deze werkruimte** in de kolom **OMS-verbinding**. 
4. Selecteer bovenaan de volgende pagina de optie **Verbinding verbreken**.
5. Selecteer **Ja** in het bevestigingsvenster.  
    Met deze actie wordt de virtuele machine losgekoppeld van beheer.

## <a name="next-steps"></a>Volgende stappen

* Zie [Track software changes in your environment with the Change Tracking solution](../log-analytics/log-analytics-change-tracking.md) (Wijzigingen in uw omgeving bijhouden met de oplossing Wijzigingen bijhouden) voor meer informatie over het beheren van wijzigingen in bestands- en registerinstellingen.
* Zie [De oplossing voor Updatebeheer in OMS](../operations-management-suite/oms-solution-update-management.md) voor meer informatie over het beheren van Windows en pakketupdates op uw virtuele machines.
