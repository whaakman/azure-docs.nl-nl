---
title: Probleemoplossing starten en stoppen van virtuele machines met Azure Automation
description: Dit artikel bevat informatie over het oplossen van starten en stoppen van virtuele machines in Azure Automation
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/13/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f503d890dcc8ba90a8a4d8bafc09d5fd8b2856e6
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804850"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Het starten/stoppen van VM's uit uur oplossing oplossen

## <a name="deployment-failure"></a>Scenario: Het starten/stoppen van VM-oplossing niet goed implementeren

### <a name="issue"></a>Probleem

Bij het implementeren van de [starten/stoppen van VM's uit uur oplossing](../automation-solution-vm-management.md), ontvangt u een van de volgende fouten:

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]”.
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

### <a name="cause"></a>Oorzaak

Implementaties mislukken mogelijk vanwege een van de volgende redenen:

1. Er bestaat al een Automation-Account met dezelfde naam in de geselecteerde regio.
2. Een beleid is dat de implementatie van de oplossing voor VM's starten/stoppen is niet toegestaan.
3. De `Microsoft.OperationsManagement`, `Microsoft.Insights`, of `Microsoft.Automation` resourcetypen niet zijn geregistreerd.
4. Uw Log Analytics-werkruimte heeft een vergrendeld.

### <a name="resolution"></a>Oplossing

Bekijk de volgende lijst voor mogelijke oplossingen voor uw probleem of plaatsen om te zoeken:

1. Automation-accounts moeten uniek zijn binnen een Azure-regio, zelfs als ze zich in verschillende resourcegroepen bevinden. Controleer uw bestaande Automation-Accounts in de doelregio.
2. Een bestaand beleid voorkomt dat een resource die is vereist voor het starten/stoppen van VM-oplossing worden geïmplementeerd. Ga naar de toewijzingen van beleid in Azure portal en controleer of u beschikt over een beleidstoewijzing die de implementatie van deze resource is niet toegestaan. Zie voor meer informatie over deze [RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md).
3. Als u wilt de starten/stoppen van VM-oplossing implementeert, wordt uw abonnement moet kunnen worden geregistreerd in de volgende Azure-resource-naamruimten:
    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Zie, [oplossen voor registratie van de resourceprovider](../../azure-resource-manager/resource-manager-register-provider-errors.md) voor meer informatie over fouten bij het registreren van providers.
4. Als u een vergrendeling in uw Log Analytics-werkruimte hebt, gaat u naar uw werkruimte in de Azure-portal en verwijder alle vergrendelingen op de bron.

## <a name="all-vms-fail-to-startstop"></a>Scenario: Niet alle virtuele machines starten/stoppen

### <a name="issue"></a>Probleem

U kunt de virtuele machine starten/stoppen-oplossing hebt geconfigureerd, maar deze niet starten of stoppen van alle virtuele machines die zijn geconfigureerd.

### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door een van de volgende redenen:

1. Een schema is niet correct geconfigureerd
2. Het RunAs-account niet correct geconfigureerd
3. Een runbook hebt tegenkomen in fouten
4. De virtuele machines zijn uitgesloten

### <a name="resolution"></a>Oplossing

Bekijk de volgende lijst voor mogelijke oplossingen voor uw probleem of plaatsen om te zoeken:

* Controleer of u een schema voor het starten/stoppen van VM-oplossing goed hebt geconfigureerd. Zie voor informatie over het configureren van een schema, de [planningen](../automation-schedules.md) artikel.

* Controleer de taakstromen voor de runbooks om te zoeken naar eventuele fouten. In de portal, gaat u naar uw Automation-Account en selecteer **taken** onder **procesautomatisering**. Uit de **taken** uiterlijk van de pagina voor de taken van een van de volgende runbooks:

  * AutoStop_CreateAlert_Child
  * AutoStop_CreateAlert_Parent
  * AutoStop_Disable
  * AutoStop_VM_Child
  * ScheduledStartStop_Base_Classic
  * ScheduledStartStop_Child_Classic
  * ScheduledStartStop_Child
  * ScheduledStartStop_Parent
  * SequencedStartStop_Parent

* Controleer of uw [RunAs-Account](../manage-runas-account.md) juiste toegangsmachtigingen voor de virtuele machines die u probeert te starten of stoppen. Zie voor informatie over het controleren van de machtigingen voor een resource, [Quick Start: Rollen die zijn toegewezen aan een gebruiker met behulp van de Azure-portal weergeven](../../role-based-access-control/check-access.md). U moet voor de toepassings-Id voor de service-principal die worden gebruikt door het uitvoeren als-Account. U kunt deze waarde wordt opgehaald door te gaan naar uw Automation-Account in Azure portal, selecteren **Run as-accounts** onder **Accountinstellingen** en te klikken op de juiste uitvoeren als-Account.

