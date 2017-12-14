---
title: JSON-indeling codes gebruiken om de status van de virtuele machine van Azure plannen | Microsoft Docs
description: In dit artikel laat zien hoe u van tekenreeksen voor JSON-tags voor het automatiseren van de planning van de virtuele machine opstarten en afsluiten.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: 9855921f4a3aa9cda8497b400d50a186d7162dc3
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure Automation-scenario: labels JSON-indeling gebruiken voor het maken van een planning voor de virtuele machine van Azure opstarten en afsluiten
Klanten willen vaak het opstarten en afsluiten van de virtuele machines om abonnement kosten te verlagen of ondersteuning voor bedrijfs- en technische vereisten te plannen.

Het volgende scenario kunt u voor het instellen van geautomatiseerde opstarten en afsluiten van uw virtuele machines met behulp van een label aangeroepen planning op een niveau van de resourcegroep of het niveau van de virtuele machine in Azure. Dit schema kan worden geconfigureerd van zondag tot zaterdag een opstarten en afsluiten.

We hebt enkele out-of-the-box-opties. Deze omvatten:

* [Virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) met automatisch schalen instellingen waarmee u in of uit te schalen.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) service met de ingebouwde mogelijkheden van de planning van bewerkingen voor opstarten en afsluiten.

Deze opties ondersteunen echter alleen specifieke scenario's en infrastructuur-as-a-service (IaaS) virtuele machines kan niet worden toegepast.

Wanneer het schema-label wordt toegepast op een resourcegroep, wordt deze ook toegepast op alle virtuele machines in die resourcegroep. Als een schema ook rechtstreeks op een virtuele machine is toegepast, hebben de laatste planning voorrang in de volgende volgorde:

1. Planning toegepast op een resourcegroep
2. Planning toegepast op een resourcegroep en de virtuele machine in de resourcegroep
3. Planning toegepast op een virtuele machine

In dit scenario wordt in wezen een JSON-tekenreeks met de opgegeven notatie en toegevoegd als de waarde voor een tag schema genoemd. Een runbook wordt vervolgens een lijst met alle resourcegroepen en virtuele machines en identificeert de planningen voor elke virtuele machine op basis van de scenario's met de eerder vermelde. Naast het doorlopen van de virtuele machines met's die zijn gekoppeld en evalueert welke actie moet worden gehouden. Bijvoorbeeld, bepaalt het welke virtuele machines moeten worden gestopt, afsluiten of genegeerd.

Deze runbooks verifiëren met behulp van de [Azure uitvoeren als-account](automation-sec-configure-azure-runas-account.md).

## <a name="download-the-runbooks-for-the-scenario"></a>Downloaden van de runbooks voor het scenario
Dit scenario bestaat uit vier PowerShell Workflow-runbooks die u van downloaden kunt de [TechNet-galerie](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) of de [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) opslagplaats voor dit project.

| Runbook | Beschrijving |
| --- | --- |
| Test ResourceSchedule |Controleert de planning van elke virtuele machine en wordt afgesloten of opgestart, afhankelijk van de planning wordt uitgevoerd. |
| Voeg ResourceSchedule |Voegt de schema-tag toe aan een groep VM of de resource. |
| Update ResourceSchedule |Hiermee wijzigt u de tag planning door deze te vervangen door een nieuwe. |
| Verwijder ResourceSchedule |Hiermee verwijdert u de schema-code uit een groep VM of de resource. |

