---
title: Verbinding maken met een virtuele Windows Server-machine | Microsoft Docs
description: Leer hoe u verbinding maakt en u aanmeldt bij een virtuele Windows-machine via de Azure-portal en het Resource Manager-implementatiemodel.
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
ms.date: 09/13/2018
ms.author: cynthn
ms.openlocfilehash: b9cce5658b705e9d3255d2662b2a0157a2e548c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409025"
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Verbinding maken met en aanmelden bij een virtuele machine in Azure waarop Windows wordt uitgevoerd
U gebruikt de knop **Verbinden** in Azure Portal om een Extern bureaublad-sessie (RDP) te starten vanaf een Windows-computer. Eerst maakt u verbinding met de virtuele machine en meld u aan.

Als u wilt verbinding maken met een Windows-VM vanaf een Mac, moet u een RDP-client voor Mac installeren zoals [Microsoft Extern bureaublad](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als u dat nog niet hebt gedaan.
2. Selecteer in het menu links **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Selecteer boven aan de pagina voor de virtuele machine, **Connect**.
2. Op de **verbinding maken met virtuele machine** pagina, selecteer de gewenste opties en selecteer **downloaden RDP-bestand**.
2. Open het gedownloade RDP-bestand en selecteer **Connect** wanneer hierom wordt gevraagd. 
2. U ontvangt een waarschuwing dat het RDP-bestand van een onbekende uitgever is. Dit is normaal gedrag. In de **verbinding met extern bureaublad** venster **Connect** om door te gaan.
   
    ![Schermafbeelding met waarschuwing over een onbekende uitgever](./media/connect-logon/rdp-warn.png)
3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Voer de referenties voor een account op de virtuele machine en selecteer vervolgens **OK**.
   
     **Lokaal account**: dit is meestal de lokale gebruikersnaam en het wachtwoord die u hebt opgegeven tijdens het maken van de virtuele machine. In dit geval is het domein de naam van de virtuele machine en het is ingevoerd als *vmname*&#92;*gebruikersnaam*.  
   
    **Virtuele machine van een domein**: als de virtuele machine deel uitmaakt van een domein, voert u de naam van de gebruiker in de indeling *domein*&#92;*gebruikersnaam*. Het account moet bovendien in de groep Administrators staan of er moet een machtiging voor externe toegang zijn verleend aan de VM.
   
    **Domeincontroller**: als de virtuele machine een domeincontroller is, voert u de gebruikersnaam en het wachtwoord van een domein administrator-account voor dat domein.
4. Selecteer **Ja** om te controleren of de identiteit van de virtuele machine en aanmelden te voltooien.
   
   ![Schermafbeelding met een bericht over het verifiëren van de identiteit van de virtuele machine](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Als de **Connect** knop in de portal is niet beschikbaar en u niet verbonden bent met Azure via een [Express Route](../../expressroute/expressroute-introduction.md) of [Site-naar-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbinding, moet u maken en uw virtuele machine een openbare IP-adres toewijzen voordat u extern bureaublad kunt gebruiken. Zie voor meer informatie, [openbare IP-adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Volgende stappen
Hebt u problemen bij het verbinding maken, Zie [problemen oplossen met extern bureaublad-verbindingen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

