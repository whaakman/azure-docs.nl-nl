---
title: Oracle-oplossingen op Azure virtual machines | Microsoft Docs
description: Meer informatie over ondersteunde configuraties en beperkingen van Oracle-installatiekopieën voor virtuele machines op Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: rogirdh
ms.custom: seodec18
ms.openlocfilehash: b62b35320ba1f4473e9b3a039d181d6a2fb58257
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743618"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Oracle-installatiekopieën voor virtuele machine en de implementatie ervan op Microsoft Azure

In dit artikel vindt u informatie over de Oracle-oplossingen op basis van installatiekopieën voor virtuele machines met Oracle op Azure Marketplace worden gepubliceerd. Als u geïnteresseerd in oplossingen voor cross-cloud-toepassingen met Oracle Cloud-infrastructuur bent, Zie [oplossingen voor Oracle-toepassingen integreren van Microsoft Azure en Oracle Cloud Infrastructure](oracle-oci-overview.md).

Als u een lijst met beschikbare installatiekopieën, voer de volgende opdracht:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

Vanaf mei 2019 zijn de volgende installatiekopieën beschikbaar:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Deze installatiekopieën worden beschouwd als 'Bring Your Own License' en als zodanig u enkel gefactureerd voor berekening, opslag en netwerken kosten in rekening gebracht door het uitvoeren van een virtuele machine.  Ervan wordt uitgegaan dat u goed een licentie voor het gebruik van Oracle-software en dat u beschikt over een huidige ondersteuningscontract bij Oracle. Oracle is mobiliteit van licenties gegarandeerd van on-premises naar Azure. Zie de gepubliceerde [Oracle en Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Opmerking voor meer informatie over mobiliteit van licenties. 

Personen kunnen ook voor kiezen op basis van hun oplossingen op een aangepaste installatiekopie ze volledig in Azure maken of uploaden van een aangepaste installatiekopie van hun on-premises-omgeving.

## <a name="support-for-jd-edwards"></a>Ondersteuning voor JD Edwards
Op basis van de Oracle-ondersteuning Opmerking [document-ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), JD Edwards EnterpriseOne versies 9.2 en hoger worden ondersteund op **openbare cloud aanbieding** die voldoet aan hun specifieke `Minimum Technical Requirements` (MTR).  U moet maken van aangepaste installatiekopieën die voldoen aan hun MTR de specificaties voor toepassingscompatibiliteit OS en software. 

## <a name="oracle-database-vm-images"></a>VM-installatiekopieën van Oracle-database
Oracle biedt actieve Oracle DB 12.1 Standard en Enterprise-edities ondersteuning in Azure op installatiekopieën voor virtuele machines op basis van Oracle Linux.  Voor de beste prestaties voor werkbelastingen voor productie van Oracle DB op Azure, moet correct het formaat van de VM-installatiekopie en beheerde schijven die worden ondersteund door Premium-opslag gebruiken. Voor instructies over hoe u snel aan de slag wilt u een Oracle-database actief en werkend in Azure met behulp van de Oracle VM-installatiekopie, gepubliceerd [probeert de Oracle DB-Quickstart-overzicht](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Gekoppelde schijf configuratie-opties

Gekoppelde schijven, is afhankelijk van de Azure Blob storage-service. Elke standard-schijf is geschikt voor een theoretisch maximum van ongeveer 500 invoer/uitvoer-bewerkingen per seconde (IOPS). Onze premium-aanbod voor schijf heeft de voorkeur voor databaseworkloads met hoge prestaties en kan maar liefst tot 5000 IOP's per schijf. U kunt één schijf als die voldoet aan uw prestatievereisten past. U kunt echter de effectieve IOPS-prestaties verbeteren als u meerdere gekoppelde schijven te gebruiken, een databasegegevens verdeeld over deze, en vervolgens met Oracle automatische Storage Management (ASM). Zie [automatische opslag van Oracle-overzicht](https://www.oracle.com/technetwork/database/index-100339.html) voor Oracle ASM specifieke informatie. Zie voor een voorbeeld van hoe u kunt installeren en configureren van Oracle Asm installeren op een virtuele Linux Azure-machine, de [installeren en configureren van Oracle geautomatiseerde Storage Management](configure-oracle-asm.md) zelfstudie.

## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle Real Application Cluster (Oracle RAC)
Oracle RAC is ontworpen voor het beperken van het uitvallen van één knooppunt in een cluster met meerdere knooppunten on-premises configuratie. Afhankelijk van twee on-premises technologieën die niet ingebouwd in grootschalige openbare cloudomgevingen zijn: gedeelde schijf en netwerk-multicast. Als uw databaseoplossing vereist dat Oracle RAC in Azure, moet u andere software van derden zodat deze technologieën. Zie voor meer informatie over Oracle RAC de [FlashGrid SkyCluster pagina](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Overwegingen voor hoge beschikbaarheid en noodherstel herstel
Als u Oracle-databases in Azure, bent u verantwoordelijk voor het implementeren van een hoge beschikbaarheid en noodherstel hersteloplossing om te voorkomen uitvaltijd. 

Hoge beschikbaarheid en herstel na noodgevallen voor Oracle Database Enterprise Edition (zonder afhankelijk te zijn Oracle RAC) kan worden bereikt over het gebruik van Azure [Data Guard, actief Data Guard](https://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), of [Oracle GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate), met twee databases op twee afzonderlijke virtuele machines. Beide virtuele machines moeten zich in dezelfde [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) om te controleren of ze elkaar via het particuliere IP-adres permanente toegang krijgen tot.  Bovendien is het raadzaam het plaatsen van de virtuele machines in dezelfde beschikbaarheidsset wilt dat Azure plaats deze in afzonderlijke foutdomeinen en upgradedomeinen. Moet u geo-redundantie, instellen van de twee databases om te repliceren tussen twee verschillende regio's en verbinding maken met de twee exemplaren met een VPN-Gateway.

De zelfstudie [implementeren Oracle Data Guard voor Azure](configure-oracle-dataguard.md) begeleidt u bij de procedure eenvoudige integratie op Azure.  

Met Oracle Data Guard, hoge beschikbaarheid kan worden bereikt met een primaire database in een virtuele machine, een secundaire (stand-by)-database in een andere virtuele machine, en één richting replicatie tussen beide instellen. Het resultaat is leestoegang tot de kopie van de database. U kunt met Oracle GoldenGate, bidirectionele replicatie tussen de twee databases configureren. Zie voor meer informatie over het instellen van een oplossing voor hoge beschikbaarheid voor uw databases met behulp van deze hulpprogramma's, [actief Data Guard](https://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) documentatie op de Oracle-website. Als u moet lezen-schrijven toegang naar de kopie van de database, kunt u [Oracle actief Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

De zelfstudie [Oracle GoldenGate implementeren op Azure](configure-oracle-golden-gate.md) begeleidt u bij de procedure eenvoudige integratie op Azure.

Naast een HA en DR-oplossing die zijn ontworpen in Azure, hebt u een back-upstrategie op locatie om uw database te herstellen. De zelfstudie [back-up en herstel een Oracle-Database](oracle-backup-recovery.md) begeleidt u bij de eenvoudige procedure voor het tot stand brengen van een consistente back-up.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Installatiekopieën van Oracle WebLogic Server-machines

* **Clustering wordt ondersteund op Enterprise-editie alleen.** U bent in licentie gegeven voor het gebruik van WebLogic clustering alleen bij gebruik van de Enterprise-editie van WebLogic Server. Gebruik geen clustering met WebLogic Server Standard Edition.
* **Multicast-UDP wordt niet ondersteund.** Azure biedt ondersteuning voor UDP-unicast-verzending, maar niet multicasting of broadcasting. WebLogic Server is vertrouwen op Azure UDP-unicast-mogelijkheden. Voor de beste resultaten vertrouwen op UDP-unicast, wordt u aangeraden dat de clustergrootte WebLogic statisch of behouden met niet meer dan 10 beheerde servers worden bewaard.
* **WebLogic Server wordt verwacht dat de openbare en particuliere poorten moet hetzelfde zijn voor T3-toegang (bijvoorbeeld bij het gebruik van Enterprise JavaBeans).** U hebt een meerlagige scenario waarin een service-laag (EJB)-toepassing wordt uitgevoerd op een WebLogic Server-cluster dat bestaat uit twee of meer virtuele machines, in een virtueel netwerk met de naam *SLWLS*. De clientlaag is in een ander subnet in hetzelfde virtuele netwerk, een eenvoudige Java-programma wilt EJB aanroepen in de servicelaag. Omdat het nodig zijn voor taakverdeling tussen de servicelaag is, moet een openbare load balancing-eindpunt worden gemaakt voor de virtuele machines in het cluster WebLogic Server. Als de particuliere poort die u opgeeft verschilt van de openbare poort (bijvoorbeeld 7006:7008), treedt er een fout, zoals het volgende op:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Dit komt doordat voor externe toegang T3, WebLogic Server wordt verwacht dat de load balancer-poort en de beheerde WebLogic server-poort moet dezelfde zijn. In het geval is voorgaande, de client toegang heeft tot poort 7006 (de load balancer-poort) en de beheerde server luistert op 7008 (de particuliere poort). Deze beperking is alleen van toepassing op T3 toegang, niet op HTTP.

   Om dit te voorkomen, gebruikt u een van de volgende tijdelijke oplossingen:

  * De dezelfde persoonlijke en openbare poortnummers gebruiken voor taakverdeling eindpunten toegewezen aan T3-toegang.
  * Bij het starten van WebLogic Server, zijn onder andere de volgende JVM-parameter:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Voor meer informatie, Zie KB-artikel **860340.1** op <https://support.oracle.com>.

* **Dynamische clustering en beperkingen voor taakverdeling.** Stel dat u wilt gebruiken een dynamische-cluster in WebLogic Server en deze beschikbaar te maken via een enkele, openbare load balancing-eindpunt in Azure. Dit kan worden gedaan als u een vaste poortnummer voor elk van de beheerde servers (niet dynamisch toegewezen uit een bereik) en meer beheerde servers dan er machines die is het bijhouden van de beheerder niet worden gestart. Dat wil zeggen, is er niet meer dan één beheerde server per virtuele machine). Als uw configuratie in meer WebLogic servers wordt gestart resulteert dan er virtuele machines (dat wil zeggen, waarbij meerdere exemplaren van WebLogic Server delen dezelfde virtuele machine), dan is het niet mogelijk voor meer dan één van deze exemplaren van WebLogic-servers verbinding maken met een specifiek poortnummer. Anderen op deze virtuele machine mislukt.

   Als u de beheerserver om toe te wijzen automatisch unieke poortnummers op de beheerde servers configureert, klikt u vervolgens is de taakverdeling niet mogelijk omdat Azure biedt geen ondersteuning voor toewijzing van een enkele openbare poort naar meerdere particuliere poorten, zoals is vereist voor deze de configuratie.
* **Meerdere exemplaren van WebLogic Server op een virtuele machine.** Afhankelijk van de vereisten van uw implementatie, kunt u overwegen meerdere exemplaren van WebLogic Server op dezelfde virtuele machine, uitgevoerd als de virtuele machine groot genoeg is. Op een middelgrote virtuele machine, waarin twee cores, kunt u bijvoorbeeld twee exemplaren van WebLogic Server uitvoeren. We raden echter nog steeds aan dat u Vermijd de introductie van single point of failure in uw architectuur, het geval zijn zou als u slechts één virtuele machine met meerdere exemplaren van WebLogic Server gebruikt. Met behulp van ten minste twee virtuele machines wordt mogelijk een betere benadering en elke virtuele machine kan vervolgens meerdere exemplaren van WebLogic Server uitvoeren. Elk exemplaar van WebLogic Server kan nog steeds deel uitmaken van hetzelfde cluster. Echter, het is momenteel niet mogelijk om Azure te gebruiken voor taakverdeling eindpunten die beschikbaar worden gesteld door dergelijke implementaties WebLogic Server binnen dezelfde virtuele machine, omdat Azure load balancer is vereist voor de servers met load balancing worden verdeeld over de unieke virtuele machines.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle-installatiekopieën voor virtuele machines van JDK
* **JDK 6 en 7 van de meest recente updates.** Terwijl het wordt aangeraden om de meest recente openbare, ondersteunde versie van Java (momenteel Java 8), maakt Azure ook JDK 6 en 7 installatiekopieën beschikbaar. Dit is bedoeld voor oudere toepassingen die zijn nog niet klaar om te worden bijgewerkt naar JDK 8. Terwijl updates van vorige JDK installatiekopieën zijn mogelijk niet langer beschikbaar voor het algemene publiek, de Microsoft-partner met Oracle, krijgt zijn de JDK 6 en 7 installatiekopieën die worden verstrekt door Azure bedoeld om een meer recente niet-openbare-update die normaal gesproken worden aangeboden door Oracle bevatten alleen een selecte groep van Oracle de klanten ondersteund. Nieuwe versies van de JDK-installatiekopieën worden na verloop van tijd met bijgewerkte versies van JDK 6 en 7 beschikbaar gesteld.

   De JDK die beschikbaar zijn in de JDK 6 en 7-installatiekopieën, en de virtuele machines en installatiekopieën die zijn afgeleid van deze kan alleen worden gebruikt in Azure.
* **64-bits JDK.** Het Oracle WebLogic Server-installatiekopieën voor virtuele machines en de VM-installatiekopieën van Oracle JDK verstrekt door Azure bevatten de 64-bits versies van Windows Server en de JDK.

## <a name="next-steps"></a>Volgende stappen
U hebt nu een overzicht van de huidige Oracle-oplossingen op basis van installatiekopieën voor virtuele machines in Microsoft Azure. De volgende stap is het implementeren van uw eerste Oracle-database in Azure.

> [!div class="nextstepaction"]
> [Een Oracle-database in Azure maken](oracle-database-quick-create.md)
