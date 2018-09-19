---
title: Subnet verkeersrouteringsmethode configureren met behulp van Azure Traffic Manager | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u Traffic Manager configureren om verkeer te routeren van specifieke subnetten.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jpconnock
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: kumud
ms.openlocfilehash: 6e5e6008741306d322ebd07bcbf144133ca4e632
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131278"
---
# <a name="direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Direct verkeer naar specifieke eindpunten op basis van gebruiker subnet met Traffic Manager

In dit artikel wordt beschreven hoe u de verkeersrouteringsmethode voor een subnet configureren. De **Subnet** verkeersrouteringsmethode kunt u een reeks IP-adresbereiken worden toegewezen aan specifieke eindpunten en wanneer een aanvraag wordt ontvangen door Traffic Manager, inspecteert de bron-IP-adres van de aanvraag en retourneert het eindpunt dat is gekoppeld. 

In het scenario beschreven in dit artikel, met behulp van subnet routering, afhankelijk van het IP-adres van de query van de gebruiker, wordt een verkeer doorgestuurd naar een interne of een productiewebsite.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt de Traffic Manager in actie bekijken, wordt in deze zelfstudie moet u het volgende implementeert:
- twee fundamentele websites die worden uitgevoerd in verschillende regio's Azure - **VS-Oost** (fungeert als interne website) en **West-Europa** (fungeert als de productiewebsite van de).
- twee test-VM's voor het testen van het Traffic Manager - een virtuele machine in **VS-Oost** en de tweede virtuele machine in **West-Europa**. 

De test-VM's worden gebruikt om te laten zien hoe Traffic Manager gebruikersverkeer routeert naar de interne website of de productiewebsite op basis van subnet van waaruit de gebruikersquery afkomstig is.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.

### <a name="create-websites"></a>Websites maken

In deze sectie maakt u twee website-instanties die de twee service-eindpunten voor Traffic Manager-profiel in twee Azure-regio's bieden. Het maken van de twee websites, bevat de volgende stappen uit:
1. Maak twee VM's voor het uitvoeren van een basic-website, één in **VS-Oost**, en de andere in **West-Europa**.
2. IIS-server installeren op elke virtuele machine en werk de standaardpagina van de website die wordt beschreven van de naam van de VM die een gebruiker is verbonden met wanneer de website te bezoeken.

#### <a name="create-vms-for-running-websites"></a>Virtuele machines maken voor het uitvoeren van websites
In deze sectie maakt u twee virtuele machines maken *myEndpointVMEastUS* en *myEndpointVMWEurope* in de **VS-Oost** en **West-Europa** Azure de regio's.

1. In de rechterbovenhoek linkerbovenhoek van de Azure portal, selecteer **een resource maken** > **Compute** > **virtuele machine met Windows Server 2016**.
2. Voer de volgende informatie voor **Basisinformatie** in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myIISVMEastUS|
    |Gebruikersnaam| Voer een gebruikersnaam naar keuze in.|
    |Wachtwoord| Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **nieuw** en typ vervolgens *myResourceGroupTM1*.|
    |Locatie| Selecteer **US - oost**.|
    |||
4. Selecteer een VM-grootte onder **Kies een grootte**.
5. Selecteer de volgende waarden voor **Instellingen** en selecteer **OK**:
    
    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk| Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet1*, voor het subnet, voer  *mySubnet*.|
    |Netwerkbeveiligingsgroep|Selecteer **Basic**, en in **openbare binnenkomende poorten selecteren** vervolgkeuzelijst, selecteer **HTTP** en **RDP** |
    |Diagnostische gegevens over opstarten|Selecteer **uitgeschakelde**.|
    |||
6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

7. Volg de stappen 1-6 nogmaals, met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |Resourcegroep | Selecteer **Nieuw** en typ *myResourceGroupTM2*|
    |Locatie|Europa -west|
    |VM-naam | myIISVMWEurope|
    |Virtueel netwerk | Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet2*, voor het subnet, voer  *mySubnet*.|
    |||
