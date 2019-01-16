---
title: Methode voor routering van subnetverkeer configureren met behulp van Azure Traffic Manager
description: In dit artikel wordt uitgelegd hoe u Traffic Manager kunt configureren om verkeer van subnetten van gebruikers naar specifieke eindpunten te routeren.
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: kumud
ms.openlocfilehash: 8243130fc9752a47661b4c80826000d573da35c8
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053071"
---
# <a name="direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Verkeer naar specifieke eindpunten routeren met Traffic Manager op basis van subnets van gebruiker

In dit artikel wordt beschreven hoe u de verkeersrouteringsmethode op basis van subnetten kunt configureren. Met de **Subnet**-verkeersrouteringsmethode kunt u een reeks IP-adresbereiken aan specifieke eindpunten koppelen als een aanvraag door Traffic Manager wordt ontvangen; de methode inspecteert het bron-IP-adres van de aanvraag en retourneert het eindpunt dat daar aan is gekoppeld. 

In deze zelfstudie wordt verkeer met behulp van subnetroutering, afhankelijk van het IP-adres van de query van de gebruiker, naar een interne website of productiewebsite gerouteerd.


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Twee virtuele machines maken met daarop een eenvoudige website in IIS
> * Twee virtuele machines voor tests maken om Traffic Manager in actie te zien
> * DNS-naam configureren voor de VM’s met behulp van IIS
> * Een Traffic Manager-profiel maken om verkeer te routeren op basis van het subnet van een gebruiker
> * VM-eindpunten toevoegen aan het Traffic Manager-profiel
> * Traffic Manager in werking zien


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
U moet voor deze zelfstudie de volgende zaken implementeren om de Traffic Manager in actie te zien:
- twee basiswebsites in verschillende Azure-regio’s - **VS-Oost** (fungeert als interne website) en **Europa - west** (fungeert als productiewebsite).
- twee virtuele machines voor het testen van de Traffic Manager - Een VM in **VS-Oost** en de tweede VM in **Europa - west**. 

De virtuele machines voor de tests worden gebruikt om te laten zien hoe Traffic Manager verkeer van gebruikers naar de interne website of de productiewebsite routeert op basis van het subnet waar de query van de gebruiker vandaan komt.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.

### <a name="create-websites"></a>Websites maken

In dit gedeelte maakt u twee website-instanties die de twee service-eindpunten voor het Traffic Manager-profiel vormen in twee Azure-regio’s. Om de twee websites te maken, moeten de volgende stappen worden uitgevoerd:
1. Maak twee virtuele machines om een eenvoudige website uit te voeren: een in **VS-Oost** en de andere in **Europa - west**.
2. Installeer IIS-server op elke VM en werk de standaardpagina van de website bij die de naam beschrijft van de virtuele machine waarmee een gebruiker is verbonden als deze de website bezoekt.

#### <a name="create-vms-for-running-websites"></a>Virtuele machines maken voor het uitvoeren van websites
In dit gedeelte maakt u twee virtuele machines *InternalWebsite* en *ProdWebsite* in de Azure-regio’s **VS-Oost** en **Europa - west**.

1. Selecteer **Een resource maken** > **Compute** > **Virtuele machine met Windows Server 2016** in de linkerbovenhoek van Azure Portal.
2. Voer de volgende informatie voor **Basisinformatie** in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|InternalWebsite|
    |Gebruikersnaam| Voer een gebruikersnaam naar keuze in.|
    |Wachtwoord| Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **Nieuw** en typ *myResourceGroupTM1*.|
    |Locatie| Selecteer **US - oost**.|
    |||
4. Selecteer een VM-grootte onder **Kies een grootte**.
5. Selecteer de volgende waarden voor **Instellingen** en selecteer **OK**:
    
    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk| Selecteer **Virtueel netwerk** in **Virtueel netwerk maken** en voer voor **Naam** *myVNet1* en voor het subnet  *mySubnet* in.|
    |Netwerkbeveiligingsgroep|Selecteer **Basic** en selecteer in de vervolgkeuzelijst **Openbare binnenkomende poorten selecteren** de opties **HTTP** en **RDP** |
    |Diagnostische gegevens over opstarten|Selecteer **Uitgeschakeld**.|
    |||
6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

7. Volg de stappen 1-6 nogmaals, met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |Resourcegroep | Selecteer **Nieuw** en typ *myResourceGroupTM2*|
    |Locatie|Europa -west|
    |VM-naam | ProdWebsite|
    |Virtueel netwerk | Selecteer **Virtueel netwerk** in **Virtueel netwerk maken** en voer voor **Naam** *myVNet2* en voor het subnet  *mySubnet* in.|
    |||
8. Het maken van de VM's duurt enkele minuten. Ga niet verder met de overige stappen voordat beide VM's zijn gemaakt.

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS installeren en de standaardwebpagina aanpassen

In dit gedeelte installeert u de IIS-server op de twee virtuele machines - *myIISVMEastUS*  & *myIISVMWEurope* en werkt u daarna de standaardpagina van de website bij. De aangepaste websitepagina geeft de naam weer van de virtuele machine waarmee u verbinding maakt als u de website in een webbrowser bezoekt.

