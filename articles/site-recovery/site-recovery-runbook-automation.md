---
title: Azure Automation-runbooks aan herstelplannen in Azure Site Recovery toevoegen | Microsoft Docs
description: Meer informatie over hoe Azure Site Recovery kan u helpen plannen voor herstel met behulp van Azure Automation uitbreiden. Leer hoe u complexe taken uit te voeren tijdens het herstel naar Azure.
services: site-recovery
documentationcenter: ''
author: ruturaj
manager: gauravd
editor: ''
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 55160f3c43d8cbfc5f8b3e6aaf26bcb911387c52
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578762"
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure Automation-runbooks aan herstelplannen toevoegen
In dit artikel wordt beschreven hoe Azure Site Recovery kan worden geïntegreerd met Azure Automation kunt u uw herstelplannen uitbreiden. Plannen voor herstel kunnen gecoördineerd herstel van virtuele machines die zijn beveiligd met Site Recovery. Plannen voor herstel werkt zowel voor replicatie naar een secundaire cloud, en voor replicatie naar Azure. Plannen voor herstel ook helpen met het herstel **accuraat**, **herhaalbare**, en **geautomatiseerde**. Als u uw virtuele machines naar Azure failover, een uitbreiding voor integratie met Azure Automation uw plannen voor herstel. U kunt deze gebruiken voor het uitvoeren van runbooks, deze bieden een krachtige geautomatiseerde taken.

