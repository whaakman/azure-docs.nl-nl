---
title: Wat is er nieuw in Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 06/10/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 2c3bc596076f3ec4f9d41f0da819ddd386fee63c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809993"
---
# <a name="whats-new-in-azure-migrate"></a>Wat is er nieuw in Azure Migrate

[Azure Migrate](migrate-services-overview.md) helpt u bij het detecteren, beoordelen en migreren van servers, apps en gegevens naar de Microsoft Azure-cloud. In dit artikel bevat een overzicht van nieuwe functies in Azure Migrate.



## <a name="azure-migrate-new-version"></a>Nieuwe versie van Azure migreren

Een nieuwe versie van Azure Migrate werd uitgebracht in juli 2019. 

- **Huidige (nieuw) versie**: Deze versie gebruiken om te maken van Azure Migrate-projecten, on-premises computers detecteren en maakt u gecoördineerde evaluaties en migraties. 
- **Vorige versie**: Voor klanten met behulp van de vorige versie van Azure Migrate (alleen voor evaluatie van on-premises VMware-VM's wordt ondersteund), moet u nu de huidige versie gebruiken. In de vorige versie, kunt u niet meer nieuwe Azure Migrate-projecten maken of uitvoeren van nieuwe detecties. U kunt nog steeds toegang tot bestaande projecten. Om dit te doen in Azure portal > alle services zoeken naar Azure migreren. Er is een koppeling voor toegang tot de oude Azure Migrate-projecten in de meldingen Azure Migrate.


## <a name="azure-migrate-features"></a>Functies van Azure migreren

De nieuwe versie van Azure Migrate biedt een aantal nieuwe functies:


- **Geïntegreerde migratieplatform**: Azure Migrate biedt nu één portal om te centraliseren, beheren en bijhouden van de migratie naar Azure, met een verbeterde distributie stroom en de portal-ervaring.
- **Hulpprogramma's voor evaluatie en migratie**: Azure Migrate biedt systeemeigen hulpprogramma's en kan worden geïntegreerd met andere Azure-services, evenals met hulpprogramma's voor independent software vendor (ISV). [Meer informatie](migrate-services-overview.md#isv-integration) over ISV-integratie.
- **Azure Migrate-evaluatie**: Met het hulpprogramma Azure Migrate-Server-evaluatie, kunt u virtuele VMware-machines en Hyper-V-VM's beoordelen voor migratie naar Azure. Ook kunt u beoordelen voor migratie met behulp van andere Azure-services en ISV-hulpprogramma's.
- **Azure Migrate-migratie**: Met behulp van het hulpprogramma voor migratie van Azure-Server migreren, kunt u on-premises VMware-machines migreren en Hyper-V-machines naar Azure, en fysieke servers, andere gevirtualiseerde servers en persoonlijke/openbare cloud-VM's. Bovendien kunt u migreren naar Azure met ISV's.
- **Azure Migrate-apparaat**: Azure Migrate implementeert een lichtgewicht toestel voor detectie en evaluatie van on-premises VMware-VM's en Hyper-V-machines.
    - Dit apparaat wordt gebruikt door Azure Migrate-Server-evaluatie en migratie van Azure Migrate Server voor de migratie zonder agent.
    - Het apparaat worden gegevens van server-metagegevens en de prestaties, continu gedetecteerd voor de toepassing van de evaluatie en migratie.  
- **VMware-VM-migratie**:  Azure Migrate servermigratie biedt een aantal methoden voor het migreren van on-premises virtuele VMware-machines naar Azure.  Een zonder agent-migratie met behulp van het apparaat Azure Migrate en een migratie op basis van een agent die gebruikmaakt van een replicatie-apparaat, en implementeert u een agent op elke virtuele machine die u wilt migreren. [Meer informatie](server-migrate-overview.md)
 - **Database-evaluatie en migratie**: Van Azure Migrate, kunt u on-premises databases voor migratie naar Azure met behulp van de Azure Database Migration Assistant beoordelen. U kunt met behulp van de Azure Database Migration Service-databases kunt migreren.
- **Web-Apps migreren**: Web-apps met behulp van een openbaar eindpunt-URL met de Azure App Service, kunt u beoordelen. U kunt voor de migratie van interne .NET-apps, downloaden en uitvoeren van de migratie-assistent van App Service. 
- **Data Box**: Grote hoeveelheden offline gegevens importeren in Azure met behulp van Azure Data Box in Azure Migrate.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
- Probeer onze zelfstudies om vast te stellen [virtuele VMware-machines](tutorial-assess-vmware.md) en [Hyper-V-machines](tutorial-assess-hyper-v.md).
