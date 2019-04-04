---
title: Mainframe-compute verplaatsen naar Azure Virtual Machines
description: Azure compute-resources vergelijken fotoprinters mainframe-capaciteit, zodat u kunt migreren en IBM z14 toepassingen moderniseren.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 8aea4a74ba84855f011dada70ea75ec0d5fb64fe
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896378"
---
# <a name="move-mainframe-compute-to-azure"></a>Mainframe-compute verplaatsen naar Azure

Mainframes hebben een uitstekende reputatie hoge betrouwbaarheid en beschikbaarheid en nog steeds de vertrouwde backbone van vele ondernemingen. Ze zijn vaak dachten dat bijna oneindige schaalbaarheid en de rekenkracht ook. Bij sommige bedrijven hebben echter de mogelijkheid van de grootste beschikbare mainframes ontgroeid. Als dit als u klinkt, biedt Azure besparingen voor flexibiliteit, bereik en infrastructuur.

Mainframe-workloads uitvoeren op Microsoft Azure, moet u weten hoe van uw mainframe mogelijkheden in vergelijking tot het Azure compute. Op basis van een IBM z14 mainframe (op dit moment de meest recente model), in dit artikel wordt uitgelegd hoe u de resultaten van de vergelijkbare ophalen in Azure.

Als u wilt beginnen, kunt u de omgevingen naast elkaar. De volgende afbeelding wordt een mainframe-omgeving voor het uitvoeren van toepassingen naar een Azure-hosting omgeving vergeleken.

![Azure-services en emulatie omgevingen bieden vergelijkbare ondersteunen en stroomlijnt de migratie](media/mainframe-compute-azure.png)

De kracht van mainframes wordt vaak gebruikt voor online transactieverwerking (OLTP)-systemen die de verwerking van miljoenen updates voor duizenden gebruikers. Deze toepassingen gebruiken vaak software voor transactieverwerking en verwerking van het scherm de invoer. Ze kunnen een klant informatie besturingselement System (CICS), informatie Management System (IMS) of transactie-Interface-pakket (TIP) gebruiken.

Zoals in de afbeelding wordt weergegeven, kan een TPM-emulator op Azure CICS en IMS werkbelastingen verwerken. Een batch-systeem emulator op Azure voert de rol van de taak Control Language (JCL). Mainframe-gegevens is gemigreerd naar Azure-databases, zoals Azure SQL Database. Azure-services of andere software die wordt gehost in Azure Virtual Machines kan worden gebruikt voor systeembeheer.

## <a name="mainframe-compute-at-a-glance"></a>Mainframe-compute in een oogopslag

In de mainframe z14 processors zijn gerangschikt in maximaal vier *activiteitenladen*. Een *lade* is gewoon een cluster van processors en chipsets. Elke sectie van de zes active centrale processor (CP)-chips kunt hebben en elke CP heeft 10 system-controller (SC) chips. In de terminologie van Intel x86 zijn er zes sockets per lade, 10 cores per socket en vier laden. Deze architectuur biedt de ruwe equivalent van 24 sockets en 240 kernen, maximum, voor een z14.

Het snelle z14 CP heeft een kloksnelheid 5.2 GHz. Normaal gesproken wordt een z14 geleverd met de CPs in het vak. Ze zijn geactiveerd behoefte. Voor ten minste vier uur van de tijd per maand ondanks het werkelijke gebruik wordt vaak een klant in rekening gebracht.

Een mainframe-processor kan worden geconfigureerd als een van de volgende typen:

- Algemeen doel (GP)-processor
- Z geïntegreerde informatie systeemprocessor (zIIP)
- Geïntegreerde functie voor Linux (IFL)-processor
- Assist systeemprocessor (SAP)
- Geïntegreerde koppeling faciliteit (Firewall)-processor

## <a name="scaling-mainframe-compute-up-and-out"></a>Vergroten/verkleinen mainframe-compute en uitbreiden

IBM-mainframes bieden de mogelijkheid om te schalen tot 240 kernen (de huidige z14 grootte voor een enkel systeem). Bovendien voor IBM-mainframes schalen via een functie met de naam van de "koppeling faciliteit" (CF). De CF kan meerdere mainframesystemen tegelijkertijd dezelfde gegevens raadplegen. Met behulp van de CF, mainframe parallelle Sysplex technologie groepen mainframe processors in clusters. Wanneer u deze handleiding is geschreven, betekent dit dat de functie parallelle Sysplex 32 groeperingen van 64 processors ondersteund. Maximaal 2048 processors kan worden gegroepeerd op deze manier uit rekencapaciteit te schalen.

Een CF kunt de rekenclusters gegevens kunnen delen met directe toegang. Het wordt gebruikt voor het vergrendelen van informatie, cache-informatie en de lijst met resources van gedeelde gegevens. Een parallelle Sysplex met behulp van een of meer CFs kunnen worden beschouwd als een 'gedeeld alles' scale-out rekencluster. Zie voor meer informatie over deze functies [parallelle Sysplex op IBM Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources) op de website van IBM.

