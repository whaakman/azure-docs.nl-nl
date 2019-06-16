---
title: Azure Automation en Log Analytics-werkruimte toewijzingen
description: In dit artikel beschrijft de toewijzingen tussen een Automation-Account en een Log Analytics-werkruimte kunnen ondersteuning bieden voor oplossing
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8afe8fed8912dd365071f1c4c5560c9f5578dcd8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66133106"
---
# <a name="workspace-mappings"></a>Toewijzingen van werkruimte

Bij het inschakelen van oplossingen zoals updatebeheer, wijzigingen bijhouden en inventaris of de VM's starten/stoppen buiten kantooruren oplossing, worden alleen bepaalde regio's worden ondersteund voor het koppelen van een Log Analytics-werkruimte en een Automation-Account. Deze toewijzing is alleen van toepassing op het Automation-Account en de Log Analytics-werkruimte. De resources die rapporteren aan uw Automation-Account of Log Analytics-werkruimte kunnen zich in andere regio's bevinden.

## <a name="supported-mappings"></a>Ondersteunde toewijzingen

De volgende tabel bevat de ondersteunde toewijzingen:

|**Log Analytics-werkruimte regio**|**Azure Automation Region**|
|---|---|
|**VS**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Canada**||
|CanadaCentral|CanadaCentral|
|**Azië en Stille Oceaan**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|West-Europa|West-Europa|
|**VS (overheid)**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> VS-Oost-toewijzing voor Log Analytics-werkruimten op Automation-Accounts is niet een exacte toewijzing van regio naar regio, maar is de juiste toewijzing.

<sup>2</sup> vanwege beperkingen van de capaciteit van de regio niet beschikbaar is bij het maken van nieuwe resources. Dit geldt ook voor Automation-Accounts en Log Analytics-werkruimten. Bestaande gekoppelde resources in de regio moeten echter nog steeds werken.

## <a name="unlink-workspace"></a>Werkruimte ontkoppelen

Als u besluit dat u niet meer wilt integreren van uw Automation-account met een Log Analytics-werkruimte, kunt u uw account rechtstreeks vanuit de Azure portal kunt ontkoppelen. Voordat u doorgaat, moet u eerst het updatebeheer, wijzigingen bijhouden en inventaris of de VM's starten/stoppen tijdens buiten kantooruren oplossingen verwijderen als u deze gebruikt. Als u ze niet verwijdert, zal dit proces worden voorkomen dat u doorgaat. Lees het artikel voor de desbetreffende oplossing die u hebt geïmporteerd voor meer informatie over de stappen die nodig zijn om deze te verwijderen.

Nadat u deze oplossingen hebt verwijderd, kunt u de volgende stappen uit als u wilt loskoppelen van uw Automation-account kunt uitvoeren.

> [!NOTE]
> Sommige oplossingen met inbegrip van eerdere versies van de Azure SQL-oplossing voor controle mogelijk gemaakt automatiseringsactiva en moet mogelijk ook worden verwijderd voordat het ontkoppelen van de werkruimte.

1. Open uw Automation-account vanuit Azure portal, en op de Automation-account selecteren pagina **gekoppelde werkruimte** onder de sectie **gerelateerde Resources** aan de linkerkant.

2. Klik op de pagina van de werkruimte ontkoppelen **werkruimte ontkoppelen**. U zult ontvangt een prompt waarin dat u wilt doorgaan.

3. Terwijl Azure Automation probeert te ontkoppelen van het account uw Log Analytics-werkruimte, u kunt de voortgang volgen onder **meldingen** in het menu.

Als u de oplossing Update Management gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Plant u update - elk hebben namen die overeenkomen met de update-implementaties die u hebt gemaakt)

* Hybrid worker-groepen gemaakt voor de oplossing - elke naam op dezelfde manier naar machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Als u de VM's starten/stoppen buiten kantooruren oplossing gebruikt, kunt indien gewenst u verwijderen van de volgende items die niet meer nodig zijn nadat u de oplossing te verwijderen.

* Starten en stoppen van VM runbook schema 's
* VM-runbooks starten en stoppen
* Variabelen

U kunt ook ook uw werkruimte ontkoppelen van uw Automation-Account van uw Log Analytics-werkruimte. Selecteer in de werkruimte **Automation-Account** onder **gerelateerde Resources**. Selecteer op de pagina Automation-Account **-account loskoppelen**.

## <a name="next-steps"></a>Volgende stappen

Informatie over hoe moet worden vrijgegeven de volgende oplossingen:

Update Management en wijzigingen bijhouden en inventaris:

* Uit een [virtuele machine](../automation-onboard-solutions-from-vm.md)
* Van uw [Automation-account](../automation-onboard-solutions-from-automation-account.md)
* Wanneer [bladeren door meerdere virtuele machines](../automation-onboard-solutions-from-browse.md)
* Uit een [runbook](../automation-onboard-solutions.md)

VM's starten/stoppen buiten kantooruren

* [Implementeren van VM's starten/stoppen buiten kantooruren](../automation-solution-vm-management.md)