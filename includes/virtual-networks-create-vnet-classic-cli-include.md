## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Het maken van een klassiek VNet met Azure CLI
U kunt Azure CLI gebruiken voor het beheer van uw Azure-resources via de opdrachtprompt op elke computer met Windows, Linux of OS X. Volg de onderstaande stappen om een VNet te maken met behulp van de Azure CLI.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../articles/cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de opdracht **azure network vnet create** uit om een VNet en een subnet te maken, zoals hieronder weergegeven. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Verwachte uitvoer:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Naam van de VNet die moet worden gemaakt. In ons scenario *TestVNet*
   * **-e (of--adresruimte)**. VNet-adresruimte. In ons scenario *192.168.0.0*
   * **-i (of de cidr-)**. Het netwerkmasker in CIDR-notatie. In ons scenario *16*.
   * **-n (of--subnet naam**). Naam van het eerste subnet. In ons scenario *FrontEnd*.
   * **-p (of--begin-ip-subnet)**. IP-adres voor het subnet of subnetadresruimte wordt gestart. In ons scenario *192.168.1.0*.
   * **-r (of--subnet cidr)**. Het netwerkmasker in CIDR-indeling voor het subnet. In ons scenario *24*.
   * **-l (of --locatie)**. De Azure-regio waar de VNet wordt gemaakt. In ons scenario *VS-midden*.
3. Voer de opdracht **azure network vnet subnet create** om een subnet te maken, zoals hieronder weergegeven. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Dit is de verwachte uitvoer voor de bovenstaande opdracht:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (of--vnet naam**. Naam van de VNet waar het subnet wordt gemaakt. In ons scenario *TestVNet*.
   * **-n (of --name)**. Naam van het nieuwe subnet. In ons scenario *back-end*.
   * **-a (of --adresvoorvoegsel)**. Subnet CIDR-blok. Vier in ons scenario *192.168.2.0/24*.
4. Voer de opdracht **azure network vnet show** uit om de eigenschappen van de nieuwe VNet weer te geven, zoals hieronder weergegeven.
   
            azure network vnet show
   
    Dit is de verwachte uitvoer voor de bovenstaande opdracht:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

