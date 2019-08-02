---
title: Meer informatie over het voorbereiden van Updatebeheer-, Wijzigingen bijhouden-en inventaris oplossingen voor meerdere Vm's in Azure Automation
description: Meer informatie over het voorbereiden van een virtuele machine van Azure met Updatebeheer, Wijzigingen bijhouden en inventaris oplossingen die deel uitmaken van Azure Automation
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/11/2019
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: cf063311e5559ddf5706df397ce744a726610000
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667347"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Oplossingen voor Updatebeheer, Wijzigingen bijhouden en inventaris inschakelen op meerdere Vm's

Azure Automation biedt oplossingen voor het beheren van beveiligings updates van het besturings systeem, het bijhouden van wijzigingen en het inventariseren van wat op uw computers is geïnstalleerd. Er zijn meerdere manieren om computers vrij te maken, u kunt de oplossing [van een virtuele machine](automation-onboard-solutions-from-vm.md), van uw Automation- [account](automation-onboard-solutions-from-automation-account.md), voor het bladeren door virtuele machines, of per [runbook](automation-onboard-solutions.md), uitvoeren. In dit artikel wordt beschreven hoe u deze oplossingen voorbereidt tijdens het bladeren door virtuele machines in Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij Azure op https://portal.azure.com

## <a name="enable-solutions"></a>Oplossingen inschakelen

Ga in het Azure Portal naar **virtuele machines**.

Gebruik de selectie vakjes om de virtuele machines te selecteren die u wilt vrijgeven met Wijzigingen bijhouden en inventaris of Updatebeheer. Onboarding is beschikbaar voor Maxi maal drie verschillende resource groepen tegelijk. Azure-Vm's kunnen in elke regio bestaan, ongeacht de locatie van uw Automation-account.

