
De Ls-serie is geoptimaliseerd voor workloads die tijdelijke opslag met lage latentie vereisen, zoals NoSQL-databases (bijvoorbeeld Cassandra, MongoDB, Cloudera en Redis). De Ls-serie biedt maximaal 32 vCPU's en maakt gebruik van een [Intel Xeon®-processor uit de E5 v3-familie](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). De Ls-serie heeft dezelfde CPU-prestaties als de G/GS-serie en wordt geleverd met 8 GiB geheugen per vCPU.  

## <a name="ls-series"></a>Ls-serie

ACU: 180-240
 
| Grootte          | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Max. aantal gegevensschijven | Maximale doorvoer voor schijven met caching en tijdelijke opslag: IOPS / MBps (cachegrootte in GiB) | Max. doorvoer voor schijf zonder caching: IOPS/MBps | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | N.V.T./N.V.T. (0)          | 5000 / 125                               | 2 / 4000       | 
| Standard_L8s  | 8    | 64   | 1388 | 16             | N.V.T./N.V.T. (0)          | 10.000 / 250                              | 4 / 8000  | 
| Standard_L16s | 16   | 128  | 2807 | 32             | N.V.T./N.V.T. (0)          | 20.000 / 500                              | 8 / 6000 - 16000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5630 | 64             | N.V.T./N.V.T. (0)          | 40.000 / 1,000                            | 8 / 20000 | 
 

De maximale schijfdoorvoer die mogelijk is met VM's uit de Ls-serie, kan worden beperkt door het aantal, de grootte en de striping van de gekoppelde schijven. Zie [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: opslag met hoge prestaties voor Azure VM-workloads) voor meer informatie. 

*Exemplaar is geïsoleerd voor hardware, speciaal voor één klant.

