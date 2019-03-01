---
title: Mainframe opnieuw hosten op Azure virtual machines | Microsoft Docs
description: Rehost uw mainframe-workloads zoals IBM Z-systemen met behulp van virtuele machines (VM's) op Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 8be0ebc486739f8826e8a1d5a5307a219ba71b6f
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192714"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Mainframe opnieuw hosten op Azure virtual machines

Workloads voor migratie van mainframe-omgevingen aan de cloud kunt u uw infrastructuur moderniseren en vaak Bespaar op kosten. Alleen secundaire code te wijzigen, zoals het bijwerken van de namen van databases, kunnen u veel werkbelastingen overbrengen naar Azure.

De term *mainframe* algemeen verwijst naar een groot systeem, maar de meeste die momenteel zijn geïmplementeerd zijn IBM System Z servers of IBM plug-compatible systemen met MVS, DOS, VSE, OS/390 of z/OS.

Een Azure virtuele machine (VM) wordt gebruikt om te isoleren en beheren van de resources voor een bepaalde toepassing op één exemplaar. Logische partities (LPARS) mainframes zoals IBM z/OS gebruiken voor dit doel. Een mainframe bijvoorbeeld één LPAR voor een regio CICS met gekoppelde COBOL-programma's en een afzonderlijke LPAR gebruiken voor IBM Db2-database. Een typische [n-tier-toepassing op Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) Azure-VM's implementeert in een virtueel netwerk dat kan worden gesegmenteerd in subnetten voor elke laag.

Azure-VM's kunnen uitvoeren, mainframe-emulatie omgevingen en compilers die ondersteuning bieden voor lift-and-shift-scenario's. Ontwikkelen en testen zijn vaak een van de eerste werkbelastingen die het migreren van een mainframe naar een Azure dev/test-omgeving. Algemene serveronderdelen waarmee kan worden geëmuleerd zijn onder andere online transactieproces (OLTP), batch en systemen voor opname van gegevens zoals in de volgende afbeelding wordt weergegeven.

![Emulatie omgevingen op Azure kunnen u z/OS gebaseerde systemen uitvoeren.](media/01-overview.png)

Sommige mainframe-werkbelastingen kunnen worden gemigreerd naar Azure met relatief veel gemak, terwijl anderen kunnen worden rehosted op Azure met behulp van een partneroplossing. Voor gedetailleerde instructies over het kiezen van een partneroplossing de [Azure Mainframe-migratie center](https://azure.microsoft.com/migration/mainframe/) kan helpen.

## <a name="set-up-devtest-environment-using-a-micro-focus-rehosting-platform"></a>Omgeving voor ontwikkelen en testen met behulp van een Micro Focus rehosting platform instellen

Micro Focus Enterprise-Server is een van de grootste mainframe opnieuw hosten van platforms beschikbaar. U kunt uw z/OS-workloads uitvoeren op een goedkopere x86 platform in Azure.

Als u wilt beginnen, Zie de volgende artikelen:

- [Micro Focus Enterprise Server 4.0 en Enterprise Developer 4.0 installeren op Azure](./microfocus/set-up-micro-focus-on-azure.md)
- [Micro Focus CICS BankDemo instellen voor Micro Focus Enterprise Developer 4.0 in Azure](./microfocus/demo.md)

## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame on Azure

TmaxSoft OpenFrame is een populaire mainframe oplossing waarmee u eenvoudig uw bestaande mainframe-activa lift- and -shift ze naar Azure opnieuw hosten. Een omgeving OpenFrame op Azure is geschikt voor ontwikkeling, demo's, tests of productie-workloads.

Om te beginnen, downloadt de [TmaxSoft OpenFrame installeren op Azure](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/) ebook.

## <a name="set-up-a-devtest-environment-using-ibm-z-devtest-120"></a>Instellen van een dev/test-omgeving met behulp van IBM Z Dev/Test 12.0

IBM Z Development- en testomgeving (IBM zD & T) stelt u een niet-productieomgeving op Azure die u voor ontwikkeling gebruiken kunt, testen en demo's van z/OS gebaseerde toepassingen.

De emulatieomgeving op Azure kunt tal van Z-instanties toepassing ontwikkelaars beheerd distributies (ADCDs) hosten. U kunt zD & T persoonlijke editie, zD & T parallelle Sysplex, en zD & T Enterprise Edition uitvoeren op Azure en Azure Stack.

Als u wilt beginnen, Zie de volgende artikelen:

-   [Instellen van IBM zD & T 12.0 op Azure](./ibm/install-ibm-z-environment.md)
-   [ADCD instellen op zD & T](./ibm/demo.md)

## <a name="migrate-ibm-db2-purescale-to-azure"></a>IBM DB2 pureScale migreren naar Azure

De IBM DB2 pureScale omgeving bevat een databasecluster voor Azure met hoge beschikbaarheid en schaalbaarheid op Linux-besturingssystemen. Hoewel niet identiek zijn aan de oorspronkelijke omgeving, biedt IBM DB2 pureScale op Linux een vergelijkbare hoge beschikbaarheid en schaalbaarheid, functies als IBM DB2 voor z/OS uitvoeren in een parallelle Sysplex-configuratie op de mainframe.

Als u wilt beginnen, Zie [IBM DB2 pureScale op Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Overwegingen

Wanneer u mainframe-workloads migreren naar Azure-infrastructuur als een dienst (IaaS), kunt u kiezen uit verschillende soorten on-demand, schaalbare IT-bronnen, met inbegrip van Azure-VM's. Azure biedt een scala aan [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/overview) en [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/overview) VM's.

### <a name="high-availability-and-failover"></a>Hoge beschikbaarheid en failover

Azure biedt op basis van toezegging service level agreements (Sla's), waarbij meerdere 9 's beschikbaarheid is de standaardinstelling en geoptimaliseerd met lokaal of op basis van een geo-replicatie van services. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

Voor Azure IaaS, zoals virtuele machines, failover is afhankelijk van specifieke functionaliteit, zoals failover clustering exemplaren en [beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets). Wanneer u Azure-platform gebruikt als een service (PaaS)-bronnen, zoals [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) en [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), het platform failovers automatisch verwerkt.

### <a name="scalability"></a>Schaalbaarheid

Mainframes doorgaans opschalen, terwijl cloud omgeving scale-out. Azure biedt een scala aan [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) en [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) grootten om te voldoen aan uw behoeften. De cloud kan ook worden geschaald omhoog of omlaag overeenkomst gebruiker exacte specificaties. Rekenkracht, opslag, en services [schaal](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling) on-demand onder een factureringsmodel op basis van gebruik.

### <a name="storage"></a>Opslag

In de cloud, hebt u een scala aan opties voor flexibele, schaalbare opslag en u betaalt alleen voor wat u nodig hebt. [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) biedt een hoge mate schaalbare opslag voor gegevensobjecten, een bestandssysteemservice voor de cloud, een berichtenarchief voor betrouwbare en een NoSQL-archief. Beheerde en onbeheerde schijven bieden voor virtuele machines, permanente, beveiligde schijfopslag.

### <a name="backup-and-recovery"></a>Back-ups maken en herstellen

Onderhouden van uw eigen site voor noodherstel, kan een kostbare toegevoegde zijn. Azure is eenvoudig te implementeren en goedkope opties voor [back-up](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup), [recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en [redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) op lokale of regionale niveaus, of via geo-redundantie.

## <a name="azure-government-for-mainframe-migrations"></a>Azure Government voor mainframe-migraties

Veel entiteiten in de openbare sector graag zou willen hun mainframe-toepassingen naar een moderne, flexibel platform verplaatsen. Microsoft Azure Government is een fysiek gescheiden instantie van cloudservices, maar verpakt voor federale, regionale en lokale overheidssystemen op basis van de algemene Microsoft Azure-platform. Het biedt beveiliging van wereldklasse, beveiliging en nalevingsservices speciaal voor Amerikaanse overheidsinstanties en hun partners.

Azure Government verdiend een Provisional Authority to Operate (P-ATO) voor invloedrijke FedRAMP voor systemen die dit type omgeving nodig hebben. 

Om te beginnen, downloadt [Microsoft Azure Government-cloud voor mainframe-toepassingen](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="learn-more"></a>Meer informatie

Als u een mainframe-migratie, onze uitgebreide overweegt [partner](partner-workloads.md) -ecosysteem is beschikbaar om u te helpen. Raadpleeg voor gedetailleerde richtlijnen over het kiezen van een partneroplossing de [Platform modernisering Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

Zie ook:

-   [Mainframe-migratie](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
-   [Problemen oplossen](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
-   [Mainframe ontrafelen aan Azure-migratie](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