Als u niet bekend bent met Azure Automation, kunt u [aanmelden](https://azure.microsoft.com/services/automation/) en [voorbeeldscripts downloaden](https://azure.microsoft.com/documentation/scripts/). Voor meer informatie en voor informatie over het organiseren van herstel naar Azure met behulp van [herstelplannen](./site-recovery-create-recovery-plans.md), Zie [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

In dit artikel wordt beschreven hoe u Azure Automation-runbooks kunt integreren in uw plannen voor herstel. Voorbeelden gebruiken we voor het automatiseren van algemene taken die voorheen handmatige interventie nodig. We ook wordt beschreven hoe u een WebTest met meerdere stappen voor herstel naar een herstelactie met één muisklik converteren.

## <a name="customize-the-recovery-plan"></a>Het herstelplan aanpassen
1. Ga naar de **siteherstel** resourceblade recovery-abonnement. In dit voorbeeld heeft het herstelplan twee virtuele machines die zijn toegevoegd, voor herstel. Als u wilt beginnen met het toevoegen van een runbook, klikt u op de **aanpassen** tabblad.

    ![Klik op de knop aanpassen](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Met de rechtermuisknop op **groep 1: Start**, en selecteer vervolgens **actie achteraf toevoegen**.

    ![Klik met de rechtermuisknop groep 1: Start en actie achteraf toevoegen](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Klik op **Kies een script**.

4. Op de **bijwerkactie** blade, de naam van het script **Hello World**.

    ![De blade van de actie bijwerken](media/site-recovery-runbook-automation-new/update-rp.png)

5. Voer de naam van een Automation-account.
    >[!NOTE]
    > Het Automation-account kan zich in een Azure-regio. Het Automation-account moet zich in hetzelfde abonnement als de Azure Site Recovery-kluis.

6. Selecteer een runbook in uw Automation-account. Dit runbook is het script dat wordt uitgevoerd tijdens het uitvoeren van het herstelplan te gaan, na het herstel van de eerste groep.

7. Om het script hebt opgeslagen, klikt u op **OK**. Het script wordt toegevoegd aan **groep 1: stappen na**.

    ![De actie na de groep 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Overwegingen voor het toevoegen van een script

* Opties voor **verwijderen van een stap** of **bijwerken van het script**, met de rechtermuisknop op het script.
* Een script kunt uitvoeren op Azure tijdens de failover van een on-premises machine naar Azure. Deze kunt ook uitvoeren op Azure als een primaire site script vóór het afsluiten, tijdens de failback vanuit Azure naar een on-premises machine.
* Wanneer een script wordt uitgevoerd, is het injects de context van een herstel-plan. Het volgende voorbeeld ziet u een contextvariabele:

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

    De volgende tabel bevat de naam en beschrijving van elke variabele in de context.

    | **De naam van variabele** | **Beschrijving** |
    | --- | --- |
    | RecoveryPlanName |De naam van de planning wordt uitgevoerd. Deze variabele kunt u verschillende acties op basis van de naam van het herstelplan uitvoeren. U kunt ook het script hergebruiken. |
    | FailoverType |Hiermee geeft u op of de failover een test is, gepland of ongepland. |
    | FailoverDirection |Geeft aan of herstel naar een primaire of secundaire site. |
    | Groeps-id |Geeft het nummer van de in het herstelplan te gaan wanneer de planning wordt uitgevoerd. |
    | VmMap |Een matrix met alle virtuele machines in de groep. |
    | VMMap sleutel |Een unieke sleutel (GUID) voor elke virtuele machine. Dit is hetzelfde als de Azure Virtual Machine Manager (VMM)-ID van de virtuele machine, indien van toepassing. |
    | SubscriptionId |De Azure-abonnement-ID waarmee de virtuele machine is gemaakt. |
    | RoleName |De naam van de Azure-virtuele machine die wordt hersteld. |
    | CloudServiceName |De naam van de Azure-cloud service is waarmee de virtuele machine is gemaakt. |
    | ResourceGroupName|Azure naam van de resourcegroep waarin de virtuele machine is gemaakt. |
    | RecoveryPointId|De tijdstempel voor wanneer de virtuele machine is hersteld. |

* Zorg ervoor dat het Automation-account de volgende modules heeft:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Alle modules moeten van compatibele versies. Een eenvoudige manier om ervoor te zorgen dat alle modules compatibel is met de nieuwste versies van alle modules.

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a>Toegang tot alle virtuele machines van de VMMap in een lus
Gebruik de volgende code om te herhalen voor alle virtuele machines van de Microsoft-VMMap:

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
> De resource-groep en de rol waarden zijn leeg wanneer het script een actie voorafgaand aan een groep Opstarten wordt. De waarden worden ingevuld alleen als de virtuele machine van die groep tijdens failover slaagt. Het script is een actie na de van de groep opstarten.

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a>Gebruik hetzelfde Automation-runbook in meerdere herstelplannen

U kunt één script gebruiken in meerdere plannen voor herstel met behulp van externe variabelen. U kunt [Azure Automation-variabelen](../automation/automation-variables.md) voor het opslaan van de parameters die u voor de uitvoering van een herstel-plan doorgeven kunt. Naam van het herstelplan toevoegt als een voorvoegsel aan de variabele, kunt u afzonderlijke variabelen voor elk plan voor herstel. Vervolgens gebruikt u de variabelen als parameters. U kunt een parameter wijzigen zonder dat u het script wijzigt, maar nog steeds de werking van het script wijzigen.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Een eenvoudige tekenreeks-variabele gebruiken in een runbookscript

In dit voorbeeld wordt een script de invoer van een Netwerkbeveiligingsgroep (NSG) en toegepast op de virtuele machines van een plan voor herstel.

Voor het script voor het detecteren van welke recovery plan wordt uitgevoerd, gebruikt u de context van de planning herstel:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

Als u wilt toepassen op een bestaande NSG, moet u weten de naam van de NSG en de naam van de NSG-resource. Deze variabelen gebruiken als invoer voor plan scripts voor herstel. U doet dit door twee variabelen die in de activa van Automation-account te maken. Voeg de naam van het herstelplan te gaan die u de parameters voor als voorvoegsel van de variabelenaam maakt toe.

1. Maak een variabele voor het opslaan van de naam van de NSG. Een voorvoegsel toegevoegd aan de naam van de variabele met behulp van de naam van het herstelplan te gaan.

    ![Maken van een variabele van de naam van NSG](media/site-recovery-runbook-automation-new/var1.png)

2. Maak een variabele voor het opslaan van de naam van de resourcegroep van de NSG. Een voorvoegsel toegevoegd aan de naam van de variabele met behulp van de naam van het herstelplan te gaan.

    ![De naam van een NSG resourcegroep maken](media/site-recovery-runbook-automation-new/var2.png)


3.  In het script, gebruikt u de volgende verwijzingscode om op te halen van de waarden van variabelen:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Gebruik de variabelen in het runbook om toe te passen van de NSG aan de netwerkinterface van de failover-VM:

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

Maak voor elk plan voor herstel, onafhankelijke variabelen zodat u het script opnieuw kunt gebruiken. Een voorvoegsel toevoegen met behulp van de naam van het herstelplan. Zie voor een volledige, end-to-end-script voor dit scenario [toevoegen van een openbare IP- en NSG aan virtuele machines tijdens de testfailover van een plan voor herstel van Site Recovery](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-to-store-more-information"></a>Een complex-variabele gebruiken voor het opslaan van meer informatie

U hebt een scenario waarin u wilt dat één script om in te schakelen op een openbaar IP-adres op specifieke virtuele machines. In een ander scenario wilt u mogelijk verschillende nsg's toepassen op verschillende VM's (niet op alle virtuele machines). U kunt een script dat opnieuw kan worden gebruikt voor een plan voor herstel. Elke herstelplan kan een variabele aantal virtuele machines hebben. Een SharePoint-herstelbewerking heeft bijvoorbeeld twee front-ends. Een eenvoudige line-of-business (LOB)-toepassing heeft slechts één front-end. U kunt geen afzonderlijke variabelen voor elk plan voor herstel maken.

In het volgende voorbeeld wordt er gebruik van een nieuwe techniek en maak een [complexe variabele](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureautomationvariable) in de activa van Azure Automation-account. U doen dit door meerdere waarden op te geven. U moet Azure PowerShell gebruiken om de volgende stappen uit:

1. In PowerShell, moet u zich aanmelden bij uw Azure-abonnement:

    ```
    Connect-AzureRmAccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Voor het opslaan van de parameters, de variabele complex te maken met behulp van de naam van het herstelplan te gaan:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. In deze variabele complexe **VMDetails** is van de VM-ID voor de beveiligde virtuele machine. Als u de ID van de VM in Azure portal, de VM-eigenschappen te bekijken. De volgende schermafbeelding ziet u een variabele waarin de details van twee virtuele machines:

    ![De VM-ID gebruiken als de GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. Gebruik deze variabele in uw runbook. Als de opgegeven VM-GUID is gevonden in de context van recovery plan, gelden de NSG op de virtuele machine:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. Lus via de virtuele machines van de recovery-abonnement context in uw runbook. Controleer of de virtuele machine bestaat **$VMDetailsObj**. Als deze bestaat, toegang krijgen tot de eigenschappen van de variabele om toe te passen van de NSG:

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

U kunt hetzelfde script gebruiken voor verschillende herstelplannen. Geef andere parameters door op te slaan de waarde die overeenkomt met een plan voor herstel in verschillende variabelen.

## <a name="sample-scripts"></a>Voorbeeldscripts

Als u wilt implementeren voorbeeldscripts naar uw Automation-account, klikt u op de **implementeren in Azure** knop.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Zie de volgende video voor een ander voorbeeld. Hierin wordt beschreven hoe u een toepassing twee lagen WordPress in Azure herstellen:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]


## <a name="additional-resources"></a>Aanvullende resources
* [Azure Automation-service uitvoeren als-account](../automation/automation-create-runas-account.md)
* [Overzicht van Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "overzicht van Azure Automation")
* [Azure Automation-voorbeeldscripts](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "voorbeeldscripts voor Azure Automation")

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-failover.md) over het uitvoeren van failovers.
