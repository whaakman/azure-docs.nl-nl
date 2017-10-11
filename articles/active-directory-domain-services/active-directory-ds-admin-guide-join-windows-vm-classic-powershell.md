---
title: 'Azure Active Directory Domain Services: Beheerdershandleiding | Microsoft Docs'
description: Windows virtuele machine toevoegen aan een beheerd domein met behulp van Azure PowerShell en het klassieke implementatiemodel.
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Windows Server een virtuele machine toevoegen aan een beheerd domein met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Klassieke Azure-portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Azure AD Domain Services biedt momenteel geen ondersteuning voor het Resource Manager-model.
>
>

Deze stappen ziet u hoe u kunt een set van Azure PowerShell-opdrachten die maken en vooraf configureren van een op basis van Windows Azure virtuele machine met behulp van een benadering bouwsteen aanpassen. Deze stappen kunt u een op basis van Windows Azure virtuele machine maken en toevoegen aan een beheerd domein van Azure AD Domain Services.

Deze stappen volgt een benadering invullen-in-the-lege waarden voor het maken van Azure PowerShell-opdrachtsets. Deze methode kan nuttig zijn als u niet bekend met PowerShell bent of u weten welke waarden u wilt moet opgeven voor een geslaagde configuratie. Geavanceerde PowerShell-gebruikers kunnen de opdrachten en vervang hun eigen waarden voor de variabelen (de regels die beginnen met '$').

Als u dit nog niet hebt gedaan, gebruikt u de instructies in [installeren en configureren van Azure PowerShell](/powershell/azure/overview) Azure PowerShell te installeren op uw lokale computer. Vervolgens opent u een Windows PowerShell-opdrachtprompt.

## <a name="step-1-add-your-account"></a>Stap 1: Uw account toevoegen
1. Typ het volgende achter de PowerShell-prompt **Add-AzureAccount** en klik op **Enter**.
2. Typ in het e-mailadres dat is gekoppeld aan uw Azure-abonnement en klikt u op **doorgaan**.
3. Typ in het wachtwoord voor uw account.
4. Klik op **aanmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Stap 2: Uw abonnement en storage-account instellen
Uw Azure-abonnement en storage-account instellen door het uitvoeren van deze opdrachten bij de opdrachtprompt van Windows PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens lang zijn en de juiste namen.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

U kunt de naam van het juiste abonnement ophalen uit de eigenschap SubscriptionName voor de uitvoer van de **Get-AzureSubscription** opdracht. U kunt de juiste opslagaccountnaam ophalen van de eigenschap Label van de uitvoer van de **Get-AzureStorageAccount** opdracht, na het uitvoeren van de **Select-AzureSubscription** opdracht.

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Stap 3: Stapsgewijze - de virtuele machine in te richten en deze koppelen aan het beheerde domein
Hier volgt de bijbehorende Azure PowerShell-opdracht ingesteld op deze virtuele machine maken met lege regels tussen elk blok omwille van de leesbaarheid.

Geef informatie op over de Windows virtuele machine moeten worden ingericht.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Zie voor de InstanceSize waarden voor D, DS of G-serie virtuele machines, [virtuele Machine en Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Bevatten informatie over het beheerde domein.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geef de naam van de cloudservice.

    $svcname="Contoso100-test"

Geef de naam van het virtuele netwerk waarmee de virtuele machine moet worden samengevoegd. Zorg ervoor dat het beheerde domein van de AAD-DS beschikbaar in dit virtuele netwerk is.

    $vnetname="MyPreviewVnet"

Selecteer het VM-afbeelding moet worden gebruikt voor het inrichten van de virtuele machine.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Configureer de virtuele machine - naam van de VM, exemplaargrootte & afbeelding moet worden gebruikt.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Lokale administrator-referenties ophalen voor de virtuele machine. Kies een sterke lokale administrator-wachtwoord.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Referenties voor een gebruikersaccount van ' AAD DC' beheerdersgroep VM koppelen aan het beheerde domein ophalen. Geef de naam van het domein - bijvoorbeeld in ons voorbeeld, we "bob" opgeven als de gebruikersnaam.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

De virtuele machine configureren - domain join vereiste & vereiste referenties opgeven.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Stel een subnet voor de virtuele machine.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optioneel: Verwijzen naar het IP-adres van het domein. Als u de IP-adressen van het beheerde domein van Azure AD Domain Services moet de DNS-servers voor het virtuele netwerk hebt ingesteld, is deze stap niet vereist.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nu de Windows-domein VM inrichten.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Script voor het inrichten van een virtuele machine van Windows en automatisch toevoegen aan een beheerd domein van AAD Domain Services
Deze set PowerShell-opdracht maakt een virtuele machine voor een line-of-business-server die:

* Maakt gebruik van de installatiekopie van het Windows Server 2012 R2 Datacenter.
* Is een extra klein virtuele machine.
* De naam van de Contoso100-test heeft.
* Automatisch wordt lid van een domein aan het beheerde domein contoso100.
* Is toegevoegd aan de hetzelfde virtuele netwerk als het beheerde domein.

Hier wordt het volledige voorbeeld van een script voor het maken van de virtuele machine van Windows en automatisch toevoegen aan het beheerde domein van Azure AD Domain Services.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Een beheerd domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
