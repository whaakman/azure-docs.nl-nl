Opslag geoptimaliseerd VM-grootten bieden hoge doorvoer en i/o en zijn ideaal voor Big Data, SQL en NoSQL-databases. In dit artikel bevat informatie over het aantal Vcpu, gegevensschijven en NIC's, evenals opslag doorvoer en bandbreedte voor elke grootte in deze groepering. 

De Ls-serie biedt maximaal 32 vCPU's en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). De Ls-serie heeft dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per vCPU.  

## <a name="ls-series"></a>Ls-serie

ACU: 180-240
 
| Grootte          | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximum aantal tijdelijke opslagdoorvoer: IOP's / MBps | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's / netwerkbandbreedte (Mbps) verwacht | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 10.000 / 250        | 2 / 4,000  | 
| Standard_L8s   | 8    | 64   | 1388 | 32   | 40,000 / 400   | 20.000 / 500       | 4 / 8,000  | 
| Standard_L16s  | 16   | 128  | 2807 | 64   | 80,000 / 800   | 40.000 / 1,000       | 8 / 6,000 - 16,000 &#8224; | 
| Standard_L32s* | 32   | 256  | 5630 | 64   | 160,000 / 1,600   | 80.000 / 2000     | 8 / 20,000 | 
 

De maximale schijfdoorvoer die mogelijk is met VM's uit de Ls-serie, kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie. 

*Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.

