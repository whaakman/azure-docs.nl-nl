---
title: Over Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 03918976935f9071ba2a7951e29195118943a8ef
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845830"
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

Dit artikel bevat een kort overzicht van Azure Migrate.

Azure Migrate helpt u bij het migreren naar Azure. Azure Migrate biedt een gecentraliseerde hub voor het bijhouden van detectie, beoordeling en migratie van on-premises infra structuur, toepassingen en gegevens naar Azure. De hub biedt Azure-hulpprogram ma's voor evaluatie en migratie, evenals onafhankelijke ISV-aanbiedingen (Independent Software Vendor) van derden. De oplossing biedt het volgende:

- **Uniform migratie platform**: Gebruik één portal om uw migratie traject naar Azure te starten, uit te voeren en bij te houden.
- **Bereik van hulpprogram ma's**: Azure Migrate biedt systeem eigen hulpprogram ma's en kan worden geïntegreerd met andere Azure-Services, evenals met ISV-hulpprogram ma's. Selecteer de juiste hulpprogram ma's voor evaluatie en migratie op basis van de vereisten van uw organisatie. 
- **Azure migrate server-evaluatie**: Gebruik het hulp programma voor Server evaluatie om on-premises VMware-Vm's en Hyper-V-Vm's te evalueren voor migratie naar Azure.
- **Migratie van Azure migrate server**: Gebruik het hulp programma voor server migratie om on-premises virtuele VMware-machines, virtuele Hyper-V-machines, Cloud-Vm's en fysieke servers naar Azure te migreren.
- **Evaluatie van Azure migrate-data base**: On-premises data bases evalueren voor migratie naar Azure.
- **Azure migrate database migratie**: On-premises data bases migreren naar Azure.


## <a name="azure-migrate-versions"></a>Azure Migrate versies

Er zijn twee versies van de Azure Migrate-service:

- **Huidige versie**: Gebruik deze versie om Azure Migrate projecten te maken, on-premises machines te detecteren en beoordelingen en migraties te organiseren. Meer [informatie](whats-new.md) over wat er nieuw is in deze versie.
- **Vorige versie**: Als u de vorige versie van Azure Migrate gebruikt (alleen de evaluatie van on-premises virtuele VMware-machines werd ondersteund), moet u nu de huidige versie gebruiken. U kunt niet meer Azure Migrate projecten maken met de vorige versie en u wordt aangeraden geen nieuwe detecties uit te voeren. Als u toegang wilt krijgen tot bestaande projecten, zoekt u in de Azure Portal > **alle services**naar **Azure migrate**. Op het dash board van Azure Migrate ziet u een melding en een koppeling voor toegang tot oude Azure Migrate projecten.

## <a name="isv-integration"></a>ISV-integratie

Naast de systeem eigen Azure-hulpprogram ma's, kan Azure Migrate worden geïntegreerd met een aantal ISV-aanbiedingen. U kunt het hulp programma identificeren dat u nodig hebt en dit toevoegen aan een Azure Migrate-project. U kunt uw migratie traject centraal volgen vanuit het Azure Migrate project, in Azure-en ISV-hulpprogram ma's.

