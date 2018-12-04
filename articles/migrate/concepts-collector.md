---
title: Over het Collector-apparaat in Azure Migrate | Microsoft Docs
description: Bevat informatie over het Collector-apparaat in Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 5a542ae23bf500125fd08338b2efd30dd42d9a8d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840908"
---
# <a name="about-the-collector-appliance"></a>Over het Collector-apparaat

 In dit artikel bevat informatie over Azure Migrate Collector.

De Azure Migrate Collector is een lichtgewicht apparaat die kan worden gebruikt voor het detecteren van een on-premises vCenter-omgeving voor evaluatiedoeleinden met de [Azure Migrate](migrate-overview.md) service vóór de migratie naar Azure.  

## <a name="discovery-method"></a>Detectiemethode

Eerder waren er twee opties voor de collector-apparaat, detectie van eenmalige en continue detectie. Het model eenmalige detectie is beëindigd omdat het vertrouwen op de vCenter-Server-instellingen voor statistieken voor verzameling van prestatiegegevens (vereist statistieken instellingen moet worden ingesteld op niveau 3) en ook verzameld gemiddelde items (in plaats van piekuren), wat leidde tot te voorzichtige grootte. Het model continue detectie zorgt ervoor dat het verzamelen van gedetailleerde gegevens en resulteert in een nauwkeurige schaling vanwege verzameling piek-items. Hieronder ziet u hoe het werkt:

Het collector-apparaat continu is verbonden met het Azure Migrate-project en continu verzamelt prestatiegegevens van virtuele machines.

- De collector profielen continu de on-premises omgeving voor het verzamelen van gegevens over het gebruik van realtime elke 20 seconden.
- Het toestel totaliseert de voorbeelden 20 seconden en maakt u één gegevenspunt om de 15 minuten.
- Punt het apparaat de hoogste waarde selecteert in de voorbeelden 20 seconden en verzendt dit naar Azure te maken van de gegevens.
- Dit model afhankelijk niet van de instellingen voor statistieken vCenter-Server om prestatiegegevens te verzamelen.
- U kunt stoppen continue profilering op elk gewenst moment van de Collector.

**Direct resultaat:** met het continue detectie-apparaat, wanneer de detectie voltooid is (het duurt enkele uren, afhankelijk van het aantal virtuele machines), u kunt onmiddellijk een evaluatie maken. Omdat de verzameling van prestatiegegevens wordt gestart wanneer u detectie, een vliegende start als u direct resultaat zoekt, moet u het criterium voor het instellen in de evaluatie als *zoals on-premises*. Voor beoordelingen op basis van prestaties, is het raadzaam om te wachten op ten minste een dag na de detectie voor betrouwbare aanbevolen groottes begon.

Alleen verzamelt prestatiegegevens continu het toestel, detecteert niet elke configuratiewijziging in de on-premises-omgeving (dat wil zeggen VM toevoegen, verwijderen en schijf toevoegen, enz.). Als er een configuratiewijziging in de on-premises omgeving is, kunt u het volgende doen om de wijzigingen door te voeren in de portal:

- Toevoegen van items (virtuele machines, schijven, kernen enz.): om deze wijzigingen in de Azure-portal door te voeren, kunt u de detectie vanaf het apparaat stoppen en opnieuw starten. Dit zorgt ervoor dat de wijzigingen worden bijgewerkt in het Azure Migrate-project.

- Verwijderen van VM’s: vanwege de manier waarop het apparaat is ontworpen, wordt het verwijderen van VM’s niet doorgevoerd, zelfs niet als u de detectie stopt en opnieuw start. Dit komt doordat gegevens uit volgende detecties worden toegevoegd aan de oudere detecties en niet worden overschreven. In dit geval kunt u eenvoudigweg de VM in de portal negeren door deze uit uw groep te verwijderen en de evaluatie opnieuw te berekenen.

> [!NOTE]
> Het apparaat voor eenmalige detectie is beëindigd als deze methode op vCenter-statistieken-instellingen van de Server voor prestaties punt van beschikbaarheid van gegevens vertrouwen en die worden verzameld gemiddelde prestatiemeteritems, wat leidde tot te voorzichtige schalen van VM's voor migratie naar Azure.

## <a name="deploying-the-collector"></a>Implementatie van de Collector

U implementeert het Collector-apparaat met behulp van een OVF-sjabloon:

- U downloaden de OVF-sjabloon van een Azure Migrate-project in Azure portal. U kunt het gedownloade bestand importeren met vCenter-Server voor het instellen van de virtuele machine van de Collector-apparaat.
- Stelt u een VM met 4 kernen, 8 GB RAM-geheugen en één schijf van 80 GB van de OVF in VMware. Het besturingssysteem is Windows Server 2012 R2 (64 bits).
- Wanneer u de Collector uitvoert, wordt een aantal controles uitgevoerd om ervoor te zorgen dat de Collector verbinding met het Azure Migrate maken kunt.

