---
title: Een Azure VM beheren met inventarisverzameling | Microsoft Docs
description: Een VM beheren met inventarisverzameling
services: automation
keywords: inventaris, automation, wijziging, bijhouden
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: hero-article
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5291d112c2cdf157543fe301d5a5c80f3fe561ae
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Een virtuele Azure-machine met inventarisverzameling beheren

Inventaris bijhouden voor een virtuele Azure-machine kan worden ingeschakeld op de resourcepagina van de computer. Deze methode biedt een gebruikersinterface op basis van een browser voor het instellen en configureren van een inventarisverzameling.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
Als u nog geen virtuele Azure-machine hebt, maakt u een [virtuele machine](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Verzamelen van inventaris inschakelen op de resourcepagina van de virtuele machine

1. Selecteer op het linkerscherm de optie **Virtuele machines**.
1. Selecteer de virtuele machine in de lijst.
1. Selecteer in het resourcemenu onder **Bewerkingen** de optie **Inventaris (preview)** .
1. Boven aan de pagina wordt een banner weergegeven waarin u wordt gewaarschuwd dat de oplossing niet is ingeschakeld. Klik op de banner om de oplossing in te schakelen.
1. Selecteer een Log Analytics-werkruimte om de gegevenslogboeken op te slaan. Als er geen werkruimten beschikbaar zijn voor deze regio, wordt u gevraagd om een standaardwerkruimte en Automation-account te maken. Klik op **Inschakelen** om de onboarding voor de computer te starten.

   ![Opties voor onboarding weergeven](./media/automation-vm-inventory/inventory-onboarding-options.png)  

1. In een statusbalk ziet u de melding dat de oplossing wordt ingeschakeld. Dit proces duurt maximaal 15 minuten. Gedurende deze tijd kunt u de blade sluiten. Als u de blade open laat, ziet u een melding wanneer de oplossing is ingeschakeld. U kunt de implementatiestatus controleren vanuit het deelvenster met meldingen.

   ![De inventarisoplossing onmiddellijk na de onboarding weergeven](./media/automation-vm-inventory/inventory-onboarded.png)

1. Wanneer de implementatie is voltooid, verdwijnt de statusbalk. Op dit moment worden er nog steeds inventarisgegevens verzameld. De gegevens zijn daarom mogelijk nog niet zichtbaar. Het duurt maximaal 24 uur voordat een volledige verzameling van de gegevens beschikbaar is.

## <a name="configure-your-inventory-settings"></a>De inventarisinstellingen configureren

Standaard worden software-, Windows Services- en Linux-daemons geconfigureerd voor de verzameling. Als u Windows-register en bestandsinventaris wilt verzamelen, configureert u de instellingen voor de inventarisverzameling.

1. Selecteer in de weergave **Inventaris (preview)** de knop **Instellingen bewerken** boven aan de pagina.
2. Als u een nieuwe instelling voor de verzameling wilt toevoegen, navigeert u naar de instellingencategorie die u wilt toevoegen, met behulp van de tabbladen **Windows-register**, **Windows-bestanden** en **Linux-bestanden**. Klik boven aan de pagina op **Toevoegen**.
3. Ga naar de [documentatiepagina voor de inventaris](https://aka.ms/configinventorydocs) om de details en beschrijvingen voor elke instelling te bekijken.

## <a name="disconnecting-your-virtual-machine-from-management"></a>De virtuele machine loskoppelen van beheer

Ga als volgt te werk om uw computer te verwijderen uit inventarisbeheer:

1. Klik in het linkermenu in Azure Portal op **Logboekanalyse** en klik vervolgens op de werkruimte die u hebt gebruikt tijdens de onboarding van de virtuele machine.
1. Selecteer op de pagina Logboekanalyse onder de categorie **Gegevensbronnen voor werkruimte** in het resourcemenu de optie **Virtuele machines**. 
1. Selecteer de virtuele machine die u wilt verwijderen uit de lijst. De machine heeft een groen vinkje naast de tekst Deze werkruimte in de kolom **OMS-verbinding**. Klik boven aan de volgende pagina op **Loskoppelen** en in het bevestigingsdialoogvenster op **Ja** om te machine los te koppelen van beheer.

## <a name="next-steps"></a>Volgende stappen

* Zie [Wijzigingen bijhouden](../log-analytics/log-analytics-change-tracking.md) voor meer informatie over het beheren van wijzigingen in bestands- en registerinstellingen op de virtuele machines.
* Zie [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) voor meer informatie over het beheren van Windows en pakketupdates op de virtuele machines

