| **Hardware** | |
| --- |---|
| Aantal CPU-kernen| 8 |
| RAM| 12 GB|
| Aantal schijven | 3 <br><br> - OS-schijf<br> - Cacheschijf van de processerver<br> - Bewaarstation (voor failback)|
| Vrije schijfruimte (cache van de processerver) | 600 GB
| Vrije schijfruimte (bewaarschijf) | 600 GB|
| **Software** | |
| Besturingssysteem | Windows Server 2012 R2 <br> Windows Server 2016 |
| Landinstelling van het besturingssysteem | Engels (en-us)|
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server-functies | Schakel de volgende functies niet in: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| Groepsbeleid| De volgende groepsbeleidsregels mag niet worden ingeschakeld op de server <br> -Voorkomen dat toegang tot de opdrachtprompt <br> -Toegang tot het register te bewerken van hulpprogramma's voorkomen <br> -Logica vertrouwen voor bestandsbijlagen <br> -Uitvoering van Script inschakelen <br> **Opmerking:** vindt meer informatie over deze groepsbeleidsregels [hier](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| Configuratie van Internet Information Service(IIS) | -Er zijn geen bestaande standaardwebsite <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) instelling  <br> -Er zijn geen bestaande websit/toepassing moet worden geluisterd op poort 443<br>|
| **Netwerk** | |
| Type netwerkinterfacekaart | VMXNET3 |
| Type IP-adres | Statisch |
| Toegang tot het internet | De server moet rechtstreeks of via een proxyserver toegang hebben tot de volgende URL's: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (niet vereist voor uitbreidbare processervers) <br> - time.nist.gov <br> - time.windows.com |
| Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport)|
