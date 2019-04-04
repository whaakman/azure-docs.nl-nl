---
title: Mainframe opnieuw hosten op Azure virtual machines
description: Rehost uw mainframe-workloads zoals IBM Z-systemen met behulp van virtuele machines (VM's) op Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 8b7c2a088dc917c319acf6cad251ad53367a14b6
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895034"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Mainframe opnieuw hosten op Azure virtual machines

Workloads voor migratie van mainframe-omgevingen aan de cloud kunt u uw infrastructuur moderniseren en vaak Bespaar op kosten. Alleen secundaire code te wijzigen, zoals het bijwerken van de namen van databases, kunnen u veel werkbelastingen overbrengen naar Azure.

In het algemeen geldt, de term *mainframe* betekent een grote computersysteem. De meeste die momenteel in gebruik zijn specifiek, IBM System Z servers of IBM plug-compatible systemen waarop MVS, DOS, VSE, OS/390 of z/OS wordt uitgevoerd.

Een Azure virtuele machine (VM) wordt gebruikt om te isoleren en beheren van de resources voor een bepaalde toepassing op één exemplaar. Logische partities (LPARS) mainframes zoals IBM z/OS gebruiken voor dit doel. Een mainframe bijvoorbeeld één LPAR voor een regio CICS met gekoppelde COBOL-programma's en een afzonderlijke LPAR gebruiken voor IBM Db2-database. Een typische [n-tier-toepassing op Azure](/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) Azure-VM's implementeert in een virtueel netwerk dat kan worden gesegmenteerd in subnetten voor elke laag.

Azure-VM's kunnen uitvoeren, mainframe-emulatie omgevingen en compilers die ondersteuning bieden voor lift-and-shift-scenario's. Ontwikkelen en testen zijn vaak een van de eerste werkbelastingen die het migreren van een mainframe naar een Azure dev/test-omgeving. Algemene serveronderdelen waarmee kan worden geëmuleerd zijn onder andere online transactieproces (OLTP), batch en systemen voor opname van gegevens zoals in de volgende afbeelding wordt weergegeven.

![Emulatie omgevingen op Azure kunnen u z/OS gebaseerde systemen uitvoeren.](media/01-overview.png)

Sommige mainframe-werkbelastingen kunnen worden gemigreerd naar Azure met relatief veel gemak, terwijl anderen kunnen worden rehosted op Azure met behulp van een partneroplossing. Voor gedetailleerde instructies over het kiezen van een partneroplossing de [Azure Mainframe-migratie center](https://azure.microsoft.com/migration/mainframe/) kan helpen.

## <a name="mainframe-migration"></a>Mainframe-migratie

Opnieuw hosten, opnieuw maken, vervangen of buiten gebruik stellen? IaaS of PaaS? Om te bepalen het juiste migratiestrategie voor uw mainframe-toepassingen, Zie de [Mainframe-migratie](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview) handleiding in het Azure Architecture Center.

## <a name="micro-focus-rehosting-platform"></a>Micro Focus opnieuw hosten platform

Micro Focus Enterprise-Server is een van de grootste mainframe opnieuw hosten van platforms beschikbaar. U kunt uw z/OS-workloads uitvoeren op een goedkopere x86 platform in Azure.

Aan de slag gaan:

- [Enterprise-Server en Enterprise-ontwikkelaar op Azure installeren](./microfocus/set-up-micro-focus-azure.md)
- [CICS BankDemo instellen voor Enterprise-ontwikkelaar op Azure](./microfocus/demo.md)
- [Enterprise-Server in een Docker-Container uitvoeren op Azure](./microfocus/run-enterprise-server-container.md)


## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame on Azure

TmaxSoft OpenFrame is een populaire mainframe rehosting oplossing gebruikt in lift-and-shift-scenario's. Een omgeving OpenFrame op Azure is geschikt voor ontwikkeling, demo's, tests of productie-workloads.

Aan de slag gaan:

- [Aan de slag met TmaxSoft OpenFrame](./tmaxsoft/get-started.md)
- [eBook downloaden](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-azure/)

## <a name="ibm-zdt-120"></a>IBM zD&T 12.0

IBM Z Development- en testomgeving (IBM zD & T) stelt u een niet-productieomgeving op Azure die u voor ontwikkeling gebruiken kunt, testen en demo's van z/OS gebaseerde toepassingen.

De emulatieomgeving op Azure kunt verschillende soorten Z exemplaren via toepassing ontwikkelaars beheerd distributies (ADCDs) hosten. U kunt zD & T persoonlijke editie, zD & T parallelle Sysplex, en zD & T Enterprise Edition uitvoeren op Azure en Azure Stack.

Aan de slag gaan:

- [Instellen van IBM zD & T 12.0 op Azure](./ibm/install-ibm-z-environment.md)
- [ADCD instellen op zD & T](./ibm/demo.md)

## <a name="ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale op Azure

De IBM DB2 pureScale-omgeving biedt een databasecluster voor Azure. Het is niet identiek zijn aan de oorspronkelijke omgeving, maar het biedt een vergelijkbare beschikbaarheid en schaal als IBM DB2 voor z/OS in een parallelle Sysplex setup uitgevoerd.

Als u wilt beginnen, Zie [IBM DB2 pureScale op Azure](/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Overwegingen

Wanneer u mainframe-workloads migreren naar Azure-infrastructuur als een dienst (IaaS), kunt u kiezen uit verschillende soorten on-demand, schaalbare IT-bronnen, met inbegrip van Azure-VM's. Azure biedt een scala aan [Linux](/azure/virtual-machines/linux/overview) en [Windows](/azure/virtual-machines/windows/overview) VM's.

### <a name="compute"></a>Compute

Azure compute-kracht vergelijkt fotoprinters aan van een mainframe-capaciteit. Als u erover een mainframe-workload naar Azure verplaatst denkt, vergelijken met de mainframe-metric van één miljoen instructies per seconde (MIPS) naar virtuele CPU's. 

Meer informatie over het [mainframe compute verplaatsen naar Azure](./concepts/mainframe-compute-azure.md).

### <a name="high-availability-and-failover"></a>Hoge beschikbaarheid en failover

Azure biedt op basis van toezegging service level agreements (Sla's). Meerdere nines beschikbaarheid is de standaardinstelling en sla's kunnen worden geoptimaliseerd met lokaal of op basis van een geo-replicatie van services. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

Met Azure IaaS, zoals een virtuele machine, bieden specifieke functies failover-ondersteuning, bijvoorbeeld voor failover clustering exemplaren en [beschikbaarheidssets](/azure/virtual-machines/windows/regions-and-availability#availability-sets). Wanneer u Azure-platform als een service (PaaS)-resources, verwerkt het platform failover automatisch. Voorbeelden zijn onder meer [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) en [Azure Cosmos DB](/azure/cosmos-db/introduction).

### <a name="scalability"></a>Schaalbaarheid

Mainframes doorgaans opschalen, terwijl cloud omgevingen scale-out. Azure biedt een scala aan [Linux](/azure/virtual-machines/linux/sizes) en [Windows](/azure/virtual-machines/windows/sizes) grootten om te voldoen aan uw behoeften. De cloud kan ook worden geschaald omhoog of omlaag overeenkomst gebruiker exacte specificaties. Rekenkracht, opslag, en services [schaal](/azure/architecture/best-practices/auto-scaling) on-demand onder een factureringsmodel op basis van gebruik.

### <a name="storage"></a>Opslag

In de cloud, hebt u een scala aan opties voor flexibele, schaalbare opslag en u betaalt alleen voor wat u nodig hebt. [Azure Storage](/azure/storage/common/storage-introduction) biedt een hoge mate schaalbare opslag voor gegevensobjecten, een bestandssysteemservice voor de cloud, een berichtenarchief voor betrouwbare en een NoSQL-archief. Beheerde en onbeheerde schijven bieden voor virtuele machines, permanente, beveiligde schijfopslag.

Meer informatie over het [mainframe-opslag naar Azure verplaatsen](./concepts/mainframe-storage-azure.md).

### <a name="backup-and-recovery"></a>Back-ups maken en herstellen

Onderhouden van uw eigen site voor noodherstel, kan een kostbare toegevoegde zijn. Azure is eenvoudig te implementeren en goedkope opties voor [back-up](/azure/backup/backup-introduction-to-azure-backup), [recovery](/azure/site-recovery/site-recovery-overview), en [redundantie](/azure/storage/common/storage-redundancy) op lokale of regionale niveaus, of via geo-redundantie.

## <a name="azure-government-for-mainframe-migrations"></a>Azure Government voor mainframe-migraties

Veel entiteiten in de openbare sector graag zou willen hun mainframe-toepassingen naar een moderne, flexibel platform verplaatsen. Microsoft Azure Government is een fysiek gescheiden instantie van de algemene Microsoft Azure-platform, verpakt voor federale, regionale en lokale overheidssystemen. Het biedt beveiliging van wereldklasse, beveiliging en nalevingsservices speciaal voor Amerikaanse overheidsinstanties en hun partners.

Azure Government verdiend een Provisional Authority to Operate (P-ATO) voor invloedrijke FedRAMP voor systemen die dit type omgeving nodig hebben.

Om te beginnen, downloadt [Microsoft Azure Government-cloud voor mainframe-toepassingen](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="next-steps"></a>Volgende stappen

Vraag onze [partners](partner-workloads.md) om u te helpen bij het migreren of uw mainframetoepassingen opnieuw hosten. Zie voor gedetailleerde richtlijnen over het kiezen van een partneroplossing de [Platform modernisering Alliance](https://www.platformmodernization.org/pages/mainframe.aspx) website.

Zie ook:

- [Technische documenten over mainframe-onderwerpen](mainframe-white-papers.md)
- [Mainframe-migratie](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Problemen oplossen](/azure/virtual-machines/troubleshooting/)
- [Duidelijke informatie over mainframe-migratie naar Azure](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
