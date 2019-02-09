---
title: Een Azure Service Fabric-cluster upgraden | Microsoft Docs
description: Meer informatie over het upgraden van de versie of de configuratie van een Azure Service Fabric-cluster.  Dit artikel wordt beschreven instelling cluster updatemodus, een upgrade van certificaten, toe te voegen toepassingspoorten, patches voor het besturingssysteem en wat u kunt verwachten wanneer de upgrades worden uitgevoerd
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: f19693853672b6274265c95b851f478b4f3ef4a2
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961887"
---
# <a name="upgrading-and-updating-an-azure-service-fabric-cluster"></a>Upgraden en het bijwerken van een Azure Service Fabric-cluster

Voor elk modern systeem is ontwerpen voor kunnen erg belangrijk voor het succes van uw product op lange termijn te bereiken. Een Azure Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Dit artikel wordt beschreven wat automatisch wordt beheerd en wat u kunt zelf configureren.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op uw cluster beheren

Zorg ervoor dat uw cluster met een [ondersteunde fabric versie](service-fabric-versions.md) altijd. Als en wanneer we kondigen wij de release van een nieuwe versie van service fabric, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf die datum is gemarkeerd voor einde van ondersteuning. De nieuwe releases worden aangekondigd op de blog van het service fabric-team. De nieuwe versie is beschikbaar voor vervolgens kiest.

14 dagen voordat het verstrijken van de release van die het cluster wordt uitgevoerd, wordt een statusgebeurtenis gegenereerd waarmee uw cluster in een waarschuwingsstatus wordt geplaatst zijn. Het cluster blijft een waarschuwingsstatus totdat u een upgrade naar een ondersteunde fabric-versie uitvoert.

U kunt instellen dat uw cluster voor het ontvangen van automatische infrastructuurupgrades als ze door Microsoft worden uitgegeven of kunt u een ondersteunde fabric-versie die u wilt dat uw cluster op.  Voor meer informatie lezen [upgrade van de Service Fabric-versie van uw cluster](service-fabric-cluster-upgrade-version-azure.md).

## <a name="fabric-upgrade-behavior-during-automatic-upgrades"></a>Fabric gedrag tijdens upgrades met automatische upgrade
Microsoft onderhoudt de fabric-code en configuratie die wordt uitgevoerd in een Azure-cluster. Wij uitvoeren automatische bewaakte upgrades van de software op basis van behoefte. Deze upgrades mogelijk code, configuratie, of beide. Om ervoor te zorgen dat uw toepassing geen gevolgen of minimale gevolgen vanwege deze upgrades heeft, uitvoeren we de upgrades in de volgende fasen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: Een upgrade wordt uitgevoerd met behulp van alle beleidsregels voor cluster health
Tijdens deze fase wordt de upgrades één upgradedomein tegelijk doorgaan en de toepassingen die worden uitgevoerd in het cluster worden uitgevoerd zonder uitvaltijd. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. Vervolgens wordt een e-mailbericht verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

* Melding dat we hadden terugdraaien van een clusterupgrade van een.
* Voorgestelde corrigerende acties, indien van toepassing.
* Het aantal dagen (n) totdat we fase 2 uitvoeren.

We proberen uit te voeren dezelfde upgrade uit een aantal keer in het geval eventuele upgrades is mislukt door redenen infrastructuur. Na de n dagen vanaf de datum waarop die het e-mailbericht is verzonden, gaan we in fase 2.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade de Canonisering en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mailadres van een geslaagde uitvoering. Dit is om te voorkomen dat u te veel e-mailberichten--ontvangt een e-mailbericht moet worden gezien als een uitzondering op de normale verzenden. We verwachten dat de meeste van de cluster-upgrades te voltooien zonder enige impact op de beschikbaarheid van uw toepassing.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: Een upgrade wordt uitgevoerd met behulp van standaard statusbeleid alleen
Het statusbeleid in deze fase worden ingesteld in zodanig dat het aantal toepassingen die in orde aan het begin van de upgrade zijn hetzelfde voor de duur van het upgradeproces blijft. Zoals in fase 1, de fase 2-upgrades één upgradedomein tegelijk doorgaan en de toepassingen die worden uitgevoerd in het cluster worden uitgevoerd zonder uitvaltijd. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) zijn heeft gehouden aan voor de duur van de upgrade.

Als de cluster-statusbeleid wordt van kracht niet voldaan, is de upgrade teruggedraaid. Vervolgens wordt een e-mailbericht verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

* Melding dat we hadden terugdraaien van een clusterupgrade van een.
* Voorgestelde corrigerende acties, indien van toepassing.
* Het aantal dagen (n) totdat we fase 3 uitvoeren.

We proberen uit te voeren dezelfde upgrade uit een aantal keer in het geval eventuele upgrades is mislukt door redenen infrastructuur. Een herinneringse-mail wordt een aantal dagen waarna n dagen zijn verzonden. Na de n dagen vanaf de datum waarop die het e-mailbericht is verzonden, gaan we in fase 3. De e-mailberichten we u in fase 2 sturen ernstig moeten worden genomen en corrigerende acties moeten worden genomen.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade de Canonisering en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mailadres van een geslaagde uitvoering.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: Een upgrade wordt uitgevoerd met behulp van agressief statusbeleid
Deze statusbeleid in deze fase zijn afgestemd op de voltooiing van de upgrade in plaats van de status van de toepassingen. Enkele upgrades van cluster terechtkomen in deze fase. Als uw cluster opgehaald voor deze fase, is er een grote kans dat uw toepassing slecht en/of beschikbaarheid verliest.

