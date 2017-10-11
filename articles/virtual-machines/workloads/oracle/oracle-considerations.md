---
title: Oracle-oplossingen in Microsoft Azure | Microsoft Docs
description: Meer informatie over ondersteunde configuraties en beperkingen van Oracle-oplossingen in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: rclaus
ms.openlocfilehash: 9174f7c8d16ff311312980fbe4d35996ec7ac832
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle-oplossingen en hun implementatie in Microsoft Azure
Dit artikel behandelt benodigde informatie om te met succes is implementeren verschillende Oracle-oplossingen in Microsoft Azure. Deze oplossingen zijn gebaseerd op de virtuele Machine installatiekopieën die zijn gepubliceerd door Oracle in Azure Marketplace. Als u een lijst met beschikbare installatiekopieën, voer de volgende opdracht:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
Vanaf 6-16-2017 de lijst met afbeeldingen zijn als volgt:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Deze installatiekopieën worden beschouwd als 'Bring Your Own License' en als zodanig u alleen gefactureerd voor berekening, opslag en netwerken kosten die zijn gemaakt door het uitvoeren van een virtuele machine.  U gebruiksrecht voor Oracle-software en dat u een huidige support-overeenkomst met Oracle hebt, wordt ervan uitgegaan. Oracle heeft licentiemobiliteit gegarandeerd on-premises naar Azure. Zie de gepubliceerde [Oracle en Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Opmerking voor meer informatie over licentiemobiliteit. 

Personen kunnen ook voor kiezen hun oplossingen baseren op aangepaste installatiekopieën ze maken in Azure of een aangepaste installatiekopieën van hun on-premises-omgevingen te uploaden.

## <a name="support-for-jd-edwards"></a>Ondersteuning voor JD Edwards
Volgens de ondersteuning van Oracle-Opmerking [Doc-ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , JD Edwards EnterpriseOne versies 9.2 en hoger worden ondersteund op **openbare cloud aanbieding** die voldoet aan hun specifieke `Minimum Technical Requirements` (MTR).  U moet maken van aangepaste installatiekopieën die voldoen aan hun MTR specificaties voor compatibiliteit van besturingssystemen en software-toepassing. 

## <a name="oracle-database-virtual-machine-images"></a>Installatiekopieën van virtuele machines voor Oracle-Database
Oracle biedt ondersteuning voor actieve Oracle DB 12.1 Standard en Enterprise-edities in Azure op installatiekopieën van virtuele machines op basis van Oracle Linux.  Voor de beste prestaties voor productieworkloads van Oracle-database in Azure, moet u goed formaat van een installatiekopie van de virtuele machine en beheerd schijven die worden ondersteund door de Premium-opslag gebruiken. Voor instructies over het snel ophalen van een Oracle-database actief in Azure met behulp van de Oracle VM-installatiekopie gepubliceerde [probeert de Oracle DB Quick Start-overzicht](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Gekoppelde schijf configuratieopties

Gekoppelde schijven zijn afhankelijk van de Azure Blob storage-service. Elke schijf die standaard is geschikt voor een theoretisch maximumaantal ongeveer 500 i/o-bewerkingen per seconde (IOPS). Onze premium schijf aanbieding voorkeur geniet voor database werklasten met hoge prestaties en maximaal 5000 IOP's per schijf kunt bereiken. Terwijl u één schijf kunt als die voldoet aan de prestatiebehoeften van uw: u de effectieve IOPS-prestaties verbeteren kunt als u meerdere gekoppelde schijven, databasegegevens verdeeld over deze en gebruik vervolgens Oracle automatische Storage Management (ASM). Zie [automatische opslag van Oracle-overzicht](http://www.oracle.com/technetwork/database/index-100339.html) voor Oracle ASM specifieke informatie. Voor een voorbeeld van het installeren en configureren van Oracle ASM op een Linux Azure VM - u kunt proberen de [installeren en configureren van Oracle geautomatiseerde Storage Management](configure-oracle-asm.md) zelfstudie.

### <a name="oracle-realtime-application-cluster-rac"></a>Oracle Realtime toepassingscluster (RAC)
Oracle RAC is ontworpen voor het beperken van het uitvallen van één knooppunt in een cluster met meerdere knooppunten on-premises configuratie.  Is afhankelijk van de twee technologieën van lokale die niet systeemeigen naar openbare cloudomgevingen hyperschaal: gedeelde schijf en netwerk multicast. Er zijn door andere bedrijven gemaakt voor oplossingen van andere leveranciers [zoals FlashGrid](https://www.flashgrid.io/oracle-rac-in-azure/) die deze technologieën als u wilt implementeren, Oracle RAC in Azure worden geëmuleerd. 

### <a name="high-availability-and-disaster-recovery-considerations"></a>Overwegingen voor hoge beschikbaarheid en noodherstel herstel
Wanneer u de Oracle-Databases in Azure, bent u verantwoordelijk voor het implementeren van een hoge beschikbaarheid en noodherstel hersteloplossing om te voorkomen dat de uitvaltijd. 

Hoge beschikbaarheid en herstel na noodgevallen voor Oracle Database Enterprise Edition (zonder RAC) op Azure kan worden bereikt met [Data Guard, actieve Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), of [Oracle Golden Gate](http://www.oracle.com/technetwork/middleware/goldengate), met twee databases in twee afzonderlijke virtuele machines. Beide virtuele machines moeten deel uitmaken van dezelfde [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) zodat ze toegang tot elkaar via het persoonlijke permanente IP-adres.  Bovendien is het raadzaam als u de virtuele machines in dezelfde beschikbaarheidsset wilt dat Azure plaats deze in domeinen met afzonderlijke fouten en upgradedomeinen.  U kunt deze twee databases repliceren tussen twee verschillende regio's en de twee exemplaren verbinden met een VPN-Gateway als u wilt dat deze geografische redundantie - hebben.

We hebben een zelfstudie '[implementeren Oracle DataGuard op Azure](configure-oracle-dataguard.md)' die wordt u begeleid bij de procedure basisinstellingen evaluatieversie dit op Azure.  

Met Oracle Data Guard hoge beschikbaarheid kan worden verkregen met een primaire database in één virtuele machine, een secundaire (stand-by) database in een andere virtuele machine en een replicatieverbinding tussen deze twee instellen. Het resultaat is leestoegang tot de kopie van de database. U kunt replicatie in twee richtingen tussen de twee databases configureren met de Oracle-GoldenGate. Zie voor meer informatie over het instellen van een oplossing voor hoge beschikbaarheid voor uw databases met behulp van deze hulpprogramma's, [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) documentatie op de Oracle-website. Als u moet lezen-schrijven toegang op de kopie van de database, kunt u [Oracle Active Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

We hebben een zelfstudie '[implementeren Oracle GoldenGate op Azure](configure-oracle-golden-gate.md)' die wordt u begeleid bij de procedure basic seup evaluatieversie dit op Azure.

Ondanks dat een oplossing voor HA en Noodherstel ontworpen in Azure, zult u om te controleren of u beschikken over een back-upstrategie voor uw database terugzetten.  We hebben een zelfstudie [back-up en herstel een Oracle-Database](oracle-backup-recovery.md) die u helpt bij de eenvoudige procedure voor het tot stand brengen van een consistant back-up.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Installatiekopieën van virtuele machines voor Oracle WebLogic Server
* **Clustering wordt ondersteund op Enterprise-editie alleen.** U mag u WebLogic alleen bij gebruik van de Enterprise Edition van WebLogic Server clustering. Gebruik geen clustering met WebLogic Server Standard Edition.
* **Multicast-UDP wordt niet ondersteund.** Azure ondersteunt unicasting UDP, maar niet multicasting of broadcasting. WebLogic Server kan afhankelijk zijn van Azure UDP-unicast-mogelijkheden. Voor de beste resultaten vertrouwen op UDP-unicast, wordt aangeraden de clustergrootte WebLogic bewaardagen statisch, of met niet meer dan 10 beheerde servers die zijn opgenomen in het cluster worden bewaard.
* **WebLogic Server verwacht openbare en particuliere poort moet hetzelfde zijn voor toegang tot T3 (bijvoorbeeld wanneer u Enterprise JavaBeans).** Neem bijvoorbeeld een meerlaagse scenario waarbij een servicetoepassing laag (EJB) wordt uitgevoerd op een WebLogic Server-cluster dat bestaat uit twee of meer virtuele machines, in een vNet met de naam **SLWLS**. De clientlaag is in een ander subnet in hetzelfde vNet met een eenvoudige Java-programma probeert aan te roepen EJB in de servicelaag. Omdat het nodig zijn voor de taakverdeling van de servicelaag, moet een openbaar eindpunt voor taakverdeling voor de virtuele Machines in het servercluster WebLogic worden gemaakt. Als de particuliere poort die u opgeeft verschilt van de openbare poort (bijvoorbeeld 7006:7008), wordt een foutbericht zoals het volgende weergegeven:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Dit komt doordat voor externe toegang T3, WebLogic Server verwacht dat de load balancer-poort en de poort van de server beheerd WebLogic hetzelfde te zijn. In het bovenstaande geval is, de client toegang heeft tot poort 7006 (de load balancer-poort) en de beheerde server luistert op 7008 (de particuliere poort). Deze beperking geldt alleen voor T3 access, niet HTTP.

   Om dit te voorkomen, moet u een van de volgende manieren gebruiken:

  * Gebruik dezelfde persoonlijke en openbare poortnummers voor taakverdeling eindpunten die zijn toegewezen aan T3-toegang.
  * De volgende JVM-parameter opnemen wanneer WebLogic Server wordt gestart:

         -Dweblogic.rjvm.enableprotocolswitch=true

Voor meer informatie, Zie KB-artikel **860340.1** op <http://support.oracle.com>.

* **Dynamische clustering en beperkingen voor taakverdeling.** Stel dat u wilt een dynamische cluster in WebLogic Server gebruiken en deze weer te geven via een enkele, openbare taakverdeling eindpunt in Azure. Dit kan gebeuren als u gebruik van een vaste poortnummer voor elk van de beheerde servers (niet dynamisch toegewezen vanuit een bereik) en meer beheerde servers, dan is het bijhouden van de beheerder van de machines niet worden gestart (dat wil zeggen, niet meer dan een beheerde server per virtuele m achine). Als uw configuratie in meer WebLogic servers worden gestart resulteert dan virtuele machines (dat wil zeggen, waarbij meerdere exemplaren van WebLogic Server delen dezelfde virtuele machine), dan is het niet mogelijk meer dan één van deze exemplaren van WebLogic Server servers verbinding maken met een specifiek poortnummer – anderen op deze virtuele machine mislukt.

   Anderzijds, als u de beheerserver om automatisch te wijzen unieke poortnummers op de beheerde servers configureert, is vervolgens taakverdeling niet mogelijk omdat Azure biedt geen ondersteuning voor toewijzing van een enkele openbare poort tot meerdere particuliere poorten, zoals is vereist voor deze configuratie.
* **Meerdere exemplaren van Weblogic Server op een virtuele machine.** Afhankelijk van de vereisten van uw implementatie kunt u de optie van meerdere exemplaren van WebLogic Server worden uitgevoerd op dezelfde virtuele machine als de virtuele machine groot genoeg is. Op een virtuele machine van gemiddelde grootte, die twee cores bevat, kunt u bijvoorbeeld twee exemplaren van WebLogic Server uitvoeren. Houd er echter rekening mee dat u dat nog steeds het is raadzaam dat u introductie van individuele foutpunten in uw architectuur vermijden, het geval zijn zou als u slechts één virtuele machine met meerdere exemplaren van WebLogic Server gebruikt. Gebruik van ten minste twee virtuele machines kan een betere benadering, en elk van deze virtuele machines kan vervolgens meerdere exemplaren van WebLogic Server uitvoeren. Elk van deze exemplaren van WebLogic Server kan nog steeds deel uitmaken van hetzelfde cluster. Opmerking, maar het is momenteel niet mogelijk voor Azure te verdelen eindpunten die beschikbaar worden gesteld door dergelijke implementaties WebLogic Server binnen dezelfde virtuele machine, omdat Azure load balancer de servers met Netwerktaakverdeling moeten moet worden verdeeld over unieke virtuele machines.

## <a name="oracle-jdk-virtual-machine-images"></a>Installatiekopieën van virtuele machines JDK Oracle
* **JDK 6 en 7 van de meest recente updates.** Terwijl het is raadzaam om de meest recente openbare, ondersteunde versie van Java (momenteel Java 8), wordt Azure ook JDK 6 en 7 installatiekopieën beschikbaar. Dit is bedoeld voor oudere toepassingen die nog niet klaar om te worden bijgewerkt naar JDK 8. Tijdens updates van installatiekopieën van het vorige JDK is mogelijk niet meer beschikbaar zijn voor het algemene publiek, de Microsoft verbinding met Oracle, krijgt zijn de JDK 6 en 7 installatiekopieën die zijn verstrekt door Azure bedoeld om een meer recente update voor niet-openbaar die normaal gesproken worden aangeboden door Oracle bevatten een selecte groep Oracle de klanten ondersteund. Nieuwe versies van de installatiekopieën JDK wordt gedurende een periode met bijgewerkte versies van JDK 6 en 7 beschikbaar gesteld.

   De JDK die beschikbaar zijn in deze JDK 6 en 7 installatiekopieën en de virtuele machines en installatiekopieën die zijn afgeleid, kan alleen worden gebruikt in Azure.
* **64-bits JDK.** Bevatten de 64-bits versies van Windows Server- en de JDK die de installatiekopieën van de virtuele machine Oracle WebLogic Server en installatiekopieën van virtuele machines Oracle JDK verstrekt door Azure.

## <a name="next-steps"></a>Volgende stappen
U hebt nu een overzicht van de huidige Oracle-oplossingen in Microsoft Azure. De volgende stap is om te gaan en implementeren van uw eerste Oracle-Database in Azure.
- Probeer de [een Oracle-Database maken op Azure](oracle-database-quick-create.md) zelfstudie aan de slag.