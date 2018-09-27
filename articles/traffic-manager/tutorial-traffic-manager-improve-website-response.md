---
title: Zelfstudie - verkeer routeren voor het verbeteren van de website-antwoord met behulp van Azure Traffic Manager | Microsoft Docs
description: Deze zelfstudie artikel wordt beschreven hoe u een Traffic Manager-profiel voor het bouwen van een zeer responsieve website te maken.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: jeconnoc
editor: ''
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: kumud
ms.openlocfilehash: 087dcda5826d96ad064c472fc897be7e61133387
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392502"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Zelfstudie: Verbeteren met Traffic Manager website-antwoord 

In deze zelfstudie wordt beschreven hoe u Traffic Manager gebruiken om te maken van een zeer responsieve website door te leiden van gebruikersverkeer naar de website met de laagste latentie. Het datacenter met de laagste latentie is normaal gesproken degene die zich het dichtst in de geografische afstand.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van twee virtuele machines met een basic-website op IIS
> * Test-twee virtuele machines om weer te geven van Traffic Manager in de actie maken
> * Configureren van DNS-naam voor de virtuele machines waarop IIS wordt uitgevoerd
> * Voor de prestaties verbeterd website een Traffic Manager-profiel maken
> * VM-eindpunten toevoegen aan Traffic Manager-profiel
> * Traffic Manager in werking zien

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt de Traffic Manager in actie bekijken, wordt in deze zelfstudie moet u het volgende implementeert:
- twee exemplaren van basic websites die worden uitgevoerd in verschillende regio's Azure - **VS-Oost** en **West-Europa**.
- twee test-VM's voor het testen van het Traffic Manager - een virtuele machine in **VS-Oost** en de tweede virtuele machine in **West-Europa**. De test-VM's worden gebruikt om te laten zien hoe Traffic Manager gebruikersverkeer routeert naar de website die wordt uitgevoerd in dezelfde regio bevinden als het biedt de laagste latentie.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.

### <a name="create-websites"></a>Websites maken

In deze sectie maakt u twee website-instanties die de twee service-eindpunten voor Traffic Manager-profiel in twee Azure-regio's bieden. Het maken van de twee websites, bevat de volgende stappen uit:
1. Maak twee VM's voor het uitvoeren van een basic-website, één in **VS-Oost**, en de andere in **West-Europa**.
2. IIS-server installeren op elke virtuele machine en werk de standaardpagina van de website die wordt beschreven van de naam van de VM die een gebruiker is verbonden met wanneer de website te bezoeken.

