

Wanneer u een toepassing webproject voor Azure maakt, kunt u een virtuele machine in Azure inrichten. U kunt vervolgens de virtuele machine configureren met extra software of de virtuele machine worden gebruikt voor diagnostische of foutopsporing doeleinden.

Volg deze stappen voor het maken van een virtuele machine wanneer u een webtoepassing maakt:

1. Klik in Visual Studio **bestand** > **nieuw** > **Project** > **Web**, en kies vervolgens **ASP.NET-webtoepassing** (onder de **Visual C#** of **Visual Basic** knooppunten).
2. In de **nieuw ASP.NET-Project** in het dialoogvenster Selecteer het type van de webtoepassing die u wilt en de Azure-sectie van het dialoogvenster (in de rechterbenedenhoek) en zorg ervoor dat de **Host in de cloud** selectievakje geselecteerde (dit selectievakje **maken van externe bronnen** in sommige installaties).
   
    ![][0]
3. Kies voor dit voorbeeld in de vervolgkeuzelijst onder Microsoft Azure **virtuele Machine (v1)**, en klik vervolgens op de **OK** knop.
4. Aanmelden bij Azure als u wordt gevraagd. De **virtuele Machine maken** dialoogvenster wordt weergegeven.
   
    ![][2]
5. In de **DNS-naam** Voer een naam voor de virtuele machine. De DNS-naam moet uniek zijn in Azure. Als de ingevoerde naam niet beschikbaar is, verschijnt er een rood uitroepteken.
6. In de **installatiekopie** kiest u de installatiekopie die u wilt op basis van de virtuele machine. U kunt een van de installatiekopieën van de standaard virtuele machine van Azure of uw installatiekopie die u hebt geüpload naar Azure.
7. Laat de **IIS inschakelen en Web Deploy** selectievakje geselecteerd tenzij u van plan bent een andere webserver installeren. Het niet mogelijk om te publiceren vanuit Visual Studio als u Web Deploy uitschakelt. U kunt IIS en Web Deploy toevoegen aan een van de ingepakte afbeeldingen van Windows Server, met inbegrip van uw eigen aangepaste installatiekopieën.
8. In de **grootte** Kies de grootte van de virtuele machine.
9. Geef de referenties aanmelden voor deze virtuele machine. Noteer deze, omdat u moet ze toegang krijgen tot de machine via Extern bureaublad.
10. In de **locatie** Kies de regio voor het hosten van de virtuele machine.
11. Klik op de **OK** knop om te beginnen met het maken van de virtuele machine. U kunt de voortgang van de bewerking in volgen de **uitvoer** venster.
    
    ![][3]
12. Wanneer de virtuele machine is ingericht, gepubliceerde scripts worden gemaakt in een **PublishScripts** knooppunt in uw oplossing. Het gepubliceerde-script wordt uitgevoerd en voorziet in een virtuele machine in Azure. De **uitvoer** venster geeft de status. Het script voert de volgende bewerkingen voor het instellen van de virtuele machine:
    
    * Als deze nog niet bestaat, maakt de virtuele machine.
    * Een opslagaccount maakt met een naam die met begint `devtest`, maar alleen als er al een opslagaccount in de opgegeven regio.
    * Maakt een cloudservice als een container voor de virtuele machine en een Webrol voor de webtoepassing maakt.
    * Hiermee configureert u Web Deploy op de virtuele machine.
    * Hiermee configureert u IIS en ASP.NET op de virtuele machine.
    
    ![][4]
13. (Optioneel) U kunt verbinding maken met de nieuwe virtuele machine. In **Server Explorer**, vouw de **virtuele Machines** knooppunt, kiest u het knooppunt voor de virtuele machine die u hebt gemaakt, en in het snelmenu, kiest u **verbinding maken met extern bureaublad**. U kunt ook in **Cloud Explorer** kunt u **openen in de Portal** in het snelmenu en maak verbinding met de virtuele machine er.
    
    ![][5]

## <a name="next-steps"></a>Volgende stappen
Als u aanpassen van de gepubliceerde scripts die u hebt gemaakt wilt, leest u meer gedetailleerde informatie op [Windows PowerShell-Scripts gebruiken om te publiceren op de ontwikkeling en testomgevingen](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
