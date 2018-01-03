## <a name="os-config"></a>IP-adressen toevoegen aan een VM-besturingssysteem

Maak verbinding met en meld u aan bij een virtuele machine die u met meerdere privé-IP-adressen hebt gemaakt. U moet alle privé-IP-adressen (met inbegrip van het primaire) die u aan de virtuele machine hebt toegevoegd, handmatig toevoegen. Voer de volgende stappen uit voor het VM-besturingssysteem:

### <a name="windows"></a>Windows

1. Typ vanaf een opdrachtprompt *ipconfig /all*.  U ziet alleen het *primaire* privé-IP-adres (via DHCP).
2. Typ *ncpa.cpl* in het opdrachtpromptvenster om het venster **Netwerkverbindingen** te openen.
3. Open de eigenschappen van de geschikte adapter: **LAN-verbinding**.
4. Dubbelklik op Internet Protocol versie 4 (IPv4).
5. Selecteer **Het volgende IP-adres gebruiken** en voer de volgende waarden in:

    * **IP-adres**: voer het *primaire* privé-IP-adres in
    * **Subnetmasker**: stel dit in op basis van uw subnet. Als het subnet bijvoorbeeld een /24 subnet is, is het subnetmasker 255.255.255.0.
    * **Standaardgateway**: het eerste IP-adres in het subnet. Als uw subnet 10.0.0.0/24 is, is het IP-adres van de gateway 10.0.0.1.
    * Klik op **De volgende DNS-serveradressen gebruiken** en voer de volgende waarden in:
        * **DNS-voorkeursserver**: als u niet uw eigen DNS-server gebruikt, voert u 168.63.129.16 in.  Als u uw eigen DNS-server gebruikt, voert u het IP-adres voor de server in.
    * Klik op de knop **Geavanceerd** en voeg extra IP-adressen toe. Voeg elk van de secundaire privé-IP-adressen die in stap 8 worden vermeld, toe aan de NIC met hetzelfde subnet dat voor het primaire IP-adres is opgegeven.
        >[!WARNING] 
        >Als u de bovenstaande stappen niet correct uitvoert, kan het zijn dat de verbinding met uw virtuele machine wordt verbroken. Zorg ervoor dat de gegevens die u hebt ingevoerd voor stap 5 kloppen voordat u doorgaat.

    * Klik op **OK** om de TCP/IP-instellingen te sluiten en vervolgens nogmaals op **OK** om de instellingen van de netwerkadapter te sluiten. Uw RDP-verbinding wordt opnieuw tot stand gebracht.

6. Typ vanaf een opdrachtprompt *ipconfig /all*. Alle IP-adressen die u hebt toegevoegd, worden weergegeven en DHCP is uitgeschakeld.
7. Windows configureren voor het gebruik van de persoonlijke IP-adres van de primaire IP-configuratie in Azure als het primaire IP-adres voor Windows. Zie [geen internettoegang van VM van Windows Azure met meerdere IP-adressen](https://support.microsoft.com/help/4040882/no-internet-access-from-azure-windows-vm-that-has-multiple-ip-addresse) voor meer informatie. 


### <a name="validation-windows"></a>Validatie (Windows)

Als u wilt controleren of u via uw secundaire IP-configuratie verbinding kunt maken met internet via de openbare IP die eraan is gekoppeld, gebruikt u de volgende opdracht nadat u de bovenstaande stappen hebt gevolgd:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Voor secundaire IP-configuraties, kunt u alleen een ping naar het Internet als de configuratie een openbaar IP-adres dat is gekoppeld heeft. Voor primaire IP-configuraties, is een openbaar IP-adres niet vereist op ping met Internet.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Open een terminalvenster.
2. Controleer of u de hoofdgebruiker bent. Voer de volgende opdracht in als u niet de hoofdgebruiker bent:

    ```bash
    sudo -i
    ```

3. Werk het configuratiebestand van de netwerkinterface bij (uitgaande van 'eth0').

    * Houd het bestaande regelitem voor dhcp. Het primaire IP-adres blijft geconfigureerd als voorheen.
    * Voeg een configuratie toe voor een extra statisch IP-adres met de volgende opdrachten:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    U moet een .CFG-bestand zien.
4. Open het bestand. U moet de volgende regels aan het einde van het bestand zien:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Voeg de volgende regels toe na de regels die zijn opgenomen in dit bestand:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Sla het bestand op met de volgende opdracht:

    ```bash
    :wq
    ```

7. Stel de netwerkinterface opnieuw in met de volgende opdracht:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Voer zowel ifdown als ifup op dezelfde regel uit als u een externe verbinding gebruikt.
    >

8. Controleer of het IP-adres is toegevoegd aan de netwerkinterface met de volgende opdracht:

    ```bash
    ip addr list eth0
    ```

    Het IP-adres dat u hebt toegevoegd, moet nu in de lijst staan.

### <a name="linux-redhat-centos-and-others"></a>Linux (Redhat, CentOS en anderen)

1. Open een terminalvenster.
2. Controleer of u de hoofdgebruiker bent. Voer de volgende opdracht in als u niet de hoofdgebruiker bent:

    ```bash
    sudo -i
    ```

3. Voer uw wachtwoord in en volg de instructies. Wanneer u de hoofdgebruiker bent, navigeert u met de volgende opdracht naar de map met netwerkscripts:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Geef een lijst weer met de gerelateerde ifcfg-bestanden met de volgende opdracht:

    ```bash
    ls ifcfg-*
    ```

    *ifcfg eth0* moet als een van de bestanden worden weergegeven.

5. Als u een IP-adres wilt toevoegen, maakt u er een configuratiebestand voor zoals hieronder wordt weergegeven. Houd er rekening mee dat er voor elke IP-configuratie één bestand moet worden gemaakt.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Open het bestand *ifcfg-eth0:0* met de volgende opdracht:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Voeg inhoud toe aan het bestand, in dit geval *eth0:0*, met de volgende opdracht. Zorg ervoor dat u de gegevens bijwerkt op basis van uw IP-adres.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Sla het bestand op met de volgende opdracht:

    ```bash
    :wq
    ```

9. Start de netwerkservices opnieuw op en controleer of de wijzigingen zijn toegepast door de volgende opdrachten uit te voeren:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Het IP-adres dat u hebt toegevoegd, *eth0:0*, moet nu in de lijst staan die wordt opgehaald.

### <a name="validation-linux"></a>Validatie (Linux)

Als u wilt controleren of u via uw secundaire IP-configuratie verbinding kunt maken met internet via de openbare IP die eraan is gekoppeld, gebruikt u de volgende opdracht:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Voor secundaire IP-configuraties, kunt u alleen een ping naar het Internet als de configuratie een openbaar IP-adres dat is gekoppeld heeft. Voor primaire IP-configuraties, is een openbaar IP-adres niet vereist op ping met Internet.

Voor virtuele Linux-machines moet u mogelijk geschikte routes toevoegen wanneer u probeert uitgaande verbindingen te valideren vanaf een secundaire NIC. Er zijn meerdere manieren om dit te doen. Zie de relevante documentatie voor uw Linux-distributie. Hieronder staat één van de mogelijke manieren:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Vervang de volgende zaken:
    - **10.0.0.5** door het privé-IP-adres waaraan een openbaar IP-adres is gekoppeld
    - **10.0.0.1** door uw standaardgateway
    - **eth2** door de naam van uw secundaire NIC