#### <a name="create-vms-for-running-websites"></a>Virtuele machines maken voor het uitvoeren van websites
In deze sectie maakt u twee virtuele machines maken *myIISVMEastUS* en *myIISVMWEurope* in de **VS-Oost** en **West-Europa** Azure-regio's.

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
7. Start Windows PowerShell op VM1 en gebruik de volgende opdrachten om de IIS-server te installeren en het standaard htm-bestand bij te werken.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![IIS installeren en aanpassen van webpagina](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. Sluit de RDP-verbinding met *myIISVMEastUS*.
9. Herhaal stap 1-8 met door het maken van een RDP-verbinding met de virtuele machine *myIISVMWEurope* binnen de *myResourceGroupTM2* resourcegroep IIS installeren en aanpassen van de standaard-webpagina.

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
    |Virtueel netwerk| Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet3*, voor het subnet, voer  *mySubnet*.|
    |Netwerkbeveiligingsgroep|Selecteer **Basic**, en in **openbare binnenkomende poorten selecteren** vervolgkeuzelijst, selecteer **HTTP** en **RDP** |
    |Diagnostische gegevens over opstarten|Selecteer **uitgeschakelde**.|
    |||

6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

7. Volg de stappen 1-5 nogmaals, met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |VM-naam | *myVMWEurope*|
    |Resourcegroep | Selecteer **bestaande**, en typ vervolgens *myResourceGroupTM2*|
    |Virtueel netwerk | Selecteer **virtueel netwerk**in **virtueel netwerk maken**, voor **naam**, voer *myVNet4*, voor het subnet, voer  *mySubnet*.|
    |||

8. Het maken van de VM's duurt enkele minuten. Ga niet verder met de overige stappen voordat beide VM's zijn gemaakt.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
Maak een Traffic Manager-profiel met gebruikersverkeer verwezen door deze te verzenden naar het eindpunt met de laagste latentie.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
2. Voer in  **Traffic Manager-profiel maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Naam                   | Deze naam moet uniek zijn binnen de zone trafficmanager.net en de resultaten in de DNS-naam, trafficmanager.net, die wordt gebruikt voor toegang tot uw Traffic Manager-profiel.                                   |
    | Routeringsmethode          | Selecteer de **prestaties** routeringsmethode.                                       |
    | Abonnement            | Selecteer uw abonnement.                          |
    | Resourcegroep          | Selecteer **nieuw** en voer *myResourceGroupTM1*. |
    | Locatie                | Selecteer **US - oost**.  Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.                              |
    |
  
    ![Een Traffic Manager-profiel maken](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

Toevoegen van de twee virtuele machines met de IIS-servers - *myIISVMEastUS*  & *myIISVMWEurope* gebruiker om verkeer te routeren naar de dichtstbijzijnde eindpunt voor de gebruiker.

1. Zoek in de zoekbalk van de portal de naam van het Traffic Manager-profiel dat u in de vorige sectie hebt gemaakt en selecteer het profiel in de weergegeven resultaten.
2. Klik in **Traffic Manager-profiel**, in de sectie **Instellingen**, op **Eindpunten** en vervolgens op **Toevoegen**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Type                    | Azure-eindpunt                                   |
    | Naam           | myEastUSEndpoint                                        |
    | Doelbrontype           | Openbaar IP-adres                          |
    | Doelbron          | **Kies een openbaar IP-adres** om de lijst met resources met openbare IP-adressen onder hetzelfde abonnement weer te geven. In **Resource**, selecteert u het openbare IP-adres met de naam *myIISVMEastUS-IP-*. Dit is het openbare IP-adres van de IIS-server-VM in VS-Oost.|
    |        |           |

4. Herhaal stappen 2 en 3 om toe te voegen een ander eindpunt met de naam *myWestEuropeEndpoint* voor het openbare IP-adres *myIISVMWEurope-IP-* dat is gekoppeld aan de virtuele machine met de naam van de IIS-server *myIISVMWEurope* .
5.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

    ![Traffic Manager-eindpunt toevoegen](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)
  

## <a name="test-traffic-manager-profile"></a>Traffic Manager-profiel testen
In deze sectie maakt testen u hoe Traffic Manager gebruikersverkeer routeert naar de dichtstbijzijnde virtuele machines met de website voor minimale latentie. Als u wilt de Traffic Manager in actie weergeven, voert u de volgende stappen uit:
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

   ![DNS-naam van Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager in werking zien
In deze sectie ziet u het Traffic Manager is de actie. 

1. Selecteer **alle resources** in het menu links en klik vervolgens in de lijst met resources op *myVMEastUS* die bevindt zich in de *myResourceGroupTM1* resourcegroep.
2. Op de **overzicht** pagina, klikt u op **Connect**, en klik vervolgens in **verbinding maken met virtuele machine**, selecteer **downloaden RDP-bestand**. 
3. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding. 
1. In een webbrowser op de virtuele machine *myVMEastUS*, typt u de DNS-naam van uw Traffic Manager-profiel om uw website weer te geven. Omdat de virtuele machine zich in **VS-Oost**, u worden doorgestuurd naar de dichtstbijzijnde website die wordt gehost op de dichtstbijzijnde IIS-server *myIISVMEastUS* die bevindt zich in **VS-Oost**.

   ![Traffic Manager-profiel testen](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Vervolgens maakt u verbinding met de virtuele machine *myVMWestEurope* zich in **West-Europa** met behulp van de stappen 1-5 en blader naar de domeinnaam van het Traffic Manager-profiel van deze virtuele machine.  Omdat de virtuele machine zich in **West-Europa**, u worden nu doorgestuurd naar de website die wordt gehost op dichtst bij de IIS-server *myIISVMWEurope* die bevindt zich in **West-Europa**. 

   ![Traffic Manager-profiel testen](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager-profiel verwijderen
Wanneer u niet meer nodig hebt, verwijdert u de resourcegroepen (**ResourceGroupTM1** en **ResourceGroupTM2**). Om dit te doen, selecteert u de resourcegroep (**ResourceGroupTM1** of **ResourceGroupTM2**), en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verkeer naar een set eindpunten distribueren](traffic-manager-configure-weighted-routing-method.md)