![Lijst met Vm's](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Gebruik de filter besturings elementen om de lijst met virtuele machines te wijzigen en klik vervolgens op het selectie vakje aan de bovenkant om alle virtuele machines in de lijst te selecteren.

Klik op de opdracht balk op **Services** en selecteer **Wijzigingen bijhouden**, **inventaris**of **updatebeheer**.

> [!NOTE]
> **Wijzigingen bijhouden** en **inventarisatie** gebruiken dezelfde oplossing, wanneer er een is ingeschakeld, de andere is ingeschakeld.

De volgende afbeelding is voor Updatebeheer. Wijzigingen bijhouden en inventaris hebben dezelfde indeling en hetzelfde gedrag.

De lijst met virtuele machines wordt gefilterd om alleen de virtuele machines weer te geven die zich in hetzelfde abonnement en dezelfde locatie bevinden. Als uw virtuele machines zich in meer dan drie resource groepen bevinden, worden de eerste drie resource groepen geselecteerd.

### <a name="resource-group-limit"></a>Beperkingen voor onboarding

Het aantal resource groepen dat u voor onboarding kunt gebruiken, wordt beperkt door de [implementatie limieten van Resource Manager](../azure-resource-manager/resource-manager-cross-resource-group-deployment.md). Implementaties van Resource Manager, niet te verwarren met update-implementaties, zijn beperkt tot 5 Resource groepen per implementatie. 2 van deze resource groepen zijn gereserveerd voor het configureren van de Log Analytics-werk ruimte, het Automation-account en gerelateerde resources om de integriteit van de onboarding te garanderen. U kunt drie resource groepen selecteren voor implementatie.

U kunt ook een runbook gebruiken voor onboarding. Zie voor meer informatie [onboard update en oplossingen voor het bijhouden van wijzigingen in azure Automation](automation-onboard-solutions.md).

Gebruik de filter besturings elementen om virtuele machines te selecteren op basis van verschillende abonnementen, locaties en resource groepen.

![Oplossing voor update beheer voor onboarding](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Bekijk de opties voor de Log Analytics-werk ruimte en het Automation-account. Een bestaande werk ruimte en een Automation-account zijn standaard geselecteerd. Als u een andere Log Analytics-werk ruimte en een Automation-account wilt gebruiken, klikt u op **aangepast** om ze te selecteren op de pagina **aangepaste configuratie** . Wanneer u een werk ruimte voor Log Analytics kiest, wordt er een controle uitgevoerd om te bepalen of deze is gekoppeld aan een Automation-account. Als er een gekoppeld Automation-account wordt gevonden, wordt het volgende scherm weer gegeven. Klik op **OK**wanneer u klaar bent.

![Werk ruimte en account selecteren](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Als de geselecteerde werk ruimte niet is gekoppeld aan een Automation-account, wordt het volgende scherm weer gegeven. Selecteer een Automation-account en klik op **OK** wanneer u klaar bent.

![Geen werk ruimte](media/automation-onboard-solutions-from-browse/no-workspace.png)

> [!NOTE]
> Bij het inschakelen van oplossingen worden slechts bepaalde regio's ondersteund voor het koppelen van een Log Analytics-werkruimte aan een Automation-Account.
>
> Zie [regio toewijzing voor Automation-account en log Analytics-werk ruimte](how-to/region-mappings.md)voor een lijst met de ondersteunde toewijzings paren.

Schakel het selectie vakje naast een wille keurige virtuele machine die u niet wilt inschakelen uit. De selectie van virtuele machines die niet kunnen worden ingeschakeld, is al opheffen.

Klik op **inschakelen** om de oplossing in te scha kelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

## <a name="unlink-workspace"></a>Werk ruimte ontkoppelen

De volgende oplossingen zijn afhankelijk van een Log Analytics-werk ruimte:

* [Updatebeheer](automation-update-management.md)
* [Tracering wijzigen](automation-change-tracking.md)
* [Vm's starten/stoppen buiten kantoor uren](automation-solution-vm-management.md)

Als u besluit dat u uw Automation-account niet meer wilt integreren met een Log Analytics-werk ruimte, kunt u uw account rechtstreeks van de Azure Portal ontkoppelen. Voordat u doorgaat, moet u eerst de eerder genoemde oplossingen verwijderen, anders kan dit proces niet worden voortgezet. Raadpleeg het artikel voor de specifieke oplossing die u hebt geïmporteerd om inzicht te krijgen in de stappen die nodig zijn om deze te verwijderen.

Nadat u deze oplossingen hebt verwijderd, kunt u de volgende stappen uitvoeren om het Automation-account te ontkoppelen.

> [!NOTE]
> Sommige oplossingen, waaronder eerdere versies van de Azure SQL-bewakings oplossing, hebben mogelijk Automation-assets gemaakt en moeten mogelijk ook worden verwijderd voordat u de werk ruimte ontkoppelt.

1. Open uw Automation-account vanuit het Azure Portal en selecteer op de pagina Automation-account de optie **gekoppelde werk ruimte** onder de sectie **Verwante resources** aan de linkerkant.

2. Klik op de pagina werk ruimte ontkoppelen op **werk ruimte ontkoppelen**.

   ![Pagina werk ruimte ontkoppelen](media/automation-onboard-solutions-from-browse/automation-unlink-workspace-blade.png).

   U ontvangt een prompt waarin u wordt gevraagd of u wilt doorgaan.

3. Terwijl Azure Automation probeert het account te ontkoppelen van uw Log Analytics-werk ruimte, kunt u de voortgang bijhouden onder **meldingen** in het menu.

Als u de oplossing Updatebeheer hebt gebruikt, kunt u eventueel de volgende items verwijderen die niet meer nodig zijn nadat u de oplossing hebt verwijderd.

* Update schema's: elk heeft een naam die overeenkomt met de update-implementaties die u hebt gemaakt.

* Hybrid worker-groepen gemaakt voor de oplossing: elke groep krijgt dezelfde naam als machine1. contoso. com _9ceb8108-26c9-4051-b6b3-227600d715c8).

Als u de oplossing Vm's starten/stoppen buiten kantoor uren hebt gebruikt, wilt u eventueel mogelijk de volgende items verwijderen die niet meer nodig zijn nadat u de oplossing hebt verwijderd.

* VM-runbook-schema's starten en stoppen
* VM-runbooks starten en stoppen
* Variabelen

U kunt ook uw werk ruimte ontkoppelen van uw Automation-account vanuit uw Log Analytics-werk ruimte. Selecteer in uw werk ruimte **Automation-account** onder **gerelateerde resources**. Selecteer op de pagina Automation-account de optie **account loskoppelen**.

## <a name="troubleshooting"></a>Problemen oplossen

Bij het onboarden van meerdere machines zijn er mogelijk computers die **niet**kunnen worden ingeschakeld. Er zijn verschillende redenen waarom sommige computers niet kunnen worden ingeschakeld. In de volgende secties worden mogelijke oorzaken voor de status **kan niet inschakelen** op een virtuele machine worden weer gegeven wanneer u probeert onboarding uit te voeren.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM-rapporten naar een andere werk ruimte\<: '\>workspacenaam '.  Configuratie wijzigen om deze functie in te scha kelen

**Oorzaak**: Deze fout geeft aan dat de virtuele machine die u probeert uit te voeren rapporteert aan een andere werk ruimte.

**Oplossing**: Klik op **als configuratie gebruiken** om het beoogde Automation-account en de log Analytics-werk ruimte te wijzigen.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>VM-rapporten naar een werk ruimte die niet beschikbaar is in dit abonnement

**Oorzaak**: De werk ruimte waarnaar de virtuele machine rapporteert:

* Bevindt zich in een ander abonnement of
* Bestaat niet meer of
* Bevindt zich in een resource groep waarvoor u geen toegangs machtigingen hebt

**Oplossing**: Zoek het Automation-account dat is gekoppeld aan de werk ruimte die door de VM wordt gerapporteerd aan en onboarding van de virtuele machine door de scope configuratie te wijzigen.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>De versie of distributie van het VM-besturings systeem wordt niet ondersteund

**Wordt** De oplossing wordt niet ondersteund voor alle Linux-distributies of alle versies van Windows.

**Oplossen** Raadpleeg de [lijst met ondersteunde clients](automation-update-management.md#clients) voor de oplossing.

### <a name="classic-vms-cannot-be-enabled"></a>Klassieke virtuele machines kunnen niet worden ingeschakeld

**Oorzaak**: Virtuele machines die gebruikmaken van het klassieke implementatie model, worden niet ondersteund.

**Oplossing**: Migreer de virtuele machine naar het Resource Manager-implementatie model. Zie [klassieke implementatie model bronnen migreren](../virtual-machines/windows/migration-classic-resource-manager-overview.md)voor meer informatie over hoe u dit doet.

### <a name="vm-is-stopped-deallocated"></a>De VM is gestopt. deallocated

**Oorzaak**: De virtuele machine **wordt niet uitgevoerd** .

**Oplossing**: Als u een virtuele machine wilt voorbereiden op een oplossing, moet de virtuele machine worden uitgevoerd. Klik op de koppeling VM-inline **starten** om de VM te starten zonder op de pagina te navigeren.

## <a name="next-steps"></a>Volgende stappen

Nu de oplossing is ingeschakeld voor uw virtuele machines, gaat u naar het artikel overzicht van Updatebeheer voor meer informatie over het weer geven van de update-evaluatie voor uw machines.

> [!div class="nextstepaction"]
> [Updatebeheer-update-evaluatie weer geven](./automation-update-management.md#viewing-update-assessments)

Aanvullende zelf studies voor de oplossingen en hoe u deze kunt gebruiken:

* [Zelf studie-updates voor uw virtuele machine beheren](automation-tutorial-update-management.md)

* [Zelf studie-software op een virtuele machine identificeren](automation-tutorial-installed-software.md)

* [Zelf studie: problemen met wijzigingen in een virtuele machine oplossen](automation-tutorial-troubleshoot-changes.md)
