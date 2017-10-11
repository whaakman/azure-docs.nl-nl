---
title: Upgrade van een Azure Service Fabric-cluster | Microsoft Docs
description: Upgrade van de Service Fabric-code en/of de configuratie die wordt uitgevoerd van een Service Fabric-cluster, inclusief het instellen van de cluster-updatemodus, certificaten, toe te voegen toepassing poorten, OS patches doen upgraden enzovoort. Wat kunt u verwachten wanneer de upgrades worden uitgevoerd?
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 7ea71ab891583c51b3c07a4d0a9f0b4f54e56669
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgrade van een Azure Service Fabric-cluster
> [!div class="op_single_selector"]
> * [Azure-Cluster](service-fabric-cluster-upgrade.md)
> * [Zelfstandige Cluster](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Ontwerpen voor kunnen is essentieel bij het bereiken van succes op lange termijn van uw product voor elk moderne systeem. Een Azure Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Dit artikel wordt beschreven wat automatisch wordt beheerd en wat u kunt zelf configureren.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op uw Cluster beheren
U kunt instellen dat uw cluster automatische fabric-upgrades ontvangen als ze door Microsoft worden uitgegeven of kunt u een ondersteunde fabric-versie die u wilt dat uw cluster op.

Dit doet u door het instellen van de clusterconfiguratie 'upgradeMode' op de portal of met behulp van Resource Manager op het moment van worden gemaakt of later op een live cluster 

> [!NOTE]
> Zorg ervoor dat het cluster een ondersteunde fabric-versie altijd uitgevoerd. Als en dat we de release van een nieuwe versie van het service fabric aankondigen, wordt de vorige versie gemarkeerd voor einde van ondersteuning na een minimum van 60 dagen na die datum. de nieuwe releases worden aangekondigd [op het service fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar om te kiezen klikt. 
> 
> 

14 dagen vóór het verstrijken van de release van die het cluster wordt uitgevoerd, een statusgebeurtenis gegenereerd die het cluster in een waarschuwingsstatus wordt geplaatst. Het cluster blijft in een waarschuwingsstatus totdat u een naar een ondersteunde fabric-versie upgrade.

### <a name="setting-the-upgrade-mode-via-portal"></a>De upgrademodus via de portal instellen
Wanneer u het cluster maakt, kunt u het cluster instellen op automatisch of handmatig.

![Create_Manualmode][Create_Manualmode]

U kunt het cluster instellen op automatisch of handmatig moet worden gebruikt vanaf een live cluster met behulp van de ervaring beheren. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Upgraden naar een nieuwe versie op een cluster dat is ingesteld op de handmatige modus via de portal.
Als u wilt upgraden naar een nieuwe versie, is hoeft u de beschikbare versie uit de vervolgkeuzelijst selecteert en slaat u. De Fabric-upgrade wordt automatisch gestart. De cluster-statusbeleid (een combinatie van het knooppunt status en de status alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als het statusbeleid cluster niet wordt voldaan, is de upgrade teruggedraaid. Schuif omlaag in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Als u de problemen dat heeft geresulteerd in de terugdraaiactie hebt opgelost, moet u de upgrade opnieuw door de dezelfde stappen als voordat initiëren.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>De upgrademodus via Resource Manager-sjabloon instellen
De configuratie 'upgradeMode' toevoegen aan de resourcedefinitie Microsoft.ServiceFabric/clusters en de 'clusterCodeVersion' ingesteld op een van de ondersteunde fabric-versies zoals hieronder wordt weergegeven en vervolgens de sjabloon te implementeren. De geldige waarden voor 'upgradeMode' zijn 'Handmatig' of 'Automatisch'

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Upgraden naar een nieuwe versie op een cluster dat is ingesteld op de handmatige modus via Resource Manager-sjabloon.
Wanneer het cluster in de handmatige modus is, om te upgraden naar een nieuwe versie, de 'clusterCodeVersion' wijzigen naar een ondersteunde versie en deze implementeren. De implementatie van de sjabloon, gang van de Fabric-upgrade opgehaald gestarte automatisch. De cluster-statusbeleid (een combinatie van het knooppunt status en de status alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als het statusbeleid cluster niet wordt voldaan, is de upgrade teruggedraaid. Schuif omlaag in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Als u de problemen dat heeft geresulteerd in de terugdraaiactie hebt opgelost, moet u de upgrade opnieuw door de dezelfde stappen als voordat initiëren.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Lijst met alle beschikbare versie ophalen voor alle omgevingen voor een bepaald abonnement
Voer de volgende opdracht en krijgt u uitvoer ongeveer als volgt.

'supportExpiryUtc' vertelt uw wanneer een bepaalde versie is verlopen of is verlopen. De meest recente versie is geen geldige datum - deze heeft de waarde ' 9999-12-31T23:59:59.9999999 ', wat betekent dat de vervaldatum is nog niet ingesteld.

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

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Fabric upgrade gedrag wanneer het cluster de modus bijwerken automatisch is
Microsoft onderhoudt op de fabric-code en de configuratie die in een Azure-cluster wordt uitgevoerd. We uitvoeren automatische gecontroleerde upgrades naar de software op een als dat nodig is. Deze upgrades mogelijk code en/of de configuratie. Om ervoor te zorgen dat uw toepassing te lijden heeft onder geen gevolgen of minimale gevolgen als gevolg van deze upgrades, voeren we de upgrades in de volgende fasen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: Een upgrade wordt uitgevoerd met behulp van alle cluster statusbeleid
Tijdens deze fase wordt de upgrades één upgradedomein tegelijk gaan en de toepassingen die worden uitgevoerd in het cluster worden uitgevoerd zonder uitvaltijd. De cluster-statusbeleid (een combinatie van het knooppunt status en de status alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als het statusbeleid cluster niet wordt voldaan, is de upgrade teruggedraaid. Vervolgens wordt een e-mailbericht verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

* Melding dat we hadden terugdraaien van de upgrade van een cluster.
* Voorgestelde corrigerende acties, indien van toepassing.
* Het aantal dagen (n) totdat we fase 2 uitvoeren.

We proberen uit te voeren dezelfde upgrade een paar keer geval eventuele upgrades is mislukt door redenen infrastructuur. Na de n dagen vanaf de datum waarop die het e-mailbericht is verzonden, gaan we voor fase 2.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade beschouwd als geslaagd en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering. Dit is om te voorkomen dat u te veel e-mails--ontvangt een e-mailbericht moet worden gezien als een uitzondering in normale tekst verzonden. We verwachten dat de meeste van de cluster-upgrades kan slagen zonder die invloed hebben op de beschikbaarheid van uw toepassing.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: Een upgrade wordt uitgevoerd met behulp van standaard statusbeleid alleen
Het statusbeleid in deze fase worden ingesteld in zodanig dat het aantal toepassingen die aan het begin van de upgrade in orde zijn hetzelfde voor de duur van het upgradeproces blijft. Als fase 1 in de fase 2-upgrades één upgradedomein tegelijk gaan en de toepassingen die worden uitgevoerd in het cluster worden uitgevoerd zonder uitvaltijd. De cluster-statusbeleid (een combinatie van het knooppunt status en de status alle toepassingen die worden uitgevoerd in het cluster) aan voor de duur van de upgrade worden gehouden.

Als de cluster-statusbeleid wordt van kracht niet voldaan, is de upgrade teruggedraaid. Vervolgens wordt een e-mailbericht verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

* Melding dat we hadden terugdraaien van de upgrade van een cluster.
* Voorgestelde corrigerende acties, indien van toepassing.
* Het aantal dagen (n) totdat we fase 3 uitvoeren.

We proberen uit te voeren dezelfde upgrade een paar keer geval eventuele upgrades is mislukt door redenen infrastructuur. Een herinnering is verzonden een paar dagen voordat n dagen actief zijn. Na de n dagen vanaf de datum waarop die het e-mailbericht is verzonden, verder we met stap 3. De e-mailberichten we u in de fase 2 sturen ernstig moeten worden gehaald en corrigerende acties moeten worden genomen.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade beschouwd als geslaagd en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: Een upgrade wordt uitgevoerd met behulp van agressieve statusbeleid
Deze statusbeleid in deze fase zijn gericht op de voltooiing van de upgrade in plaats van de status van de toepassingen. In deze fase eindigen heel weinig cluster-upgrades. Als uw cluster in deze fase wordt, is er een goede kans dat uw toepassing slecht functioneert en/of beschikbaarheid verliezen.

Net als bij de andere twee fasen, upgrades voor fase 3 voortgezet één upgradedomein tegelijk.

Als het statusbeleid cluster niet wordt voldaan, is de upgrade teruggedraaid. We proberen uit te voeren dezelfde upgrade een paar keer geval eventuele upgrades is mislukt door redenen infrastructuur. Het cluster is hierna is vastgemaakt, zodat deze niet langer ondersteuning en/of upgrades ontvangt.

Een e-mailbericht met deze informatie wordt verzonden naar de eigenaar van het abonnement, samen met de corrigerende acties. We verwachten niet alle clusters om op te halen in een status waarbij de fase 3 is mislukt.

Als de cluster-statusbeleid wordt voldaan, wordt de upgrade beschouwd als geslaagd en als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen e-mailbevestiging van een geslaagde uitvoering.

## <a name="cluster-configurations-that-you-control"></a>Clusterconfiguraties die u beheert
Naast de mogelijkheid om het cluster upgrademodus, hier zijn de configuraties die u in een live cluster wijzigen kunt.

### <a name="certificates"></a>Certificaten
U kunt nieuwe toevoegen of verwijderen van certificaten voor het cluster en de client via de portal eenvoudig. Raadpleeg [dit document voor gedetailleerde instructies](service-fabric-cluster-security-update-certs-azure.md)

![Schermafbeelding van certificaatvingerafdrukken in de Azure portal.][CertificateUpgrade]

### <a name="application-ports"></a>Poorten van de toepassing
U kunt poorten van de toepassing wijzigen door de Load Balancer resource-eigenschappen die gekoppeld aan het knooppunttype zijn te wijzigen. U kunt de portal gebruiken of kunt u rechtstreeks Resource Manager PowerShell gebruiken.

U opent een nieuwe poort op alle virtuele machines in een knooppunttype door het volgende doen:

1. Een nieuwe test toevoegen aan de geschikte load balancer.
   
    Als u uw cluster hebt geïmplementeerd met behulp van de portal, de load balancers zijn met de naam 'LB-naam van de Resource-group-NodeTypename', één voor elk knooppunttype. Omdat de load balancer-namen uniek alleen binnen een resourcegroep zijn, wordt het aanbevolen als u naar deze onder een bepaalde resourcegroep zoekt.
   
    ![Schermafbeelding van een test toe te voegen aan een load balancer in de portal.][AddingProbes]
2. Een nieuwe regel toevoegen aan de load balancer.
   
    Een nieuwe regel toevoegen aan dezelfde load balancer met behulp van de test op die u in de vorige stap hebt gemaakt.
   
    ![Een nieuwe regel toe te voegen aan een load balancer in de portal.][AddingLBRules]

### <a name="placement-properties"></a>Plaatsingseigenschappen
U kunt aangepaste plaatsingseigenschappen die u wilt gebruiken in uw toepassingen toevoegen voor elk van de knooppunttypen. NodeType is een standaardeigenschap die u gebruiken kunt zonder deze expliciet toe te voegen.

> [!NOTE]
> Raadpleeg voor meer informatie over het gebruik van plaatsingsbeperkingen, knooppunteigenschappen en hoe ze kunnen worden gedefinieerd aan de sectie 'Plaatsing beperkingen en eigenschappen van knooppunt' in het Service Fabric-Cluster Resource Manager-Document op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Capaciteit metrische gegevens
U kunt voor elk van de knooppunttypen aangepaste capaciteit metrische gegevens die u wilt gebruiken in uw toepassingen en rapporteren over de belasting toevoegen. Raadpleeg voor meer informatie over het gebruik van metrische gegevens voor capaciteit om te rapporteren over de belasting op de Service Fabric-Cluster Resource Manager-documenten op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md) en [metrische gegevens en de belasting](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Instellingen voor fabric-upgrade - Health-beleid
U kunt opgeven om de gezondheid van aangepaste beleidsregels voor fabric-upgrade. Als u uw cluster automatische fabric-upgrades hebt ingesteld, krijgen deze beleidsregels toegepast op de fase-1 van de automatische fabric-upgrades.
Als u hebt uw cluster voor handmatige fabric upgrades ingesteld, krijgen deze beleidsregels telkens wanneer die u een nieuwe versie activatie van het systeem ere van de fabric-upgrade in uw cluster selecteert toegepast. Als u het beleid niet overschrijven, worden de standaardwaarden gebruikt.

U kunt de aangepaste statusbeleid opgeven of de huidige instellingen onder de blade 'fabric-upgrade' bekijken door het selecteren van de geavanceerde instellingen voor de upgrade. Bekijk de volgende afbeelding voor het. 

![Aangepaste statusbeleid beheren][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Fabric-instellingen voor uw cluster aanpassen
Raadpleeg [service fabric-clusterinstellingen fabric](service-fabric-cluster-fabric-settings.md) op wat en hoe u ze kunt aanpassen.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS patches op de virtuele machines die gezamenlijk van het cluster
Raadpleeg [Patch Orchestration toepassing](service-fabric-patch-orchestration-application.md) waarop kan worden geïmplementeerd op het cluster om patches te installeren via Windows Update op een geregiseerde manier, de tijd om de beschikbare services te houden. 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Upgrades voor het besturingssysteem op de virtuele machines die gezamenlijk van het cluster
Als u de installatiekopie van het besturingssysteem op de virtuele machines van het cluster upgraden moet, moet u dit één VM doen op een tijdstip. U bent zelf verantwoordelijk voor deze upgrade--er is momenteel geen automation voor deze.

## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van enkele van de [service fabric-clusterinstellingen fabric](service-fabric-cluster-fabric-settings.md)
* Meer informatie over hoe u [in en uit het cluster schalen](service-fabric-cluster-scale-up-down.md)
* Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
