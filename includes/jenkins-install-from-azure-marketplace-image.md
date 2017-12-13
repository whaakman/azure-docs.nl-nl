1. Open in uw browser de [Azure Marketplace-installatiekopie voor Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Selecteer **ophalen IT nu**.

    ![Selecteer deze nu GIT om het installatieproces voor de Jenkins Marketplace-installatiekopie te starten.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Bekijk de informatie over de prijzen details en voorwaarden en selecteer **doorgaan**.

    ![Jenkins Marketplace installatiekopie prijzen en voorwaarden informatie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Selecteer **maken** de Jenkins-server configureren in de Azure portal. 

    ![Installeer de Jenkins Marketplace-installatiekopie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. In de **basisbeginselen** tabblad, geeft u de volgende waarden:

    - **Naam** -Voer `Jenkins`.
    - **Gebruiker** -Geef de gebruikersnaam op voor gebruik bij het aanmelden bij de virtuele machine waarop Jenkins wordt uitgevoerd.
    - **Verificatietype** : Selecteer **wachtwoord**.
    - **Wachtwoord** -Voer het wachtwoord moet worden gebruikt bij het aanmelden bij de virtuele machine waarop Jenkins wordt uitgevoerd.
    - **Bevestig het wachtwoord** -Geef het wachtwoord moet worden gebruikt bij het aanmelden bij de virtuele machine waarop Jenkins wordt uitgevoerd opnieuw.
    - **Versietype Jenkins** : Selecteer **LTS**.
    - **Abonnement** -het Azure-abonnement waaraan u wilt installeren Jenkins selecteren.
    - **Resourcegroep** : Selecteer **nieuw**, en voer een naam voor de resourcegroep die fungeert als een logische container voor de verzameling van resources die gezamenlijk uw Jenkins-installatie.
    - **Locatie** : Selecteer **VS-Oost**.

    ![Voer de verificatie- en groepsgegevens voor Jenkins op het tabblad basis.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Selecteer **OK** om door te gaan naar de **instellingen** tabblad. 

1. In de **instellingen** tabblad, geeft u de volgende waarden:

    - **De grootte van** -formaat van de juiste optie voor uw Jenkins virtuele machine.
    - **VM-schijftype** – Geef de harde schijf (harde schijf) of SSD (solid-state drive) om aan te geven welk type opslagschijf is toegestaan voor de Jenkins virtuele machine.
    - **Openbaar IP-adres** -naam van het IP-adres wordt standaard ingesteld op de Jenkins-naam die u hebt opgegeven in de vorige pagina en het achtervoegsel - IP. U kunt de optie die standaard wijzigen.
    - **Domeinnaamlabel** -Geef de waarde voor de volledige URL naar de Jenkins virtuele machine.

    ![Voer de virtuele machine-instellingen voor Jenkins op het tabblad instellingen.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Selecteer **OK** om door te gaan naar de **samenvatting** tabblad.

1. Wanneer de **samenvatting** tabblad weergegeven, wordt de informatie hebt ingevoerd, wordt gevalideerd. Eenmaal u ziet de **validatie geslaagd** bericht, selecteer **OK**. 

    ![Het tabblad Overzicht wordt weergegeven en valideert de geselecteerde opties.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Wanneer de **maken** tabblad worden weergegeven, selecteert u **maken** de Jenkins virtuele machine maken. Wanneer de server klaar is, wordt een melding weergegeven in de Azure portal.

    ![Jenkins is gereed melding.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)