U kunt virtuele machines in Azure maken met behulp van de Server Explorer in Visual Studio.

## <a name="create-an-azure-virtual-machine-in-server-explorer"></a>Maken van een virtuele machine van Azure in Server Explorer
Bij het maken van een virtuele machine in de [Azure Management Portal](http://go.microsoft.com/fwlink/?LinkID=253103), u kunt ook een virtuele machine in Azure maken met behulp van opdrachten in Server Explorer. Virtuele machines kan bijvoorbeeld worden gebruikt om een front-end achter een algemene taakverdeling openbaar eindpunt.

### <a name="to-create-a-new-virtual-machine"></a>Een nieuwe virtuele machine maken
1. Open in Server Explorer, de **Azure** knooppunt en klik op **virtuele Machines**.
2. Klik in het contextmenu op **virtuele Machine maken**.
   
    De **maken van een nieuwe virtuele Machine** wizard wordt weergegeven.
   
    ![De opdracht van de virtuele Machine maken](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)
3. Op de **Kies een abonnement** pagina, selecteer een abonnement moet worden gebruikt bij het maken van de virtuele machine en klik vervolgens op **volgende**.
   
    Als u bent niet aangemeld bij Azure, klikt u op **aanmelden** aan te melden. Selecteer vervolgens uw Azure-abonnement in de vervolgkeuzelijst als deze nog niet is geselecteerd.
4. Op de **selecteert u de installatiekopie van een virtuele Machine** pagina, selecteert u een type installatiekopie in de **afbeeldingstype** dropdown keuzelijst en selecteer vervolgens de installatiekopieën van een virtuele machine in de **installatiekopienaam** keuzelijst met invoervak . Wanneer u bent klaar, klikt u op **volgende**.
   
    ![Selecteer een pagina van de installatiekopie van virtuele machine](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)
   
    U kunt de volgende afbeeldingstypen.
   
   * **Installatiekopieën van het openbare** geeft een lijst van virtuele machine installatiekopieën van besturingssystemen en server-software, zoals Windows Server en SQL Server.
   * **MSDN-installatiekopieën** geeft een lijst van installatiekopieën van virtuele machines van de software beschikbaar voor MSDN-abonnees, zoals Visual Studio en Microsoft Dynamics.
   * **Installatiekopieën van het particuliere** lijsten gespecialiseerde en gegeneraliseerd installatiekopieën van virtuele machines die u hebt gemaakt.
     
     Zie voor meer informatie over gespecialiseerde en gegeneraliseerde virtuele machines, [VM-installatiekopie](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). Zie [het vastleggen van een virtuele Windows-Machine voor gebruik als een sjabloon](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) voor informatie over het inschakelen van een virtuele machine naar een sjabloon kunt u snel maken van nieuwe vooraf virtuele machines geconfigureerde.
     
     U kunt klikken op de naam van een virtuele machine-installatiekopie voor informatie over de installatiekopie aan de rechterkant van de pagina.
     
     > [!NOTE]
     > U kunt geen toevoegen installatiekopieën van virtuele machine naar de **openbare afbeeldingen** of **MSDN-installatiekopieën** geeft een lijst van omdat ze alleen-lezen. Alle virtuele machines die u maakt worden toegevoegd aan de **persoonlijke afbeeldingen** lijst.
     > 
     > 
     
     Als u een MSDN-abonnee met een abonnement van de Visual Studio-niveau, kunt u een vooraf gemaakte virtuele machine van Azure met Visual Studio, evenals enkele andere afbeeldingen. Zie voor meer informatie [een virtuele Machine maken in Visual Studio door de installatiekopie met behulp van installatiekopieën van Visual Studio 2013-galerie voor MSDN-abonnees](http://visualstudio2013msdngalleryimage.azurewebsites.net) en [MSDN-abonnement](https://www.visualstudio.com/products/msdn-subscriptions-vs). |
5. Op de **basisinstellingen van de virtuele Machine** pagina, Geef een computernaam op en voeg vervolgens de specificaties voor de virtuele machine, met inbegrip van de grootte en een gebruikersnaam en wachtwoord. Wanneer u bent klaar, klikt u op **volgende**.
   
    Gebruikt u de nieuwe naam en het wachtwoord aan te melden bij de computer met behulp van extern bureaublad, dus is het een goed idee om schrijf ze op als u vergeet. Nadat u een virtuele machine van Azure in Visual Studio hebt gemaakt, kunt u de grootte en andere instellingen in de [Azure Management Portal](http://go.microsoft.com/fwlink/?LinkID=253103).
   
   > [!NOTE]
   > Als u een groter formaat voor de virtuele machine kiest, kunnen extra kosten toepassen. Zie [prijsinformatie voor virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor meer informatie.
   > 
   > 
6. Virtuele machines die zijn gemaakt in Visual Studio vereisen een cloudservice. Op de **Cloud Service-instellingen** pagina, selecteert u een cloudservice voor de virtuele machine of klik op **< Nieuw >** in de vervolgkeuzelijst als u een cloud service of wilt gebruiken, een nieuwe nog geen hebt. Een opslagaccount is ook vereist, dus kiest u een opslagaccount (of een nieuw opslagaccount maken) in de **opslagaccount** vervolgkeuzelijst. Zie [Inleiding tot Microsoft Azure Storage](../articles/storage/common/storage-introduction.md) voor meer informatie.
7. Als u wilt opgeven van een virtueel netwerk (dit is optioneel), selecteert u deze in de keuzelijsten met vervolgkeuzelijst voor virtueel netwerk en Subnet.
   
    Virtuele machines die deel van een beschikbaarheidsset uitmaken zijn geïmplementeerd op verschillende foutdomeinen. Zie [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) voor meer informatie.
8. Als u wilt dat uw virtuele machine te behoren tot een beschikbaarheidsset (ook optioneel), selecteert u de **opgeven van een beschikbaarheidsset** selectievakje in en kies vervolgens een beschikbaarheidsset in de vervolgkeuzelijst. Als u bent klaar, kiest u de **volgende** knop.
   
    De virtuele machine aan een beschikbaarheidsset toe te voegen, kunt u uw toepassing beschikbaar blijven tijdens netwerkfouten, hardwarefouten lokale schijf en geplande uitvaltijd. U moet gebruiken de [Azure Management Portal](http://go.microsoft.com/fwlink/?LinkID=253103) voor het maken van virtuele netwerken, subnetten en beschikbaarheid. Zie [de beschikbaarheid van virtuele Machines beheren](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/) voor meer informatie.
9. Op de **eindpunten** pagina, geeft u de openbare eindpunten die u wilt dat beschikbaar voor gebruikers van uw virtuele machine. U kunt bijvoorbeeld kiezen om het inschakelen van HTTP (poort 80) naast de extern bureaublad en PowerShell-eindpunten zijn standaard ingeschakeld. Als u wilt een eindpunt toevoegen, kiest u een in de **poortnaam** vervolgkeuzelijst in een keuzelijst en kies vervolgens de **toevoegen** knop. Als u wilt verwijderen een eindpunt, kies de rode **X** naast de naam in de lijst met eindpunten.
   
    ![De pagina eindpunten in de wizard virtuele machines.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)
   
    De eindpunten die beschikbaar zijn, is afhankelijk van de cloudservice die u hebt geselecteerd voor uw virtuele machine. Zie [Azure Service-eindpunten](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) voor meer informatie.
   
   > [!NOTE]
   > Inschakelen van openbare eindpunten beschikbaar services op de virtuele machine met het internet. Moet installeren en de eindpunten en services goed geconfigureerd op de virtuele machine, zoals instelling toegangsbeheerlijsten (ACL's) voor de eindpunten. Zie [ingesteld van eindpunten aan een virtuele Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) voor meer informatie.
   > 
   > 
10. Nadat u bent met klaar de virtuele machine-instellingen configureren, kiest u de **maken** om te maken van de virtuele machine.
    
     Omdat Azure de virtuele machine, maakt de **Azure Activity Log** geeft de voortgang van de bewerking voor het maken van virtuele machine.
    
     ![Virtuele machine-activiteitenlogboek - uitgevoerd.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)
    
     U kunt alleen de gegevens van de virtuele machine bekijken de **virtuele Machines** tabblad de **Azure Activity Log**.
    
     ![Activiteitenlogboek van virtuele machine - voltooid.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)
    
     Als de bewerking voltooid is, wordt de nieuwe virtuele machine weergegeven onder de **virtuele Machines** knooppunt in Server Explorer. U kunt zich aanmelden bij deze door te klikken op de **verbinding maken met behulp van extern bureaublad** snelkoppeling.
    
     ![Virtuele machine wordt weergegeven in Server Explorer.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## <a name="manage-your-virtual-machines"></a>Uw virtuele machines beheren
U kunt ook weergeven of wijzigen van instellingen voor de virtuele machine op de pagina van de virtuele machine configuratie naast afgesloten, verbinding maken met vernieuwen en controlepunten toevoegen aan de geselecteerde virtuele machine. U kunt:

* Wijzig de grootte van de virtuele machine.
* Selecteer de beschikbaarheid instellen voor gebruik met de virtuele machine.
* Toevoegen, verwijderen of wijzigen van instellingen voor openbare eindpunten.
* Toevoegen, verwijderen of extensies van virtuele machine configureren.
* Informatie weergeven over de schijven die zijn gekoppeld aan de virtuele machine.

### <a name="view-or-change-virtual-machine-settings"></a>Instellingen van de virtuele machine weergeven of wijzigen
1. Kies in Server Explorer uw virtuele machine in de **Azure Virtual Machines** knooppunt.
2. Kies in het snelmenu **configureren** om de configuratiepagina van de virtuele machine weer te geven.
   
    ![De configuratiepagina van de virtuele machine van Azure](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)
3. Gegevens van de virtuele machine weergeven of wijzigen.

### <a name="save-or-restore-the-status-of-your-virtual-machine"></a>Opslaan of de status van de virtuele machine herstellen
Als u uw virtuele machine configureren en software op deze installeren, is het een goed idee om uw voortgang regelmatig door te maken van controlepunten voor virtuele machines opslaan. Een controlepunt is geen momentopname of een installatiekopie van de huidige status van uw virtuele machine. Als er iets mis met de virtuele machine gaat of als u wilt opnieuw configureren van de virtuele machine, kunt u tijd besparen door dat deze wordt hersteld naar een eerdere status van controlepunt in plaats van vanaf het begin beginnen.

### <a name="to-create-a-virtual-machine-checkpoint"></a>Controlepunt van virtuele machines maken
1. Kies in Server Explorer uw virtuele machine in de **Azure Virtual Machines** knooppunt.
2. Kies in het snelmenu **configureren** om de configuratiepagina van de virtuele machine weer te geven.
3. Kies op de configuratiepagina van de **installatiekopie vastleggen** knop.
   
    ![Knop voor configuratie van Azure-pagina vastleggen](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)
   
    De **virtuele Machine vastleggen** dialoogvenster wordt weergegeven.
   
    ![In het dialoogvenster Azure capture-virtuele machine](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)
4. Geef een label van de installatiekopie en de beschrijving. Een standaardlabel en een beschrijving zijn opgegeven, maar u kunt deze desgewenst door uw eigen overschrijven.
5. Als u al hebt Sysprep uitgevoerd op deze virtuele machine, selecteert u de **ik heb Sysprep uitgevoerd op de virtuele machine** vak.
   
    Sysprep is een hulpprogramma dat onder andere systemen-specifieke gegevens worden verwijderd van de versie van de virtuele machine van Windows, waardoor het sjabloon dat anderen kunnen gebruiken. Zie [het vastleggen van een virtuele Windows-Machine voor gebruik als een sjabloon](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) voor meer informatie. Back-up van de virtuele machine voordat Sysprep wordt uitgevoerd.
6. Nadat u bent met klaar de capture-instellingen configureren, kiest u de **vastleggen** knop om het controlepunt te maken.
   
    Omdat Azure het controlepunt maakt de **Azure Activity Log** geeft de voortgang van de bewerking.
   
    ![Vastleggen van een controlepunt van virtuele machines](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)
   
    Wanneer het controlepunt is voltooid, ziet u in de **Azure Activity Log**.
   
    ![Controlepunt is voltooid](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## <a name="to-manage-virtual-machine-checkpoints"></a>Voor het beheren van controlepunten voor virtuele machines
### <a name="to-restore-a-virtual-machine-to-a-previously-saved-state"></a>Een virtuele machine te herstellen naar een eerder opgeslagen status
* Volg de stappen die worden beschreven in [stapsgewijze: uitvoeren Cloud herstelt van Microsoft Azure Virtual Machines met behulp van PowerShell - deel 2](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### <a name="to-delete-a-checkpoint"></a>Een controlepunt verwijderen
1. Ga naar de [Azure-beheerportal](http://go.microsoft.com/fwlink/?LinkID=253103).
2. Kies op de configuratiepagina van de virtuele machine de **installatiekopieën** boven op het tabblad van de pagina.
3. Kies het controlepunt dat u wilt verwijderen, en kies vervolgens de **verwijderen** knop aan de onderkant van de pagina.

## <a name="shut-down-your-virtual-machine"></a>De virtuele machine afsluiten
1. Kies de virtuele machine die u wilt afsluiten in Server Explorer de **Azure Virtual Machines** knooppunt.
2. In het snelmenu, kiest u de **afsluiten** opdracht of kies **configureren** weergeven van de configuratiepagina van de virtuele machine en kies vervolgens de **afsluiten** knop.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken van virtuele machines, [maken van een virtuele Machine waarop Linux](../articles/virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [maken van een virtuele machine waarop Windows wordt uitgevoerd in de Azure preview portal](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

