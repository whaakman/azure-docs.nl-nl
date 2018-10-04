---
title: Een Azure Service Fabric-cluster upgraden | Microsoft Docs
description: Upgrade van de Service Fabric-code en/of de configuratie die wordt uitgevoerd van een Service Fabric-cluster, inclusief het instellen van de cluster-updatemodus, een upgrade van certificaten, toe te voegen toepassingspoorten, uitvoeren van patches voor het besturingssysteem, enzovoort. Wat kunt u verwachten wanneer de upgrades worden uitgevoerd?
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
ms.date: 8/10/2017
ms.author: aljo
ms.openlocfilehash: 2fd62f8709bddfd981f4b1358c97d0acbaf7f12d
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269096"
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Een Azure Service Fabric-cluster upgraden
> [!div class="op_single_selector"]
> * [Azure-Cluster](service-fabric-cluster-upgrade.md)
> * [Zelfstandige Cluster](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Voor elk modern systeem is ontwerpen voor kunnen erg belangrijk voor het succes van uw product op lange termijn te bereiken. Een Azure Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Dit artikel wordt beschreven wat automatisch wordt beheerd en wat u kunt zelf configureren.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op uw Cluster beheren
U kunt instellen dat uw cluster voor het ontvangen van automatische infrastructuurupgrades als ze door Microsoft worden uitgegeven of kunt u een ondersteunde fabric-versie die u wilt dat uw cluster op.

U doen dit door het instellen van de clusterconfiguratie 'upgradeMode' in de portal of met behulp van Resource Manager op het moment van maken of hoger op een live cluster 

> [!NOTE]
> Zorg ervoor dat u uw cluster met een ondersteunde fabric-versie altijd behouden. Als en wanneer we kondigen wij de release van een nieuwe versie van service fabric, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf die datum is gemarkeerd voor einde van ondersteuning. De nieuwe releases worden aangekondigd [op de blog van het service fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar voor vervolgens kiest. 
> 
> 

14 dagen voordat het verstrijken van de release van die het cluster wordt uitgevoerd, wordt een statusgebeurtenis gegenereerd waarmee uw cluster in een waarschuwingsstatus wordt geplaatst zijn. Het cluster blijft een waarschuwingsstatus totdat u een upgrade naar een ondersteunde fabric-versie uitvoert.

### <a name="setting-the-upgrade-mode-via-portal"></a>Instellen van de upgrademodus via de portal
U kunt het cluster instellen op automatisch of handmatig wanneer u het cluster maakt.

![Create_Manualmode][Create_Manualmode]

U kunt het cluster instellen op automatisch of handmatig wanneer u op een live cluster met behulp van de ervaring beheren. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Een upgrade uitvoeren naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via de portal.
Als u wilt upgraden naar een nieuwe versie, is hoeft u alleen maar selecteert u de versie in de vervolgkeuzelijst en opslaan. De Fabric-upgrade wordt automatisch gestart. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. Schuif omlaag in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Als u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, moet u de upgrade opnieuw starten door dezelfde stappen als voordat te volgen.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>De upgrademodus via Resource Manager-sjabloon instellen
De configuratie 'upgradeMode' toevoegen aan de resourcedefinitie Microsoft.ServiceFabric/clusters en de 'clusterCodeVersion' ingesteld op een van de ondersteunde fabric-versies zoals hieronder wordt weergegeven en vervolgens de sjabloon te implementeren. De geldige waarden voor 'upgradeMode' zijn 'Handmatig' of 'Automatisch'

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Een upgrade uitvoeren naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via Resource Manager-sjabloon.
Als het cluster in de modus voor handmatige is, om te upgraden naar een nieuwe versie, de 'clusterCodeVersion' wijzigen naar een ondersteunde versie en deze implementeren. De implementatie van de sjabloon, gang van de Fabric-upgrade wordt gestart automatisch. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. Schuif omlaag in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Als u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, moet u de upgrade opnieuw starten door dezelfde stappen als voordat te volgen.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Lijst alle beschikbare versie voor alle omgevingen voor een bepaald abonnement ophalen
Voer de volgende opdracht en deze krijgt u uitvoer die vergelijkbaar is met dit.

'supportExpiryUtc' geeft aan dat uw wanneer een bepaalde release is verlopen of is verlopen. De nieuwste versie beschikt niet over een geldige datum - heeft een waarde van "9999-12-31T23:59:59.9999999 ', wat betekent dat de vervaldatum is nog niet ingesteld.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Fabric upgrade gedrag als de modus bijwerken van het cluster automatisch is
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
Deze statusbeleid in deze fase zijn afgestemd op de voltooiing van de upgrade in plaats van de status van de toepassingen. Heel weinig upgrades van cluster terechtkomen in deze fase. Als uw cluster opgehaald voor deze fase, is er een grote kans dat uw toepassing slecht en/of beschikbaarheid verliest.

Net als bij de andere twee fasen, upgrades voor fase 3 gaan één upgradedomein tegelijk.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. We proberen uit te voeren dezelfde upgrade uit een aantal keer in het geval eventuele upgrades is mislukt door redenen infrastructuur. Het cluster is hierna vastgemaakt, zodat deze niet langer ondersteuning en/of upgrades ontvangt.

Een e-mailbericht met deze informatie wordt verzonden naar de eigenaar van het abonnement, samen met de corrigerende acties. We verwachten niet dat alle clusters toegang te krijgen tot een status waarbij de fase 3 is mislukt.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade de Canonisering en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mailadres van een geslaagde uitvoering.

## <a name="cluster-configurations-that-you-control"></a>Configuraties van clusters die u beheert
Naast de mogelijkheid om in te stellen het cluster upgrademodus, Hier volgen de configuraties die u op een live cluster wijzigen kunt.

### <a name="certificates"></a>Certificaten
U kunt nieuwe toevoegen of verwijderen van certificaten voor het cluster en de client via de portal gemakkelijk. Raadpleeg [dit document voor gedetailleerde instructies](service-fabric-cluster-security-update-certs-azure.md)

![Schermafbeelding waarin de vingerafdrukken van het certificaat wordt in de Azure-portal.][CertificateUpgrade]

### <a name="application-ports"></a>Toepassingspoorten
U kunt poorten wijzigen door het veranderen van de Load Balancer-resource-eigenschappen die gekoppeld aan het knooppunttype zijn. U kunt de portal gebruiken, of kunt u Resource Manager PowerShell direct.

Als u wilt een nieuwe poort op alle virtuele machines in een knooppunttype openen, het volgende doen:

1. Voeg een nieuwe test toe aan de juiste load balancer.
   
    Als u uw cluster geïmplementeerd met behulp van de portal, de load balancers zijn met de naam "LB-naam van de Resource-group-NodeTypename", één voor elk knooppunttype. Aangezien de load balancer-namen uniek zijn binnen een resourcegroep, is het raadzaam als u deze onder een bepaalde resourcegroep zoeken.
   
    ![Schermafbeelding van een test toevoegen aan een load balancer in de portal.][AddingProbes]
2. Voeg een nieuwe regel toe aan de load balancer.
   
    Een nieuwe regel toevoegen aan dezelfde load balancer met behulp van de test die u in de vorige stap hebt gemaakt.
   
    ![Een nieuwe regel toevoegen aan een load balancer in de portal.][AddingLBRules]

### <a name="placement-properties"></a>Plaatsingseigenschappen
U kunt aangepaste plaatsingseigenschappen die u wilt gebruiken in uw toepassingen toevoegen voor elk van de knooppunttypen. NodeType is een standaardeigenschap die u gebruiken kunt zonder expliciet toe te voegen.

> [!NOTE]
> Raadpleeg voor meer informatie over het gebruik van plaatsingsbeperkingen eigenschappen van het knooppunt en het definiëren van deze naar de sectie 'Beperkingen en knooppunt plaatsingseigenschappen' in het Service Fabric Cluster Resource Manager-Document op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Capaciteit metrische gegevens
U kunt aangepaste metrische gegevens over capaciteit die u wilt gebruiken in uw toepassingen om te rapporteren over de belasting toevoegen voor elk van de knooppunttypen. Raadpleeg voor meer informatie over het gebruik van metrische gegevens over capaciteit om te rapporteren over de belasting op de Service Fabric Cluster Resource Manager-documenten op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md) en [metrische gegevens en belasting](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Instellingen voor fabric-upgrade - status beleidsregels
U kunt de gezondheid van aangepaste beleidsregels voor fabric-upgrade. Als u uw cluster hebt ingesteld op automatische infrastructuurupgrades, krijgen deze beleidsregels toegepast op de fase-1 van de automatische infrastructuurupgrades.
Als u hebt uw cluster voor handmatige fabric upgrades ingesteld, krijgen deze beleidsregels telkens wanneer u een nieuwe versie activeren van het systeem een vliegende start de fabric-upgrade in uw cluster toegepast. Als u het beleid niet overschrijven, worden de standaardwaarden gebruikt.

U kunt de aangepaste statusbeleid opgeven of de huidige instellingen op de blade 'fabric-upgrade' controleren door het selecteren van de geavanceerde instellingen van de upgrade. Bekijk de volgende afbeelding voor het. 

![Aangepaste statusbeleid beheren][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Fabric-instellingen voor uw cluster aanpassen
Raadpleeg [service fabric-cluster infrastructuurinstellingen](service-fabric-cluster-fabric-settings.md) op wat er en hoe u deze kunt aanpassen.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Patches voor het besturingssysteem op de virtuele machines die gezamenlijk het cluster
Raadpleeg [Patch Orchestration toepassing](service-fabric-patch-orchestration-application.md) die kunnen worden geïmplementeerd in uw cluster om patches te installeren via Windows Update op een gecoördineerde manier, te houden van de beschikbare services voortdurend. 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Upgrades voor het besturingssysteem op de virtuele machines die gezamenlijk het cluster
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
