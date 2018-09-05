---
title: In Azure Migrate collector-apparaat | Microsoft Docs
description: Biedt een overzicht van het Collector-apparaat en hoe dit moet worden geconfigureerd.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/25/2018
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: 74caf0ab052e1f6558dc20d15d84c01177b3f9cb
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665577"
---
# <a name="collector-appliance"></a>Collector-apparaat

[Azure Migrate](migrate-overview.md) beoordeelt on-premises werkbelastingen voor migratie naar Azure. In dit artikel bevat informatie over het gebruik van het Collector-apparaat.



## <a name="overview"></a>Overzicht

Een Azure Migrate Collector is een lichtgewicht apparaat die kan worden gebruikt voor het detecteren van uw on-premises vCenter-omgeving. Dit apparaat on-premises VMware-machines wordt gedetecteerd, en stuurt metagegevens van deze naar de Azure Migrate-service.

Het Collector-apparaat is een OVF die u via de Azure Migrate-project downloaden kunt. Het instantiëren van een virtuele VMware-machine met 4 kernen, 8 GB RAM-geheugen en één schijf van 80 GB. Het besturingssysteem van het apparaat is Windows Server 2012 R2 (64 bits).

U kunt de Collector maken door de stappen te volgen hier: [over het maken van de Collector-VM](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Diagram van de collector-communicatie

![Diagram van de collector-communicatie](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Onderdeel      | Voor communicatie met   | Vereiste poort                            | Reden                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Collector      | Azure Migrate-service | TCP 443                                  | Collector moet kunnen communiceren met de service via de SSL-poort 443 |
| Collector      | vCenter Server        | Standaard 443                             | Collector moet kunnen communiceren met de vCenter-server. Deze verbinding maakt met vCenter op 443 standaard. Als het vCenter op een andere poort luistert, is die poort beschikbaar als uitgaande poort in de collector |
| Collector      | RDP|   | TCP 3389 | U kunt RDP bij de Collector-computer |





## <a name="collector-pre-requisites"></a>Collector-vereisten

De Collector moet worden doorgegeven van enkele vereiste controles om ervoor te zorgen kan verbinding maken met de Azure Migrate-service en de gedetecteerde gegevens uploaden. In dit artikel wordt gekeken naar elk van de vereisten en weten waarom is het vereist.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Het collector-apparaat moet worden verbonden met internet om de gedetecteerde computers gegevens te verzenden. U kunt de computer in een van de volgende twee manieren verbinding met internet.

1. U kunt de Collector als u directe verbinding met internet wilt configureren.
2. U kunt de Collector verbinding via een proxyserver te configureren.
    * Als de proxyserver verificatie is vereist, kunt u de gebruikersnaam en wachtwoord opgeven in de verbindingsinstellingen.
    * De IP-adres/de FQDN van de proxyserver moet de vorm http://IPaddress of http://FQDN. Alleen http-proxy wordt ondersteund.

> [!NOTE]
> Op HTTPS gebaseerde proxy-servers worden niet ondersteund door de collector.

#### <a name="internet-connectivity-with-intercepting-proxy"></a>Verbinding met Internet met proxy onderscheppen

Als de proxy-server kunt u verbinding maken met internet een onderschept proxy is, moet u de proxy-certificaat importeren in de collector-VM. Hieronder vindt u stapsgewijze instructies voor hoe u het certificaat in de collector-VM importeren kunt.

1. In de collector-VM, gaat u naar **startmenu** en open **computercertificaten beheren**.
2. In het hulpprogramma voor certificaten in het linkerdeelvenster onder **certificaten - lokale Computer**, vinden **vertrouwde uitgevers**. Onder **vertrouwde uitgevers**, klikt u op **certificaten** om te zien van de lijst van certificaten in het deelvenster aan de rechterkant.

    ![Hulpprogramma voor certificaten](./media/concepts-intercepting-proxy/certificates-tool.png)

3. Kopieer uw proxy-certificaat u aan bij de collector-VM. Mogelijk moet u contact opnemen met het team van de beheerder netwerk in uw organisatie om dit certificaat te verkrijgen.
4. Dubbelklik op het certificaat om dit te openen. Klik op **certificaat installeren**. Hiermee gaat u naar de wizard Certificaat importeren.
5. In de Wizard Certificaat importeren voor Store-locatie, kies **lokale computer**. **Klik op volgende**.

    ![Certificaatarchieflocatie](./media/concepts-intercepting-proxy/certificate-store-location.png)

6. Kies de optie **alle certificaten in het onderstaande archief plaatsen**. Klik op **Bladeren** en selecteer **vertrouwde uitgevers** uit de lijst met certificaten die zijn beschikbaar. Klik op **Volgende**.

    ![Archief met certificaten](./media/concepts-intercepting-proxy/certificate-store.png)
    
7. Klik op **Voltooien**. Hiermee wordt het certificaat importeren. 
8. Desgewenst kunt u controleren of dat het certificaat wordt geïmporteerd door het openen van het hulpprogramma certificaten zoals in stap 1 en 2 hierboven.
9. Controleer op de Azure Migrate collector-app of dat de controle van het internet verbinding is geslaagd.


#### <a name="whitelisting-urls-for-internet-connection"></a>URL's in de Whitelist aan voor verbinding met internet

De vereiste controle is voltooid als de Collector verbinding met internet via de opgegeven instellingen maken kan. De connectiviteitscontrole is gevalideerd door verbinding te maken met een lijst van URL's die in de volgende tabel. Als u een URL-gebaseerde firewallproxy voor het beheren van uitgaande connectiviteit, moet u ervoor zorgen aan lijst met geaccepteerde deze de vereiste URL's:

**URL** | **Doel**  
--- | ---
*.portal.azure.com | Vereist om te controleren van de verbinding met de Azure-service en valideren van tijdsynchronisatie problemen.

Bovendien de controle ook probeert om te valideren van verbinding met de volgende URL's, maar niet de controle mislukt als deze niet toegankelijk. Lijst met toegestane adressen configureren voor de volgende URL's is optioneel, maar u moet uitvoeren van handmatige stappen voor het beperken van de vereiste controle.

**URL** | **Doel**  | **Wat gebeurt er als u geen lijst met toegestane adressen**
--- | --- | ---
*.oneget.org:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.
*.windows.net:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.
*.windowsazure.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.
*.powershellgallery.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.
*.msecnd.net:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.
*.visualstudio.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module. | PowerCLI-installatie mislukt. De module handmatig installeren.

### <a name="time-is-in-sync-with-the-internet-server"></a>Tijd is gesynchroniseerd met de internet-server

De Collector moet worden gesynchroniseerd met de tijdserver op internet om te controleren of dat de aanvragen voor de service worden geverifieerd. De portal.azure.com url moet bereikbaar is vanaf de Collector zodat de tijd kan worden gevalideerd. Als de machine niet gesynchroniseerd is, moet u de kloktijd op de Collector-VM zodat deze overeenkomen met de huidige tijd, als volgt wijzigen:

1. Open een opdrachtprompt met beheerdersrechten op de virtuele machine.
1. Om te controleren of de tijdzone, w32tm /tz worden uitgevoerd.
1. De om tijd te synchroniseren, w32tm/resync worden uitgevoerd.

### <a name="collector-service-should-be-running"></a>Collector-service moet worden uitgevoerd

De Azure Migrate Collector-service moet worden uitgevoerd op de machine. Deze service wordt automatisch gestart wanneer de computer wordt opgestart. Als de service wordt niet uitgevoerd, kunt u beginnen het *Azure Migrate Collector* service via het Configuratiescherm. De Collector-service is verantwoordelijk voor het verbinding maken met de vCenter-server, de machine metagegevens en prestatiegegevens verzamelen en deze verzenden naar de service.

### <a name="vmware-powercli-65"></a>VMware PowerCLI 6.5

De VMware PowerCLI powershell-module moet worden geïnstalleerd zodat de Collector met de vCenter-server en de query voor de MachineDetails van de en de prestatiegegevens communiceren kan. De powershell-module wordt automatisch gedownload en geïnstalleerd als onderdeel van de vereiste controle. Automatisch downloaden is vereist voor enkele URL's in de whitelist opgenomen, mislukt die u moet ofwel openen door in de whitelist aan of de module handmatig installeren.

Installeer de module handmatig met behulp van de volgende stappen uit:

1. Als u wilt de PowerCli op Collector installeren zonder internetverbinding, volgt u de stappen die in [deze koppeling](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. Nadat u de PowerShell-module hebt geïnstalleerd op een andere computer die toegang tot internet heeft, de bestanden kopiëren VMware.* vanaf deze computer met de Collector-computer.
3. De vereiste controles voor opnieuw starten en controleren of PowerCLI is geïnstalleerd.

## <a name="connecting-to-vcenter-server"></a>Verbinding maken met vCenter-Server

De Collector moet verbinding maken met de vCenter-Server en kunnen opvragen voor de virtuele machines, de metagegevens en hun prestatiemeteritems. Deze gegevens worden gebruikt door het project voor het berekenen van een evaluatie.

1. Voor verbinding met de vCenter-Server, kan een alleen-lezen-account met machtigingen als opgegeven in de volgende tabel worden gebruikt om de detectie uitvoeren.

    |Taak  |Vereiste rol/account  |Machtigingen  |
    |---------|---------|---------|
    |Collector-apparaat op basis van detectie    | U moet ten minste een alleen-lezen-gebruiker        |Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen         |

2. Alleen deze datacenters die toegankelijk voor de vCenter-account dat is opgegeven zijn is toegankelijk voor detectie.
3. U moet de vCenter-FQDN-naam/IP-adres verbinding maken met de vCenter-server opgeven. Standaard wordt er verbinding gemaakt via de poort 443. Als u de vCenter om te luisteren naar een ander poortnummer hebt geconfigureerd, kunt u deze opgeven als onderdeel van het adres van de server in de vorm IPAddress:Port_Number of FQDN:Port_Number.
4. De instellingen voor statistieken voor de vCenter-Server moeten worden ingesteld op niveau 3 voordat u begint met implementatie. Als het niveau lager dan 3 is, de detectie wordt voltooid, maar prestatiegegevens voor appopslag en netwerken worden niet verzameld. Aanbevelingen voor de evaluatie van de grootte wordt in dit geval worden gebaseerd op prestatiegegevens voor CPU en geheugen en configuratiegegevens voor schijf en netwerkadapters. [Lees meer](./concepts-collector.md) over welke gegevens worden verzameld en hoe dit invloed heeft op de evaluatie.
5. De Collector moet een netwerk verbinding met de vCenter-server hebben.

> [!NOTE]
> Alleen worden vCenter Server 5.5, 6.0 en 6.5 officieel ondersteund.

> [!IMPORTANT]
> U wordt aangeraden dat u de algemene krachtigste (3) voor het statistiekniveau zo instellen dat alle items correct worden verzameld. Als u ingesteld op een lager niveau vCenter hebt, mogelijk slechts een paar items volledig, worden verzameld met de rest is ingesteld op 0. De evaluatie kan onvolledige gegevens wordt weergegeven.

### <a name="selecting-the-scope-for-discovery"></a>Het bereik voor detectie selecteren

Eenmaal verbinding hebben met de vCenter-, kunt u een scope om te detecteren. Een bereik selecteert, worden alle virtuele machines van het pad opgegeven vCenter-voorraad gedetecteerd.

1. Het bereik mag een datacenter, een map of een ESXi-host.
2. U kunt alleen een bereik tegelijk selecteren. Als u wilt meer virtuele machines hebt geselecteerd, kunt u voltooien van de ene detectie en opnieuw opstarten van het detectieproces met een nieuwe scope.
3. U kunt alleen een bereik dat is selecteren *minder dan 1500 virtuele machines*.

## <a name="specify-migration-project"></a>Migratieproject opgeven

Wanneer de on-premises VMware vCenter is verbonden en een bereik is opgegeven, kunt u nu de projectdetails voor migratie die moeten worden gebruikt voor detectie en evaluatie. Geef de project-ID en sleutel en maak verbinding.

## <a name="start-discovery-and-view-collection-progress"></a>Start de voortgang van detectie en weergeven

Zodra de detectie is gestart, wordt de virtuele machines van vCenter worden gedetecteerd en hun metagegevens en prestaties worden verzonden naar de server. De voortgang weergegeven ook krijgt u van de volgende id's:

1. ID van de collector: Een unieke ID die is toegewezen aan de Collector-computer. Deze ID wordt niet gewijzigd voor een bepaalde computer in verschillende detecties. U kunt deze ID in het geval van fouten tijdens het rapporteren van het probleem aan Microsoft Support.
2. Sessie-ID: Een unieke ID voor de taak wordt uitgevoerd. U kunt verwijzen naar dezelfde Sessionid in de portal wanneer de detectietaak is voltooid. Deze ID voor elke taak wordt gewijzigd. In het geval van fouten, kunt u deze ID rapporteren aan Microsoft Support.

### <a name="what-data-is-collected"></a>Welke gegevens worden verzameld?

De taak detecteert de volgende statische metagegevens over de geselecteerde virtuele machines.

1. VM-weergavenaam (op vCenter)
2. Pad van de inventaris van de virtuele machine (host/map in vCenter)
3. IP-adres
4. MAC-adres
5. Besturingssysteem
5. Aantal kernen, schijven, NIC 's
6. Grootte van geheugen, schijf-grootten
7. En prestatiemeteritems van de VM, schijf en netwerk, zoals vermeld in de onderstaande tabel.

De volgende tabel bevat de prestatiemeteritems die worden verzameld en bevat ook de resultaten van de evaluatie die worden beïnvloed als een bepaald item is niet verzameld.

|Teller                                  |Niveau    |Het niveau van per apparaat  |Evaluatie van de impact                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.Average                        | 1       |N.v.t.                |Aanbevolen VM-grootte en kosten                    |
|Mem.Usage.Average                        | 1       |N.v.t.                |Aanbevolen VM-grootte en kosten                    |
|virtualDisk.read.average                 | 2       |2                 |Grootte van de schijf, opslagkosten en VM-grootte         |
|virtualDisk.write.average                | 2       |2                 |Grootte van de schijf, opslagkosten en VM-grootte         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Grootte van de schijf, opslagkosten en VM-grootte         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Grootte van de schijf, opslagkosten en VM-grootte         |
|NET.Received.Average                     | 2       |3                 |VM-grootte en netwerk kosten                        |
|NET.transmitted.Average                  | 2       |3                 |VM-grootte en netwerk kosten                        |

> [!WARNING]
> Als u een hoger statistiekniveau zojuist hebt ingesteld, duurt om op een dag voor het genereren van de prestatiemeteritems. Dus aangeraden de detectie uit te voeren na één dag.

### <a name="time-required-to-complete-the-collection"></a>Benodigde tijd voor het voltooien van de verzameling

De Collector gegevens van de machine wordt alleen gedetecteerd en verzendt ze naar het project. Het project kan extra tijd voordat de gedetecteerde gegevens worden weergegeven in de portal en u kunt beginnen met het maken van een evaluatie duren.

Op basis van het aantal virtuele machines in het geselecteerde bereik, duurt het maximaal 15 minuten voor het verzenden van de metagegevens van de statische aan het project. Nadat de metagegevens van de statische beschikbaar via de portal is, kunt u het overzicht van de machines in de portal en beginnen met het maken van groepen. Een evaluatie kan niet worden gemaakt totdat de taak is voltooid en het project is verwerkt de gegevens. Nadat de taak voltooid op de Collector, duurt het maximaal één uur voor de prestatiegegevens beschikbaar zijn in de portal op basis van het aantal virtuele machines in de geselecteerde scope.

## <a name="locking-down-the-collector-appliance"></a>Het collector-apparaat te vergrendelen
Het is raadzaam die continu Windows-updates worden uitgevoerd op het collector-apparaat. Als een collector gedurende 60 dagen niet wordt bijgewerkt, is de collector automatisch-afsluiten van de machine gestart. Als een detectie wordt uitgevoerd, de machine wordt niet worden uitgeschakeld, zelfs als deze na de periode van 60 dagen. Na de detectietaak is voltooid, wordt de machine worden uitgeschakeld. Als u van de collector gedurende meer dan 45 dagen gebruikmaakt, raden wij houden van de machine bijgewerkt op alle tijden door actieve Windows update.

We raden u ook aan de volgende stappen uit voor het beveiligen van uw apparaat
1. Geen delen of beheerderswachtwoorden misplace met niet-gemachtigde gebruikers.
2. Sluit het apparaat als niet in gebruik.
3. Plaats het apparaat in een beveiligd netwerk.
4. Zodra de werkzaamheden van de migratie voltooid is, verwijdert u het apparaat-exemplaar. Zorg ervoor dat de schijf een back-up van bestanden (vmdk's), ook niet verwijderen omdat de schijven mogelijk vCenter-referenties in cache op deze.

## <a name="how-to-upgrade-collector"></a>Upgrade uitvoeren van de Collector

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. Download de meest recente [upgradepakket](https://aka.ms/migrate/col/upgrade_9_14) (versie 1.0.9.14).
2. Om ervoor te zorgen dat de gedownloade hotfix beveiligd is, open opdrachtvenster voor beheerders en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (gebruik voorbeeld C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Kopieer het zip-bestand naar de Azure Migrate collector virtuele machine (collector-apparaat).
4. Met de rechtermuisknop op het zip-bestand en selecteer Alles uitpakken.
5. Met de rechtermuisknop op Setup.ps1 en selecteer uitvoeren met PowerShell en volg de instructies op het scherm om de update te installeren.

### <a name="list-of-updates"></a>Lijst met updates

#### <a name="upgrade-to-version-10914"></a>Upgrade uitvoeren naar versie 1.0.9.14

Hash-waarden voor de upgrade [1.0.9.14 pakket](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

#### <a name="upgrade-to-version-10913"></a>Upgrade uitvoeren naar versie 1.0.9.13

Hash-waarden voor de upgrade [1.0.9.13 pakket](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

#### <a name="upgrade-to-version-10911"></a>Upgrade uitvoeren naar versie 1.0.9.11

Hash-waarden voor de upgrade [1.0.9.11 pakket](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

#### <a name="upgrade-to-version-1097"></a>Upgrade uitvoeren naar versie 1.0.9.7

Hash-waarden voor de upgrade [pakket 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="next-steps"></a>Volgende stappen

[Instellen van een evaluatie voor on-premises VMware-machines](tutorial-assessment-vmware.md)