**ISV** | **Functie**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Evalueren
[Apparaat 42](https://docs.device42.com/) | Evalueren
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Evalueren
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Evalueren
[Cohuur technologie](https://www.corenttech.com/AzureMigrate/) | Beoordelen en migreren
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migreren

### <a name="selecting-an-isv-tool"></a>Een ISV-hulp programma selecteren

Nadat u een ISV-hulp programma aan een Azure Migrate-project hebt toegevoegd, kunt u aan de slag met het hulp programma door een licentie te verkrijgen of u aan te melden voor een gratis proef versie, in overeenstemming met het ISV-beleid. In elk hulp programma is er een optie om verbinding te maken met Azure Migrate. Volg de instructies en documentatie van het hulp programma om het hulp programma te verbinden met Azure Migrate. Licentie verlening voor ISV-hulpprogram ma's is in overeenstemming met het ISV-licentie model.

## <a name="azure-migrate-server-assessment"></a>Evaluatie van Azure Migrate-server

Azure Migrate server-evaluatie detecteert en evalueert on-premises VMware-Vm's en Hyper-V-Vm's voor migratie naar Azure. Het helpt u bij het identificeren van het volgende:

- **Azure-gereedheid:** Bepaal of on-premises machines gereed zijn voor migratie naar Azure.
- **Azure-grootte:** Schatting van de grootte van virtuele Azure-machines na de migratie.
- **Schatting van de kosten van Azure:** Maakt een schatting van de kosten voor het uitvoeren van on-premises servers in Azure.
- **Afhankelijkheids visualisatie:** Identificeer afhankelijkheden tussen servers en de beste manier om afhankelijke servers naar Azure te verplaatsen. 

Server assessment maakt gebruik van een licht gewicht dat u on-premises implementeert en registreert bij Server evaluatie.

- Het apparaat detecteert on-premises machines, maakt verbinding met de server-evaluatie en verzendt voortdurend meta gegevens en aan prestaties gerelateerde data Azure Migrate.
- Detectie is zonder agent. Er hoeft niets te worden geïnstalleerd op gedetecteerde Vm's.
- Nadat de computers zijn gedetecteerd, verzamelt u deze in groepen die doorgaans bestaan uit Vm's die u samen wilt migreren.
- U maakt een evaluatie voor een groep. U kunt de evaluatie vervolgens analyseren om uw migratie strategie te bepalen.

## <a name="azure-migrate-server-migration"></a>Migratie van Azure Migrate server

Azure Migrate server migratie helpt u bij het migreren van on-premises VMware-Vm's, Hyper-V-Vm's, fysieke servers, andere gevirtualiseerde machines en open bare Cloud-Vm's naar Azure. U kunt machines migreren nadat u ze hebt geëvalueerd, of zonder een evaluatie. 

## <a name="azure-migrate-database-assessment"></a>Evaluatie van Azure Migrate-data base

Azure Migrate integreert met Data Migration Assistant (DMA) om on-premises SQL Server data bases te evalueren voor migratie naar Azure SQL DB, Azure SQL Managed instance of Azure-Vm's waarop SQL Server wordt uitgevoerd. DMA biedt informatie over mogelijke blokkerings problemen voor migratie. Deze bevat niet-ondersteunde functies, evenals nieuwe functies waarvan u na de migratie kunt profiteren en helpt u bij het identificeren van het juiste pad voor database migratie. [Meer informatie](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).


## <a name="azure-migrate-database-migration"></a>Azure Migrate database migratie

Azure Migrate integreert met de Azure Database Migration Service (DMS) om on-premises data bases naar Azure te migreren. Gebruik DMS om on-premises data bases te migreren naar Azure-Vm's waarop SQL, Azure SQL DB en Azure SQL Managed instances worden uitgevoerd. [Meer informatie](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-assessment-and-migration"></a>Evaluatie en migratie van web-apps

Vanuit de Azure Migrate hub kunt u on-premises web-apps evalueren en migreren naar Azure.

- **Online web-apps evalueren**: Gebruik Azure App Service Migration Assistant om on-premises websites te evalueren voor migratie naar Azure App Service.
- **Web-apps migreren**: U kunt .NET-en PHP-web-apps migreren naar Azure met behulp van Azure App Service Migration Assistant.

[Meer informatie.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Offlinegegevensmigratie

U kunt de Data Box offline familie van producten gebruiken om grote hoeveel heden gegevens naar Azure te verplaatsen. [Meer informatie](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Volgende stappen

- Probeer onze zelf studies voor het beoordelen van virtuele [VMware-machines](tutorial-assess-vmware.md) en [virtuele Hyper-V-machines](tutorial-assess-hyper-v.md).
- Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
