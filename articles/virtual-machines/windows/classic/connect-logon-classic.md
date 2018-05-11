---
title: Meld u aan bij een klassieke virtuele machine in Azure | Microsoft Docs
description: De Azure portal gebruiken om aan te melden met een Windows-virtuele machine gemaakt met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 87ecc65d2d4802ae826f3260b66b26e0bbe414e6
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Aanmelden bij een virtuele Windows-machine via de Azure-portal
In de Azure-portal, gebruikt u de **Connect** om te beginnen een extern bureaublad-sessie en meld u aan bij een virtuele machine van Windows.

Wilt u toch verbinding maken met een Linux-VM? Zie [aanmelden met een virtuele machine met Linux](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over het aanmelden bij een virtuele machine met het Resource Manager-model [hier](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine
1. Meld u aan bij Azure Portal.
2. Klik op de virtuele machine die u wilt openen. De naam wordt weergegeven in de **alle resources** deelvenster.

    ![Virtuele-machine-locaties](./media/connect-logon/azureportaldashboard.png)

1. Klik op de **Connect** knop op de eigenschappenpagina van de virtuele machine. 
2. In de **verbinding maken met virtuele machine** pagina, houden selecteert de gewenste opties en klik op **downloaden RDP-bestand**.
2. Open het gedownloade RDP-bestand en klik op **Connect** wanneer u wordt gevraagd. 
2. U ontvangt een waarschuwing dat het `.rdp`-bestand van een onbekende uitgever is. Dit is normaal. Klik in het venster Extern bureaublad op **Verbinden** om door te gaan.
   
    ![Schermafbeelding met waarschuwing over een onbekende uitgever](./media/connect-logon/rdp-warn.png)
3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de referenties voor een account op de virtuele machine en klik vervolgens op **OK**.
   
     **Lokaal account** - Dit is meestal de gebruikersnaam en het wachtwoord van het lokaal account dat u hebt opgegeven toen u de virtuele machine hebt gemaakt. In dit geval is het domein de naam van de virtuele machine en het is ingevoerd als *vmname*&#92;*gebruikersnaam*.  
   
    **In het domein opgenomen VM** - Als de virtuele machine deel uitmaakt van een domein, voert u de gebruikersnaam in met de volgende indeling: *Domein*&#92;*Gebruikersnaam*. Het account moet bovendien in de groep Administrators staan of er moet een machtiging voor externe toegang zijn verleend aan de VM.
   
    **Domeincontroller** - Als de VM een domeincontroller is, gebruikt u de gebruikersnaam en het wachtwoord van een domeinbeheerdersaccount voor het domein.
4. Klik op **Ja** om de identiteit van de virtuele machine te controleren en het aanmelden te voltooien.
   
   ![Schermafbeelding met een bericht over het verifiëren van de identiteit van de virtuele machine](./media/connect-logon/cert-warning.png)

## <a name="next-steps"></a>Volgende stappen
* Als de **Connect** knop is niet actief of er andere problemen met de extern bureaublad-verbinding, probeert u de configuratie opnieuw ingesteld. Klik op **externe toegang opnieuw instellen** vanuit het dashboard van de virtuele machine.

    ![Opnieuw instellen van externe toegang](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Probeer de fabrieksinstellingen voor problemen met het wachtwoord. Klik op **wachtwoord opnieuw instellen** aan de linkerkant van de virtuele machine dashboard onder **ondersteuning + probleemoplossing**.

    ![Wachtwoord opnieuw instellen](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Als deze tips werken niet of niet wat u nodig hebt, gaat u naar [problemen met extern bureaublad-verbindingen naar een op basis van Windows Azure virtuele Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dit artikel leidt u door het opsporen en oplossen van veelvoorkomende problemen.
