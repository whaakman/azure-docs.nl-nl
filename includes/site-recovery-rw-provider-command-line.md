UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP-adres voor gegevensoverdracht] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameters:

* /ServerMode: Verplicht. Hiermee wordt aangegeven of zowel de configuratieserver als de processerver moet worden geïnstalleerd, of alleen de processerver. Invoerwaarden: CS, PS.
* InstallLocation: Verplicht. De map waarin de onderdelen worden geïnstalleerd.
* /MySQLCredsFilePath. Verplicht. Het pad naar het bestand waarin de referenties voor de MySQL-server worden opgeslagen. Het bestand moet de volgende indeling hebben:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Verplicht. De locatie van het bestand met kluisreferenties
* /EnvType. Verplicht. Het type installatie. Waarden: VMware, NonVMware
* /PSIP en /CSIP. Verplicht. Het IP-adres van de processerver en de configuratieserver.
* /PassphraseFilePath. Verplicht. De locatie van het bestand met de wachtwoordzin.
* /BypassProxy. Optioneel. Hiermee wordt aangegeven dat de configuratieserver zonder proxy verbinding moet maken met Azure.
* /ProxySettingsFilePath. Optioneel. Proxy-instellingen (de standaardproxy vereist verificatie of een aangepaste proxy). Het bestand moet de volgende indeling hebben:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "<IP-adres>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. Optioneel. Het poortnummer voor de replicatiegegevens.
* SkipSpaceCheck. Optioneel. Hiermee wordt aangegeven dat de controle op vrije ruimte in de cache moet worden overgeslagen.
* AcceptThirdpartyEULA. Verplicht. Hiermee wordt de gebruiksrechtovereenkomst van derden geaccepteerd.
* ShowThirdpartyEULA. Verplicht. Hiermee wordt de gebruiksrechtovereenkomst van derden weergegeven. Indien opgegeven als invoer, worden alle andere parameters genegeerd.


<!--HONumber=Feb17_HO2-->


