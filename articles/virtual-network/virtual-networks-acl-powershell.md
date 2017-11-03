---
title: Azure-eindpunt toegangsbeheerlijsten beheren | PowerShell | Klassieke | Microsoft Docs
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
ms.openlocfilehash: c3476908447380ccd7e8b9c0f1c2a55ae763cc1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a>Eindpunt toegangsbeheerlijsten met PowerShell in het klassieke implementatiemodel beheren
U kunt maken en beheren van netwerk-toegangsbeheerlijsten (ACL's) voor eindpunten met behulp van Azure PowerShell of in de beheerportal. In dit onderwerp vindt u procedures voor ACL algemene taken die u kunt uitvoeren met behulp van PowerShell. Zie voor een overzicht van Azure PowerShell cmdlets [Azure Management-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721). Zie voor meer informatie over ACL's, [wat is er een netwerk lijst ACL (Access Control)?](virtual-networks-acl.md). Als u wilt uw ACL's beheren met behulp van de beheerportal, Zie [ingesteld van eindpunten aan een virtuele Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Netwerk-ACL's beheren met behulp van Azure PowerShell
U kunt Azure PowerShell-cmdlets gebruiken om te maken, verwijderen en configureren (set) netwerk toegangsbeheerlijsten (ACL's). Vindt u enkele voorbeelden van een aantal manieren waarop die u een ACL die met behulp van PowerShell kunt configureren.

Voor het ophalen van een volledige lijst van de ACL-PowerShell-cmdlets, kunt u het volgende:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Maken van een netwerk-ACL met regels die toegang vanaf een extern subnet verlenen
Het volgende voorbeeld ziet u een manier om een nieuwe ACL met regels die te maken. Deze ACL wordt vervolgens toegepast op het eindpunt van een virtuele machine. De ACL-regels in het volgende voorbeeld wordt toegang toestaan via een extern subnet. Maakt een nieuwe netwerk-ACL met regels voor toestaan voor een extern subnet, open een Azure PowerShell ISE. Kopieer en plak het script onder configuratie van het script met uw eigen waarden en voer het script.

1. Het nieuwe netwerk-ACL-object maken.
   
        $acl1 = New-AzureAclConfig
2. Een regel waarmee de toegang van een extern subnet instellen. In het onderstaande voorbeeld stelt u de regel *100* (dit heeft voorrang op de regel 200 en hoger) om toe te staan van het externe subnet *10.0.0.0/8* toegang tot het eindpunt van de virtuele machine. De waarden vervangt door uw eigen configuratievereisten. De naam "SharePoint ACL-config" moet worden vervangen door de beschrijvende naam die u wilt aanroepen met deze regel.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Herhaal de cmdlet, waarbij de waarden vervangt door uw eigen configuratievereisten voor extra regels. Zorg ervoor dat u de regel wijzigen number volgorde in overeenstemming met de volgorde waarin u wilt dat de regels moeten worden toegepast. De minder regel heeft voorrang op een hoger nummer.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Vervolgens kunt u een nieuw eindpunt (toevoegen) maken of de ACL voor een bestaand eindpunt (Set) instellen. In dit voorbeeld wordt een nieuwe virtuele machine eindpunt 'web' aangeroepen toevoegen en bijwerken van het eindpunt van de virtuele machine met de ACL-instellingen.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. Vervolgens de cmdlets met elkaar combineren en voer het script. In dit voorbeeld bevindt de gecombineerde cmdlets eruit als volgt:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Verwijderen van een netwerk-ACL-regel waarmee de toegang van een extern subnet
Het volgende voorbeeld ziet u een manier om een netwerk-ACL-regel te verwijderen.  Als u een regel netwerk ACL met regels voor toestaan voor een extern subnet, open een Azure PowerShell ISE. Kopieer en plak het script onder configuratie van het script met uw eigen waarden en voer het script.

1. Eerste stap is het ophalen van het netwerk ACL-object voor het eindpunt van de virtuele machine. Vervolgens verwijdert u de ACL-regel. In dit geval wordt het wilt verwijderen op regel-ID. Hiermee wordt de regel-ID 0 alleen verwijderd uit de Toegangsbeheerlijst. Dit wordt niet verwijderd van de ACL-object van het eindpunt van de virtuele machine.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Vervolgens moet u de netwerk-ACL-object toepassen op het eindpunt van de virtuele machine en werk de virtuele machine.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Een netwerk-ACL verwijderen uit het eindpunt van een virtuele machine
In bepaalde gevallen, is het raadzaam een netwerk-ACL-object verwijderen uit het eindpunt van een virtuele machine. Open hiertoe een Azure PowerShell ISE. Kopieer en plak het script onder configuratie van het script met uw eigen waarden en voer het script.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
[Wat is een netwerk lijst ACL (Access Control)?](virtual-networks-acl.md)