Toepassingen kunnen deze functies gebruiken om prestaties uitschalen en de hoge beschikbaarheid. Voor informatie over hoe CICS parallelle Sysplex met CF gebruiken kunt, downloaden de [IBM CICS en de faciliteit koppeling: Een stapje verder](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf) redbook.

## <a name="azure-compute-at-a-glance"></a>Azure-rekenen in een oogopslag

Sommige mensen denken ten onrechte dat op basis van Intel-servers zijn niet zo krachtig zijn als mainframes. De nieuwe core compacte, op basis van Intel-systemen hebben echter als veel rekencapaciteit als mainframes. Deze sectie beschrijft de opties voor Azure infrastructure-as-a-service (IaaS) voor computing en opslag. Azure biedt ook opties voor platform-as-a-service (PaaS), maar in dit artikel richt zich op de IaaS-mogelijkheden die vergelijkbare mainframe-capaciteit bieden.

Virtuele Machines van Azure bieden de rekenkracht in een bereik van grootten en -typen. In Azure, is een virtuele processors (vCPU) ongeveer gelijk is aan een kern van een mainframe.

Het bereik van Azure VM-groottes biedt op dit moment van 1 tot 128 vcpu's. Typen virtuele machines (VM) zijn geoptimaliseerd voor bepaalde workloads. De volgende lijst ziet u bijvoorbeeld de VM-typen (huidige op dit moment) en hoe deze worden aanbevolen gebruikt:

| Grootte     | Type en de beschrijving                                                                 |
|----------|--------------------------------------------------------------------------------------|
| D-serie | Algemeen gebruik met 64 vCPU en tot wel 3,5 GHz kloksnelheid                           |
| E-serie | Geheugen geoptimaliseerd met maximaal 64 vcpu 's                                                 |
| F-serie | Berekenen geoptimaliseerd met maximaal 64 vcpu's en kloksnelheid 3..7 GHz                       |
| H-serie | Geoptimaliseerd voor high performance computing (HPC)-toepassingen                          |
| L-serie | Opslag geoptimaliseerd voor toepassingen met hoge doorvoer ondersteund door databases, zoals NoSQL |
| M-serie | Grootste Computing en geheugen geoptimaliseerde virtuele machines met maximaal 128 vcpu 's                        |