8. Het maken van de VM's duurt enkele minuten. Ga niet verder met de overige stappen voordat beide VM's zijn gemaakt.

   ![Een virtuele machine maken](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS installeren en de standaardwebpagina aanpassen

In deze sectie maakt u de IIS-server installeren op de twee virtuele machines - *myIISVMEastUS*  & *myIISVMWEurope*, en werk vervolgens de standaardpagina van de website. De aangepaste website wordt de naam van de virtuele machine waarmee u verbinding maakt wanneer u de website via een webbrowser bezoekt.

1. Selecteer **alle resources** in het menu links en klik vervolgens in de lijst met resources op *myIISVMEastUS* die bevindt zich in de *myResourceGroupTM1* resourcegroep.
2. Op de **overzicht** pagina, klikt u op **Connect**, en klik vervolgens in **verbinding maken met virtuele machine**, selecteer **downloaden RDP-bestand**. 
3. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding.
6. Ga op de serverdesktop naar **Windows Systeembeheer**>**Serverbeheer**.
7. Start Windows PowerShell op *myIISVMEastUS* en IIS-server installeren en bijwerken van de standaard htm-bestand met de volgende opdrachten.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my test website server - " + $env:computername)
    ```
8. Sluit de RDP-verbinding met *myIISVMEastUS*.
9. Herhaal stap 1-6 met door het maken van een RDP-verbinding met de virtuele machine *myIISVMWEurope* binnen de *myResourceGroupTM2* resourcegroep IIS installeren en aanpassen van de standaard-webpagina.
10. Start Windows PowerShell op *myIISVMWEurope* en IIS-server installeren en bijwerken van de standaard htm-bestand met de volgende opdrachten.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my production website server - " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configureren van DNS-namen voor de virtuele machines waarop IIS wordt uitgevoerd

Traffic Manager routes gebruikersverkeer op basis van DNS-naam van de service-eindpunten. In deze sectie configureert u de DNS-namen voor de IIS-servers - *myIISVMEastUS* en *myIISVMWEurope*.

1. Klik op **alle resources** selecteren in het menu links en klik in de lijst met resources *myIISVMEastUS* die bevindt zich in de *myResourceGroupTM1* resourcegroep.
2. Op de **overzicht** pagina onder **DNS-naam**, selecteer **configureren**.
3. Op de **configuratie** pagina, onder het label van DNS-naam, Voeg een unieke naam en selecteer vervolgens **opslaan**.
4. Herhaal stap 1-3 voor de virtuele machine met de naam *myIISVMWEurope* die bevindt zich in de *myResourceGroupTM1* resourcegroep.

### <a name="create-test-vms"></a>Maken van test-VM's

In deze sectie maakt u een virtuele machine maken (*mVMEastUS* en *myVMWestEurope*) in elke Azure-regio (**VS-Oost** en **West-Europa**. U gebruikt deze VM's om te testen hoe Traffic Manager verkeer routeert naar de dichtstbijzijnde IIS-server wanneer u naar de website bladert.

1. In de rechterbovenhoek linkerbovenhoek van de Azure portal, selecteer **een resource maken** > **Compute** > **virtuele machine met Windows Server 2016**.
2. Voer de volgende informatie voor **Basisinformatie** in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVMEastUS|
    |Gebruikersnaam| Voer een gebruikersnaam naar keuze in.|
    |Wachtwoord| Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **Bestaande** en selecteer vervolgens *myResourceGroupTM1*.|
    |||

4. Selecteer een VM-grootte onder **Kies een grootte**.
5. Selecteer de volgende waarden voor **Instellingen** en selecteer **OK**:
    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk| Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet3*, voor het subnet, voer  *mySubnet3*.|
    |Netwerkbeveiligingsgroep|Selecteer **Basic**, en in **openbare binnenkomende poorten selecteren** vervolgkeuzelijst, selecteer **HTTP** en **RDP** |
    |Diagnostische gegevens over opstarten|Selecteer **uitgeschakelde**.|
    |||

6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

7. Volg de stappen 1-5 nogmaals, met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |VM-naam | *myVMWEurope*|
    |Resourcegroep | Selecteer **bestaande**, en typ vervolgens *myResourceGroupTM2*|
    |Virtueel netwerk | Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet4*, voor het subnet, voer  *mySubnet4*.|
    |||

8. Het maken van de VM's duurt enkele minuten. Ga niet verder met de overige stappen voordat beide VM's zijn gemaakt.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
Een Traffic Manager-profiel waarmee u specifieke eindpunten op basis van de bron-IP-adres van de aanvraag terug kunt maken.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
2. Voer in  **Traffic Manager-profiel maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Naam                   | Deze naam moet uniek zijn binnen de zone trafficmanager.net en de resultaten in de DNS-naam, trafficmanager.net, die wordt gebruikt voor toegang tot uw Traffic Manager-profiel.                                   |
    | Routeringsmethode          | Selecteer de **Subnet** routeringsmethode.                                       |
    | Abonnement            | Selecteer uw abonnement.                          |
    | Resourcegroep          | Selecteer **bestaande** en voer *myResourceGroupTM1*. |
    | |                              |
    |
  
    ![Een Traffic Manager-profiel maken](./media/traffic-manager-subnet-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

Toevoegen van de twee virtuele machines met de IIS-servers - *myIISVMEastUS*  & *myIISVMWEurope* gebruiker om verkeer te routeren op basis van het subnet van de query van de gebruiker.

1. Zoek in de zoekbalk van de portal de naam van het Traffic Manager-profiel dat u in de vorige sectie hebt gemaakt en selecteer het profiel in de weergegeven resultaten.
2. Klik in **Traffic Manager-profiel**, in de sectie **Instellingen**, op **Eindpunten** en vervolgens op **Toevoegen**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Type                    | Azure-eindpunt                                   |
    | Naam           | myTestWebSiteEndpoint                                        |
    | Doelbrontype           | Openbaar IP-adres                          |
    | Doelbron          | **Kies een openbaar IP-adres** om de lijst met resources met openbare IP-adressen onder hetzelfde abonnement weer te geven. In **Resource**, selecteert u het openbare IP-adres met de naam *myIISVMEastUS-IP-*. Dit is het openbare IP-adres van de IIS-server-VM in VS-Oost.|
    |  Routering-subnet-instellingen    |   Voeg de IP-adres van *myVMEastUS* testen van de virtuele machine. Elke gebruikersquery die afkomstig zijn van deze virtuele machine worden omgeleid naar de *myTestWebSiteEndpoint*.    |

4. Herhaal stappen 2 en 3 om toe te voegen een ander eindpunt met de naam *myProductionEndpoint* voor het openbare IP-adres *myIISVMWEurope-IP-* dat is gekoppeld aan de virtuele machine met de naam van de IIS-server *myIISVMWEurope* . Voor **routering subnetinstellingen**, voeg de IP-adres van de test-VM - *myVMWestEurope*. Elke gebruikersquery van deze virtuele machine worden doorgestuurd naar het eindpunt - test *myProductionWebsiteEndpoint*.
5.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

    ![Traffic Manager-eindpunt toevoegen](./media/traffic-manager-subnet-routing-method/customize-endpoint-with-subnet-routing-eastus.png)
  
## <a name="test-traffic-manager-profile"></a>Traffic Manager-profiel testen
In deze sectie maakt testen u hoe Traffic Manager routeert gebruikersverkeer van een bepaald subnet naar een bepaald eindpunt. Als u wilt de Traffic Manager in actie weergeven, voert u de volgende stappen uit:
1. Bepaal de DNS-naam van uw Traffic Manager-profiel.
2. Traffic Manager als volgt in actie weergeven:
    - Van de virtuele testmachine (*myVMEastUS*) dat zich bevindt in de **VS-Oost** regio, in een webbrowser, blader naar de DNS-naam van uw Traffic Manager-profiel.
    - Van de virtuele testmachine (*myVMEastUS*) dat zich bevindt in de **West-Europa** regio, in een webbrowser, blader naar de DNS-naam van uw Traffic Manager-profiel.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Bepalen van de DNS-naam van Traffic Manager-profiel
In deze zelfstudie kunt voor het gemak u de DNS-naam van het Traffic Manager-profiel gaat u naar de websites. 

U kunt de DNS-naam van het Traffic Manager-profiel als volgt bepalen:

1.  Zoek in de zoekbalk van de portal de naam van het **Traffic Manager-profiel** dat u in de vorige sectie hebt gemaakt. Klik op het Traffic Manager-profiel in de resultaten die worden weergegeven.
1. Klik op **Overzicht**.
2. Het **Traffic Manager-profiel** geeft de DNS-naam weer van het Traffic Manager-profiel dat u zojuist hebt gemaakt. In productie-implementaties configureert u een aangepaste domeinnaam om te verwijzen naar de Traffic Manager-domeinnaam, met behulp van een DNS CNAME-record.

   ![DNS-naam van Traffic Manager](./media/traffic-manager-subnet-routing-method/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager in werking zien
In deze sectie ziet u het Traffic Manager is de actie. 

1. Selecteer **alle resources** in het menu links en klik vervolgens in de lijst met resources op *myVMEastUS* die bevindt zich in de *myResourceGroupTM1* resourcegroep.
2. Op de **overzicht** pagina, klikt u op **Connect**, en klik vervolgens in **verbinding maken met virtuele machine**, selecteer **downloaden RDP-bestand**. 
3. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding. 
1. In een webbrowser op de virtuele machine *myVMEastUS*, typt u de DNS-naam van uw Traffic Manager-profiel om uw website weer te geven. Nadat de virtuele machine *myVMEastUS* IP-adres is gekoppeld aan het eindpunt *myIISVMEastUS*, de webbrowser wordt gestart voor de website testserver - *myIISVMEastUS*.

   ![Traffic Manager-profiel testen](./media/traffic-manager-subnet-routing-method/test-traffic-manager.png)

2. Vervolgens maakt u verbinding met de virtuele machine *myVMWestEurope* zich in **West-Europa** met behulp van de stappen 1-5 en blader naar de domeinnaam van het Traffic Manager-profiel van deze virtuele machine. Nadat de virtuele machine *myVMWestEurope* IP-adres is gekoppeld aan het eindpunt *myIISVMEastUS*, de webbrowser wordt gestart voor de website testserver - *myIISVMWEurope*. 
  
## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager-profiel verwijderen
Wanneer u niet meer nodig hebt, verwijdert u de resourcegroepen (**ResourceGroupTM1** en **ResourceGroupTM2**). Om dit te doen, selecteert u de resourcegroep (**ResourceGroupTM1** of **ResourceGroupTM2**), en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [verkeersrouteringsmethode gewogen](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [prioriteit routeringsmethode](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [geografische routeringsmethode](traffic-manager-configure-geographic-routing-method.md).


