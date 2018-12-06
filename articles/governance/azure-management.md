---
title: Overzicht van Azure Management
description: Overzicht van de gebieden van beheer voor Azure-toepassingen en bronnen met koppelingen naar inhoud op Azure-beheerhulpprogramma's.
author: DCtheGeek
manager: carmonm
ms.service: governance
ms.topic: article
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: f058bf8195c56681be0419a0a30f3d814e5d1906
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960829"
---
# <a name="management-in-azure"></a>Beheer in Azure

Governance in Azure is slechts één aspect van Azure Management. In dit artikel worden kort de verschillende beheergebieden beschreven die nodig zijn om uw toepassingen en resources te implementeren en te onderhouden in Azure. Ook worden er koppelingen naar documentatie geboden om u op weg te helpen.

Beheer refereert aan de taken en processen die nodig zijn om uw zakelijke toepassingen te onderhouden, evenals de resources die deze toepassingen ondersteunen. Azure heeft meerdere services en hulpprogramma's die samenwerken om volledig beheer te bieden voor al uw toepassingen, waar deze ook worden uitgevoerd: in Azure, maar ook in de cloud en on-premises. Inzicht in de verschillende beschikbare hulpprogramma's en in de wijze waarop deze gezamenlijk kunnen worden gebruikt in diverse beheerscenario's is de eerste stap in het ontwerpen van een volledige beheeromgeving.

In het volgende diagram ziet u de verschillende beheergebieden die nodig zijn om een toepassing of resource te onderhouden. U kunt deze reeks gebieden zien als een levenscyclus, een zich steeds herhalend proces waarin alle onderdelen in een bepaalde volgorde nodig zijn gedurende de levensduur van een resource. Het proces begint met de initiële implementatie, vervolgt met de actieve periode en eindigt wanneer de resource buiten gebruik wordt gesteld.

![Beheerfunctionaliteit](../monitoring/media/management-overview/management-capabilities.png)

Geen enkele Azure-service voldoet volledig aan de vereisten van een bepaald beheergebied. Dit kan alleen worden bereikt door meerdere, met elkaar samenwerkende services te gebruiken. Sommige services bieden gerichte functionaliteit, zoals Application Insights, waarmee webtoepassingen worden bewaakt. Andere bieden algemene functies, zoals Log Analytics, waarmee beheergegevens voor andere services worden opgeslagen, zodat u verschillende soorten gegevens kunt analyseren die door verschillende services zijn verzameld.

In de volgende gedeelten worden kort de diverse beheergebieden beschreven. Ook vindt u er koppelingen naar gedetailleerde inhoud over de belangrijkste Azure-services die voor deze gebieden bedoeld zijn.

## <a name="monitor"></a>Controleren

Bewaken is het verzamelen en analyseren van gegevens om de prestaties, status en beschikbaarheid van uw zakelijke toepassingen te bepalen, evenals de resources waar deze toepassingen van afhankelijk zijn. Een effectieve bewakingsstrategie geeft u inzicht in de precieze werking van de verschillende onderdelen van uw toepassingen. Ook stelt deze u in staat om de bedrijfstijd te verbeteren door u pro-actief op de hoogte te houden van belangrijke kwesties, zodat u deze kunt oplossen voordat ze tot problemen leiden. In [Bewaking van Azure-toepassingen en -resources](../monitoring/monitoring-overview.md) vindt u een overzicht van bewaking in Azure en leest u met welke services u een bewakingsstrategie kunt maken.

## <a name="configure"></a>Configureren

Configureren heeft betrekking op de initiële implementatie en configuratie van toepassingen en resources, en op het voortdurende onderhoud ervan met patches en updates. Automatisering van deze taken via scripts en beleid zorgt ervoor dat u redundantie wegneemt en zo de benodigde tijd en moeite beperkt, terwijl u de nauwkeurigheid en efficiëntie verbetert. [Azure Automation](../automation/automation-intro.md) biedt het merendeel van de services die bestemd zijn om configuratietaken te automatiseren. Naast runbooks om processen te automatiseren, biedt Azure Automation ook update- en configuratiebeheer. Hiermee kunt u configuraties beheren met behulp van beleid, en updates identificeren en implementeren.

## <a name="govern"></a>Governance

Governance biedt mechanismen en processen om de controle over uw toepassingen en resources in Azure te behouden. Dat omvat het plannen van initiatieven en het stellen van strategische prioriteiten.
Governance in Azure wordt hoofdzakelijk geïmplementeerd via twee services. Met [Azure Policy](../azure-policy/azure-policy-introduction.md) kunt u beleidsdefinities maken, toewijzen en beheren waarmee verschillende regels en acties voor uw resources worden afgedwongen, zodat deze resources blijven voldoen aan uw bedrijfsstandaarden en service level agreements. Met [Azure Cost Management by Cloudyn](../cost-management/overview.md) kunt u het gebruik van de cloud en de uitgaven voor uw Azure-resources en andere cloudproviders (waaronder AWS en Google) bijhouden.

## <a name="secure"></a>Beveiligen

Beveiliging van uw toepassingen, resources en gegevens beheren, moet u een combinatie van de beoordeling van bedreigingen, verzamelen en analyseren van beveiligingsgegevens en ervoor te zorgen dat uw toepassingen en resources zijn ontworpen en geconfigureerd op een veilige manier. Beveiligingsanalyse beveiligingsbewaking en bedreigingsanalyse worden geleverd door Azure Security Center, waaronder geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor hybride cloudworkloads. Raadpleeg ook [Inleiding tot Azure-beveiliging](../security/azure-security.md) voor uitgebreide informatie over beveiliging in Azure en richtlijnen voor het veilig configureren van Azure-resources.

## <a name="protect"></a>Beschermen

Bescherming heeft te maken met het altijd beschikbaar houden van uw toepassingen en gegevens, zelfs in het geval van storingen waar u niets aan kunt doen. Bescherming in Azure wordt door twee services geboden. [Azure Backup](../backup/backup-introduction-to-azure-backup.md)biedt back-up en herstel van uw gegevens in de cloud of on-premises. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) zorgt voor hoge beschikbaarheid van uw toepassingen door te zorgen voor bedrijfscontinuïteit en onmiddellijk herstel in het geval van nood.

## <a name="migrate"></a>Migreren

Migratie heeft betrekking op het overhevelen naar de Azure-cloud van workloads die momenteel on-premises worden uitgevoerd.
[Azure Migrate](../migrate/migrate-overview.md) is een service waarmee u de geschiktheid voor migratie kunt beoordelen. Zo kunt u onder meer de grootte en kosten van virtuele on-premises machines in Azure bepalen op basis van de prestaties. Azure Site Recovery kan u helpen bij de daadwerkelijk migratie van virtuele machines [van on-premises](../site-recovery/migrate-tutorial-on-premises-azure.md) of [vanuit Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md). [Azure Database Migration](../dms/dms-overview.md) helpt u bij de migratie van meerdere databasebronnen naar Azure-gegevensplatforms.
