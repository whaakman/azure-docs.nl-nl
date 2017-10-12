---
title: Uw eerste Azure Virtual Network maken | Microsoft Docs
description: Lees hoe u een Azure Virtual Network (VNet) maakt, twee virtuele machines (VM's) met het VNet verbindt en verbinding maakt met de VM's.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
ms.openlocfilehash: e653764d7cb514d50b44fadd0cc5963dd404d99e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-virtual-network"></a>Uw eerste virtuele netwerk maken

Lees hoe u een virtueel netwerk (VNet) met twee subnetten maakt, twee virtuele machines (VM's) maakt en elke VM met een van de subnetten verbindt, zoals in de volgende afbeelding wordt weergegeven:

![Virtueel-netwerkdiagram](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Een virtueel Azure-netwerk (VNet) is een weergave van uw eigen netwerk in de cloud. U kunt uw Azure-netwerkinstellingen beheren en DHCP-adresblokkeringen, DNS-instellingen, beveilingsbeleidsregels en routering definiëren. Lees het artikel [Virtual Network overview](virtual-networks-overview.md) (Overzicht van virtueel netwerk) voor meer informatie over VNet-concepten. Voltooi de volgende stappen om de resources te maken die in de afbeelding worden weergegeven:

1. [Maak een VNet met twee subnetten](#create-vnet).
2. [Maak twee VM's, elk met één netwerkinterface (NIC)](#create-vms), en koppel aan elke NIC een netwerkbeveiligingsgroep (NSG).
3. [Maak verbinding van en naar de VM's](#connect-to-from-vms).
4. [Verwijder alle resources](#delete-resources). Voor een aantal resources die in deze oefening worden gemaakt, worden voor het inrichten kosten in rekening gebracht. Als u, nadat u de oefening hebt voltooid, de kosten wilt beperken, moet u de stappen in dit gedeelte voltooien om de gemaakte resources te verwijderen.

Nadat u de stappen in dit artikel hebt voltooid, beschikt u over basiskennis met betrekking tot het gebruik van een VNet. Er worden vervolgstappen gegeven, zodat u meer informatie krijgt over hoe u VNets op een dieper niveau kunt gebruiken.

## <a name="create-vnet"></a>Een virtueel netwerk met twee subnetten maken

Voltooi de volgende stappen om een virtueel netwerk met twee subnetten te maken. Verschillende subnetten worden meestal gebruikt om de verkeersstroom tussen subnetten te beheren.

1. Meld u aan bij [Azure Portal](<https://portal.azure.com>). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free). 
2. Klik in het deelvenster **Favorieten** van de portal op **Nieuw**.
3. Klik op de blade **Nieuw** op **Netwerken**. Klik op de blade **Netwerken** op **Virtueel netwerk**, zoals in de volgende afbeelding wordt weergegeven:

    ![Virtueel-netwerkdiagram](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  Houd op de blade **Virtueel netwerk** de optie *Resource Manager* geselecteerd als het implementatiemodel en klik op **Maken**.
5.  Voer op de blade **Virtueel netwerk maken** die nu wordt weergegeven, de volgende waarden in en klik vervolgens op **Maken**:

    |**Instelling**|**Waarde**|**Details**|
    |---|---|---|
    |**Naam**|*MyVNet*|De naam moet uniek zijn binnen de resourcegroep.|
    |**Adresruimte**|*10.0.0.0/16*|U kunt elke gewenste adresruimte opgeven in CIDR-notatie.|
    |**Subnetnaam**|*Front-end*|De subnetnaam moet uniek zijn binnen het virtuele netwerk.|
    |**Subnetadresbereik**|*10.0.0.0/24*| Het bereik dat u opgeeft, moet voorkomen binnen de adresruimte die u voor het virtuele netwerk hebt gedefinieerd.|
    |**Abonnement**|*[Uw abonnement]*|Selecteer een abonnement om het VNet in te maken. Een VNet bestaat binnen één abonnement.|
    |**Resourcegroep**|**Nieuwe maken:** *MyRG*|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
    |**Locatie**|*VS - west*| Doorgaans wordt de locatie geselecteerd die het dichtst bij uw fysieke locatie ligt.|

    Het duurt een paar seconden voordat het VNet is gemaakt. Nadat dit is gemaakt, wordt het Azure Portal-dashboard weergegeven.

6. Nu het virtuele netwerk is gemaakt, klikt u in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op het virtuele netwerk **MyVNet** op de blade **Alle resources**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u *MyVNet* invoeren in het vak **Filteren op naam...** om gemakkelijk toegang te krijgen tot het VNet.
7. De blade **MyVNet** wordt geopend en geeft informatie over het VNet weer, zoals in de volgende afbeelding wordt getoond:

    ![Virtueel-netwerkdiagram](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Klik, zoals in de vorige afbeelding wordt weergegeven, op **Subnetten** om een lijst van de subnetten binnen het VNet weer te geven. Het enige bestaande subnet is **Front-end**. Dit is het subnet dat u in stap 5 hebt gemaakt.
9. Klik op de blade MyVNet - Subnetten op **+ Subnetten** om een subnet te maken met de volgende informatie. Klik op **OK** om het subnet te maken:

    |**Instelling**|**Waarde**|**Details**|
    |---|---|---|
    |**Naam**|*Back-end*|De naam moet uniek zijn binnen het virtuele netwerk.|
    |**Adresbereik**|*10.0.1.0/24*|Het bereik dat u opgeeft, moet voorkomen binnen de adresruimte die u voor het virtuele netwerk hebt gedefinieerd.|
    |**Netwerkbeveiligingsgroep** en **Routetabel**|*Geen* (standaard)|Netwerkbeveiligingsgroepen (NSG's) worden verderop in dit artikel besproken. Lees het artikel [User-defined routes](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.|

10. Nadat het nieuwe subnet aan het VNet is toegevoegd, kunt u de blade **MyVNet – Subnetten** sluiten. Sluit vervolgens de blade **Alle resources**.

## <a name="create-vms"></a>Virtuele machines maken

Nu het VNet en de subnetten zijn gemaakt, kunt u de VM's maken. Voor deze oefening voeren beide VM's het Windows Server-besturingssysteem uit. Ze kunnen echter elk besturingssysteem uitvoeren dat door Azure wordt ondersteund, waaronder verschillende Linux-distributies.

### <a name="create-web-server-vm"></a>De webserver-VM maken

Voltooi de volgende stappen om de webserver-VM te maken:

1. Klik in het deelvenster Favorieten van Azure Portal op **Nieuw**, **Compute** en **Windows Server 2016 Datacenter**.
2. Klik op de blade **Windows Server 2016 Datacenter** op **Maken**.
3. Selecteer op de blade **Basisbeginselen** die nu wordt weergegeven, de volgende waarden of voer deze in en klik op **OK**:

    |**Instelling**| **Waarde**|**Details**|
    |---|---|---|
    |**Naam**|*MyWebServer*|Deze virtuele machine fungeert als een webserver waarmee internetresources verbinding maken.|
    |**Type VM-schijf**|*SSD*|
    |**Gebruikersnaam**|*Uw keuze*|
    |**Wachtwoord en wachtwoord bevestigen**|*Uw keuze*|
    | **Abonnement**|*<Your subscription>*|Het abonnement moet hetzelfde zijn dat u in stap 5 hebt geselecteerd in het gedeelte [Een virtueel netwerk met twee subnetten maken](#create-vnet) van dit artikel. Het VNet waarmee u een VM verbindt, moet zich in hetzelfde abonnement bevinden als de virtuele machine.|
    |**Resourcegroep**|**Bestaande gebruiken:** selecteer *MyRG*|Hoewel we dezelfde resourcegroep gebruiken als met het VNet, hoeven de resources zich niet in dezelfde resourcegroep te bevinden.|
    |**Locatie**|*VS - west*|De locatie moet dezelfde locatie zijn die u in stap 5 hebt opgegeven in het gedeelte [Een virtueel netwerk met twee subnetten maken](#create-vnet) van dit artikel. De virtuele machines en de VNets waarmee ze verbinding maken, moeten zich op dezelfde locatie bevinden.|

4. Klik op de blade **Kies een grootte** op *DS1_V2 Standard* en vervolgens op **Selecteren**. Lees het artikel over [VM-grootten voor Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor een lijst met alle VM-grootten voor Windows die door Azure worden ondersteund.
5. Selecteer op de blade **Instellingen** de volgende waarden of voer deze in en klik op **OK**:

    |**Instelling**|**Waarde**|**Details**|
    |---|---|---|
    |**Opslag: beheerde schijven gebruiken**|*Ja*||
    |**Virtueel netwerk**| Selecteer *MyVNet*|U kunt elk VNet selecteren dat zich op dezelfde locatie bevindt als de virtuele machine die u maakt. Lees het artikel [Virtual network](virtual-networks-overview.md) (Virtueel netwerk) voor meer informatie over VNets en subnetten.|
    |**Subnet**|Selecteer *Front-end*|U kunt elk subnet selecteren dat zich binnen het VNet bevindt.|
    |**Openbaar IP-adres**|Accepteer de standaardwaarde|Met een openbaar IP-adres kunt u vanaf internet verbinding maken met de virtuele machine. In het artikel [IP addresses](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) (IP-adressen) vindt u meer informatie over openbare IP-adressen.|
    |**Netwerkbeveiligingsgroep (firewall)**|Accepteer de standaardwaarde|Klik op de standaard-NSG **(nieuwe) MyWebServer-nsg** die de portal heeft gemaakt, om de instellingen weer te geven. U ziet dat de blade **Netwerkbeveiligingsgroep maken** die nu wordt geopend, één regel voor binnenkomende verbindingen heeft waarmee TCP/3389-verkeer (RDP) van elk bron-IP-adres mogelijk wordt.|
    |**Alle andere waarden**|Accepteer de standaardwaarden|Lees het artikel [About VMs](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Informatie over virtuele machines) voor meer informatie over de overige instellingen.|

    Met netwerkbeveiligingsgroepen (NSG's) kunt u regels voor binnenkomende en uitgaande verbindingen maken voor het type netwerkverkeer dat van en naar de virtuele machine kan stromen. Standaard wordt al het binnenkomende verkeer naar de virtuele machine geweigerd. U kunt voor een productiewebserver aanvullende regels voor binnenkomende verbindingen toevoegen voor TCP/80 (HTTP) en TCP/443 (HTTPS). Er is geen regel voor uitgaand verkeer, omdat al het uitgaande verkeer standaard wordt toegestaan. U kunt regels toevoegen of verwijderen om het verkeer volgens uw beleid te beheren. Lees het artikel [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md) voor meer informatie over NSG's.

6.  Controleer de instellingen op de blade **Samenvatting** en klik op **OK** om de virtuele machine te maken. Terwijl de virtuele machine wordt gemaakt, wordt op het dashboard van de portal een statustegel weergegeven. Het maken van de virtuele machine kan een paar minuten duren. U hoeft niet te wachten totdat de virtuele machine klaar is. U kunt verder gaan met de volgende stap, terwijl de virtuele machine wordt gemaakt.

### <a name="create-database-server-vm"></a>De databaseserver-VM maken

Voltooi de volgende stappen om de databaseserver-VM te maken:

1.  Klik in het deelvenster Favorieten op **Nieuw**, **Compute** en **Windows Server 2016 Datacenter**.
2.  Klik op de blade **Windows Server 2016 Datacenter** op **Maken**.
3.  Selecteer op de **blade Grondbeginselen** de volgende waarden of voer deze in. Klik vervolgens op **OK**:

    |**Instelling**|**Waarde**|**Details**|
    |---|---|---|
    |**Naam**|*MyDBServer*|Deze VM fungeert als een databaseserver waarmee de webserver verbinding maakt, maar waarmee internet geen verbinding kan maken.|
    |**Type VM-schijf**|*SSD*||
    |**Gebruikersnaam**|Uw keuze||
    |**Wachtwoord en wachtwoord bevestigen**|Uw keuze||
    |**Abonnement**|<Your subscription>|Het abonnement moet hetzelfde zijn dat u in stap 5 hebt geselecteerd in het gedeelte [Een virtueel netwerk met twee subnetten maken](#create-vnet) van dit artikel.|
    |**Resourcegroep**|**Bestaande gebruiken:** selecteer *MyRG*|Hoewel we dezelfde resourcegroep gebruiken als met het VNet, hoeven de resources zich niet in dezelfde resourcegroep te bevinden.|
    |**Locatie**|*VS - west*|De locatie moet dezelfde locatie zijn die u in stap 5 hebt opgegeven in het gedeelte [Een virtueel netwerk met twee subnetten maken](#create-vnet) van dit artikel.|

4.  Klik op de blade **Kies een grootte** op *DS1_V2 Standard* en vervolgens op **Selecteren**.
5.  Selecteer op de blade **Instellingen** de volgende waarden of voer deze in en klik op **OK**:

    |**Instelling**|**Waarde**|**Details**|
    |----|----|---|
    |**Opslag: beheerde schijven gebruiken**|*Ja*||
    |**Virtueel netwerk**|Selecteer *MyVNet*|U kunt elk VNet selecteren dat zich op dezelfde locatie bevindt als de virtuele machine die u maakt.|
    |**Subnet**|Selecteer *Back-end* door in het vak **Subnet** te klikken. Selecteer vervolgens **Back-end** op de blade **Een subnet kiezen**|U kunt elk subnet selecteren dat zich binnen het VNet bevindt.|
    |**Openbaar IP-adres**|Geen: klik op het standaardadres en vervolgens op **Geen** op de blade **Openbaar IP-adres kiezen**|Zonder een openbaar IP-adres kunt u alleen verbinding maken met de VM vanuit een andere VM die met hetzelfde VNet is verbonden. U kunt hier niet rechtstreeks verbinding mee maken vanaf internet.|
    |**Netwerkbeveiligingsgroep (firewall)**|Accepteer de standaardwaarde| Net als de standaard-NSG die voor de MyWebServer-VM is gemaakt, heeft deze NSG ook dezelfde standaardregel voor binnenkomende verbindingen. U kunt voor een databaseserver een aanvullende regel voor binnenkomende verbindingen voor TCP/1433 (MS SQL) toevoegen. Er is geen regel voor uitgaand verkeer, omdat al het uitgaande verkeer standaard wordt toegestaan. U kunt regels toevoegen of verwijderen om het verkeer volgens uw beleid te beheren.|
    |**Alle andere waarden**|Accepteer de standaardwaarden||

6.  Controleer de instellingen op de blade **Samenvatting** en klik op **OK** om de virtuele machine te maken. Terwijl de virtuele machine wordt gemaakt, wordt op het dashboard van de portal een statustegel weergegeven. Het maken van de virtuele machine kan een paar minuten duren. U hoeft niet te wachten totdat de virtuele machine klaar is. U kunt verder gaan met de volgende stap, terwijl de virtuele machine wordt gemaakt.

## <a name="review"></a>Resources controleren

Hoewel u zelf één VNet en twee virtuele machines hebt gemaakt, heeft Azure Portal enkele aanvullende resources in de resourcegroep MyRG gemaakt. Controleer de inhoud van de resourcegroep MyRG door de volgende stappen te voltooien:

1. Klik in het deelvenster **Favorieten** op **Meer services**.
2. In het deelvenster **Meer services** typt u *Resourcegroepen* in het vak waarin het woord *Filter* staat. Klik op **Resourcegroepen** wanneer deze optie wordt weergegeven in de gefilterde lijst.
3. Klik in het deelvenster **Resourcegroepen** op de resourcegroep *MyRG*. Als u in uw abonnement veel bestaande resourcegroepen hebt, kunt u *MyRG* typen in het vak dat de tekst *Filteren op naam...* bevat om snel toegang tot de resourcegroep MyRG te krijgen.
4.  Op de blade **MyRG** ziet u dat de resourcegroep 12 resources bevat. Dit wordt in de volgende afbeelding weergegeven:

    ![Inhoud resourcegroep](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Lees de overzichtsartikelen [Virtual machine](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Virtuele machine), [Disk](../virtual-machines/windows/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Schijf) en [Storage account](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Opslagaccount) voor meer informatie over virtuele machines, schijven en opslagaccounts. De twee standaard-NSG's die in de portal voor u zijn gemaakt, worden weergegeven. Ook kunt u zien dat er in de portal twee netwerkinterfaceresources zijn gemaakt. Met een netwerkinterface (NIC) kan een virtuele machine via het VNet verbinding maken met andere resources. Lees het artikel [NIC](virtual-network-network-interface.md) voor meer informatie over NIC's. In de portal is ook één openbare IP-adresresource gemaakt. Openbare IP-adressen zijn één instelling voor een openbare IP-adresresource. In het artikel [IP addresses](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) (IP-adressen) vindt u meer informatie over openbare IP-adressen.

## <a name="connect-to-from-vms"></a>Verbinding maken met de virtuele machines

U hebt een VNet en twee virtuele machines gemaakt. U kunt nu verbinding maken met de virtuele machines door de stappen in de volgende gedeelten te voltooien:

### <a name="connect-from-internet"></a>Verbinding met de webserver-VM maken via internet

Als u via internet verbinding met de webserver-VM wilt maken, voert u de volgende stappen uit:

1. Open in de portal de resourcegroep MyRG door de stappen in het gedeelte [Resources controleren](#review) van dit artikel uit te voeren.
2. Klik op de blade **MyRG** op de **MyWebServer**-VM.
3. Klik op de blade **MyWebServer** op **Verbinding maken**, zoals in de volgende afbeelding wordt weergegeven:

    ![Verbinding maken met de webserver-VM](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Sta de browser toe om het bestand *MyWebServer.rdp* te downloaden en open dit.
5. Als er een dialoogvenster wordt weergegeven waarin wordt gemeld dat de uitgever van de externe verbinding niet kan worden geverifieerd, klikt u op **Verbinding maken**.
6. Wanneer u uw referenties invoert, zorgt u ervoor dat u zich aanmeldt met de gebruikersnaam en het wachtwoord die u hebt opgegeven in stap 3 van het gedeelte [De webserver-VM maken](#create-web-server-vm) van dit artikel. Als het vak **Windows-beveiliging** dat nu wordt weergegeven, niet de juiste referenties weergeeft, moet u mogelijk op **Meer keuzemogelijkheden** en **Een ander account gebruiken** klikken, zodat u de juiste gebruikersnaam en het juiste wachtwoord kunt opgeven. Klik op **OK** om verbinding te maken met de virtuele machine.
7. Als het vak **Verbinding met extern bureaublad** wordt weergegeven met de melding dat de identiteit van de externe computer niet kan worden geverifieerd, klikt u op **Ja**.
8. U bent nu via internet verbonden met de MyWebServer-VM. Laat de verbinding met het externe bureaublad open om de stappen in het volgende gedeelte te voltooien.

De externe verbinding verloopt via het openbare IP-adres dat is toegewezen aan de openbare IP-adresresource die de portal heeft gemaakt in stap 5 van het gedeelte [Een virtueel netwerk met twee subnetten maken](#create-vnet) van dit artikel. De verbinding is toegestaan, omdat de standaardregel die in de NSG **MyWebServer-nsg** is gemaakt, TCP/3389 (RDP) voor binnenkomende verbindingen naar de virtuele machine toestaat vanaf elk bron-IP-adres. Als u via een andere poort verbinding met de virtuele machine probeert te maken, mislukt de verbinding, tenzij u aanvullende regels voor binnenkomende verbindingen aan de NSG toevoegt die de extra poorten toestaat.

>[!NOTE]
>Als u aanvullende regels voor binnenkomende verbindingen aan de NSG toevoegt, zorgt u ervoor dat dezelfde poorten op de Windows-firewall zijn geopend, anders mislukt de verbinding.
>

### <a name="connect-to-internet"></a>Verbinding met internet maken via de webserver-VM

Als u uitgaande verbindingen met internet wilt verbinden via de webserver-VM, voert u de volgende stappen uit:

1. Als u nog geen externe verbinding met de MyWebServer-VM hebt geopend, maakt u een externe verbinding met de virtuele machine door de stappen in het gedeelte [Verbinding met de webserver-VM maken via internet](#connect-from-internet) van dit artikel te voltooien.
2. Open Internet Explorer via het Windows-bureaublad. Klik in het dialoogvenster **Internet Explorer 11 instellen** op **Aanbevolen instellingen niet gebruiken**. Klik vervolgens op **OK**. Het is raadzaam de aanbevolen instellingen voor een productieserver te accepteren.
3. Typ [bing.com](http:www.bing.com) in de adresbalk van Internet Explorer. Als er een dialoogvenster van Internet Explorer wordt weergegeven, klikt u op **Toevoegen**. Klik vervolgens op **Toevoegen** in het dialoogvenster **Vertrouwde sites** en ten slotte op **Sluiten**. Herhaal dit proces voor alle andere dialoogvensters van Internet Explorer.
4. Voer op de zoekpagina van Bing *whatsmyipaddress* in en klik vervolgens op de knop Vergrootglas. Bing retourneert het openbare IP-adres dat is toegewezen aan de openbare IP-adresresource die in de portal is gemaakt tijdens het maken van de virtuele machine. Als u de instellingen voor de resource **MyWebServer-IP** bekijkt, ziet u dat hetzelfde IP-adres aan de openbare IP-adresresource is toegewezen, zoals in de volgende afbeelding wordt weergegeven. Het IP-adres dat aan de virtuele machine is toegewezen, is echter anders.

    ![Verbinding maken met de webserver-VM](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Laat de verbinding met het externe bureaublad open om de stappen in het volgende gedeelte te voltooien.

U kunt vanaf de virtuele machine verbinding met internet maken, omdat alle uitgaande connectiviteit van de virtuele machine standaard is toegestaan. U kunt uitgaande connectiviteit beperken door regels aan de NSG toe te voegen die zijn toegepast op de NIC, op het subnet waarmee de NIC is verbonden of op beide.

Als de virtuele machine via de portal is gestopt (toewijzing opgeheven), kan het openbare IP-adres veranderen. Als u wilt dat het openbare IP-adres nooit verandert, kunt u voor het IP-adres de statische toewijzingsmethode gebruiken in plaats van de dynamische toewijzingsmethode (de standaardinstelling). Lees het artikel [IP address types and allocation methods](virtual-network-ip-addresses-overview-arm.md) (Typen IP-adressen en toewijzingsmethoden) voor meer informatie over de verschillen tussen de toewijzingsmethoden.

### <a name="webserver-to-dbserver"></a>Verbinding met de databaseserver-VM maken via de webserver-VM

Als u via de webserver-VM verbinding met de databaseserver-VM wilt maken, voert u de volgende stappen uit:

1. Als u nog geen externe verbinding met de MyWebServer-VM hebt geopend, maakt u een externe verbinding met de virtuele machine door de stappen in het gedeelte [Verbinding met de webserver-VM maken via internet](#connect-from-internet) van dit artikel te voltooien.
2. Klik op de knop Start in de linkerbenedenhoek van het Windows-bureaublad en typ *extern bureaublad*. Als in de menulijst Start **Verbinding met extern bureaublad** wordt weergegeven, klikt u hierop.
3. Voer in het dialoogvenster **Verbinding met extern bureaublad** *MyDBServer* in voor de computernaam en klik op **Verbinding maken**.
4. Geef de gebruikersnaam en wachtwoorden op die u in stap 3 van het gedeelte [De databaseserver-VM maken](#create-database-server-vm) van dit artikel hebt ingevoerd. Klik vervolgens op **OK**.
5. Als er een dialoogvenster wordt weergegeven met de melding dat de identiteit van de externe computer niet kan worden geverifieerd, klikt u op **Ja**.
6. Laat de verbinding met het externe bureaublad naar beide servers open om de stappen in de volgende sectie te voltooien.

U kunt verbinding maken met de databaseserver-VM via de webserver-VM vanwege de volgende redenen:

- Binnenkomende TCP/3389-verbindingen zijn ingeschakeld voor elke bron-IP in de standaard-NSG die in stap 5 van het gedeelte [De databaseserver-VM maken](#create-database-server-vm) van dit artikel is gemaakt.
- U hebt de verbinding gestart vanaf de webserver-VM, die is verbonden met hetzelfde VNet als de databaseserver-VM. Als u verbinding wilt maken met een virtuele machine waaraan geen openbaar IP-adres is toegewezen, moet u verbinding maken vanaf een andere virtuele machine die met hetzelfde VNet is verbonden, zelfs als de virtuele machine is verbonden met een ander subnet.
- Hoewel de virtuele machines aan verschillende subnetten zijn verbonden, maakt Azure standaardroutes waardoor connectiviteit tussen subnetten mogelijk wordt. U kunt de standaardroutes echter overschrijven door uw eigen routes te maken. Lees het artikel [User-defined routes](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over routering in Azure.

Als u probeert via internet een externe verbinding met de databaseserver-VM te starten, zoals u in het gedeelte [Verbinding met de webserver-VM maken via internet](#connect-from-internet) van dit artikel hebt gedaan, ziet u dat de optie **Verbinding maken** niet beschikbaar is. De optie Verbinding maken is niet beschikbaar, omdat er geen openbaar IP-adres aan de virtuele machine is toegewezen, waardoor binnenkomende verbindingen hiernaartoe via internet niet mogelijk zijn.

### <a name="connect-to-the-internet-from-the-database-server-vm"></a>Verbinding met internet maken via de databaseserver-VM

Als u uitgaande verbindingen met internet tot stand wilt brengen via de databaseserver-VM, voert u de volgende stappen uit:

1. Als u nog geen externe verbinding met de MyDBServer-VM via de MyWebServer-VM hebt geopend, voltooit u de stappen in het gedeelte [Verbinding met de databaseserver-VM maken via de webserver-VM](#webserver-to-dbserver) van dit artikel.
2. Open Internet Explorer vanaf het Windows-bureaublad op de MyDBServer-VM en reageer op de dialoogvensters zoals u in stap 2 en 3 hebt gedaan in het gedeelte [Verbinding met internet maken via de webserver-VM](#connect-to-internet) van dit artikel.
3. Typ in de adresbalk [bing.com](http:www.bing.com).
4. Klik op **Toevoegen** in het dialoogvenster van Internet Explorer dat wordt weergegeven. Klik vervolgens op **Toevoegen** en **Sluiten** in het dialoogvenster **Vertrouwde sites**. Herhaal deze stappen als er nog andere dialoogvensters worden weergegeven.
5. Voer op de zoekpagina van Bing *whatsmyipaddress* in en klik vervolgens op de knop Vergrootglas. Bing retourneert het openbare IP-adres dat door de Azure-infrastructuur momenteel is toegewezen aan de virtuele machine. 6. Sluit het externe bureaublad naar de MyDBServer-VM vanaf de MyWebServer-VM. Sluit vervolgens de externe verbinding naar de MyWebServer-VM.

De uitgaande verbinding met internet is toegestaan, omdat al het uitgaande verkeer standaard is toegestaan, ook al is er aan de MyDBServer-VM geen openbare IP-adresresource toegewezen. Alle virtuele machines kunnen standaard uitgaand verbinding maken met internet, of er nu een openbare IP-adresresource aan de virtuele machine is toegewezen of niet. Via internet kunt u echter geen verbinding maken met het openbare IP-adres, zoals wel het geval is bij een MyWebServer-VM waaraan een openbare IP-adresresource is toegewezen.

## <a name="delete-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

1. Als u de resourcegroep MyRG wilt weergeven die u in dit artikel hebt gemaakt, voert u stap 1-3 uit in het gedeelte [Resources controleren](#review) van dit artikel. Bekijk nogmaals de resources in de resourcegroep. Als u de resourcegroep MyRG via de voorgaande stappen hebt gemaakt, ziet u de 12 resources die in de afbeelding worden weergegeven in stap 4.
2. Klik op de blade MyRG op de knop **Verwijderen**.
3. De portal vereist dat u de naam van de resourcegroep typt om te bevestigen dat u deze wilt verwijderen. Als u andere resources ziet dan de resources die in stap 4 van het gedeelte [Resources controleren](#review) van dit artikel worden weergegeven, klikt u op **Annuleren**. Als alleen de 12 resources worden weergegeven die u in het kader van dit artikel hebt gemaakt, typt u *MyRG* als naam van de resourcegroep. Klik vervolgens op **Verwijderen**. Als u een resourcegroep verwijdert, worden alle resources binnen de resourcegroep verwijderd. Zorg er dus altijd voor dat u de inhoud van een resourcegroep bevestigt, voordat u deze verwijdert. Alle resources die zich in de resourcegroep bevinden en de resourcegroep zelf worden verwijderd. Dit proces duurt enkele minuten.

## <a name="next-steps"></a>Volgende stappen

In deze oefening hebt u een VNet en twee virtuele machines gemaakt. U hebt tijdens het maken van deze virtuele machines een aantal aangepaste instellingen opgegeven en verschillende standaardinstellingen geaccepteerd. We raden u aan de volgende artikelen te lezen voordat u productie-VNets en -VM's implementeert, zodat u inzicht hebt in alle beschikbare instellingen:

- [Virtuele netwerken](virtual-networks-overview.md)
- [Openbare IP-adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Netwerkinterfaces](virtual-network-network-interface.md)
- [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md)
- [Virtuele machines](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
