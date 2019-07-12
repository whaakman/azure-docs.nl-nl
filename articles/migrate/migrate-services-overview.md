---
title: Over Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 083eaaa5be35d515a477ce6286f226b267569e1e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840351"
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

Dit artikel bevat een kort overzicht van Azure Migrate.

Azure Migrate helpt u bij het migreren naar Azure. Azure Migrate biedt een centrale hub voor het volgen van detectie, beoordeling en migratie van on-premises infrastructuur, toepassingen en gegevens naar Azure. De hub biedt Azure-hulpprogramma's voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen. De oplossing biedt het volgende:

- **Geïntegreerde migratieplatform**: Gebruik één portal om te starten, uitvoeren en bijhouden van de migratie naar Azure.
- **Reeks hulpmiddelen**: Azure Migrate biedt systeemeigen hulpprogramma's en kan worden geïntegreerd met andere Azure-services, evenals met ISV's. Selecteer de juiste evaluatie en migratie hulpprogramma's, op basis van de behoeften van uw organisatie. 
- **Azure Migrate Server-evaluatie**: Gebruik het hulpprogramma voor Server-evaluatie voor de beoordeling van on-premises VMware-VM's en Hyper-V-machines voor migratie naar Azure.
- **Azure Migrate-servermigratie**: Gebruik het hulpprogramma voor migratie van de Server voor het migreren van on-premises VMware-VM's, Hyper-V-machines, cloud virtuele machines en fysieke servers naar Azure.
- **Azure Migrate de evaluatie van Database**: On-premises databases voor migratie naar Azure te beoordelen.
- **Azure Migrate databasemigratie**: On-premises databases naar Azure migreren.


## <a name="azure-migrate-versions"></a>Azure Migrate-versies

Er zijn twee versies van de Azure Migrate-service:

- **Huidige versie**: Deze versie gebruiken om te maken van Azure Migrate-projecten, on-premises computers detecteren en maakt u gecoördineerde evaluaties en migraties. [Meer informatie](whats-new.md) over wat er nieuw in deze versie.
- **Vorige versie**: Als u de vorige versie van Azure Migrate (alleen voor evaluatie van on-premises VMware-VM's wordt ondersteund) gebruikt, moet u nu de huidige versie. U kunt niet meer met behulp van de vorige versie Azure Migrate-projecten maken of uitvoeren van nieuwe detecties. U kunt echter nog steeds toegang tot bestaande projecten. Om dit te doen, in Azure portal > **alle services**, zoeken naar **Azure Migrate**. Op het dashboard Azure Migrate is een melding en een koppeling voor toegang tot de oude Azure Migrate-projecten.

## <a name="isv-integration"></a>ISV-integratie

Naast de systeemeigen Azure-hulpprogramma's, Azure Migrate kan worden geïntegreerd met een aantal ISV-aanbiedingen. U identificeren het hulpprogramma die u nodig hebt en deze toevoegen aan een Azure Migrate-project. U kunt de migratie van binnen het Azure Migrate-project, centraal bijhouden in Azure en ISV's.

**ISV** | **Functie**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Evalueren
[Apparaat 42](https://docs.device42.com/) | Evalueren
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Evalueren
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Evalueren
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Evalueren en migreren
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migreren

### <a name="selecting-an-isv-tool"></a>Een ISV-gereedschap selecteren

Na het toevoegen van een ISV-hulpprogramma op een Azure Migrate-project, aan de slag met het hulpprogramma voor het verkrijgen van een licentie of zich aanmelden voor een gratis proefversie, in overeenstemming met de ISV-beleid. Er is een optie verbinding maken met Azure Migrate in elk hulpprogramma. Volg de instructies voor het hulpprogramma en de documentatie, als u het hulpprogramma verbinding met Azure Migrate wilt.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate Server-evaluatie

Azure Migrate-Server-evaluatie wordt gedetecteerd en on-premises VMware-VM's en Hyper-V-machines beoordeelt voor migratie naar Azure. Dit kunt u het volgende identificeren:

- **Azure-gereedheid:** Bepaal of het on-premises computers zijn gereed voor migratie naar Azure.
- **Azure grootte:** Maak een schatting van de grootte van virtuele Azure-machines na de migratie.
- **Schatting van de Azure kosten:** Maakt een schatting van de kosten voor het uitvoeren van on-premises servers in Azure.
- **Visualisatie van afhankelijkheden:** Identificeer de afhankelijkheden tussen servers en de beste manier om de afhankelijke servers verplaatsen naar Azure. 

Server-evaluatie maakt gebruik van een lichtgewicht apparaat dat u on-premises implementeren en bij de Server-evaluatie registreren.

- Het apparaat on-premises computers detecteert, maakt verbinding met Server-evaluatie en voortdurend stuurt metagegevens en gegevens met betrekking tot prestaties naar Azure migreren.
- Detectie vindt plaats zonder agent. Er niets hoeft te worden geïnstalleerd op de gedetecteerde virtuele machines.
- Nadat de computers zijn gedetecteerd, kunt u ze in groepen die doorgaans bestaan uit virtuele machines die u wilt migreren samen verzamelen.
- U maakt een evaluatie voor een groep. Vervolgens kunt u de evaluatie, om te achterhalen van uw strategie voor clientmigratie analyseren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate-servermigratie

Azure migratie-Server migreren kunt u voor het migreren van on-premises VMware-VM's, Hyper-V VM's, fysieke servers, andere virtuele machines, en openbare cloud-VM's naar Azure. U kunt machines na beoordeling van deze of zonder een evaluatie migreren. 

## <a name="azure-migrate-database-assessment"></a>Azure Migrate Database-evaluatie

Azure Migrate kan worden geïntegreerd met Data Migration Assistant (DMA) om te beoordelen van on-premises SQL Server-databases voor migratie naar Azure SQL DB, Azure SQL Managed Instance of virtuele Azure-machines waarop SQL Server wordt uitgevoerd. DMA bevat informatie over mogelijke blokkerende problemen voor de migratie. Het identificeert niet-ondersteunde functies, evenals nieuwe functies die u van na de migratie profiteren kunt en u helpt bij het identificeren van het juiste pad voor migratie van de database. [Meer informatie](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).


## <a name="azure-migrate-database-migration"></a>Azure Migrate databasemigratie

Azure Migrate kan worden geïntegreerd met de Azure Database Migration Service (DMS), het migreren van on-premises databases naar Azure. DMS gebruiken voor het migreren van on-premises database naar virtuele Azure-machines met SQL, Azure SQL DB en Azure SQL Managed Instances. [Meer informatie](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-assessment-and-migration"></a>Web-app evaluatie en migratie

U kunt vanuit de Azure Migrate-hub, beoordelen en on-premises web-apps migreren naar Azure.

- **Web-apps online beoordelen**: Gebruik Azure App Service Migration Assistant voor de beoordeling van on-premises websites voor migratie naar Azure App Service.
- **Web-apps migreren**: .NET- en PHP-web-apps migreren naar Azure met behulp van Azure App Service Migration Assistant.

[Meer informatie.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Offlinesynchronisatie van gegevens migreren

U kunt de offline Data Box-familie van producten te verplaatsen van grote hoeveelheden gegevens naar Azure. [Meer informatie](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Volgende stappen

- Probeer onze zelfstudies om vast te stellen [virtuele VMware-machines](tutorial-assess-vmware.md) en [Hyper-V-machines](tutorial-assess-hyper-v.md).
- Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
