1. Voer het installatiebestand voor de geïntegreerde Setup uit.
2. In **voordat u begint**, selecteer **de configuratieserver en processerver installeren**.

    ![Voordat u begint](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. Klik bij **Licentievoorwaarden voor software van derden** op **Ik ga akkoord** om MySQL te downloaden en te installeren.

    ![Software van derden](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. Selecteer bij **Registratie** de registratiesleutel die u hebt gedownload uit de kluis.

    ![Registratie](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. Geef bij **Internetinstellingen** op hoe de provider die op de configuratieserver wordt uitgevoerd, via internet verbinding moet maken met Azure Site Recovery. Zorg ervoor dat u hebt de vereiste URL's toegestaan.

    - Als u wilt verbinding maken met de proxy die momenteel ingesteld op de machine, selecteer **verbinding maken met Azure Site Recovery via een proxyserver**.
    - Als u wilt dat de Provider rechtstreeks verbinding maakt, selecteert u **rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
    - Als de bestaande proxy verificatie is vereist of als u wilt een aangepaste proxy gebruikt voor verbinding met de Provider, selecteer **verbinding maken met aangepaste proxyinstellingen**, en geeft u het adres, poort en referenties.
     ![Firewall](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. Tijdens Setup wordt in **Controle op vereisten** gecontroleerd of de installatie kan worden uitgevoerd. Als er een waarschuwing wordt weergegeven over **Synchronisatiecontrole voor algemene tijd**, moet u controleren of de tijd op de systeemklok (instellingen voor **datum en tijd**) overeenkomt met de tijdzone.

    ![Vereisten](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. Maak bij **MySQL-configuratie** referenties voor aanmelden bij de MySQL-serverinstantie die is geïnstalleerd.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. Selecteer bij **Details van de omgeving** of u virtuele VMware-machines wilt repliceren. Als u bent, klikt u vervolgens controleert Setup of PowerCLI 6.0 is geïnstalleerd.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. Selecteer bij **Installatielocatie** waar u de binaire bestanden wilt installeren en de cache wilt opslaan. Het station dat u selecteert, moet ten minste 5 GB vrije schijfruimte bevatten, maar wij raden u aan een cachestation te gebruiken met minstens 600 GB vrije ruimte.

    ![Installatielocatie](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. Geef bij **Netwerk selecteren** de listener op (netwerkadapter en SSL-poort) met behulp waarvan de configuratieserver replicatiegegevens verzendt en ontvangt. Poort 9443 is de standaardpoort voor het verzenden en ontvangen van replicatieverkeer, maar u kunt dit poortnummer aanpassen aan de vereisten van de omgeving. Naast poort 9443 wordt ook poort 443 geopend. Deze wordt door een webserver gebruikt om replicatiebewerkingen in te delen. Gebruik poort 443 niet voor het verzenden of ontvangen van replicatieverkeer.

    ![Netwerk selecteren](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Lees de informatie bij **Samenvatting** en klik op **Installeren**. Wanneer de installatie is voltooid, wordt er een wachtwoordzin gegenereerd. U hebt deze nodig bij het inschakelen van de replicatie. Kopieer de wachtwoordzin daarom en bewaar deze op een veilige locatie.

    ![Samenvatting](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Na voltooiing van de registratie wordt de server weergegeven op de blade **Instellingen** > **Servers** in de kluis.
