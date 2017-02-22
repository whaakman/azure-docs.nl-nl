---
title: Verbinding maken met een virtuele Windows Server-machine | Microsoft Docs
description: Leer hoe u verbinding maakt en u aanmeldt bij een virtuele Windows-machine via de Azure-portal en het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 9077746d8ffe968504f1dde90ed5f76dd1facc19


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Verbinding maken met en aanmelden bij een virtuele machine in Azure waarop Windows wordt uitgevoerd
U gebruikt de knop **Verbinden** in de Azure-portal om een Extern bureaublad-sessie te starten. Eerst maakt u verbinding met de virtuele machine en vervolgens meldt u zich aan.

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als u dat nog niet hebt gedaan.
2. Klik in het menu Hub op **Virtuele machines**.
3. Selecteer de virtuele machine in de lijst.
4. Klik op de blade voor de virtuele machine op **Verbinden**.
   
    ![Schermafbeelding van de Azure-portal waarin wordt getoond hoe u verbinding maakt met uw virtuele machine.](./media/virtual-machines-windows-connect-logon/connect.png)
   
   > [!TIP]
   > Als de knop **Verbinden** in de portal grijs is en u niet met Azure bent verbonden via een [Snelle route](../expressroute/expressroute-introduction.md) of een [VPN-tussen-sites](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)-verbinding, moet u een openbaar IP-adres maken en toewijzen aan uw virtuele machine voordat u Extern bureaublad kunt gebruiken. Er is meer informatie over [openbare IP-adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md) beschikbaar.
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Volgende stappen
Zie [Problemen oplossen met Extern bureaublad-verbindingen](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) als u problemen hebt wanneer u verbinding probeert te maken. Dit artikel leidt u door het opsporen en oplossen van veelvoorkomende problemen.




<!--HONumber=Nov16_HO2-->


