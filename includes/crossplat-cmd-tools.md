# <a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Opdrachtregelprogramma's van Azure voor Mac en Linux gebruiken
In deze handleiding wordt beschreven hoe u de opdrachtregelprogramma's van Azure voor Mac en Linux gebruikt om services in Azure te maken en te beheren. De behandelde scenario's zijn **De hulpprogramma's installeren**, **Publicatie-instellingen importeren**, **Websites in Azure maken en beheren** en **Virtuele Azure-machines maken en beheren**. Voor uitgebreide naslagdocumentatie raadpleegt u de [Azure command-line tool for Mac and Linux Documentation][reference-docs] (Documentatie bij het opdrachtregelprogramma van Azure voor Mac en Linux). 

## <a name="table-of-contents"></a>Inhoudsopgave
* [Wat zijn de opdrachtregelprogramma's van Azure voor Mac en Linux?](#Overview)
* [De opdrachtregelprogramma's van Azure voor Mac en Linux installeren](#Download)
* [Een Azure-account maken](#CreateAccount)
* [Publicatie-instellingen downloaden en importeren](#Account)
* [Een Azure-website maken en beheren](#WebSites)
* [Een virtuele Azure-machine maken en beheren](#VMs)

<h2><a id="Overview"></a>Wat zijn de opdrachtregelprogramma's van Azure voor Mac en Linux?</h2>

De opdrachtregelprogramma's van Azure voor Mac en Linux bestaan uit een reeks opdrachtregelprogramma's voor het implementeren en beheren van Azure-services.

De ondersteunde taken zijn:

* Publicatie-instellingen importeren.
* Azure-websites maken en beheren.
* Virtuele Azure-machines maken en beheren.

Voor een volledige lijst met ondersteunde opdrachten typt u `azure -help` op de opdrachtregel nadat u de hulpprogramma's hebt geïnstalleerd of raadpleegt u de [naslagdocumentatie][reference-docs].

<h2><a id="Download">De opdrachtregelprogramma's van Azure voor Mac en Linux installeren</a></h2>

De volgende lijst bevat informatie voor het installeren van de opdrachtregelprogramma's, afhankelijk van uw besturingssysteem:

* **Mac**: Download de [Azure SDK Installer][mac-installer]. Open het gedownloade PKG-bestand en voer de installatiestappen uit.
* **Linux**: Installeer de nieuwste versie van [Node.js] [ nodejs-org] (zie [Install Node.js via Package Manager][install-node-linux] (Node.js installeren via Pakketbeheer) en voer de volgende opdracht uit:
  
        npm install azure-cli -g
  
    **Opmerking**: Mogelijk moet u deze opdracht uitvoeren met verhoogde bevoegdheden:
  
        sudo npm install azure-cli -g
* **Windows**: Voer de Windows Installer (MSI-bestand) uit die hier beschikbaar is: [Opdrachtregelprogramma's van Azure][windows-installer].

Als u de installatie wilt testen, typt u `azure` achter de opdrachtregel. Als de installatie is voltooid, ziet u een lijst met alle beschikbare `azure`-opdrachten.

<h2><a id="CreateAccount"></a>Een Azure-account maken</h2>

U hebt een Azure-account nodig om de opdrachtregelprogramma's van Azure voor Mac en Linux te gebruiken.

Open een webbrowser, blader naar [http://www.windowsazure.com][windowsazuredotcom] en klik in de rechterbovenhoek op **gratis account**.

![Azure-website][Azure Web Site]

Volg de instructies voor het maken van een account.

<h2><a id="Account"></a>Publicatie-instellingen downloaden en importeren</h2>

Als u aan de slag wilt gaan, moet u eerst de publicatie-instellingen downloaden en importeren. Vervolgens kunt u de hulpprogramma's gebruiken om Azure Services te maken en te beheren. Gebruik de opdracht `account download` om de publicatie-instellingen te downloaden:

    azure account download

Hiermee opent u uw standaardbrowser en wordt u gevraagd u aan te melden bij de Beheerportal. Wanneer u zich hebt aangemeld, wordt het bestand `.publishsettings` gedownload. Noteer waar dit bestand wordt opgeslagen.

Importeer vervolgens het bestand `.publishsettings` door de volgende opdracht uit te voeren, waarbij u `{path to .publishsettings file}` vervangt door het pad naar uw `.publishsettings`-bestand:

    azure account import {path to .publishsettings file}

U kunt alle informatie die met de opdracht <code>import</code> is opgeslagen, verwijderen met behulp van de opdracht <code>account clear</code>:

    azure account clear

Voor een overzicht van de opties voor `account`-opdrachten gebruikt u de `-help`-optie:

    azure account -help

Wanneer u de publicatie-instellingen hebt geïmporteerd, moet u uit veiligheidsoverwegingen het bestand `.publishsettings` verwijderen.

> [!NOTE]
> Wanneer u publicatie-instellingen importeert, worden de referenties voor toegang tot uw Azure-abonnement opgeslagen in de map `user`. De map `user` wordt beveiligd door het besturingssysteem. Het wordt echter aanbevolen om extra stappen te ondernemen om de map `user` te versleutelen. U kunt dit op de volgende manieren doen:    
> 
> * In Windows wijzigt u de mapeigenschappen of gebruikt u BitLocker.
> * Op een Mac schakelt u FileVault voor de map in.
> * In Ubuntu gebruikt u de functie om de basismap te versleutelen. Andere Linux-distributies bieden equivalente functies.
> 
> 

Nu kunt u Azure-websites en virtuele Azure-machines gaan maken en beheren.  

<h2><a id="WebSites"></a>Een Azure-website maken en beheren</h2>

### <a name="create-a-website"></a>Een website maken
Als u een Azure-website wilt maken, maakt u eerst een lege map met de naam `MySite` en bladert u naar die map.

Voer daarna de volgende opdracht uit:

    azure site create MySite --git

De uitvoer van deze opdracht bevat de standaard-URL voor de zojuist gemaakte website. Met de optie `--git` kunt u git gebruiken om naar uw website te publiceren door git-opslagplaatsen te maken in zowel de lokale toepassingsmap als in het datacenter van uw website. Als uw lokale map al een git-opslagplaats is, wordt met de opdracht een nieuwe, voor de bestaande opslagplaats externe opslagplaats toegevoegd, die naar de bibliotheek in het datacenter van uw website wijst.

U kunt de opdracht `azure site create` uitvoeren met een van de volgende opties:

* `--location [location name]`. Met deze optie kunt u de locatie opgeven van het datacenter waarin uw website is gemaakt, bijvoorbeeld 'VS - west'. Als u deze optie weglaat, wordt u gevraagd een locatie te kiezen.
* `--hostname [custom host name]`. Met deze optie kunt u een aangepaste hostnaam voor uw website opgeven.

Daarna kunt u inhoud in de map van uw website plaatsen. Gebruik de gewone git-stroom (`git add`, `git commit`) om uw inhoud door te voeren. Gebruik de volgende git-opdracht om de inhoud van uw website naar Azure te pushen: 

    git push azure master

### <a name="set-up-publishing-from-github"></a>Publicatie vanuit GitHub instellen
Als u continue publicatie vanuit een GitHub-opslagplaats wilt instellen, gebruikt u bij het maken van uw site de optie `--GitHub`:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Als u een lokale kloon van een GitHub-opslagplaats hebt of als u een opslagplaats hebt met één externe verwijzing naar een GitHub-opslagplaats, wordt met deze opdracht automatisch code in de GitHub-opslagplaats naar uw site gepubliceerd. Daarna worden alle wijzigingen die naar de GitHub-opslagplaats worden gepusht, automatisch naar uw site gepubliceerd.

Wanneer u publiceren vanuit GitHub instelt, wordt standaard gebruikgemaakt van de hoofdvertakking. Als u een andere vertakking wilt gebruiken, voert u vanuit uw lokale opslagplaats de volgende opdracht uit:

    azure site repository <branch name>

### <a name="configure-app-settings"></a>App-instellingen configureren
App-instellingen zijn sleutel-/waardeparen waarover uw toepassing in runtime kan beschikken. Als er app-instellingen worden ingesteld voor een Azure-website, overschrijven deze de gelijknamige sleutel die is gedefinieerd in het Web.config-bestand van uw website. Voor Node.js- en PHP-toepassingen zijn app-instellingen beschikbaar als omgevingsvariabelen. In het volgende voorbeeld ziet u hoe u een sleutel-/waardepaar instelt:

    azure site config add <key>=<value> 

Voor een overzicht van alle sleutel-/waardeparen gebruikt u de volgende opdracht:

    azure site config list 

Als u de sleutel kent en de waarde ervan wilt zien, gebruikt u:

    azure site config get <key> 

Als u de waarde van een bestaande sleutel wilt wijzigen, moet u eerst de bestaande sleutel wissen en de sleutel vervolgens opnieuw toevoegen. De opdracht om te wissen is:

    azure site config clear <key> 

### <a name="list-and-show-sites"></a>Lijst van sites weergeven
Gebruik de volgende opdracht om een lijst van uw websites weer te geven:

    azure site list

Voor gedetailleerde informatie over een site gebruikt u de opdracht `site show`. In het volgende voorbeeld worden de details weergegeven voor `MySite`:

    azure site show MySite

### <a name="stop-start-or-restart-a-site"></a>Een site stoppen, starten of opnieuw starten
Met de opdrachten `site stop`, `site start` of `site restart` kunt u een site stoppen, starten of opnieuw starten:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

### <a name="delete-a-site"></a>Een site verwijderen
Met de opdracht `site delete` kunt u een site verwijderen:

    azure site delete MySite

Als u een van bovenstaande opdrachten uitvoert vanuit de map waarin u `site create` hebt uitgevoerd, hoeft u de sitenaam `MySite` niet als laatste parameter op te geven.

Voor een volledige lijst van `site`-opdrachten, gebruikt u de optie `-help`:

    azure site -help 

<h2><a id="VMs"></a>Een virtuele Azure-machine maken en beheren</h2>

Een virtuele Azure-machine is gemaakt op basis van een installatiekopie van een virtuele machine (een VHD-bestand) die u opgeeft of die beschikbaar is in de galerie met installatiekopieën. Gebruik de opdracht `vm image list` als u wilt zien welke installatiekopieën er beschikbaar zijn:

    azure vm image list

Met de opdracht `vm create` kunt u een virtuele machine inrichten en starten vanuit een van de beschikbare installatiekopieën. In het volgende voorbeeld ziet u hoe u een virtuele Linux-machine maakt (`myVM` genoemd) vanuit een installatiekopie in de galerie met installatiekopieën (CentOS 6.2). De naam en het wachtwoord van de rootgebruiker voor de virtuele machine zijn respectievelijk `myusername` en `Mypassw0rd`. (De parameter `--location` geeft aan in welk datacenter de virtuele machine is gemaakt. Als u de parameter `--location` weglaat, wordt u gevraagd een locatie te kiezen.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

U zou ook de vlag `--ssh` (Linux) of de vlag `--rdp` (Windows) aan `vm create` kunnen doorgeven om externe verbindingen met de zojuist gemaakte virtuele machine in te schakelen.

Als u een virtuele machine echter liever inricht vanuit een aangepaste installatiekopie, kunt u met de opdracht `vm image create` een installatiekopie maken vanuit een VHD-bestand en vervolgens de opdracht `vm create` gebruiken om de virtuele machine in te richten. In het volgende voorbeeld ziet u hoe u een Linux-installatiekopie maakt (`myImage` genoemd) op basis van een lokale VHD-bestand. (De parameter `--location` geeft aan waar de installatiekopie is opgeslagen.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

In plaats van een installatiekopie te maken op basis van een lokaal VHD-bestand kunt u ook een installatiekopie maken op basis van een VHD-bestand dat is opgeslagen in Azure Blob Storage. U kunt dit doen met de parameter `blob-url`:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Nadat er een installatiekopie is gemaakt, kunt u met `vm create` een virtuele machine inrichten op basis van de installatiekopie. Met onderstaande opdracht maakt u een virtuele machine met de naam `myVM` op basis van bovenstaande installatiekopie (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nadat u een virtuele machine hebt ingericht, wilt u misschien eindpunten maken om (bijvoorbeeld) externe toegang tot uw virtuele machine mogelijk te maken. In het volgende voorbeeld wordt de opdracht `vm create endpoint` gebruikt om externe poort 22 en lokale poort 22 op `myVM` te openen:

    azure vm endpoint create myVM 22 22

U kunt gedetailleerde informatie over een virtuele machine (zoals IP-adres, DNS-naam en informatie over eindpunten) krijgen met de opdracht `vm show`:

    azure vm show myVM

Gebruik een van de volgende opdrachten als u de virtuele machine wilt afsluiten, starten of opnieuw wilt opstarten:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Als u de virtuele machine wilt verwijderen, gebruikt u de opdracht `vm delete`:

    azure vm delete myVM

Voor een volledige lijst van opdrachten voor het maken en beheren van virtuele machines gebruikt u de optie `-h`:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com



<!--HONumber=Jan17_HO5-->


