---
title: Beheer van Azure - bewaking | Microsoft Docs
description: Azure heeft verschillende services en hulpprogramma's die samenwerken, zodat het volledige beheer voor niet alleen uw toepassingen die worden uitgevoerd in Azure, maar ook in andere clouds en on-premises bieden.  Dit artikel bevat een beschrijving op hoog niveau van de verschillende gebieden van beheer en koppelingen naar inhoud op Azure-hulpprogramma's voor het beheren van uw cloud-toepassingen en bronnen.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: bwren
ms.openlocfilehash: b20283e1189e4f1a3555e2dd8d25972c9a677cd6
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="azure-management---monitoring"></a>Azure Management - Monitoring

Bewaken in Azure is een aspect van Azure Management.  In dit artikel is een korte beschrijving van de verschillende gebieden van beheer dat is vereist voor het implementeren en onderhouden van uw toepassingen en bronnen in Azure met koppelingen naar documentatie om u op weg.

## <a name="management-in-azure"></a>Beheer in Azure

Beheer verwijst naar de taken en processen die zijn vereist voor het behouden van uw zakelijke toepassingen en de resources die deze ondersteunen.  Azure heeft verschillende services en hulpprogramma's die samenwerken, zodat het volledige beheer voor niet alleen uw toepassingen die worden uitgevoerd in Azure, maar ook in andere clouds en on-premises bieden.  Inzicht in de verschillende beschikbare hulpprogramma's en hoe ze kunnen worden gebruikt samen voor een verscheidenheid aan scenario's voor beheer, is de eerste stap bij het ontwerpen van een omgeving met volledig beheer.

Het volgende diagram illustreert de verschillende gebieden van beheer die nodig zijn voor het onderhouden van een toepassing of bron.  Deze gebieden worden beschouwd in termen van de levenscyclus waarvoor elk continue achter elkaar is vereist via de levensduur van een resource.  Hiermee start u de initiële implementatie via het blijven functioneren en ten slotte wanneer deze buiten gebruik gesteld.

![Beheermogelijkheden voor](media/management-overview/management-capabilities.png)


Er is geen één Azure-service volledig gevuld door de vereisten van een bepaalde beheer-gebied, maar in plaats daarvan elke worden gerealiseerd door meerdere services die samenwerken.  Sommige services bieden bepaalde functionaliteit, zoals Application Insights die biedt bewaking van webtoepassingen.  Anderen bieden algemene functies zoals logboekanalyse die beheergegevens voor andere services zodat u opslaat kunt het analyseren van gegevens van verschillende typen die door andere services worden verzameld.  

De volgende secties beschrijven de verschillende beheerterreinen kort en bevatten koppelingen naar gedetailleerde informatie over de Azure-services die zijn bedoeld om ze te behandelen.

## <a name="monitor"></a>Controleren
Bewaking wordt het verzamelen en analyseren van gegevens om te bepalen van de prestaties, status en beschikbaarheid van uw zakelijke toepassingen en de resources die deze afhankelijk is. Een strategie voor een effectieve controle krijgt u inzicht in de gedetailleerde werking van de verschillende onderdelen van uw toepassing en uw bedrijfstijd verbeteren door het verwittigen van kritieke problemen proactief zodat u deze oplossen kunt voordat ze problemen.  U kunt een overzicht van de bewaking in Azure die gebruikt voor een strategie voor een controle op verschillende services worden lezen [bewaking Azure-toepassingen en bronnen](monitoring-overview.md).


## <a name="configure"></a>Configureren
Configureer verwijst naar de eerste implementatie en configuratie van toepassingen en bronnen en hun doorlopend onderhoud met patches en updates.  Automatisering van deze taken via de script- en -beleid kunt u redundantie, de tijd en moeite minimaliseren en de nauwkeurigheid en efficiëntie verhogen elimineren.  [Azure Automation](..\automation\automation-intro.md) het merendeel van de services voor het automatiseren van configuratietaken biedt.  Naast de runbooks voor het automatiseren van processen, biedt de configuratie en beheer van updates die u helpen bij het beheren van configuratie via beleid en in te identificeren en implementeren van updates.

## <a name="govern"></a>Bepalen
Governance biedt mechanismen en processen controle over uw toepassingen en bronnen in Azure.  Dit omvat het plannen van uw initiatieven en strategische prioriteiten.  Beheeracties in Azure wordt voornamelijk geïmplementeerd met twee services.  [Azure beleid](../azure-policy/azure-policy-introduction.md) kunt u maakt, toewijst en, beleidsdefinities die verschillende regels en acties via uw resources, afdwingen zodat deze resources met uw bedrijfsnormen te blijven en serviceovereenkomsten beheren. [Azure kostenbeheer door Cloudyn](../cost-management/overview.md) kunt u gebruik van cloud- en uitgaven voor uw Azure-resources en andere cloudproviders zoals AWS en Google bijhouden.

## <a name="secure"></a>Beveiligen
Beveiliging van uw toepassingen, bronnen en gegevens beheren, moet u een combinatie van de beoordeling van bedreigingen, verzamelen en analyseren van beveiligingsgegevens en ervoor te zorgen dat uw toepassingen en bronnen worden ontworpen en geconfigureerd op een veilige wijze.  Bewaking en threat beveiligingsanalyse worden geleverd door [Azure Security Center](../security-center/security-center-intro.md) waaronder unified beveiligingsbeheer en geavanceerde threat protection over hybride cloudwerkbelastingen.  U ziet ook [Inleiding tot Azure-beveiliging](../security/azure-security.md) voor uitgebreide informatie over beveiliging in Azure en richtlijnen over het configureren van veilig Azure-resources.


## <a name="protect"></a>Beschermen
Beveiliging verwijst om ervoor te zorgen dat uw toepassingen en gegevens altijd beschikbaar, zelfs in het geval van storingen afgezien van het besturingselement zijn.  Beveiliging in Azure wordt geleverd door twee services.  [Azure Backup](../backup/backup-introduction-to-azure-backup.md) biedt back-up en herstel van uw gegevens in de cloud of on-premises.    [Azure Site Recovery](../site-recovery/site-recovery-overview.md) zorgt ervoor dat hoge beschikbaarheid van uw toepassing door op te geven voor bedrijfscontinuïteit en onmiddellijk herstel in het geval van een noodherstel.

## <a name="migrate"></a>Migreren 
Migratie verwijst naar een overgang werkbelastingen lokale momenteel wordt uitgevoerd in de Azure-cloud.  [Azure migreren](../migrate/migrate-overview.md) is een service waarmee u beoordelen de geschiktheid van migratie kunt, met inbegrip van formaat op basis van prestaties en kosten schattingen van de lokale virtuele machines in Azure.  Azure Site Recovery kunt u de daadwerkelijke migratie van virtuele machines uitvoeren [van on-premises](../site-recovery/migrate-tutorial-on-premises-azure.md) of [van Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md).  [Migratie van Azure Database](../dms/dms-overview.md) helpt u bij het migreren van meerdere databasebronnen naar Azure Data platforms.