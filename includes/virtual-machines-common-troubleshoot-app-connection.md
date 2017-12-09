Er zijn diverse redenen wanneer u mag niet beginnen of verbinding maken met een toepassing die wordt uitgevoerd op Azure een virtuele machine (VM). Oorzaken zijn onder meer de toepassing niet actief of luistert op de verwachte poorten, de luisterpoort geblokkeerd of netwerken regels niet goed doorgeven verkeer naar de toepassing. Dit artikel wordt beschreven methodische wijze om te zoeken en los het probleem.

Als u verbinding maakt met uw virtuele machine met behulp van RDP of SSH problemen ondervindt, raadpleegt u een van de volgende artikelen eerst:

* [Problemen met extern bureaublad-verbindingen naar een op basis van Windows Azure virtuele Machine](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Secure Shell (SSH)-verbindingen met een op basis van Linux virtuele machine van Azure oplossen](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../articles/resource-manager-deployment-model.md). In dit artikel komen beide modellen aan de orde, maar u wordt aangeraden voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="quick-start-troubleshooting-steps"></a>Stappen voor probleemoplossing voor snel starten
Als u verbinding maken met een toepassing hebt, probeert u de volgende algemene stappen voor probleemoplossing. Probeer verbinding te maken voor uw toepassing opnieuw na elke stap:

