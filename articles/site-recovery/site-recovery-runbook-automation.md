---
title: Azure Automation-runbooks toevoegen aan in de Azure Site Recovery-herstelplannen | Microsoft Docs
description: Meer informatie over hoe Azure Site Recovery kunt herstelplannen met behulp van Azure Automation uitbreiden. Informatie over het uitvoeren van complexe taken tijdens het herstel naar Azure.
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: af5de1c262bc55b1aa7513ca91b68eb50b44dbb7
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure Automation-runbooks aan herstelplannen toevoegen
In dit artikel wordt beschreven hoe Azure Site Recovery kan worden geïntegreerd met Azure Automation kunt u uw herstelplannen uitbreiden. Plannen voor herstel kunnen herstel van virtuele machines die zijn beveiligd met Site Recovery indelen. Plannen voor herstel werkt zowel voor replicatie naar een secundaire cloud, en voor replicatie naar Azure. De herstelplannen ook zorgt u ervoor dat het herstel **accuraat**, **herhaalbare**, en **geautomatiseerde**. Als u uw virtuele machines in Azure een failover, uitgebreid integratie met Azure Automation uw plannen voor herstel. U kunt het uitvoeren van runbooks die krachtige automatiseringstaken bieden.

Als u niet bekend met Azure Automation bent, kunt u [aanmelden](https://azure.microsoft.com/services/automation/) en [voorbeeldscripts downloaden](https://azure.microsoft.com/documentation/scripts/). Voor meer informatie en voor informatie over het organiseren van herstel naar Azure met behulp van [herstelplannen](https://azure.microsoft.com/blog/?p=166264), Zie [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

In dit artikel wordt beschreven hoe u Azure Automation-runbooks kunt integreren in uw plannen voor herstel. We gebruiken voorbeelden voor het automatiseren van algemene taken die eerder handmatige interventie vereist. We ook wordt beschreven hoe een herstelbewerking met meerdere stappen converteren naar een herstelactie met één klik.

## <a name="customize-the-recovery-plan"></a>Het herstelplan aanpassen
1. Ga naar de **siteherstel** recovery plan resource-blade. Bijvoorbeeld heeft het herstelplan twee virtuele machines die zijn toegevoegd voor herstel. Om te beginnen met het toevoegen van een runbook, klikt u op de **aanpassen** tabblad.

    ![Klik op de knop aanpassen](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Met de rechtermuisknop op **groep 1: Start**, en selecteer vervolgens **post actie toevoegen**.

    ![Klik met de rechtermuisknop groep 1: Start en post actie toevoegen](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Klik op **kiest u een script**.

4. Op de **bijwerken actie** blade, de naam van het script **Hallo wereld**.

    ![De Update-actie-blade](media/site-recovery-runbook-automation-new/update-rp.png)

5. Voer de naam van een Automation-account.
    >[!NOTE]
    > Het Automation-account kan zich in een Azure-regio. Het Automation-account moet zich in hetzelfde abonnement als de Azure Site Recovery-kluis.

6. Selecteer een runbook in uw Automation-account. Dit runbook is het script dat wordt uitgevoerd tijdens de uitvoering van het herstelplan na het herstel van de eerste groep.

7. Als u wilt het script opslaat, klikt u op **OK**. Het script wordt toegevoegd aan **groep 1: na stappen**.

    ![Groep na acties 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Overwegingen voor het toevoegen van een script

* Opties voor **verwijderen van een stap** of **bijwerken van het script**, met de rechtermuisknop op het script.
* Een script kunt uitvoeren in Azure tijdens de failover van een lokale machine naar Azure. Deze kunt ook uitvoeren op Azure als een primaire site script voordat wordt afgesloten, tijdens de failback vanuit Azure naar een lokale machine.
* Wanneer een script wordt uitgevoerd, injects deze de context van een herstel plan. Het volgende voorbeeld ziet u een variabele context:

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    De volgende tabel bevat de naam en beschrijving van iedere variabele in de context.

    | **Naam variabele** | **Beschrijving** |
    | --- | --- |
    | RecoveryPlanName |De naam van de planning wordt uitgevoerd. Deze variabele kunt u verschillende acties op basis van de naam voor het herstel uitvoeren. U kunt ook het script hergebruiken. |
    | FailoverType |Hiermee geeft u op of de failover een test is, gepland of ongepland. |
    | FailoverDirection |Geeft aan of herstel naar een primaire of secundaire site. |
    | Groeps-id |Identificeert het nummer van de in het herstelplan bij de planning wordt uitgevoerd. |
    | VmMap |Een matrix van alle virtuele machines in de groep. |
    | VMMap sleutel |Een unieke sleutel (GUID) voor elke virtuele machine. Dit is hetzelfde als Azure Virtual Machine Manager (VMM)-ID van de virtuele machine, indien van toepassing. |
    | SubscriptionId |De Azure-abonnement-ID waarmee de virtuele machine is gemaakt. |
    | Rolnaam |De naam van de virtuele machine van Azure die wordt hersteld. |
    | CloudServiceName |De Azure-cloud servicenaam waaronder de virtuele machine is gemaakt. |
    | resourceGroupName|De Azure-resource groepsnaam waaronder de virtuele machine is gemaakt. |
    | RecoveryPointId|Het tijdstempel voor wanneer de virtuele machine is hersteld. |

* Zorg ervoor dat het Automation-account de volgende modules heeft:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Alle modules moeten van compatibele versies. Er is een eenvoudige manier om ervoor te zorgen dat alle modules compatibel zijn met de nieuwste versies van alle modules.

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a>Toegang tot alle virtuele machines van de VMMap in een lus
De volgende code gebruiken voor alle virtuele machines van de Microsoft-VMMap lus:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is to ensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> De resource-groep en de rol naamwaarden zijn leeg wanneer het script een vooraf actie aan een groep opstarten is. De waarden worden ingevuld alleen als de virtuele machine van die groep tijdens failover slaagt. Het script is een na actie van de groep opstarten.

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a>Automation-runbook in meerdere herstelplannen gebruiken

U kunt één script gebruiken in meerdere herstelplannen met behulp van externe variabelen. U kunt [Azure Automation-variabelen](../automation/automation-variables.md) voor het opslaan van de parameters die u voor de uitvoering van een herstel plan doorgeven kunt. Door de naam voor het herstel toe te voegen als voorvoegsel aan de variabele, kunt u afzonderlijke variabelen voor elke herstelplan maken. Vervolgens gebruikt u de variabelen als parameters. U kunt een parameter wijzigen zonder het script worden gewijzigd, maar nog steeds de werking van het script wijzigen.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Een eenvoudige string-variabele in een runbookscript gebruiken

In dit voorbeeld wordt een script haalt de invoer van een Netwerkbeveiligingsgroep (NSG) en toegepast op de virtuele machines van een herstelplan.

Voor het script voor het detecteren van welke recovery plan wordt uitgevoerd, gebruikt u de context van de planning herstel:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

Als u wilt toepassen op een bestaande NSG, moet u de naam van de NSG en de naam van de resourcegroep NSG weten. Deze variabelen gebruiken als invoer voor herstel plan scripts. U doet dit door twee variabelen in de activa van Automation-account te maken. Voeg de naam van het herstelplan die u bij het maken van de parameters voor als een voorvoegsel aan de variabelenaam toe.

1. Maak een variabele voor het opslaan van de naam van de NSG. Een voorvoegsel toevoegen aan de variabelenaam met behulp van de naam van het herstelplan.

    ![Maak een NSG naamvariabele](media/site-recovery-runbook-automation-new/var1.png)

2. Een variabele voor het opslaan van de naam van de NSG resourcegroep maken. Een voorvoegsel toevoegen aan de variabelenaam met behulp van de naam van het herstelplan.

    ![De groepsnaam van een NSG-resource maken](media/site-recovery-runbook-automation-new/var2.png)


3.  Gebruik de volgende verwijzingscode om de waarden van variabelen in het script:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Gebruik de variabelen in het runbook de NSG toepassen op de netwerkinterface van de failover-VM:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

Voor elke herstelplan onafhankelijke variabelen te maken zodat u het script opnieuw kunt gebruiken. Een voorvoegsel toevoegen met behulp van de naam voor het herstel. Zie voor een script is voltooid, end-to-end voor dit scenario, [toevoegen van een openbare IP-adres en het NSG aan virtuele machines tijdens de testfailover van een herstelplan Site Recovery](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-to-store-more-information"></a>Een complex-variabele gebruiken voor het opslaan van meer informatie

Overweeg een scenario waarin u wilt dat één script om in te schakelen op een openbaar IP-adres op specifieke virtuele machines. In een ander scenario wilt u mogelijk verschillende nsg's toepassen op andere virtuele machines (niet op alle VM's). U kunt een script dat opnieuw kan worden gebruikt voor een herstelplan maken. Elke herstelplan kan een variabele aantal VM's hebben. Een SharePoint-herstelbewerking heeft bijvoorbeeld twee front-ends. Een basic line-of-business (LOB)-toepassing heeft slechts één front-end. U kunt afzonderlijke variabelen voor elke herstelplan kan niet maken. 

In het volgende voorbeeld wordt een nieuwe techniek gebruiken en maak een [complex variabele](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in de Azure Automation-account. U doet dit door geven meerdere waarden. U moet Azure PowerShell gebruiken om de volgende stappen uit:

1. In PowerShell, moet u zich aanmelden bij uw Azure-abonnement:

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Maak de variabele complex met behulp van de naam van het herstelplan voor het opslaan van de parameters:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. In deze variabele complex **VMDetails** ID van de VM is voor de beveiligde virtuele machine. Als u de ID van de VM in de Azure portal, de VM-eigenschappen te bekijken. De volgende schermafbeelding ziet een variabele die de details van de twee virtuele machines worden opgeslagen:

    ![ID van de VM gebruiken als de GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. Gebruik deze variabele in uw runbook. Als de opgegeven VM-GUID is gevonden in de context van recovery plan, moet u het NSG toepassen op de virtuele machine:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. In uw runbook doorloopt u de virtuele machines van de context van recovery-abonnement. Controleer of de virtuele machine bestaat in **$VMDetailsObj**. Als dit bestaat, toegang tot de eigenschappen van de variabele de NSG toepassen:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If the VM exists in the context, this will not b Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

U kunt hetzelfde script gebruiken voor andere herstelplannen. Geef andere parameters door het opslaan van de waarde die overeenkomt met een herstelplan in verschillende variabelen.

## <a name="sample-scripts"></a>Voorbeeldscripts

Als u wilt implementeren voorbeeldscripts op uw Automation-account, klikt u op de **implementeren in Azure** knop.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Zie de volgende video voor een ander voorbeeld. Laat het herstellen van een toepassing van de twee lagen WordPress in Azure:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a>Aanvullende bronnen
* [Azure Automation-service uitvoeren als-account](../automation/automation-sec-configure-azure-runas-account.md)
* [Overzicht van Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation-overzicht")
* [Azure Automation-voorbeeldscripts](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-voorbeeldscripts")
