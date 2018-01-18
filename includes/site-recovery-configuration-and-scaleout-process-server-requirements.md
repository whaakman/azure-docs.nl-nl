| **Onderdeel** | **Vereiste** |
| --- |---|
| CPU-kernen| 8 |
| RAM | 12 GB|
| Aantal schijven | 3, met inbegrip van de OS-schijf, proces server cache schijf en bewaarstation voor failback |
| Vrije schijfruimte (cache van de processerver) | 600 GB
| Vrije schijfruimte (bewaarschijf) | 600 GB|
| Besturingssysteem  | Windows Server 2012 R2 <br> Windows Server 2016 |
| Landinstelling van het besturingssysteem | Engels (en-us)|
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server-functies | Schakel niet in deze rollen: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| Groepsbeleid| Geen groepsbeleid voor deze inschakelen: <br> -Voorkomen dat toegang tot de opdrachtprompt <br> -Toegang tot het register te bewerken van hulpprogramma's voorkomen <br> -Logica vertrouwen voor bestandsbijlagen <br> -Uitvoering van Script inschakelen <br> [Meer informatie](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| IIS | -Er zijn geen bestaande standaardwebsite <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) instelling  <br> -Niet bestaande reeds websitetoepassing geluisterd op poort 443<br>|
| NIC-type | VMXNET3 (indien geïmplementeerd als een VMware-VM) |
| Type IP-adres | Statisch |
| Toegang tot het internet | De server heeft toegang tot deze URL's: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (niet vereist voor uitbreidbare processervers) <br> - time.nist.gov <br> - time.windows.com |
| Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport)|