1. Selecteer in het linkermenu **Alle resources** en klik in de lijst met resources op *myIISVMEastUS*, die zich in de resourcegroep *myresourceGroupTM1* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** en selecteer vervolgens in **Verbinding maken met virtuele machine** de optie **RDP-bestand downloaden**. 
3. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding.
6. Ga op de serverdesktop naar **Windows Systeembeheer**>**Serverbeheer**.
7. Start Windows PowerShell op virtuele machine *InternalWebsite* en gebruik de volgende opdrachten om de IIS-server te installeren en het standaard htm-bestand bij te werken.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```
8. Sluit de RDP-verbinding met *InternalWebsite*.
9. Herhaal stap 1-6 door een RDP-verbinding met de virtuele machine *ProdWebsite* in de resourcegroep *myResourceGroupTM2* te maken, zodat u IIS kunt installeren en de standaard webpagina kunt aanpassen.
10. Start Windows PowerShell op de virtuele machine *ProdWebsite* en gebruik de volgende opdrachten om de IIS-server te installeren en het standaard htm-bestand bij te werken.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>DNS-namen voor virtuele machines configureren met behulp van IIS

Traffic Manager routeert gebruikersverkeer op basis van de DNS-naam van de service-eindpunten. In dit gedeelte configureert u de DNS-namen voor de IIS-server - *InternalWebsite* en *ProdWebsite*.

1. Klik in het linkermenu op **Alle resources** en selecteer in de lijst met resources *InternalWebsite* in de resourcegroep *myresourceGroupTM1*.
2. Selecteer op de pagina **Overzicht** onder **DNS-naam** de optie **Configureren**.
3. Voeg op de pagina **Configuratie** onder het label DNS-naam een unieke naam toe en selecteer vervolgens **Opslaan**.
4. Herhaal stap 1-3 voor de VM-naam *ProdWebsite* in de resourcegroep *myResourceGroupTM2*.

### <a name="create-test-vms"></a>Test-VM’s maken

In dit gedeelte maakt u een virtuele machine (*UserVMUS* en *UserVMEurope*) in elke Azure-regio (**VS-Oost** en **Europa - west**). U zult deze virtuele machines gebruiken om te testen hoe Traffic Manager verkeer naar de dichtstbijzijnde IIS-server routeert als u de website bezoekt.

1. Selecteer **Een resource maken** > **Compute** > **Datacenter met Windows Server 2016** in de linkerbovenhoek van Azure Portal.
2. Voer de volgende informatie voor **Basisinformatie** in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|*UserVMUS*|
    |Gebruikersnaam| Voer een gebruikersnaam naar keuze in.|
    |Wachtwoord| Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **Bestaande** en selecteer vervolgens *myResourceGroupTM1*.|
    |||

4. Selecteer een VM-grootte onder **Kies een grootte**.
5. Selecteer de volgende waarden voor **Instellingen** en selecteer **OK**:
    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk| Selecteer **Virtueel netwerk** in **Virtueel netwerk maken** en voer voor **Naam** *myVNet3* en voor het subnet *mySubnet3* in.|
    |Netwerkbeveiligingsgroep|Selecteer **Basic** en selecteer in de vervolgkeuzelijst **Openbare binnenkomende poorten selecteren** de opties **HTTP** en **RDP** |
    |Diagnostische gegevens over opstarten|Selecteer **Uitgeschakeld**.|
    |||

6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

7. Volg de stappen 1-5 nogmaals, maar dan met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |VM-naam | *UserVMEurope*|
    |Resourcegroep | Selecteer **Bestaande** en typ vervolgens *myResourceGroupTM2*|
    |Virtueel netwerk | Selecteer **Virtueel netwerk** in **Virtueel netwerk maken** en voer voor **Naam** *myVNet4* en voor het subnet *mySubnet4* in.|
    |||

8. Het maken van de VM's duurt enkele minuten. Ga niet verder met de overige stappen voordat beide VM's zijn gemaakt.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
Maak een Traffic Manager-profiel waarmee u specifieke eindpunten kunt retourneren op basis van het bron-IP-adres van de aanvraag.

1. Selecteer linksboven in het scherm de optie **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
2. Voer in  **Traffic Manager-profiel maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Naam                   | Deze naam moet uniek zijn binnen de zone trafficmanager.net en resulteert in de DNS-naam, trafficmanager.net, die wordt gebruikt voor het openen van uw Traffic Manager-profiel.                                   |
    | Routeringsmethode          | Selecteer de routeringsmethode **Subnet**.                                       |
    | Abonnement            | Selecteer uw abonnement.                          |
    | Resourcegroep          | Selecteer **Bestaande** en voer *myResourceGroupTM1* in. |
    | |                              |
    |
  
    ![Een Traffic Manager-profiel maken](./media/tutorial-traffic-manager-subnet-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

Voeg de twee virtuele machines toe waarop de IIS-servers worden uitgevoerd - *InternalWebsite*  & *ProdWebsite* - om verkeer van gebruikers te routeren op basis van het subnet van de aanvraag van de gebruiker.

1. Zoek in de zoekbalk van de portal de naam van het Traffic Manager-profiel dat u in de vorige sectie hebt gemaakt en selecteer het profiel in de weergegeven resultaten.
2. Klik in **Traffic Manager-profiel**, in de sectie **Instellingen**, op **Eindpunten** en vervolgens op **Toevoegen**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Type                    | Azure-eindpunt                                   |
    | Naam           | myInternalWebSiteEndpoint                                        |
    | Doelbrontype           | Openbaar IP-adres                          |
    | Doelbron          | **Kies een openbaar IP-adres** om het overzicht van resources met openbare IP-adressen onder hetzelfde abonnement weer te geven. Selecteer in **Resource** het openbare IP-adres met de naam *InternalWebsite-ip*. Dit is het openbare IP-adres van de IIS-server VM in VS-Oost.|
    |  Instellingen voor subnetroutering    |   Voeg het IP-adres van de test-VM *UserVMUS* toe. Elke gebruikersquery die van deze virtuele machine afkomstig is, zal naar de *InternalWebSiteEndpoint* worden omgeleid.    |

4. Herhaal stap 2 en 3 om een ander eindpunt met de naam *ProdWebsiteEndpoint* toe te voegen voor het openbare IP-adres *ProdWebsite-ip* dat aan de virtuele machine *ProdWebsite* voor de IIS-server is gekoppeld. Voeg voor **Instellingen voor subnetroutering** het IP-adres van de test-VM toe - *UserVMEurope*. Elke gebruikersquery van deze test-VM zal naar het eindpunt *myProdWebsiteEndpoint* worden gerouteerd.
5.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven in **Traffic Manager-profiel**, samen met de controlestatus **Online**.

 
## <a name="test-traffic-manager-profile"></a>Traffic Manager-profiel testen
In dit gedeeltetest u hoe de Traffic Manager gebruikersverkeer vanaf een bepaald subnet naar een specifiek eindpunt routeert. Voer de volgende stappen uit om de Traffic Manager in actie te zien:
1. Bepaal de DNS-naam van uw Traffic Manager-profiel.
2. Zie Traffic Manager als volgt in werking:
    - Surf op de test-VM (*UserVMUS*) in de regio **VS-Oost** in een webbrowser naar de DNS-naam van uw Traffic Manager-profiel.
    - Surf op de test-VM (*UserVMEurope*) in de regio **Europa - west** in een webbrowser naar de DNS-naam van uw Traffic Manager-profiel.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>De DNS-naam van het Traffic Manager-profiel vaststellen
In deze zelfstudie maakt u voor het gemak gebruik van de DNS-naam van het Traffic Manager-profiel om de websites te bezoeken. 

U kunt de DNS-naam van het Traffic Manager-profiel als volgt vaststellen:

1.  Zoek in de zoekbalk van de portal de naam van het **Traffic Manager-profiel** dat u in de vorige sectie hebt gemaakt. Klik op het Traffic Manager-profiel in de resultaten die worden weergegeven.
1. Klik op **Overzicht**.
2. Het **Traffic Manager-profiel** geeft de DNS-naam weer van het Traffic Manager-profiel dat u zojuist hebt gemaakt. In productie-implementaties configureert u een aangepaste domeinnaam om met behulp van een DNS CNAME-record naar de Traffic Manager-domeinnaam te verwijzen.

### <a name="view-traffic-manager-in-action"></a>Traffic Manager in werking zien
In dit gedeelte kunt u Traffic Manager in werking zien. 

1. Selecteer in het linkermenu **Alle resources** en klik in de lijst met resources op *myUserVMUS*, die zich in de resourcegroep *myresourceGroupTM1* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** en selecteer vervolgens in **Verbinding maken met virtuele machine** de optie **RDP-bestand downloaden**. 
3. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding. 
1. Typ in een webbrowser op de virtuele machine *UserVMUS* de DNS-naam van uw Traffic Manager-profiel om uw website weer te geven. Omdat het IP-adres van de virtuele machine *UserVMUS* aan het eindpunt *myInternalWebsiteEndpoint* is gekoppeld, opent de webbrowser de testserver - *InternalWebsite*.

2. Maak vervolgens met behulp van de stappen 1-5 verbinding met de virtuele machine *UserVMEurope* in **Europa - west** en surf vanaf deze VM naar de domeinnaam van het Traffic Manager-profiel. Omdat het IP-adres van de virtuele machine *UserVMEurope* aan het eindpunt *myProductionWebsiteEndpoint* is gekoppeld, opent de webbrowser de testserver - *ProdWebsite*. 
  
## <a name="delete-the-traffic-manager-profile"></a>Het Traffic Manager-profiel verwijderen
Verwijder de resourcegroepen (**ResourceGroupTM1** en **ResourceGroupTM2**) als deze niet meer nodig zijn. Selecteer daarvoor de resourcegroep (**ResourceGroupTM1** of **ResourceGroupTM2**) en selecteer vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gewogen verkeersrouteringsmethode](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [de routeringsmethode met prioriteit](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [geografische verkeersrouteringsmethode](traffic-manager-configure-geographic-routing-method.md).