## <a name="install-and-configure-this-scenario"></a>Dit scenario installeren en configureren
### <a name="install-and-publish-the-runbooks"></a>De runbooks installeren en publiceren
Na het downloaden van de runbooks, kunt u deze importeren met behulp van de procedure in [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Elk runbook gepubliceerd nadat deze is geïmporteerd in uw Automation-account.

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a>Toevoegen van een planning aan het runbook testen ResourceSchedule
Volg deze stappen voor de planning voor het runbook testen ResourceSchedule inschakelt. Dit is het runbook dat controleert of welke virtuele machines moet worden gestart, afsluiten of ongewijzigd worden gelaten.

1. Open uw Automation-account van de Azure-portal en klik vervolgens op de **Runbooks** tegel.
2. Op de **Test ResourceSchedule** blade, klikt u op de **planningen** tegel.
3. Op de **planningen** blade, klikt u op **toevoegen van een planning**.
4. Op de **planningen** blade Selecteer **een planning aan uw runbook koppelen**. Selecteer vervolgens **Maak een nieuwe planning**.
5. Op de **nieuwe planning** blade, typ de naam van dit schema, bijvoorbeeld: *HourlyExecution*.
6. Voor de planning **Start**, de starttijd instellen op een reeks uur.
7. Selecteer **terugkeerpatroon**, en vervolgens voor **herhaald elke interval**, selecteer **1 uur**.
8. Controleer **instellen dat verlopen** is ingesteld op **Nee**, en klik vervolgens op **maken** om op te slaan van het nieuwe schema.
9. Op de **planning Runbook** Selecteer opties blade **Parameters en uitvoerinstellingen**. In de Test-ResourceSchedule **Parameters** blade, voer de naam van uw abonnement in de **SubscriptionName** veld.  Dit is de enige parameter die zijn voor het runbook vereist.  Wanneer u klaar bent, klikt u op **OK**.

De planning van het runbook moet eruitzien als in het volgende wanneer dit voltooid:

![Geconfigureerde Test ResourceSchedule runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a>De JSON-tekenreeks voor opmaak
Deze oplossing in feite duurt een JSON-tekenreeks met de opgegeven notatie en toegevoegd als de waarde voor een label aangeroepen planning. Een runbook wordt vervolgens een lijst met alle resourcegroepen en virtuele machines en identificeert de planningen voor elke virtuele machine.

Het runbook wordt via de virtuele machines met's die zijn gekoppeld en controleert welke acties moeten worden genomen. Hier volgt een voorbeeld van hoe de oplossingen moeten worden opgemaakt:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Hier volgt een aantal gedetailleerde informatie over deze structuur:

1. De indeling van dit JSON-structuur is geoptimaliseerd voor het omzeilen van de beperking van 256 tekens van de waarde van een enkel label in Azure.
2. *TzId* vertegenwoordigt de tijdzone van de virtuele machine. Deze ID kan worden verkregen met behulp van de TimeZoneInfo .NET-klasse in een PowerShell-sessie--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![GetSystemTimeZones in PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Weekdagen worden aangeduid met een numerieke waarde van nul tot en met zes. De waarde nul is gelijk aan zondag.
   * De begintijd wordt weergegeven met de **S** kenmerk en de waarde ervan zich in een 24-uurs notatie.
   * De tijd worden afgesloten of end wordt weergegeven met de **E** kenmerk en de waarde ervan zich in een 24-uurs notatie.

     Als de **S** en **E** kenmerken elke hebben een waarde van nul (0), de virtuele machine blijft in zijn huidige staat op het moment van evaluatie.
3. Als u overslaan van de evaluatie voor een specifieke dag van de week wilt, niet een sectie toevoegen voor die dag van de week. In het volgende voorbeeld wordt alleen maandag wordt geëvalueerd en worden genegeerd, de andere dagen van de week:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Tag-resourcegroepen of VM 's
Als u wilt afsluiten virtuele machines, moet u code van de virtuele machines of de resourcegroepen waar ze zich bevinden. Virtuele machines waarvoor geen een label voor de planning worden niet geëvalueerd. Ze zijn niet daarom gestart of afgesloten.

Er zijn twee manieren aan resourcegroepen label of VM's met deze oplossing. U kunt dit doen rechtstreeks vanuit de portal. Of u kunt de toevoegen ResourceSchedule, Update-ResourceSchedule en verwijder ResourceSchedule runbooks.

### <a name="tag-through-the-portal"></a>Tag via de portal
Volg deze stappen voor het labelen van een virtuele machine of de resourcegroep op de portal:

1. De JSON-tekenreeks plat en controleer of er geen spaties.  Uw JSON-tekenreeks moet er als volgt uitzien:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Selecteer de **Tag** pictogram voor een groep VM of de resource toe te passen van dit schema.

   ![Optie voor VM-tag](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Labels zijn gedefinieerd een sleutel-waardepaar te volgen. Type **planning** in de **sleutel** veld en plak de JSON-tekenreeks in de **waarde** veld. Klik op **Opslaan**. Uw nieuwe code wordt nu weergegeven in de lijst met labels voor uw resource.

   ![VM planning label](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>Label van PowerShell
Alle geïmporteerde runbooks bevatten help-informatie aan het begin van het script dat wordt beschreven hoe u de runbooks rechtstreeks vanuit PowerShell uitvoeren. U kunt de toevoegen ScheduleResource en Update ScheduleResource runbooks aanroepen vanuit PowerShell. U doen dit door de vereiste parameters waarmee u kunt maken of bijwerken van de schema-code op een groep VM of de resource buiten de portal wordt doorgegeven.

Als u wilt maken, toevoegen en verwijderen van de labels via PowerShell, moet u eerst [uw PowerShell-omgeving instellen voor Azure](/powershell/azure/overview). Nadat u de installatie hebt voltooid, kunt u doorgaan met de volgende stappen.

### <a name="create-a-schedule-tag-with-powershell"></a>Een label schema maken met PowerShell
1. Open een PowerShell-sessie. Gebruik vervolgens het volgende voorbeeld om te verifiëren met uw uitvoeren als-account en om op te geven van een abonnement:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definieer de planning hash-tabel. Hier volgt een voorbeeld van hoe moet worden opgebouwd:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Definieer de parameters die voor het runbook vereist zijn. In het volgende voorbeeld wordt een virtuele machine ontwikkelt:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Als u een resourcegroep bent tagging, verwijdert u de *VMName* parameter van de hash $params tabel als volgt:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Voer het runbook toevoegen ResourceSchedule met de volgende parameters om de schema-tag te maken:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. Voor het bijwerken van een resourcegroep of code van de virtuele machine uitvoeren van de **Update ResourceSchedule** runbook met de volgende parameters:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>Verwijderen van een label planning met PowerShell
1. Open een PowerShell-sessie en voer de volgende om te verifiëren met uw uitvoeren als-account en te selecteren en een abonnement opgeven:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definieer de parameters die voor het runbook vereist zijn. In het volgende voorbeeld wordt een virtuele machine ontwikkelt:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Als u een label van een resourcegroep verwijderen wilt, verwijdert u de *VMName* parameter van de hash $params tabel als volgt:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Uitvoeren van het runbook verwijderen ResourceSchedule om de code van de planning te verwijderen:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. Uitvoeren voor het bijwerken van een resourcegroep of code van de virtuele machine, het runbook verwijderen ResourceSchedule met de volgende parameters:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> We raden aan dat u proactief bewaken deze runbooks (en de status van de virtuele machines) om te controleren of uw virtuele machines wordt afgesloten omlaag en dienovereenkomstig gestart.
>

Om de details van de Test-ResourceSchedule runbooktaak weergeven in de Azure portal, selecteer de **taken** tegel van het runbook. Het taakoverzicht geeft de invoerparameters en de uitvoerstroom weer, naast algemene informatie over de taak en eventuele uitzonderingen.

Het **Taakoverzicht** bevat berichten van de uitvoer-, waarschuwings- en foutstromen. Selecteer de tegel **Uitvoer** om gedetailleerde resultaten van de uitvoering van het runbook weer te geven.

![Test ResourceSchedule uitvoer](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie voor meer informatie over runbooktypen, en hun voordelen en beperkingen, [Azure Automation-runbooktypen](automation-runbook-types.md).
* Zie voor meer informatie over PowerShell-script ondersteuningsfuncties [systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Zie voor meer informatie over logboekregistratie van runbook- en uitvoer, [Runbook uitvoer en berichten in Azure Automation](automation-runbook-output-and-messages.md).
* Zie voor meer informatie over een Azure uitvoeren als-account en uw runbooks verifiëren met behulp van deze [runbooks met Azure uitvoeren als-account verifiëren](automation-sec-configure-azure-runas-account.md).