Net als bij de andere twee fasen, upgrades voor fase 3 gaan één upgradedomein tegelijk.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. We proberen uit te voeren dezelfde upgrade uit een aantal keer in het geval eventuele upgrades is mislukt door redenen infrastructuur. Het cluster is hierna vastgemaakt, zodat deze niet langer ondersteuning en/of upgrades ontvangt.

Een e-mailbericht met deze informatie wordt verzonden naar de eigenaar van het abonnement, samen met de corrigerende acties. We verwachten niet dat alle clusters toegang te krijgen tot een status waarbij de fase 3 is mislukt.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade de Canonisering en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mailadres van een geslaagde uitvoering.

## <a name="manage-certificates"></a>Certificaten beheren
Maakt gebruik van service Fabric [x.509-certificaten](service-fabric-cluster-security.md) dat u wanneer u een cluster opgeeft voor het beveiligen van communicatie tussen clusterknooppunten en verifiëren van clients maakt. U kunt toevoegen, bijwerken of verwijderen van certificaten voor het cluster en de client in de [Azure-portal](https://portal.azure.com) of met behulp van PowerShell/Azure CLI.  Voor meer informatie lezen [toevoegen of verwijderen van certificaten](service-fabric-cluster-security-update-certs-azure.md)

## <a name="open-application-ports"></a>Poorten openen
U kunt poorten wijzigen door het veranderen van de Load Balancer-resource-eigenschappen die gekoppeld aan het knooppunttype zijn. U kunt de Azure portal gebruiken of kunt u PowerShell/Azure CLI. Lees voor meer informatie, [Open toepassingspoorten voor een cluster](create-load-balancer-rule.md).

## <a name="define-node-properties"></a>Eigenschappen van het knooppunt definiëren
Soms wilt u mogelijk om ervoor te zorgen dat bepaalde workloads worden uitgevoerd alleen op bepaalde typen knooppunten in het cluster. Bijvoorbeeld enkele workload mogelijk GPU's of SSD's en andere niet. Voor elk van de knooppunttypen in een cluster, kunt u eigenschappen van aangepaste knooppunt toevoegen aan de clusterknooppunten. Plaatsingsbeperkingen zijn de instructies die zijn gekoppeld aan afzonderlijke services ervan die voor een of meer eigenschappen van het knooppunt selecteren. Plaatsingsbeperkingen definiëren waar services moeten worden uitgevoerd.

Lees voor meer informatie over het gebruik van plaatsingsbeperkingen, eigenschappen van het knooppunt en hoe u ze definiëren [knooppunteigenschappen en plaatsingsbeperkingen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="add-capacity-metrics"></a>Metrische gegevens over capaciteit toevoegen
U kunt aangepaste metrische gegevens over capaciteit die u wilt gebruiken in uw toepassingen om te rapporteren over de belasting toevoegen voor elk van de knooppunttypen. Raadpleeg voor meer informatie over het gebruik van metrische gegevens over capaciteit om te rapporteren over de belasting op de Service Fabric Cluster Resource Manager-documenten op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md) en [metrische gegevens en belasting](service-fabric-cluster-resource-manager-metrics.md).

## <a name="set-health-policies-for-automatic-upgrades"></a>Beleid instellen voor automatische upgrades
U kunt aangepaste statusbeleid voor fabric-upgrade. Als u uw cluster hebt ingesteld op automatische infrastructuurupgrades, krijgen deze beleidsregels toegepast op de fase-1 van de automatische infrastructuurupgrades.
Als u hebt uw cluster voor handmatige fabric upgrades ingesteld, krijgen deze beleidsregels telkens wanneer u een nieuwe versie activeren van het systeem een vliegende start de fabric-upgrade in uw cluster toegepast. Als u het beleid niet overschrijven, worden de standaardwaarden gebruikt.

U kunt de aangepaste statusbeleid opgeven of de huidige instellingen op de blade 'fabric-upgrade' controleren door het selecteren van de geavanceerde instellingen van de upgrade. Bekijk de volgende afbeelding voor het. 

![Aangepaste statusbeleid beheren][HealthPolices]

## <a name="customize-fabric-settings-for-your-cluster"></a>Fabric-instellingen voor uw cluster aanpassen
Veel verschillende configuratie-instellingen kunnen worden aangepast op een cluster, zoals het betrouwbaarheidsniveau dat van de cluster- en knooppunt-eigenschappen. Lees voor meer informatie, [infrastructuurinstellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>Patch uitvoeren voor het besturingssysteem in de clusterknooppunten
De patch orchestration-toepassing (POA) is een Service Fabric-toepassing waarmee het besturingssysteem op een Service Fabric-cluster zonder uitvaltijd patches worden geautomatiseerd. De [Patch Orchestration-toepassing voor Windows](service-fabric-patch-orchestration-application.md) of [Patch Orchestration-toepassing voor Linux](service-fabric-patch-orchestration-application-linux.md) kan worden geïmplementeerd in uw cluster om patches te installeren op een gecoördineerde manier terwijl de services beschikbare voortdurend. 

## <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Upgrades voor het besturingssysteem op de virtuele machines die gezamenlijk het cluster
Als u de installatiekopie van het besturingssysteem op de virtuele machines van het cluster bijwerkt moet, u moet dit doen één virtuele machine op een tijdstip. U bent verantwoordelijk voor deze upgrade--er is momenteel geen automatisering voor dit.

## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van enkele van de [service fabric-cluster infrastructuurinstellingen](service-fabric-cluster-fabric-settings.md)
* Meer informatie over het [in en uit een cluster schalen](service-fabric-cluster-scale-up-down.md)
* Meer informatie over [upgrades van toepassingen](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
