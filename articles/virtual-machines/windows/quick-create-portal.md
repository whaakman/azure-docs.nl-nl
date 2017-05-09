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
ms.date: 04/13/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: df6f1b86b706d58a5c07a4f3de43a1872da61511
ms.contentlocale: nl-nl
ms.lasthandoff: 05/03/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Een virtuele Windows-machine maken met Azure Portal

Virtuele Azure-machines kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken en configureren van virtuele machines en alle verwante resources. In deze Quick Start gaat u een virtuele machine maken via Azure Portal. Als de implementatie is voltooid, maken we verbinding met de server en installeren we ISS.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-virtual-machine"></a>Virtuele machine maken

2. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

3. Selecteer **Compute** op de blade **Nieuw**. Selecteer *Windows Server 2016 Datacenter* op de blade **Compute** en klik daarna op **Maken**.

4. Vul het formulier **Basisinformatie** in voor de virtuele machine. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Selecteer een **resourcegroep** of maak een nieuwe. Een resourcegroep is een logische container waarin Azure-resources worden gemaakt en waarin ze collectief worden beheerd. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

5. Kies een grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 

    ![Schermopname van VM-grootten](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

6. Op de blade Instellingen selecteert u *Ja* onder **Managed Disks gebruiken**. Laat voor de rest de standaardinstellingen staan en klik op **OK**.

7. Klik op de pagina Overzicht op **OK** om de implementatie van de virtuele machine te starten.

8. Voor het bewaken van de implementatiestatus klikt u op de virtuele machine. U vindt de virtuele machine op het Azure Portal-dashboard, of door **Virtuele machines** te selecteren in het menu links. Wanneer de virtuele machine is gemaakt, verandert de status van *Implementeren* in *In uitvoering*.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Als u verkeer voor IIS wilt toestaan, moet u poort 80 openen voor webverkeer. Deze stap helpt u bij het maken van een regel voor een netwerkbeveiligingsgroep (NSG) om binnenkomende verbindingen op poort 80 toe te staan.

1. Ga op de blade van de virtuele machine naar de sectie **Essentials** en klik op de naam van de **resourcegroep**.
2. Klik op de blade van de resourcegroep op de **netwerkbeveiligingsgroep** in de lijst met resources. De naam van de netwerkbeveiligingsgroep moet bestaan uit de naam van de VM met *-nsg* toegevoegd aan het einde.
3. Klik op de kop **Binnenkomende beveiligingsregel** om de lijst met regels voor binnenkomende verbindingen te openen. De lijst moet al een regel voor RDP bevatten.
4. Klik op **+ Toevoegen** om de blade **Inkomende beveiligingsregel toevoegen** te openen.
5. In typ bij **Naam** *IIS*. Zorg ervoor dat het **poortbereik** is ingesteld op *80* en dat **Actie** is ingesteld op *Toestaan*. Klik op **OK**.


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

1. Klik op de knop **Verbinden** op de blade van de virtuele machine. Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

3. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**.

4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.


## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

Open op de virtuele machine een PowerShell-opdrachtprompt en voer de volgende opdracht uit om IIS te installeren en de lokale firewall-regel in te schakelen om webverkeer toe te staan:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Kijk op de blade voor de virtuele machine wat de waarde is voor *openbare IP-adres* en gebruik dit adres om naar de standaardwebpagina te gaan. 

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>De virtuele machine verwijderen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. Hiervoor selecteert u de resourcegroep op de blade van de virtuele machine en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor de installatie van een rol en de configuratie van de firewall](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[CLI-voorbeelden voor VM-implementatie verkennen](cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

