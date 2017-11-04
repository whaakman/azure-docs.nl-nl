1. Meld u aan bij de [klassieke Azure-portal](http://manage.windowsazure.com).  
2. Klik op de opdrachtbalk aan de onderkant van het venster **nieuw**.
3. Onder **Compute**, klikt u op **virtuele Machine**, en klik vervolgens op **uit galerie**.
   
    ![Maak een nieuwe virtuele Machine][Image1]
4. Onder de **SUSE** groep, selecteert u een installatiekopie van de virtuele machine OpenSUSE en klik op de pijl om door te gaan.
5. Op de eerste **Virtuele-machineconfiguratie** pagina:
   
   * Typ een **virtuele-machinenaam**, zoals 'testlinuxvm'. De naam moet tussen 3 en 15 tekens bevatten, kunnen alleen letters, cijfers en afbreekstreepjes bevatten en moet beginnen met een letter en eindigen met een letter of een cijfer.
   * Controleer of de **laag** en kies een **grootte**. De laag bepaalt de grootte die u kunt kiezen uit. De grootte van invloed op de kosten voor het gebruik ervan, alsmede configuratie-opties, zoals hoeveel gegevens u schijven kunnen koppelen. Zie voor meer informatie [grootten voor virtuele machines](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Typ een **nieuwe gebruikersnaam**, of accepteer de standaardnaam **azureuser**. Deze naam is toegevoegd aan het bestand met de Sudoers.
   * Bepaal welk type **verificatie** te gebruiken. Zie voor richtlijnen voor algemene wachtwoord [sterke wachtwoorden](http://msdn.microsoft.com/library/ms161962.aspx).
6. Op de volgende **Virtuele-machineconfiguratie** pagina:
   
   * De standaard **Maak een nieuwe cloudservice**.
   * In de **DNS-naam** typt u een unieke DNS-naam te gebruiken als onderdeel van het adres, zoals 'testlinuxvm'.
   * In de **regio/affiniteit groep/virtueel netwerk** Selecteer een regio waar deze virtuele-installatiekopie wordt gehost.
   * Onder **eindpunten**, blijven de SSH-eindpunt. U kunt nu toevoegen of toevoegen, wijzigen of verwijderen nadat de virtuele machine is gemaakt.
     
     > [!NOTE]
     > Als u wilt dat een virtuele machine een virtueel netwerk, gebruikt u **moet** opgeven van het virtuele netwerk bij het maken van de virtuele machine. Nadat u de virtuele machine hebt gemaakt, kan u een virtuele machine toevoegen aan een virtueel netwerk. Zie voor meer informatie [Virtual Network-overzicht](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. Op de laatste **Virtuele-machineconfiguratie** up, behoud de standaardinstellingen en klik op het vinkje om het te voltooien.

De portal geeft een lijst van de nieuwe virtuele machine onder **virtuele Machines**. Terwijl de status wordt vermeld als **(Provisioning genoemd)**, de virtuele machine wordt ingesteld. Wanneer de status wordt vermeld als **met**, kunt u op met de volgende stap.

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele Machine
U SSH of PuTTY verbinding maken met de virtuele machine, afhankelijk van het besturingssysteem op de computer maakt u verbinding met:

* Vanaf een computer waarop Linux wordt uitgevoerd, SSH te gebruiken. Typ het volgende achter de opdrachtprompt:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Typ het wachtwoord van de gebruiker.
* Vanaf een computer waarop Windows wordt uitgevoerd, moet u PuTTY gebruiken. Als u niet geïnstalleerd hebt, downloaden via de [PuTTY-downloadpagina][PuTTYDownload].
  
    Sla **putty.exe** naar een map op uw computer. Open een opdrachtprompt, gaat u naar die map en voer **putty.exe**.
  
    Typ de hostnaam, bijvoorbeeld 'testlinuxvm.cloudapp.net' en '22' voor de **poort**.
  
    ![PuTTY scherm][Image6]  

## <a name="update-the-virtual-machine-optional"></a>Werk de virtuele Machine (optioneel)
1. Nadat u met de virtuele machine verbonden bent, kunt u eventueel systeemupdates en -patches installeren. Voor het uitvoeren van de update, typt u:
   
    `$ sudo zypper update`
2. Selecteer **Software**, klikt u vervolgens **Online-Update** voor een lijst met beschikbare updates. Selecteer **accepteren** begint met de installatie en het toepassen van alle nieuwe beschikbaar patches (met uitzondering van de optionele velden).
3. Nadat de installatie wordt uitgevoerd, selecteert u **voltooien**.  Uw systeem is nu up-to-date te houden.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
