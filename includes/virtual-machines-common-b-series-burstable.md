
De B-serie VM-familie kunt u kiezen welke VM-grootte biedt u de prestaties van de vereiste base-niveau voor uw workload met de mogelijkheid om het CPU-prestaties tot wel 100% van een Intel® Broadwell E5-2673 v4 burst 2.3 GHz of een Intel® Haswell 2,4 GHz E5-2673 v3-processor vCPU.

De B-serie VM's zijn ideaal voor workloads die niet moet de volledige prestaties van de CPU continu, zoals webservers, kleine databases en ontwikkeling en testen van omgevingen. Deze werkbelastingen hebben doorgaans burstable prestatie-eisen. De B-reeks biedt u de mogelijkheid om aan te schaffen van een VM-grootte van de basislijn en de VM-instantie wordt opgebouwd tegoed bij het gebruik van minder dan de basislijn. Wanneer de virtuele machine zijn tegoed verzameld, kan de virtuele machine burst boven de basislijn met maximaal 100% van de vCPU wanneer uw toepassing hoger CPU-prestaties vereist.

De B-reeks wordt geleverd in de volgende zes VM-grootten:

| Grootte          | de vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Basis-CPU-prestaties van virtuele machine | Maximum aantal CPU-prestaties van virtuele machine | Tegoed gestort / uur | Maximale gestort tegoed |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | 10%                            | 100%                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | 20%                            | 100%                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40%                            | 200%                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60%                            | 200%                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90%                            | 400%                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135%                           | 800%                      | 81                    | 1944               |




## <a name="q--a"></a>Vragen en antwoorden 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>V: hoe krijgt u 135% basislijnprestaties van een virtuele machine?
**Een**: de 135% wordt gedeeld tussen de 8 vCPU van waaruit de VM-grootte. Als uw toepassing maakt gebruik van 4 van de 8 kernen werkt batchverwerking en elk van deze 4 vCPU worden uitgevoerd op 30% gebruik zou de totale hoeveelheid VM CPU-prestaties 120% gelijk.  Dit betekent dat uw virtuele machine op basis van de delta 15% van de basislijnprestaties van uw tegoed-tijd zou bouwen.  Maar het betekent ook dat wanneer er tegoed beschikbaar waarmee dezelfde virtuele machine kunt 100% van alle 8 vCPU de geeft die VM een maximale CPU-prestaties van 800%.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>V: hoe kan ik mijn saldo en het verbruik van controleren
**Een**: We 2 nieuwe metrische gegevens introductie in de komende weken de **tegoed** metrische gegevens kunt u om weer te geven hoeveel tegoed uw virtuele machine heeft gestort en de **ConsumedCredit** metriek wordt weergegeven hoeveel CPU-tegoed uw virtuele machine van de bank is verbruikt.    U mag deze metrische gegevens in het deelvenster metrische gegevens weergeven in de portal of programmatisch via de Azure-Monitor API's.

Zie voor meer informatie over toegang krijgen tot de metrische gegevens voor Azure [overzicht van metrische gegevens in Microsoft Azure](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>V: hoe kan ik tegoed samengevoegd?
**Een**: de tarieven opeenstapeling en het verbruik van de virtuele machine zodanig zijn ingesteld dat een virtuele machine uitgevoerd op precies het niveau van de algemene prestatiegegevens heeft een net opeenstapeling, noch verbruik van tegoed sturen.  Een virtuele machine heeft een verhoging van het net in een tegoed wanneer deze wordt uitgevoerd onder het niveau van de algemene prestatiegegevens en heeft een net afname in tegoed wanneer de virtuele machine wordt gebruikt door de CPU-capaciteit van meer dan de base prestatieniveau.

**Voorbeeld**: implementatie van een virtuele machine met behulp van de grootte van de B1ms voor mijn databasetoepassing klein tijd en aanwezigheid. Deze grootte kan mijn toepassing maximaal 20% van een vCPU gebruiken als mijn basislijn.2 tegoed per minuut die kan worden gebruikt of bank. 

Mijn toepassing is bezet aan het begin en einde van mijn werkdag werknemers tussen 7:00-9:00 uur en 4:00-18:00 uur. Tijdens de overige 20 uur van de dag is mijn toepassing doorgaans op inactief, gebruikmakend van 10% van de vCPU. Voor de niet-piekuren ik verdienen 0,2 tegoed per minuut, maar alleen gebruiken 0.l tegoed per minuut, dus mijn VM wordt bank.1 x 60 = 6 tegoed per uur.  Voor de 20 uur die ik buiten piektijden, zal ik 120 tegoed bank.  

Tijdens de piekuren mijn toepassing het gemiddelde van het gebruik van 60% vCPU, ik nog steeds 0,2 tegoed per minuut behalen, maar ik 0,6 tegoed per minuut in beslag nemen, voor een net kosten van.4 tegoed een minuut of 24 x 60 =.4 krediet per uur. Ik heb 4 uur per dag van piekgebruik, zodat het kost 4 x 24 = 96 krediet voor mijn piekgebruik.

Als ik de 120 tegoed die ik behaald buiten piektijden nemen en het 96 tegoed die ik voor mijn piektijden gebruikt afgetrokken, bank ik een extra 24 tegoed per dag die ik voor andere lichtflitsen activiteit kunt gebruiken.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>V: de B-serie ondersteunt gegevensschijven Premium-opslag?
**Een**: Ja, alle B-serie grootten gegevensschijven Premium-opslag ondersteund.   
    


    

    
