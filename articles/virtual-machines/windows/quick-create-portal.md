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
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 98ed23b8031c56ba642427e817e48a4e7a5830fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Een virtuele Windows-machine maken met Azure Portal

Virtuele Azure-machines kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken en configureren van virtuele machines en alle verwante resources. In deze Quick Start gaat u een virtuele machine maken en een webserver installeren op de VM.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 

3. Geef de informatie van de virtuele machine op. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 

    ![Schermopname van VM-grootten](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. Handhaaf de standaardwaarden in **Instellingen** en klik op **OK**. 

6. Klik op de pagina Overzicht op **OK** om de implementatie van de virtuele machine te starten.

7. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend.


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Maak via een extern bureaublad verbinding met de virtuele machine.

1. Klik op de knop **Verbinden** in de eigenschappen van de virtuele machine. Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

3. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**.

4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.


## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

Start op de virtuele machine een PowerShell-sessie en voer de volgende opdracht uit om IIS te installeren.

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wanneer u klaar bent, sluit u de RDP-sessie af en geeft u de VM-eigenschappen op in de Azure Portal.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Een netwerkbeveiligingsgroep (NSG) beveiligt binnenkomend en uitgaand verkeer. Wanneer een VM wordt gemaakt in de Azure Portal, wordt een inkomende regel gemaakt op poort 3389 voor RDP-verbindingen. Omdat deze VM fungeert als host voor een webserver, moet een NSG-regel worden gemaakt voor poort 80.

1. Klik op de virtuele machine op de naam van de **Resourcegroep**.
2. Selecteer de **netwerkbeveiligingsgroep**. De NSG kan worden geïdentificeerd met behulp van de kolom **Type**. 
3. Klik op het menu links bij instellingen op **Beveiligingsregels voor binnenkomend verkeer**.
4. Klik op **Toevoegen**.
5. Typ bij **Naam** **http**. Zorg ervoor dat het **poortbereik** is ingesteld op 80 en **Actie** is ingesteld op **Toestaan**. 
6. Klik op **OK**.


## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Als IIS is geïnstalleerd en poort 80 is geopend voor de VM, is de webserver nu toegankelijk vanaf het internet. Open een webbrowser en voer het openbare IP-adres van de VM in. U vindt het openbare IP-adres onder *Virtual Machines* in Azure Portal.

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. Om dit te doen, selecteert u de resourcegroep voor de VM en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Snel starten hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkbeveiligingsgroepregel gemaakt en een webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
