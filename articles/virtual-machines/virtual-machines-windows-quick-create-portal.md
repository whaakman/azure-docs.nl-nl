---
title: Azure Quick Start - Een Windows VM-portal maken | Microsoft Docs
description: Azure Quick Start - Een Windows VM-portal maken
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: a13ac5ab425ccbbe53d77cb9f5a8ebf02d009370
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Een virtuele Windows-machine maken met Azure Portal

Virtuele Azure-machines kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken en configureren van virtuele machines en alle verwante resources. In deze Quick Start gaat u een virtuele machine maken via Azure Portal. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-virtual-machine"></a>Virtuele machine maken

2. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

3. Selecteer **Compute** op de blade **Nieuw**. Selecteer **Windows Server 2016 Datacenter** op de blade **Compute** en klik daarna op **Maken**.

4. Vul het formulier **Basisinformatie** in voor de virtuele machine. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Selecteer een **resourcegroep** of maak een nieuwe. Een resourcegroep is een logische container waarin Azure-resources worden gemaakt en waarin ze collectief worden beheerd. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/virtual-machine-quick-start/create-windows-vm-portal-basic-blade.png)  

5. Kies een grootte voor de virtuele machine en klik op **Selecteren**. 

6. Op de blade Instellingen selecteert u **Ja** onder **Managed Disks gebruiken**. Laat voor de rest de standaardinstellingen staan en klik op **OK**.

7. Klik op de pagina Overzicht op **OK** om de implementatie van de virtuele machine te starten.

8. Voor het bewaken van de implementatiestatus klikt u op de virtuele machine. U vindt de virtuele machine op het Azure Portal-dashboard, of door **Virtuele machines** te selecteren in het menu links. Wanneer de virtuele machine is gemaakt, verandert de status van **Implementeren** in **In uitvoering**.

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

1. Klik op de knop **Verbinden** op de blade van de virtuele machine. Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

3. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**.

4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

## <a name="delete-virtual-machine"></a>De virtuele machine verwijderen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. Hiervoor selecteert u de resourcegroep op de blade van de virtuele machine en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor de installatie van een rol en de configuratie van de firewall](./virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[CLI-voorbeelden voor VM-implementatie verkennen](./virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
