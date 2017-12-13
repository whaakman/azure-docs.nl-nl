1. Navigeer naar uw Jenkins virtuele machine in uw browser. Aangezien het dashboard Jenkins niet toegankelijk via niet-beveiligde HTTP is, verschijnt een bericht dat u wilt gebruiken van SSH naar tunnel naar de virtuele machine.

    ![Jenkins bevat informatie waarin wordt uitgelegd hoe u via SSH verbinding maken met de Jenkins virtuele machine.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ssh-instructions.png)

1. Open een venster bash of terminal met toegang tot SSH.

1. Voer de volgende `ssh` uitvoert, vervangt de  **&lt;username >** en  **&lt;domain >** tijdelijke aanduidingen door de waarden die zijn opgegeven bij het maken van de Jenkins virtuele machine:

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 <username>@<domain>.eastus.cloudapp.azure.com
    ```

1. Ga als volgt de `ssh` prompts aan te melden bij de Jenkins virtuele machine.

1. Voer de volgende opdracht voor het ophalen van het eerste wachtwoord voor het ontgrendelen Jenkins.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. Het wachtwoord dat wordt weergegeven in het venster bash of terminal naar het Klembord kopiëren.

1. Navigeer in uw browser naar `http://localhost:8080`.

1. Plak het wachtwoord eerder hebt opgehaald in de **beheerderswachtwoord** veld en selecteert u een **doorgaan**.

    ![Jenkins een eerste wachtwoord die u gebruiken moet voor het ontgrendelen van de virtuele machine van Jenkins de eerste keer dat u verbinding maakt met opgeslagen.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-unlock.png)

1. Selecteer **voorgestelde invoegtoepassingen installeren**.

    ![Jenkins kunt u gemakkelijk een verzameling van voorgestelde plugins na de eerste vermelding installeren](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-customize.png)

1. Op de **eerste Admin-gebruiker maken** pagina, de gebruiker met beheerdersrechten en het wachtwoord voor het dashboard Jenkins maken en selecteer **opslaan en voltooien**.

1. Op de **Jenkins is klaar!** pagina **aan de slag met Jenkins**.

    ![Eenmaal Jenkins is geïnstalleerd en geconfigureerd voor toegang, kunt u met behulp van het maken en bouwen van taken kunt starten.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ready.png)