- [Meer informatie](tutorial-assessment-vmware.md#create-the-collector-vm) over het maken van de Collector.


## <a name="collector-prerequisites"></a>Collector-vereisten

De Collector moet slagen voor een aantal controles om ervoor te zorgen deze verbinding kan maken met de Azure Migrate-service via internet en het uploaden van gegevens gedetecteerd.

- **Controleer de internetverbinding**: de Collector verbinding kan maken met internet rechtstreeks of via een proxyserver.
    - Het controleren van de vereisten controleert of de verbinding met [URL's voor vereiste en optionele](#connect-to-urls).
    - Als u een directe verbinding met internet hebt, is geen specifieke actie vereist is, dan ervoor te zorgen dat de Collector de vereiste URL's kan bereiken.
    - Als u verbinding via een proxy maakt, let dan op de [onderstaande vereisten](#connect-via-a-proxy).
- **Tijdsynchronisatie controleren**: de Collector moeten worden gesynchroniseerd met de tijdserver op internet om te controleren of de aanvragen voor de service worden geverifieerd.
    - De portal.azure.com url moet bereikbaar is vanaf de Collector zodat de tijd kan worden gevalideerd.
    - Als de machine is niet gesynchroniseerd, moet u de klok op de Collector-VM zodat deze overeenkomen met de huidige tijd verandert. Doen dit open een beheerder op de virtuele machine, voer **w32tm /tz** om te controleren of de tijdzone. Voer **w32tm/resync** de tijd te synchroniseren.
- **Controleer de collector-service die wordt uitgevoerd**: de Azure Migrate Collector-service moet worden uitgevoerd op de Collector-VM.
    - Deze service wordt automatisch gestart wanneer de computer wordt opgestart.
    - Als de service niet actief is, start u deze in het Configuratiescherm.
    - De Collector-service maakt verbinding met vCenter-Server, de VM-metagegevens en de prestaties gegevens verzamelt en verzendt dit naar de service Azure Migrate.
- **Controleren van VMware PowerCLI 6.5 is geïnstalleerd**: de VMware PowerCLI 6.5 PowerShell-module moet worden geïnstalleerd op de Collector-VM, zodat deze met de vCenter-Server communiceren kan.
    - Als de Collector toegang heeft tot de URL's die nodig zijn om de module te installeren, wordt deze installatie automatisch tijdens de implementatie van de Collector.
    - Als de Collector u de module tijdens de implementatie installeren kunt, moet u [handmatig installeren](#install-vwware-powercli-module-manually).
- **Controleer de verbinding met vCenter-Server**: de Collector moeten kunnen vCenter-Server en query's uitvoeren voor virtuele machines, de metagegevens en prestatiemeteritems. [Vereisten controleren](#connect-to-vcenter-server) om verbinding te maken.


### <a name="connect-to-the-internet-via-a-proxy"></a>Verbinding maken met internet via een proxy

- Als de proxyserver verificatie is vereist, kunt u de gebruikersnaam en wachtwoord opgeven bij het instellen van de Collector.
- De IP-adres/de FQDN van de proxyserver moet worden opgegeven als *http://IPaddress* of *http://FQDN*.
- Alleen HTTP-proxy wordt ondersteund. Op HTTPS gebaseerde proxy-servers worden niet ondersteund door de Collector.
- Als de proxy-server een onderschept proxy is, moet u de proxy-certificaat importeren met de Collector-VM.
    1. In de collector-VM, gaat u naar **startmenu** > **computercertificaten beheren**.
    2. In het hulpprogramma voor certificaten onder **certificaten - lokale Computer**, vinden **vertrouwde uitgevers** > **certificaten**.

        ![Hulpprogramma voor certificaten](./media/concepts-intercepting-proxy/certificates-tool.png)

    3. Kopieer de proxy-certificaat naar de collector-VM. U moet mogelijk opvragen bij uw netwerkbeheerder.
    4. Dubbelklik op het certificaat en klik op **certificaat installeren**.
    5. In de Wizard Certificaat importeren > locatie van de Store, kiest u **lokale computer**.

    ![Certificaatarchieflocatie](./media/concepts-intercepting-proxy/certificate-store-location.png)

    6. Selecteer **alle certificaten in het onderstaande archief plaatsen** > **Bladeren** > **vertrouwde uitgevers**. Klik op **voltooien** om het certificaat te importeren.

    ![Archief met certificaten](./media/concepts-intercepting-proxy/certificate-store.png)

    7. Controleer of het certificaat wordt geïmporteerd zoals verwacht en controleer dat het internet connectiviteit controle werkt zoals verwacht.




### <a name="connect-to-urls"></a>Verbinding maken met URL 's

De connectiviteitscontrole is gevalideerd door verbinding te maken met een lijst met URL's.

**URL** | **Details**  | **Het controleren van vereisten**
--- | --- | ---
*.portal.azure.com | Controleert de connectiviteit met de Azure-service en tijdsynchronisatie. | Toegang tot URL vereist.<br/><br/> Controle van vereisten mislukt als er geen verbinding.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| Voor het downloaden van de PowerShell-module voor vCenter PowerCLI. | Toegang tot URL's is optioneel.<br/><br/> Controle van vereisten wordt niet mislukken.<br/><br/> Installatie van de automatische module op de Collector-VM mislukt. U moet de module handmatig installeren.


### <a name="install-vmware-powercli-module-manually"></a>VMware PowerCLI-module handmatig installeren

1. Installeer de module met [stappen](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Deze stappen wordt zowel online als offline-installatie beschreven.
2. Als de Collector-VM is offline en installeren op de module op een andere computer met toegang tot internet, moet u de bestanden te kopiëren VMware.* vanaf deze computer met de Collector-VM.
3. U kunt de controles op vereisten om te controleren of PowerCLI is geïnstalleerd opnieuw na de installatie.

### <a name="connect-to-vcenter-server"></a>Verbinding maken met vCenter Server

De Collector verbinding maakt met de vCenter-Server en query's voor VM-metagegevens en prestatiemeteritems. Dit is wat u nodig hebt voor de verbinding.

- Alleen worden vCenter Server 5.5, 6.0 en 6.5 ondersteund.
- U hebt een alleen-lezen-account nodig met de machtigingen die zijn samengevat hieronder voor detectie. Alleen datacenters die toegankelijk zijn met het account is toegankelijk voor detectie.
- Standaard u verbinding maken met vCenter-Server met een FQDN-naam of IP-adres. VCenter-Server op een andere poort luistert, u verbinding maken met met behulp van het formulier *IPAddress:Port_Number* of *FQDN:Port_Number*.
- Voor het verzamelen van prestatiegegevens voor opslag en netwerken, de instellingen voor statistieken voor vCenter-Server moet worden ingesteld op niveau 3.
- Als het niveau lager dan drie, detectie werkt is, maar de gegevens worden niet verzameld. Sommige items kunnen worden verzameld, maar andere worden ingesteld op nul.
- Als de prestatiegegevens voor opslag en netwerken is niet verzameld, worden aanbevelingen voor de evaluatie van de grootte op basis van prestatiegegevens voor CPU en geheugen en configuratiegegevens voor schijf en netwerkadapters.
- De Collector moet een netwerk verbinding met de vCenter-server hebben.

#### <a name="account-permissions"></a>Accountmachtigingen

**Account** | **Machtigingen**
--- | ---
Ten minste een alleen-lezen-gebruikersaccount | Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen   


## <a name="collector-communications"></a>Communicatie van de collector

De collector communiceert zoals in het volgende diagram en de volgende tabel wordt samengevat.

![Diagram van de collector-communicatie](./media/tutorial-assessment-vmware/portdiagram.PNG)


**Collector communiceert met** | **Poort** | **Details**
--- | --- | ---
Azure Migrate-service | TCP 443 | Collector communiceert met Azure Migrate-service via SSL 443.
vCenter Server | TCP 443 | De Collector moet in staat om te communiceren met de vCenter-Server zijn.<br/><br/> Standaard maakt deze verbinding met vCenter op 443.<br/><br/> Als de vCenter-Server op een andere poort luistert, moet deze poort beschikbaar als uitgaande poort in de Collector zijn.
RDP | TCP 3389 |


## <a name="securing-the-collector-appliance"></a>Het Collector-apparaat beveiligen

U wordt aangeraden de volgende stappen uit voor het beveiligen van het Collector-apparaat:

- Geen delen of beheerderswachtwoorden misplace met niet-gemachtigde gebruikers.
- Sluit het apparaat als niet in gebruik.
- Plaats het apparaat in een beveiligd netwerk.
- Nadat de migratie is voltooid, verwijdert u het apparaat-exemplaar.
- Bovendien na de migratie ook verwijderen de schijf-back-upbestanden (vmdk's), omdat de schijven mogelijk vCenter-referenties in cache op deze.

## <a name="os-license-in-the-collector-vm"></a>OS-licentie in de collector-VM

De collector wordt geleverd met een licentie voor de evaluatie van Windows Server 2012 R2 die 180 dagen geldig is. Als de evaluatieperiode is verlopen voor de collector-VM, is het aanbevolen om een nieuwe OVA downloaden en een nieuwe toepassing maken.

## <a name="updating-the-os-of-the-collector-vm"></a>Bijwerken van het besturingssysteem van de Collector-VM

Hoewel het collector-apparaat een evaluatielicentie gedurende 180 dagen heeft, moet u continu bijwerken van het besturingssysteem op het apparaat om te voorkomen dat automatisch afsluiten omlaag van het apparaat.

- Als de Collector wordt niet in verband met 60 dagen bijgewerkt, begint deze automatisch afsluiten van de machine.
- Als een detectie wordt uitgevoerd, de machine wordt niet worden uitgeschakeld, zelfs als er 60 dagen zijn verstreken. De machine worden uitgeschakeld nadat de detectie is voltooid.
- Als u de Collector gedurende meer dan 60 dagen gebruikt hebt, raden wij houden van de machine bijgewerkt op alle tijden door actieve Windows update.

## <a name="upgrading-the-collector-appliance-version"></a>Bijwerken van de Collector-apparaat-versie

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. Download de [nieuwste vermeld upgradepakket](concepts-collector-upgrade.md)
2. Om ervoor te zorgen dat de gedownloade hotfix beveiligd is, open opdrachtvenster voor beheerders en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (gebruik voorbeeld C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Kopieer het zip-bestand naar de Azure Migrate collector virtuele machine (collector-apparaat).
4. Met de rechtermuisknop op het zip-bestand en selecteer Alles uitpakken.
5. Met de rechtermuisknop op Setup.ps1 en selecteer uitvoeren met PowerShell en volg de instructies op het scherm om de update te installeren.

## <a name="discovery-process"></a>Discovery-proces

Nadat het apparaat is ingesteld, kunt u de detectie kunt uitvoeren. Dit is hoe het werkt:

- U kunt een detectie uitvoeren door een bereik. Alle virtuele machines in het pad van de voorraad opgegeven vCenter wordt gedetecteerd.
    - U instellen een bereik op een tijdstip.
    - Het bereik kan zijn 1500 VM's of minder.
    - Het bereik mag een datacenter-, map- of ESXi-host.
- Nadat u verbinding met vCenter-Server, hebt u verbinding maken met een migration-project voor de verzameling op te geven.
- Virtuele machines zijn gedetecteerd en hun metagegevens en prestaties worden verzonden naar Azure. Deze acties zijn onderdeel van een taak.
    - Het Collector-apparaat wordt een specifieke Collector-ID die is permanent voor een bepaalde virtuele machine, ook detecties gegeven.
    - Een actieve taak krijgt een bepaalde sessie-ID. De ID voor elke taak wordt gewijzigd, en kan worden gebruikt voor het oplossen van problemen.

### <a name="collected-metadata"></a>Verzamelde metagegevens

Het collector-apparaat wordt de volgende metagegevens van de configuratie voor elke virtuele machine gedetecteerd. De configuratiegegevens voor de virtuele machines zijn beschikbaar een uur nadat u de detectie begint.

- Naam van de virtuele machine weergegeven (op de vCenter-Server)
- Pad van de inventaris van de virtuele machine (de host/map op de vCenter-Server)
- IP-adres
- MAC-adres
- Besturingssysteem
- Aantal kernen, schijven, NIC 's
- Grootte van geheugen, schijf-grootten
- Prestatiemeteritems van de VM, schijf en netwerk.

#### <a name="performance-counters"></a>Prestatiemeteritems

 Het collector-apparaat verzamelt de volgende prestatiemeteritems voor elke virtuele machine van de ESXi-host met een interval van 20 seconden. Deze items zijn prestatiemeteritems van vCenter en hoewel de terminologie aangeeft dat deze gemiddelde, de voorbeelden 20 seconden real-time-prestatiemeteritems zijn. De prestatiegegevens voor de virtuele machines wordt gestart steeds beschikbaar in de portal twee uur nadat u de detectie hebt gestart. Het is raadzaam om te wachten op ten minste een dag voor het maken van beoordelingen op basis van prestaties om nauwkeurige juiste formaat aanbevelingen te krijgen. Als u direct resultaat zoekt, kunt u evaluaties maken met het criterium voor het instellen als *zoals on-premises* die wordt geen rekening gehouden met de prestatiegegevens voor de juiste grootte.

**Teller** |  **Gevolgen voor de evaluatie**
--- | ---
CPU.Usage.Average | Aanbevolen VM-grootte en kosten  
Mem.Usage.Average | Aanbevolen VM-grootte en kosten  
virtualDisk.read.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.write.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberReadAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberWriteAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
NET.Received.Average | Berekent de VM-grootte                          
NET.transmitted.Average | Berekent de VM-grootte     

## <a name="next-steps"></a>Volgende stappen

[Instellen van een evaluatie voor on-premises VMware-machines](tutorial-assessment-vmware.md)
