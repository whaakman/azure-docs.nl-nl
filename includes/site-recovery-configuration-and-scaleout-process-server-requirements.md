| **Hardware** | |
| --- |---|
| Aantal CPU-kernen| 8 |
| RAM| 12 GB|
| Aantal schijven | 3 <br><br> - OS-schijf<br> - Cacheschijf van de processerver<br> - Bewaarstation (voor failback)|
| Vrije schijfruimte (cache van de processerver) | 600 GB
| Vrije schijfruimte (bewaarschijf) | 600 GB|
| **Software** | |
| Besturingssysteem | Windows Server 2012 R2 |
| Landinstelling van het besturingssysteem | Engels (en-us)|
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Windows Server-functies | Schakel de volgende functies niet in: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| **Netwerk** | |
| Type netwerkinterfacekaart | VMXNET3 |
| Type IP-adres | Statisch |
| Toegang tot het internet | De server moet rechtstreeks of via een proxyserver toegang hebben tot de volgende URL's: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (niet vereist voor uitbreidbare processervers) <br> - time.nist.gov <br> - time.windows.com |
| Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport)|


<!--HONumber=Feb17_HO3-->