* Virtuele machines kunnen niet worden gestart of gestopt als ze expliciet bent wordt uitgesloten. Virtuele machines uitgesloten op set in de **External_ExcludeVMNames** variabele in het Automation-Account de oplossing wordt geïmplementeerd op. Het volgende voorbeeld laat zien hoe u query's uitvoeren die waarde met PowerShell.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="some-vms-fail-to-startstop"></a>Scenario: Sommige van mijn VM's niet starten of stoppen

### <a name="issue"></a>Probleem

U kunt de virtuele machine starten/stoppen-oplossing hebt geconfigureerd, maar deze niet starten of stoppen van sommige van de VM's geconfigureerd.

### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door een van de volgende redenen:

1. Als de sequence-scenario, kan een label worden ontbreekt of is onjuist
2. De virtuele machine kan worden uitgesloten
3. Het RunAs-account mogelijk niet voldoende machtigingen op de virtuele machine
4. De virtuele machine kan hebben iets dat het starten of stoppen is gestopt

### <a name="resolution"></a>Oplossing

Bekijk de volgende lijst voor mogelijke oplossingen voor uw probleem of plaatsen om te zoeken:

* Wanneer u de [reeks scenario](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags) van de VM starten/stoppen tijdens uitschakelen uur oplossing, moet u ervoor zorgen elke virtuele machine die u wilt starten en stoppen, is de juiste code. Zorg ervoor dat de virtuele machines die u wilt beginnen zijn de `sequencestart` tag en de virtuele machines die u wilt stoppen hebben de `sequencestop` tag. Beide tags moeten een positief geheel getal. U kunt een query die vergelijkbaar is met het volgende voorbeeld gebruiken om te zoeken naar alle virtuele machines met de labels en de bijbehorende waarden.

  ```powershell-interactive
  Get-AzureRmResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* Virtuele machines kunnen niet worden gestart of gestopt als ze expliciet bent wordt uitgesloten. Virtuele machines uitgesloten op set in de **External_ExcludeVMNames** variabele in het Automation-Account de oplossing wordt geïmplementeerd op. Het volgende voorbeeld laat zien hoe u query's uitvoeren die waarde met PowerShell.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Om te starten en stoppen van VM's, moet het RunAs-account voor het Automation-account de juiste machtigingen voor de virtuele machine hebben. Zie voor informatie over het controleren van de machtigingen voor een resource, [Quick Start: Rollen die zijn toegewezen aan een gebruiker met behulp van de Azure-portal weergeven](../../role-based-access-control/check-access.md). U moet voor de toepassings-Id voor de service-principal die worden gebruikt door het uitvoeren als-Account. U kunt deze waarde wordt opgehaald door te gaan naar uw Automation-Account in Azure portal, selecteren **Run as-accounts** onder **Accountinstellingen** en te klikken op de juiste uitvoeren als-Account.

* Als de virtuele machine is een probleem opgetreden bij het starten of de toewijzing ongedaan maken, kan dit probleem worden veroorzaakt door een probleem op de virtuele machine zelf. Sommige voorbeelden of potentiële problemen zijn, een update wordt toegepast wanneer u wilt afsluiten, een service vastloopt, en meer). Navigeer naar uw VM-resource en controleer de **activiteitenlogboeken** om te zien of er fouten zijn opgetreden in de logboeken. U kunt ook proberen om aan te melden bij de VM om te zien of er fouten zijn opgetreden in de gebeurtenislogboeken. Zie voor meer informatie over het oplossen van uw virtuele machine, [probleemoplossing voor Azure virtual machines](../../virtual-machines/troubleshooting/index.md)

## <a name="custom-runbook"></a>Scenario: Mijn aangepaste runbook kan niet worden mijn VMs starten en stoppen

### <a name="issue"></a>Probleem

U hebt een aangepaste runbook gemaakt of gedownload vanuit de PowerShell Gallery en niet goed werkt.

### <a name="cause"></a>Oorzaak

De oorzaak van de fout wordt mogelijk een van de vele dingen. Ga naar uw Automation-Account in Azure portal en selecteer **taken** onder **procesautomatisering**. Uit de **taken** pagina, gaat u naar taken in het runbook, om eventuele taakmislukkingen weer te geven.

### <a name="resolution"></a>Oplossing

Het is raadzaam om te gebruiken de [starten/stoppen van VM's uit uur oplossing](../automation-solution-vm-management.md) starten en stoppen van virtuele machines in Azure Automation. Deze oplossing is gemaakt door Microsoft. Aangepaste runbooks worden niet ondersteund door Microsoft. U kunt een oplossing voor uw aangepaste runbook vinden door naar de pagina de [runbook probleemoplossing](runbooks.md) artikel. Dit artikel bevat algemene richtlijnen en probleemoplossing voor runbooks van alle typen.

## <a name="dont-start-stop-in-sequence"></a>Scenario: Virtuele machines niet starten of stoppen in de juiste volgorde

### <a name="issue"></a>Probleem

De virtuele machines die u hebt geconfigureerd in de oplossing niet starten of stoppen in de juiste volgorde.

### <a name="cause"></a>Oorzaak

Dit wordt veroorzaakt door onjuiste codering op de virtuele machines.

### <a name="resolution"></a>Oplossing

Voer de volgende stappen uit om ervoor te zorgen dat de oplossing correct is geconfigureerd.

1. Zorg ervoor dat alle virtuele machines worden gestart of gestopt hebben een `sequencestart` of `sequencestop` tag, afhankelijk van uw situatie. Deze tags moeten een positief geheel getal als waarde. VM's worden verwerkt in oplopende volgorde op basis van deze waarde.
2. Zorg ervoor dat de resourcegroepen voor de virtuele machines worden gestart of gestopt in de `External_Start_ResourceGroupNames` of `External_Stop_ResourceGroupNames` variabelen, afhankelijk van uw situatie.
3. Uw wijzigingen testen door het uitvoeren van de `SequencedStartStop_Parent` runbook met de parameter WHATIF ingesteld op True om uw wijzigingen te bekijken.

Zie voor meer gedetailleerde en aanvullende instructies over het gebruik van de oplossing starten en stoppen van VM's in de reeks [starten/stoppen van VM's in de reeks](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags).

## <a name="403"></a>Scenario: Starten/stoppen van VM-taak is mislukt met de status van 403 verboden 

### <a name="issue"></a>Probleem

Vindt u de taken die is mislukt met een `403 forbidden` fout voor het starten/stoppen van VM's uit uur oplossing runbooks.

### <a name="cause"></a>Oorzaak

Dit probleem kan worden veroorzaakt door een onjuist geconfigureerde of verlopen Run As-Account. Dit kan ook worden vanwege onvoldoende machtigingen voor de VM-resources met de Automation-Accounts uitvoeren als-Account.

### <a name="resolution"></a>Oplossing

Om te controleren of uw uitvoeren als-account juist is geconfigureerd, gaat u naar uw Automation-Account in Azure portal en selecteer **Run as-accounts** onder **Accountinstellingen**. Hier ziet u de status van uw uitvoeren als-accounts, als een uitvoeren als-Account is onjuist geconfigureerd of verlopen de status van dit wordt weergegeven.

Als uw uitvoeren als-account is [onjuist geconfigureerde](../manage-runas-account.md#misconfiguration), u moet verwijderen en opnieuw maken van uw uitvoeren als-Account.

Volg de stappen die worden vermeld op als het certificaat voor uw uitvoeren als-Account is verlopen, [zelfondertekend certificaat vernieuwen](../manage-runas-account.md#cert-renewal) om het certificaat te vernieuwen.

Het probleem kan worden veroorzaakt door ontbrekende machtigingen. Zie voor informatie over het controleren van de machtigingen voor een resource, [Quick Start: Rollen die zijn toegewezen aan een gebruiker met behulp van de Azure-portal weergeven](../../role-based-access-control/check-access.md). U moet voor de toepassings-Id voor de service-principal die worden gebruikt door het uitvoeren als-Account. U kunt deze waarde wordt opgehaald door te gaan naar uw Automation-Account in Azure portal, selecteren **Run as-accounts** onder **Accountinstellingen** en te klikken op de juiste uitvoeren als-Account.

## <a name="other"></a>Scenario: Mijn probleem niet wordt weergegeven boven

### <a name="issue"></a>Probleem

U ondervindt een probleem of een onverwacht resultaat bij het gebruik van de VM's starten/stoppen buiten kantooruren-oplossing die niet wordt vermeld op deze pagina.

### <a name="cause"></a>Oorzaak

Aantal keren dat fouten kunnen worden veroorzaakt door een verouderde versie van de oplossing.

### <a name="resolution"></a>Oplossing

Het is raadzaam om op te lossen veel fouten, verwijderen en bijwerken van de oplossing. Zie voor informatie over het bijwerken van de oplossing, [bijwerken van het starten/stoppen van VM's uit uur oplossing](../automation-solution-vm-management.md#update-the-solution).

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
