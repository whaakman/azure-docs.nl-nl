---
title: Een virtuele Azure-machine beheren met inventarisverzameling | Microsoft Docs
description: Een virtuele machine beheren met inventarisverzameling
services: automation
ms.service: automation
ms.subservice: change-inventory-management
keywords: inventaris, automation, wijziging, bijhouden
author: jennyhunter-msft
ms.author: jehunte
ms.date: 02/06/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c0caece57885dfa3c26d368990a6af8e5b14eed6
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823991"
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Een virtuele Azure-machine met inventarisverzameling beheren

U kunt Inventaris bijhouden voor een virtuele Azure-machine inschakelen vanaf de resourcepagina van de computer. U kunt inventarisgegevens verzamelen en weergeven voor software, bestanden, Linux-daemons, Windows-services en Windows-registersleutels op uw computers. Deze methode biedt een gebruikersinterface op basis van een browser voor het instellen en configureren van een inventarisverzameling.

## <a name="before-you-begin"></a>Voordat u begint

Als u nog geen abonnement op Azure hebt, [maak dan een gratis account](https://azure.microsoft.com/free/).

In dit artikel wordt ervan uitgegaan dat u hebt een virtuele machine voor het configureren van de oplossing op. Als u nog geen virtuele Azure-machine hebt, [maak dan een virtuele machine](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Verzamelen van inventaris inschakelen op de resourcepagina van de virtuele machine

1. Selecteer in het linkerdeelvenster van de Azure-portal de optie **Virtuele machines**.
2. Selecteer een virtuele machine in de lijst met virtuele machine.
3. Op de **Resource** menu onder **Operations**, selecteer **voorraad**.
4. Selecteer een werkruimte voor logboekanalyse voor het opslaan van de logboeken van uw gegevens.
    Als er geen werkruimte beschikbaar is voor deze regio, wordt u gevraagd om een standaardwerkruimte en een Automation-account te maken.
5. Selecteer **Inschakelen** om de onboarding voor uw computer te starten.

   ![Opties voor onboarding weergeven](./media/automation-vm-inventory/inventory-onboarding-options.png)

    In een statusbalk ziet u de melding dat de oplossing wordt ingeschakeld. Dit proces duurt maximaal 15 minuten. Gedurende deze tijd kunt u het venster sluiten of kunt u deze openen en het ontvangt u een melding wanneer de oplossing is ingeschakeld. U kunt de implementatiestatus controleren vanuit het deelvenster met meldingen.

   ![De inventarisoplossing onmiddellijk na de onboarding weergeven](./media/automation-vm-inventory/inventory-onboarded.png)

Wanneer de implementatie is voltooid, verdwijnt de statusbalk. Het systeem is nog bezig met het verzamelen van inventarisgegevens en de gegevens zijn daarom mogelijk nog niet zichtbaar. Het duurt maximaal 24 uur voordat een volledige verzameling van de gegevens beschikbaar is.

## <a name="configure-your-inventory-settings"></a>De inventarisinstellingen configureren

Standaard worden software-, Windows Services- en Linux-daemons geconfigureerd voor verzameling. Als u Windows-register- en bestandsinventarisgegevens wilt verzamelen, configureer dan de instellingen voor de inventarisverzameling.

1. In de **voorraad** weergave, selecteer de **instellingen bewerken** knop aan de bovenkant van het venster.
2. Als u een nieuwe instelling voor de verzameling wilt toevoegen, selecteert u de tabbladen **Windows-register**, **Windows-bestanden** en **Linux-bestanden** om naar de instellingencategorie te gaan die u wilt toevoegen.
3. Selecteer de relevante categorie en klik op **toevoegen** aan de bovenkant van het venster.

De volgende tabellen bevatten informatie over elke eigenschap die kan worden geconfigureerd voor de verschillende categorieën.

### <a name="windows-registry"></a>Windows-register

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Bepaalt of de instelling wordt toegepast        |
|Itemnaam     | Beschrijvende naam van het bestand dat moet worden bijgehouden        |
|Groep     | De naam van een groep voor het logisch groeperen van bestanden        |
|Windows-registersleutel   | Het pad voor het bestand, bijvoorbeeld: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup      |

### <a name="windows-files"></a>Windows-bestanden

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Bepaalt of de instelling wordt toegepast        |
|Itemnaam     | Beschrijvende naam van het bestand dat moet worden bijgehouden        |
|Groep     | De naam van een groep voor het logisch groeperen van bestanden        |
|Pad invoeren     | Het pad voor het controleren op het bestand, bijvoorbeeld: "C:\temp\myfile.txt"

### <a name="linux-files"></a>Linux-bestanden

|Eigenschap  |Description  |
|---------|---------|
|Ingeschakeld     | Bepaalt of de instelling wordt toegepast        |
|Itemnaam     | Beschrijvende naam van het bestand dat moet worden bijgehouden        |
|Groep     | De naam van een groep voor het logisch groeperen van bestanden        |
|Pad invoeren     | Het pad voor het controleren op het bestand, bijvoorbeeld: "/etc/*.conf"       |
|Padtype     | Type item voor het bijhouden van wijzigingen. Mogelijke waarden zijn Bestand en Map        |
|Recursie     | Bepaalt of recursie wordt gebruikt bij het zoeken naar het item dat moet worden bijgehouden.        |
|Sudo gebruiken     | Deze instelling bepaalt of sudo wordt gebruikt bij het controleren op het item.         |
|Koppelingen     | Deze instelling bepaalt hoe symbolische koppelingen worden afgehandeld bij het doorlopen van mappen.<br> **Negeren** - Symbolische koppelingen worden genegeerd en de bestanden/mappen waarnaar wordt verwezen, worden niet opgenomen<br>**Volgen** - Symbolische koppelingen worden gevolgd tijdens recursie en de bestanden/mappen waarnaar wordt verwezen, worden opgenomen<br>**Beheren** - Symbolische koppelingen worden gevolgd en de afhandeling van de geretourneerde inhoud kan worden gewijzigd      |

## <a name="manage-machine-groups"></a>Computergroepen beheren

Inventarisatie kunt u maken en machine-groepen in Azure Monitor logboeken weergeven. Computergroepen zijn verzamelingen van computers die zijn gedefinieerd door een query in de logboeken van Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Naar weergave uw machine groepen selecteren de **Machine groepen** tabblad op de pagina inventaris.

![Machine-groepen op de pagina inventaris weergeven](./media/automation-vm-inventory/inventory-machine-groups.png)

Een computergroep selecteren in de lijst, wordt de pagina van de groepen Machine geopend. Deze pagina bevat informatie over de Machinegroep. Deze gegevens omvatten de log analytics-query die wordt gebruikt voor het definiëren van de groep. Aan de onderkant van de pagina wordt een lijst met resultatenpagina's van de machines die deel van die groep uitmaken.

![Machine groep-pagina weergeven](./media/automation-vm-inventory/machine-group-page.png)

Klik op de **+ kloon** knop voor het klonen van de Machinegroep. Hier moet u geven de groep een nieuwe naam en een alias voor de groep. De definitie van de kan op dit moment worden gewijzigd. Na het wijzigen van de query drukt u op **valideren query** om een voorbeeld van de machines die zouden worden geselecteerd. Wanneer u tevreden met de groep bent klikt u op **maken** om de Machinegroep te maken

Als u maken van een nieuwe mchine-groep wilt, selecteert u **+ maken van een computergroep**. Deze knop opent u de **maakt u een pagina van de groep machine** waar u de nieuwe groep kunt definiëren. Klik op **maken** om de groep te maken.

![Nieuwe computergroep maken](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-virtual-machine-from-management"></a>De virtuele machine loskoppelen van beheer

Ga als volgt te werk om uw virtuele machine te verwijderen uit inventarisbeheer:

1. Selecteer in het linkermenu van de Azure-portal de optie **Log Analytics** en selecteer de werkruimte die u hebt gebruikt tijdens de onboarding van de virtuele machine.
2. Selecteer in het venster **Log Analytics** in het menu **Resource** de optie **Virtuele machines** in de categorie **Gegevensresources voor werkruimte**.
3. Selecteer in de lijst de virtuele machine die u wilt loskoppelen. De virtuele machine heeft een groen vinkje naast de tekst **Deze werkruimte** in de kolom **OMS-verbinding**.

   >[!NOTE]
   >OMS wordt nu aangeduid als Azure Monitor-Logboeken.
   
4. Selecteer bovenaan de volgende pagina de optie **Verbinding verbreken**.
5. Selecteer **Ja** in het bevestigingsvenster.
    Met deze actie wordt de virtuele machine losgekoppeld van beheer.

## <a name="next-steps"></a>Volgende stappen

* Zie [Track software changes in your environment with the Change Tracking solution](../log-analytics/log-analytics-change-tracking.md) (Wijzigingen in uw omgeving bijhouden met de oplossing Wijzigingen bijhouden) voor meer informatie over het beheren van wijzigingen in bestands- en registerinstellingen.
* Zie voor meer informatie over het beheren van Windows en pakketupdates op uw virtuele machines, [de updatebeheer-oplossing in Azure](../operations-management-suite/oms-solution-update-management.md).

