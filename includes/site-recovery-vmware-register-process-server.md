1. Maak verbinding met de virtuele machine van de processerver via Verbinding met extern bureaublad.
2. U kunt cspsconfigtool.exe openen door te klikken op de snelkoppeling op het bureaublad. (Het hulpprogramma wordt automatisch gestart als dit de eerste keer is dat u zich bij de processerver aanmeldt).
  * De volledig gekwalificeerde naam (FQDN) of het IP-adres van de configuratieserver
  * De poort waarop de configuratieserver luistert. De waarde hiervoor moet 443 zijn
  * De wachtwoordzin om verbinding te maken met de configuratieserver.
  * De gegevensoverdrachtpoort die voor deze processerver moet worden geconfigureerd. Wijzig de standaardwaarde niet tenzij u deze in uw omgeving in een ander poortnummer hebt gewijzigd.

    ![Processerver registreren](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Klik op de knop Opslaan om de configuratie op te slaan.
4. Als de registratie is voltooid, wordt de processerver weergegeven onder uw configuratieserver. Deze kan nu voor failback worden gebruikt.

> [!TIP]
> Het hulpprogramma om de processerver te configureren, wordt gestart als u dubbelklikt op de snelkoppeling **cspsconfigtool** die zich op het bureaublad van de virtuele machine bevindt.


<!--HONumber=Feb17_HO4-->