Zie voor meer informatie over beschikbare virtuele machines [Virtual Machine-serie](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

Een mainframe z14 kan tot 240 kernen hebben. Z14 mainframes Gebruik echter bijna nooit alle kernen voor een enkele toepassing of werkbelasting. In plaats daarvan een mainframe-werkbelastingen in logische partities (LPARs) scheidt, en de LPARs hebben classificaties: MIPS (miljoenen van instructies Per seconde) of MSU (miljoen Service-eenheid). Bij het bepalen van de vergelijkbare VM-grootte die nodig zijn voor een mainframe-workload wordt uitgevoerd op Azure, beoordeling van de multi-factor Authentication in de MIPS (of MSU).

Hier volgen algemene schattingen:

-   150 MIPS per vCPU

-   1000 MIPS per processor

Om te bepalen het juiste VM-grootte voor een bepaalde werkbelasting in een LPAR, moet u eerst de virtuele machine voor de werkbelasting optimaliseren. Vervolgens kunt u bepalen het aantal vcpu's die nodig zijn. Een voorzichtige schatting is 150 MIPS per vCPU. Op basis van deze schatting, bijvoorbeeld kan een VM uit de F-serie met 16 vcpu's eenvoudig ondersteunen een IBM Db2-werkbelasting die afkomstig zijn van een LPAR met 2.400 MIPS.

## <a name="azure-compute-scale-up"></a>Azure-rekenen omhoog

De M-serie VM's kunnen maximaal 128 vcpu's (op het moment dat dit artikel is geschreven) worden geschaald. Met behulp van de conservatieve schatting van de 150 MIPS per vCPU, de M-serie VM is gelijk aan ongeveer 19.000 MIPS. De algemene regel voor het schatten van MIPS voor een mainframe is 1000 MIPS per processor. Een mainframe z14 kan tot 24 processor en ongeveer 24.000 MIPS bieden voor een enkele mainframe-systeem.

De grootste één z14 mainframe heeft ongeveer 5000 MIPS van meer dan de grootste virtuele machine die beschikbaar zijn in Azure. Het is nog belangrijk om te vergelijken hoe werkbelastingen zijn geïmplementeerd. Als een mainframe-systeem zowel een toepassing als een relationele database is, deze doorgaans op de dezelfde fysieke mainframe zijn geïmplementeerd, elk in een eigen LPAR. Dezelfde oplossing op Azure wordt vaak geïmplementeerd met behulp van één virtuele machine voor de toepassing en een virtuele machine afzonderlijk, voldoende grootte voor de database.

Bijvoorbeeld, als een M64 vCPU-systeem biedt ondersteuning voor de toepassing en een vCPU M96 wordt gebruikt voor de database, ongeveer 150 vcpu's nodig zijn, of ongeveer 24.000 MIPS zoals in de volgende afbeelding wordt weergegeven.

![Vergelijking van de werkbelasting van implementaties van 24.000 MIPS](media/mainframe-compute-mips.png)

De aanpak is om te migreren van LPARs met afzonderlijke virtuele machines. Vervolgens Azure eenvoudig omhoog wordt geschaald uitgaande de grootte die nodig zijn voor de meeste toepassingen die zijn geïmplementeerd op een enkele mainframe-systeem.

## <a name="azure-compute-scale-out"></a>Azure-rekenen scale-out

Een van de voordelen van een oplossing op basis van Azure is de mogelijkheid om uit te schalen. Vergroten/verkleinen is bijna onbegrensd compute-capaciteit beschikbaar zijn voor een toepassing. Azure ondersteunt meerdere methoden voor rekenkracht uitschalen:

- **De taakverdeling in een cluster.** In dit scenario wordt een toepassing kunt gebruiken een [netwerktaakverdeler](/azure/load-balancer/load-balancer-overview) of resourcemanager te verdelen van de werkbelasting tussen meerdere virtuele machines in een cluster. Als er meer capaciteit nodig is rekenkracht, worden extra virtuele machines toegevoegd aan het cluster.

- **Virtuele-machineschaalsets.** In dit scenario ' burst ' een toepassing kan worden geschaald naar extra [rekenresources](/azure/virtual-machine-scale-sets/overview) op basis van gebruik van de virtuele machine. Wanneer de vraag valt, kan het aantal virtuele machines in een schaalset ook uitvallen, ervoor te zorgen dat efficiënt gebruik van compute-kracht.

- **PaaS schalen.** Azure PaaS-aanbiedingen schaal rekenresources. Bijvoorbeeld, [Azure Service Fabric](/azure/service-fabric/service-fabric-overview) compute-resources om te voldoen aan verhogingen in het volume aan aanvragen kan worden toegewezen.

- **Kubernetes-clusters.** Toepassingen op Azure kunnen gebruikmaken van [Kubernetes-clusters](/azure/aks/concepts-clusters-workloads) voor compute-services voor de opgegeven resources. Azure Kubernetes Service (AKS) is een beheerde service die wordt georganiseerd Kubernetes-knooppunten, -adresgroepen en -clusters op Azure.

Als u de juiste methode voor het schalen van rekenresources, is het belangrijk om te begrijpen hoe Azure en mainframes verschillen. De sleutel is hoe, of als: gegevens worden gedeeld door de compute-resources. In Azure, gegevens (standaard) niet normaal gesproken wordt gedeeld door meerdere virtuele machines. Als het delen van gegevens is vereist voor meerdere virtuele machines in een scale-out Reken-cluster, de gedeelde gegevens moet zich bevinden in een resource die ondersteuning biedt voor deze functionaliteit. In Azure omvat het delen van gegevens opslag zoals in de volgende sectie wordt beschreven.

## <a name="azure-compute-optimization"></a>Azure compute-optimalisatie

U kunt elke laag van de verwerking in een Azure-architectuur optimaliseren. Gebruik de meest geschikte type van de VM's en functies voor elke omgeving. De volgende afbeelding toont een mogelijke patroon voor het implementeren van virtuele machines in Azure ter ondersteuning van een CICS-toepassing die gebruikmaakt van Db2. Op de primaire site, de productie, worden VM's vóór productie en testen geïmplementeerd met hoge beschikbaarheid. De secundaire site is voor back-up en herstel na noodgevallen.

Elke laag kan ook juiste disaster recovery services bieden. Bijvoorbeeld, productie- en database-VM's mogelijk een ' hot ' of ' warm ' is hersteld, terwijl een koude herstel de ontwikkeling en tests virtuele machines wordt ondersteund.

![Implementatie met maximale beschikbaarheid die ondersteuning biedt voor herstel na noodgevallen](media/mainframe-compute-dr.png)

## <a name="next-steps"></a>Volgende stappen

- [Mainframe-migratie](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Mainframe opnieuw hosten op Azure Virtual Machines](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Mainframe-opslag verplaatsen naar Azure](mainframe-storage-Azure.md)

### <a name="ibm-resources"></a>IBM-resources

- [Parallelle Sysplex op IBM-Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS en de faciliteit koppeling: Een stapje verder](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Het maken van de vereiste gebruikers voor een Db2-pureScale-functie installeren](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - exemplaar-opdracht maken](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale geclusterde Database-oplossing](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Government

- [Microsoft Azure Government-cloud voor mainframe-toepassingen](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft en FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Meer resources voor migratie

- [Platform-modernisering Alliance: IBM Db2 on Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Azure Virtual Datacenter Lift- and -Shift-handleiding](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
