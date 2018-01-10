---
title: Meld u aan bij een klassieke virtuele machine in Azure | Microsoft Docs
description: De Azure portal gebruiken om aan te melden met een Windows-virtuele machine gemaakt met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
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
ms.openlocfilehash: c35bfeaa81f89dc5a636d67ed9a2a930d689c54a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
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

3. Klik op **Connect** op de opdrachtbalk op het dashboard van de virtuele machine.

    ![Pictogram voor de virtuele machine verbinding maken](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## <a name="log-on-to-the-virtual-machine"></a>Aanmelden bij de virtuele machine
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Volgende stappen
* Als de **Connect** knop is niet actief of er andere problemen met de extern bureaublad-verbinding, probeert u de configuratie opnieuw ingesteld. Klik op **externe toegang opnieuw instellen** vanuit het dashboard van de virtuele machine.

    ![Opnieuw instellen van externe toegang](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Probeer de fabrieksinstellingen voor problemen met het wachtwoord. Klik op **wachtwoord opnieuw instellen** aan de linkerkant van de virtuele machine dashboard onder **ondersteuning + probleemoplossing**.

    ![Wachtwoord opnieuw instellen](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Als deze tips werken niet of niet wat u nodig hebt, gaat u naar [problemen met extern bureaublad-verbindingen naar een op basis van Windows Azure virtuele Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dit artikel leidt u door het opsporen en oplossen van veelvoorkomende problemen.
