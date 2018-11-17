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
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: 3e71199c19fffae0bb7dfa6a59245f1d43cd5065
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51855124"
---
# <a name="upgrade-the-service-fabric-version-of-a-cluster"></a>De Service Fabric-versie van een cluster upgraden

Voor elk modern systeem is ontwerpen voor kunnen erg belangrijk voor het succes van uw product op lange termijn te bereiken. Een Azure Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. In dit artikel wordt beschreven hoe u de versie van Service Fabric die worden uitgevoerd in uw Azure-cluster een upgrade uitvoert.

U kunt instellen dat uw cluster voor het ontvangen van automatische infrastructuurupgrades als ze door Microsoft worden uitgegeven of kunt u een ondersteunde fabric-versie die u wilt dat uw cluster op.

U doen dit door het instellen van de clusterconfiguratie 'upgradeMode' in de portal of met behulp van Resource Manager op het moment van maken of hoger op een live cluster 

> [!NOTE]
> Zorg ervoor dat u uw cluster met een ondersteunde fabric-versie altijd behouden. Als en wanneer we kondigen wij de release van een nieuwe versie van service fabric, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf die datum is gemarkeerd voor einde van ondersteuning. De nieuwe releases worden aangekondigd [op de blog van het service fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar voor vervolgens kiest. 
> 
> 

14 dagen voordat het verstrijken van de release van die het cluster wordt uitgevoerd, wordt een statusgebeurtenis gegenereerd waarmee uw cluster in een waarschuwingsstatus wordt geplaatst zijn. Het cluster blijft een waarschuwingsstatus totdat u een upgrade naar een ondersteunde fabric-versie uitvoert.

## <a name="set-the-upgrade-mode-in-the-azure-portal"></a>De upgrademodus instellen in Azure portal
U kunt het cluster instellen op automatisch of handmatig wanneer u het cluster maakt.

![Create_Manualmode][Create_Manualmode]

U kunt het cluster instellen op automatisch of handmatig wanneer u op een live cluster met behulp van de ervaring beheren. 

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Een upgrade uitvoeren naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via de portal.
Als u wilt upgraden naar een nieuwe versie, is hoeft u alleen maar selecteert u de versie in de vervolgkeuzelijst en opslaan. De Fabric-upgrade wordt automatisch gestart. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid. Schuif omlaag in dit document voor meer informatie over het instellen van deze aangepaste statusbeleid. 

Als u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, moet u de upgrade opnieuw starten door dezelfde stappen als voordat te volgen.

![Manage_Automaticmode][Manage_Automaticmode]

## <a name="set-the-upgrade-mode-using-a-resource-manager-template"></a>De upgrade-modus met behulp van Resource Manager-sjabloon instellen
De configuratie 'upgradeMode' toevoegen aan de resourcedefinitie Microsoft.ServiceFabric/clusters en de 'clusterCodeVersion' ingesteld op een van de ondersteunde fabric-versies zoals hieronder wordt weergegeven en vervolgens de sjabloon te implementeren. De geldige waarden voor 'upgradeMode' zijn 'Handmatig' of 'Automatisch'

![ARMUpgradeMode][ARMUpgradeMode]

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Een upgrade uitvoeren naar een nieuwe versie op een cluster dat is ingesteld op handmatige modus via Resource Manager-sjabloon.
Als het cluster in de modus voor handmatige is, om te upgraden naar een nieuwe versie, de 'clusterCodeVersion' wijzigen naar een ondersteunde versie en deze implementeren. De implementatie van de sjabloon, gang van de Fabric-upgrade wordt gestart automatisch. De cluster-statusbeleid (een combinatie van het knooppuntstatus en de status van alle toepassingen die worden uitgevoerd in het cluster) om te worden gehouden tijdens de upgrade.

Als de cluster-statusbeleid wordt niet voldaan, is de upgrade teruggedraaid.  

Als u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, moet u de upgrade opnieuw starten door dezelfde stappen als voordat te volgen.

## <a name="set-custom-health-polices-for-upgrades"></a>Aangepaste status instellen beleidsregels voor upgrades van de
U kunt de gezondheid van aangepaste beleidsregels voor fabric-upgrade. Als u uw cluster hebt ingesteld op automatische infrastructuurupgrades, wordt deze beleidsregels worden toegepast op de [fase 1 van de automatische infrastructuurupgrades](service-fabric-cluster-upgrade.md#fabric-upgrade-behavior-during-automatic-upgrades).
Als u hebt uw cluster voor handmatige fabric upgrades ingesteld, krijgen deze beleidsregels telkens wanneer u een nieuwe versie activeren van het systeem een vliegende start de fabric-upgrade in uw cluster toegepast. Als u het beleid niet overschrijven, worden de standaardwaarden gebruikt.

U kunt de aangepaste statusbeleid opgeven of de huidige instellingen op de blade 'fabric-upgrade' controleren door het selecteren van de geavanceerde instellingen van de upgrade. Bekijk de volgende afbeelding voor het. 

![Aangepaste statusbeleid beheren][HealthPolices]

## <a name="list-all-available-versions-for-all-environments-for-a-given-subscription"></a>Lijst met alle beschikbare versies voor alle omgevingen voor een bepaald abonnement
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
