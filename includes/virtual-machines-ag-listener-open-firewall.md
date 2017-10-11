In deze stap maakt u een firewallregel om de testpoort voor het eindpunt voor netwerktaakverdeling (zoals eerder opgegeven 59999) te openen en een andere regel voor de poort beschikbaarheidsgroeplistener openen. Omdat u het eindpunt taakverdeling op de virtuele machines die replica's van beschikbaarheidsgroepen bevatten gemaakt, moet u de testpoort en de listener-poort op de betreffende virtuele machines te openen.

1. Start op virtuele machines die als host fungeren van replica's, **Windows Firewall met geavanceerde beveiliging**.

2. Met de rechtermuisknop op **regels voor binnenkomende verbindingen**, en klik vervolgens op **nieuwe regel**.

3. Op de **regeltype** pagina **poort**, en klik vervolgens op **volgende**.

4. Op de **protocollen en poorten** pagina **TCP**, type **59999** in de **specifieke lokale poorten** vak en klik vervolgens op  **Volgende**.

5. Op de **actie** pagina, houden **de verbinding toestaan** geselecteerd en klik vervolgens op **volgende**.

6. Op de **profiel** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**.

7. Op de **naam** pagina in de **naam** tekst, geeft u de regelnaam van een, zoals **altijd op Listener-test poort**, en klik vervolgens op **voltooien**.

8. Herhaal de voorgaande stappen voor de poort beschikbaarheidsgroeplistener (zoals opgegeven eerder in de parameter $EndpointPort van het script) en geef vervolgens een naam voor de desbetreffende regel, zoals **altijd op Listener-poort**.

