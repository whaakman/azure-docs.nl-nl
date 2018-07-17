---
title: 'Snelstart: Een virtuele Windows-machine maken op de Azure Portal | Microsoft Docs'
description: In deze snelstartgids leert u hoe u de Azure Portal  gebruikt om een virtuele Windows-machine te maken
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/03/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f5a92d421bbf7bfe485252c148d5f64ae2fb8e23
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916112"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Snelstart: Een virtuele Windows-machine maken op de Azure Portal

Virtuele Azure-machines (VM's)kunnen worden gemaakt via Azure Portal. Deze methode biedt een gebruikersinterface op basis van een browser voor het maken van VM's en van alle verwante resources. In deze snelstart wordt beschreven hoe u de Azure Portal gebruikt voor het implementeren van een virtuele machine (VM) in Azure waarop Windows Server 2016 wordt uitgevoerd. Om uw VM in actie te zien, voert u een externe bureaubladsessie voor de virtuele machine uit en installeert u de IIS-webserver.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Kies in de linkerbovenhoek van Azure Portal **Een resource maken**.

2. Zoek via het zoekvak boven de lijst met Azure Marketplace-resources naar **Windows Server 2016 Datacenter**, selecteer dit en kies **Maken**.

3. Geef een VM-naam op, zoals *myVM*, laat het schijftype als *SSD* en geef dan een gebruikersnaam op, zoals *azuregebruiker*. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](faq.md#what-are-the-password-requirements-when-creating-a-vm).

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. Kies voor het maken van een **nieuwe** resourcegroep en geef vervolgens een naam op, zoals *myResourceGroup*. Kies uw **locatie** en selecteer dan **OK**.

4. Selecteer een grootte voor de VM. U kunt bijvoorbeeld filteren op *Rekentype* of *Schijftype*. Een voorgestelde VM-grootte is *D2s_v3*. Klik op **Selecteren** nadat u een grootte hebt gekozen.

    ![Schermopname van VM-grootten](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. Ga op de pagina **Instellingen** naar **Netwerk** > **Netwerkbeveiligingsgroep** > **Openbare binnenkomende poorten selecteren**, en selecteer **HTTP** en **RDP (3389)** in de vervolgkeuzelijst. Laat de overige standaardwaarden staan en selecteer **OK**.

6. Selecteer **Maken** op de overzichtspagina om de implementatie van de VM te starten.

7. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend.

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Maak via een extern bureaublad verbinding met de virtuele machine. Deze instructies geven aan hoe u vanaf een Windows-computer verbinding maakt met uw VM. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

1. Klik op de knop **Verbinden** op de eigenschappenpagina van de virtuele machine. 

    ![Verbinding maken met een Azure VM vanaf de portal](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. Laat op de pagina **Verbinding maken met virtuele machine** de standaardopties staan om verbinding te maken met de DNS-naam via poort 3389 en klik op **RDP-bestand downloaden**.

2. Open het gedownloade RDP-bestand en klik op **Verbinden** wanneer dit wordt gevraagd. 

3. Selecteer in het venster **Windows-beveiliging** **Meer opties** en vervolgens **Een ander account gebruiken**. Typ de gebruikersnaam als *naamvanvm*\\*gebruikersnaam*, voer het wachtwoord in dat u hebt gemaakt voor de virtuele machine, en klik vervolgens op **OK**.

4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om de verbinding te maken.

## <a name="install-web-server"></a>Webserver installeren

Als u uw VM in actie wilt zien, installeert u de IIS-webserver. Open een PowerShell-prompt op de virtuele machine en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wanneer u klaar bent, sluit u de externe-bureaubladverbinding met de virtuele machine.


## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Selecteer de VM in de portal en gebruik in het overzicht van de VM de knop **Klik om te kopiëren** rechts van het IP-adres om het adres te kopiëren. Plak het adres vervolgens in een tabblad van de browser. De standaardwelkomstpagina van IIS wordt geopend en deze ziet er als volgt uit:

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de VM niet meer nodig hebt, kunt u de resourcegroep, de machine zelf én alle gerelateerde resources verwijderen. Wanneer u dit wilt doen, selecteert u de resourcegroep voor de virtuele machine en selecteert u **Verwijderen**. Vervolgens bevestigt u de naam van de resourcegroep die u wilt verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkpoort geopend voor internetverkeer en een eenvoudige webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