* De virtuele machine opnieuw opstarten
* Het eindpunt opnieuw / firewall-regels / netwerk beveiligingsregels voor de groep (NSG)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
  * [Klassieke model - eindpunten Cloudservices beheren](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Verbinding maken vanaf een andere locatie, zoals een andere virtuele Azure-netwerk
* De virtuele machine implementeren
  * [Implementeer Windows VM opnieuw](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Virtuele Linux-machine implementeren](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Maak de virtuele machine

Zie voor meer informatie [probleemoplossing eindpunt connectiviteit (RDP/SSH/HTTP, enz. fouten)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Gedetailleerd overzicht voor het oplossen van problemen
Er zijn vier hoofdgebieden oplossen van problemen met de toegang van een toepassing die wordt uitgevoerd op een virtuele machine van Azure.

![problemen oplossen kan toepassing niet starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. De toepassing die wordt uitgevoerd op virtuele machine van Azure.
   * De toepassing zelf correct is gestart?
2. De virtuele machine van Azure.
   * De virtuele machine zelf is juist worden uitgevoerd en reageren op aanvragen?
3. Netwerk van Azure-eindpunten.
   * Cloud service-eindpunten voor virtuele machines in het klassieke implementatiemodel.
   * Netwerkbeveiligingsgroepen en binnenkomende NAT-regels voor virtuele machines in de Resource Manager-implementatiemodel.
   * Kan de stroom van gebruikers aan de virtuele machine/toepassing op de verwachte poorten verkeer?
4. Uw Internet-edge-apparaat.
   * Firewallregels erin verhinderen verkeer stroomt correct?

Voor clientcomputers die toegang hebben tot de toepassing via een site-naar-site VPN- of ExpressRoute-verbinding, zijn de belangrijkste gebieden die problemen kunnen veroorzaken de toepassing en de virtuele machine van Azure.

Volg deze stappen om te bepalen van de bron van het probleem en de correctie.

## <a name="step-1-access-application-from-target-vm"></a>Stap 1: De toepassing openen vanaf het doel VM
Probeert te krijgen tot de toepassing met het juiste clientprogramma van de virtuele machine waarop deze wordt uitgevoerd. Gebruik de lokale hostnaam, het lokale IP-adres of de loopback-adres (127.0.0.1).

![toepassing rechtstreeks vanuit de virtuele machine starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Bijvoorbeeld, als de toepassing een webserver is, open een browser op de virtuele machine en probeert te krijgen tot een webpagina op de virtuele machine wordt gehost.

Als u toegang hebt tot de toepassing, gaat u naar [stap 2](#step2).

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* De toepassing wordt uitgevoerd op de virtuele doelmachine.
* De toepassing luistert op de verwachte TCP en UDP-poorten.

Gebruik op Windows en Linux gebaseerde virtuele machines, de **netstat - a** opdracht om de actieve controlepoorten weer te geven. Bekijk de uitvoer voor de verwachte poorten waarop uw toepassing moet luisteren. De toepassing opnieuw starten of configureren voor het gebruik van de verwachte poorten, indien nodig en probeer het opnieuw toegang krijgen tot de toepassing lokaal.

## <a id="step2"></a>Stap 2: Toegang tot de toepassing van een andere virtuele machine in hetzelfde virtuele netwerk
Probeer toegang tot de toepassing van een andere virtuele machine, maar in hetzelfde virtuele netwerk, met behulp van de VM-hostnaam of het toegewezen Azure public, private of provider IP-adres. Voor virtuele machines is gemaakt met behulp van het klassieke implementatiemodel, het openbare IP-adres van de cloudservice niet te gebruiken.

![toepassing van een andere virtuele machine starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Bijvoorbeeld, als de toepassing een webserver is, probeert te krijgen tot een webpagina vanuit een browser op een andere virtuele machine in hetzelfde virtuele netwerk.

Als u toegang hebt tot de toepassing, gaat u naar [stap 3](#step3).

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* De hostfirewall op de doel-virtuele machine toestaat de binnenkomende aanvraag en het uitgaande antwoord verkeer.
* Inbraakdetectie of netwerkbeheer-software op de doel-virtuele machine met toestaat het verkeer.
* Cloud Services-eindpunten of Netwerkbeveiligingsgroepen bieden de mogelijkheid het verkeer:
  * [Klassieke model - eindpunten Cloudservices beheren](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager-model - Netwerkbeveiligingsgroepen beheren](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
* Een afzonderlijk onderdeel dat wordt uitgevoerd in uw virtuele machine in het pad tussen de test virtuele machine en de virtuele machine, zoals een load balancer of een firewall, toestaat het verkeer.

Gebruik Windows Firewall met geavanceerde beveiliging om te bepalen of de firewallregels voor binnenkomend en uitgaand verkeer van uw toepassing sluiten op een Windows-gebaseerde virtuele machine.

## <a id="step3"></a>Stap 3: Toegang tot de toepassing van buiten het virtuele netwerk
Probeer de toepassing openen vanaf een computer buiten het virtuele netwerk als de virtuele machine waarop de toepassing wordt uitgevoerd. Gebruik een ander netwerk als de oorspronkelijke clientcomputer.

![toepassing starten vanaf een computer buiten het virtuele netwerk](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Bijvoorbeeld, als de toepassing een webserver is, probeert te krijgen tot de webpagina vanuit een browser op een computer die zich niet in het virtuele netwerk.

Als u geen toegang de toepassing tot, controleert u of de volgende instellingen:

* Gemaakt met het klassieke implementatiemodel voor virtuele machines:
  
  * Controleer of dat de endpoint-configuratie voor de virtuele machine het binnenkomende verkeer, met name het protocol (TCP of UDP) en de openbare en persoonlijke poortnummers toestaat.
  * Controleer of dat toegangsbeheerlijsten (ACL's) op het eindpunt niet voorkomen binnenkomend verkeer van het Internet dat.
  * Zie voor meer informatie [ingesteld van eindpunten aan een virtuele Machine](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Voor virtuele machines gemaakt met het implementatiemodel van Resource Manager:
  
  * Controleer of dat de binnenkomende NAT-regelconfiguratie voor de virtuele machine het binnenkomende verkeer, met name het protocol (TCP of UDP) en de openbare en persoonlijke poortnummers toestaat.
  * Controleer of dat Netwerkbeveiligingsgroepen bieden de mogelijkheid de binnenkomende aanvraag en antwoord uitgaand verkeer.
  * Zie voor meer informatie [Wat is een netwerkbeveiligingsgroep (NSG)?](../articles/virtual-network/virtual-networks-nsg.md)

Als de virtuele machine of het eindpunt lid is van een set met gelijke taakverdeling is:

* Controleer of de test-protocol (TCP of UDP) en het poortnummer juist zijn.
* Als de test-protocol en poort verschilt van de set met gelijke taakverdeling-protocol en poort:
  * Controleer of de toepassing luistert op de test-protocol (TCP of UDP) en het poortnummer (Gebruik **netstat-a** op de doel-virtuele machine).
  * Controleer of dat de hostfirewall op de doel-virtuele machine de inkomende test-aanvraag en uitgaande test antwoord verkeer toestaat.

Als u toegang hebt tot de toepassing, zorg ervoor dat uw randapparaat Internet toestaat:

* Het verkeer voor het verzoek van uitgaande toepassing vanaf de clientcomputer naar de virtuele machine van Azure.
* De binnenkomende aanvraag antwoord verkeer van de virtuele machine van Azure.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Stap 4 als u geen toegang de toepassing tot gebruikt IP-Controleer of de instellingen controleren. 

Zie voor meer informatie [Azure-netwerk bewakingsoverzicht](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Aanvullende bronnen
[Problemen met extern bureaublad-verbindingen naar een op basis van Windows Azure virtuele Machine](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Secure Shell (SSH)-verbindingen met een op basis van Linux virtuele machine van Azure oplossen](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

