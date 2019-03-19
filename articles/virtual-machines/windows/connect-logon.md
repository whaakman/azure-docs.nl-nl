---
title: Verbinding maken met een virtuele Windows Server-machine | Microsoft Docs
description: Leer hoe u verbinding maken met en meld u aan met een Windows-VM met behulp van de Azure-portal en het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: c14b6e16a16f325681722203eaa05a435b141208
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57990615"
---
# <a name="how-to-connect-and-sign-on-to-an-azure-virtual-machine-running-windows"></a>Hoe u verbinding maken met en meld u aan met een Azure-machine waarop Windows wordt uitgevoerd
U gebruikt de knop **Verbinden** in Azure Portal om een Extern bureaublad-sessie (RDP) te starten vanaf een Windows-computer. Eerst maakt u verbinding met de virtuele machine en meld u op.

Als u wilt verbinding maken met een Windows-VM vanaf een Mac, moet u een RDP-client voor Mac installeren zoals [Microsoft Extern bureaublad](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als u dat nog niet hebt gedaan.
2. Selecteer in het menu links **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Selecteer boven aan de pagina voor de virtuele machine, **Connect**.
2. Op de **verbinding maken met virtuele machine** pagina, selecteert u de juiste IP-adres en poort. In de meeste gevallen moeten het standaard IP-adres en de poort worden gebruikt. Selecteer **RDP-bestand downloaden**. Als de virtuele machine een just-in-time-beleid is ingesteld heeft, moet u eerst selecteert u de **toegang aanvragen** knop om toegang te vragen voordat u het RDP-bestand kunt downloaden. Zie voor meer informatie over het beleid voor just-in-time [beheer van de virtuele machine toegang met just in time-beleid](../../security-center/security-center-just-in-time.md).
2. Open het gedownloade RDP-bestand en selecteer **Verbinden** wanneer dit wordt gevraagd. 
2. U ontvangt een waarschuwing dat de `.rdp` bestand afkomstig is van een onbekende uitgever. Dit is normaal gedrag. In de **verbinding met extern bureaublad** venster **Connect** om door te gaan.
   
    ![Schermafbeelding met waarschuwing over een onbekende uitgever](./media/connect-logon/rdp-warn.png)
3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Voer de referenties voor een account op de virtuele machine en selecteer vervolgens **OK**.
   
     **Lokaal account**: Dit is meestal de lokale gebruikersnaam en het wachtwoord die u hebt opgegeven tijdens het maken van de virtuele machine. In dit geval is het domein de naam van de virtuele machine en het is ingevoerd als *vmname*&#92;*gebruikersnaam*.  
   
    **Virtuele machine van een domein**: Als de virtuele machine deel uitmaakt van een domein, voert u de naam van de gebruiker in de indeling *domein*&#92;*gebruikersnaam*. Het account moet bovendien in de groep Administrators staan of er moet een machtiging voor externe toegang zijn verleend aan de VM.
   
    **Domeincontroller**: Als de virtuele machine een domeincontroller is, voert u de gebruikersnaam en het wachtwoord van een domein administrator-account voor het domein.
4. Selecteer **Ja** om te controleren of de identiteit van de virtuele machine en aanmelden te voltooien.
   
   ![Schermafbeelding met een bericht over het verifiëren van de identiteit van de virtuele machine](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Als de **Connect** knop in de portal is niet beschikbaar en u niet verbonden bent met Azure via een [Express Route](../../expressroute/expressroute-introduction.md) of [Site-naar-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbinding, moet u maken en uw virtuele machine een openbare IP-adres toewijzen voordat u extern bureaublad kunt gebruiken. Zie voor meer informatie, [openbare IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="connect-to-the-virtual-machine-using-powershell"></a>Verbinding maken met de virtuele machine met behulp van PowerShell

Als u met behulp van PowerShell en de AzureRM-module geïnstalleerd hebt u mogelijk ook verbinding maken met behulp van de `Get-AzRemoteDesktopFile` cmdlet, zoals hieronder wordt weergegeven.

In dit voorbeeld wordt de RDP-verbinding, die u begeleidt bij dezelfde stappen als hierboven onmiddellijk gestart.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -Launch
```

U kunt ook het RDP-bestand voor toekomstig gebruik opslaan.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -LocalPath "C:\Path\to\folder"
```

## <a name="next-steps"></a>Volgende stappen
Hebt u problemen bij het verbinding maken, Zie [problemen oplossen met extern bureaublad-verbindingen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

