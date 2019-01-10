---
title: Azure-eindpunt toegangscontrolelijsten beheren | PowerShell | Klassieke | Microsoft Docs
description: Informatie over het beheren van ACL's met PowerShell
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 1f6c14c15d4930902ced642bd02d1d1833e0b361
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54154807"
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a>Eindpunt toegangscontrolelijsten in het klassieke implementatiemodel met behulp van PowerShell beheren
U kunt maken en beheren van netwerk toegangsbeheerlijsten (ACL's) voor eindpunten met behulp van Azure PowerShell of in de beheerportal. In dit onderwerp vindt u procedures voor ACL algemene taken die u kunt uitvoeren met behulp van PowerShell. Zie voor een overzicht van Azure PowerShell cmdlets [Azure Management-Cmdlets](https://go.microsoft.com/fwlink/?LinkId=317721). Zie voor meer informatie over ACL's [wat is er een netwerk lijst met ACL (Access Control)?](virtual-networks-acl.md). Als u wilt de ACL's beheren met behulp van de Management Portal, Zie [instellen van eindpunten aan een virtuele Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Netwerk-ACL's beheren met behulp van Azure PowerShell
U kunt Azure PowerShell-cmdlets gebruiken om te maken, verwijderen en configureren (set) netwerk toegangsbeheerlijsten (ACL's). We hebben een paar voorbeelden van enkele van de manieren waarop die u kunt een ACL met behulp van PowerShell configureren opgenomen.

Als u wilt ophalen van een volledige lijst van de ACL-PowerShell-cmdlets, kunt u een van de volgende gebruiken:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Een netwerk-ACL met regels die toegang vanuit een extern subnet verlenen maken
In het volgende voorbeeld ziet u een manier om een nieuwe ACL met regels die te maken. Deze ACL wordt vervolgens toegepast op het eindpunt van een virtuele machine. De ACL-regels in het onderstaande voorbeeld de toegang van een extern subnet toestaat. Voor het maken van een nieuwe netwerk-ACL met regels voor toestaan voor een extern subnet, opent u een Azure PowerShell ISE. Kopieer en plak het script onder het script door uw eigen waarden configureren en voer het script.

1. Het nieuwe netwerk-ACL-object maken.
   
        $acl1 = New-AzureAclConfig
2. Een regel die toegang vanuit een extern subnet toestaat instellen. In het voorbeeld hieronder, stelt u de regel *100* (dit heeft voorrang op regel 200 en hoger) waarmee het externe subnet *10.0.0.0/8* toegang tot het eindpunt van de virtuele machine. Vervang de waarden door uw eigen configuratievereisten. De naam 'SharePoint ACL config' moet worden vervangen door de beschrijvende naam die u wilt aanroepen met deze regel.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Voor extra regels, herhaalt u de cmdlet, vervang de waarden door uw eigen configuratievereisten. Zorg dat u de regel number volgorde in overeenstemming met de volgorde waarin u de regels worden toegepast. De minder regel heeft voorrang op het hogere getal.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Vervolgens kunt u een nieuw eindpunt (toevoegen) maken of de ACL voor een bestaand eindpunt (Set) instellen. In dit voorbeeld zullen we een nieuwe virtuele machine-eindpunt met de naam 'web' toevoegen en bijwerken van het eindpunt van de virtuele machine met de ACL-instellingen.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. Vervolgens de cmdlets combineren en voer het script. In dit voorbeeld zou de gecombineerde cmdlets er als volgt:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Een netwerk-ACL-regel die toegang vanuit een extern subnet toestaat verwijderen
In het volgende voorbeeld ziet u een manier om een netwerk ACL-regel te verwijderen.  Als u wilt verwijderen van een netwerk-ACL-regel met regels voor toestaan voor een extern subnet, opent u een Azure PowerShell ISE. Kopieer en plak het script onder het script door uw eigen waarden configureren en voer het script.

1. Eerste stap is om op te halen van de netwerk ACL-object voor het eindpunt van de virtuele machine. Vervolgens verwijdert u de ACL-regel. In dit geval wordt het wilt verwijderen van regel-ID. Hiermee wordt de regel-ID 0 alleen verwijderd uit de Toegangsbeheerlijst. De ACL-object wordt niet verwijderd uit het eindpunt van de virtuele machine.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Vervolgens moet u de netwerk ACL-object toepassen op het eindpunt van de virtuele machine en werk de virtuele machine.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Een netwerk-ACL van het eindpunt van een virtuele machine verwijderen
In bepaalde scenario's, kunt u een netwerk-ACL-object verwijderen uit het eindpunt van een virtuele machine. Om dit te doen, opent u een Azure PowerShell ISE. Kopieer en plak het script onder het script door uw eigen waarden configureren en voer het script.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
[Wat is een netwerk lijst met ACL (Access Control)?](virtual-networks-acl.md)

