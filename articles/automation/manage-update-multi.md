---
title: Updates voor meerdere virtuele machines in Azure beheren | Microsoft Docs
description: Geef virtuele Azure-machines vrij om updates te beheren.
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: 89bf87f27fdf276068cba261fc6ae1660307e0b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Updates voor meerdere virtuele machines in Azure beheren

Met Updatebeheer kunt u updates en patches voor uw virtuele Azure-machines beheren.
Vanuit uw [Azure Automation](automation-offering-get-started.md)-account kunt u virtuele machines snel vrijgeven, de status van de beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of de updates zijn toegepast op alle virtuele machines waarvoor Updatebeheer is ingeschakeld.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende zaken nodig om de stappen in deze handleiding uit te voeren:

* Een Azure Automation-account. Zie [Azure Uitvoeren-als-account](automation-sec-configure-azure-runas-account.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.
* Een virtuele machine van Azure Resource Manager (niet Klassiek). Zie [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor instructies voor het maken van een VM

## <a name="enable-update-management-for-azure-virtual-machines"></a>Updatebeheer voor virtuele Azure-machines inschakelen

1. Open in Azure Portal het Automation-account.
2. Selecteer aan de linkerkant van het scherm **Updatebeheer**.
3. Klik boven aan het scherm op **Azure-VM toevoegen**.
    ![VM's vrijgeven](./media/manage-update-multi/update-onboard-vm.png)
4. Selecteer een virtuele-machine om vrij te geven. Het scherm **Updatebeheer inschakelen** wordt weergegeven.
5. Klik op **Inschakelen**.

   ![Updatebeheer inschakelen](./media/manage-update-multi/update-enable.png)

Updatebeheer is ingeschakeld voor uw virtuele machine.

## <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster.
U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Plan een nieuwe update-implementatie voor één of meer virtuele machines door te klikken op **Update-implementatie plannen** boven aan het scherm **Updatebeheer**. Geef het volgende op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Type besturingssysteem**: selecteer Windows of Linux.
* **Bij te werken computers**: selecteer de virtuele machines die u wilt bijwerken.

  ![De virtuele machines die u wilt bijwerken selecteren](./media/manage-update-multi/update-select-computers.png)

* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. De classificatietypen zijn:
  * Essentiële updates
  * Beveiligingsupdates
  * Updatepakketten
  * Functiepakketten
  * Servicepacks
  * Definitie-updates
  * Hulpprogramma's
  * Updates
* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven.
   U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Klik op de optie Terugkerend onder Terugkeerpatroon om een terugkerende planning in te stellen.

   ![Scherm met instellingen voor de updateplanning](./media/manage-update-multi/update-set-schedule.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden.  Dit zorgt ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde servicevensters.

Nadat u klaar bent met het configureren van de planning, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat het tabblad **Gepland** de implementatieplanning die u zojuist hebt gemaakt weergeeft.

> [!WARNING]
> Voor updates waarvoor opnieuw moet worden opgestart, wordt de virtuele machine automatisch opnieuw opgestart.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status **Gedeeltelijk mislukt**.

![Status van update-implementatie ](./media/manage-update-multi/update-view-results.png)

Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

In de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* Niet geprobeerd: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van het opgegeven onderhoudsvenster.
* Geslaagd: de update is voltooid
* Mislukt: de update is mislukt

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheren van de implementatie van de update op de virtuele doelmachine.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="next-steps"></a>Volgende stappen

* Zie [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) voor meer informatie over Updatebeheer.