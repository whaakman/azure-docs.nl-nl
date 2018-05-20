---
title: Collector toestel in Azure migreren | Microsoft Docs
description: Biedt een overzicht van het toestel Collector en configureert.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: d0dd310a1f6dff389a4d3dd41dc389b7117272fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="collector-appliance"></a>Collector toestel

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. In dit artikel bevat informatie over het gebruik van het toestel Collector.



## <a name="overview"></a>Overzicht

Een Collector voor het migreren van Azure is een lichtgewicht toestel die kan worden gebruikt voor het detecteren van uw on-premises vCenter-omgeving. Dit toestel detecteert lokale VMware-machines en stuurt de metagegevens over deze naar de service Azure migreren.

Het toestel Collector is een OVF die u van het project Azure migreren downloaden kunt. Het instantiëren van een virtuele VMware-machine met 4 kernen, 8 GB RAM-geheugen en één schijf van 80 GB. Het besturingssysteem van het toestel is Windows Server 2012 R2 (64 bits).

U kunt de Collector maken door de stappen te volgen hier - [het maken van de VM-Collector](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Diagram van de collector-communicatie

![Diagram van de collector-communicatie](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Onderdeel      | Voor communicatie met   | Vereiste poort                            | Reden                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Collector      | Azure Migrate-service | TCP 443                                  | Collector moet kunnen communiceren met de service via de SSL-poort 443 |
| Collector      | vCenter Server        | Standaard 443                             | Collector moet communiceren met de vCenter-server. Deze verbinding maakt met vCenter op 443 standaard. Als de vCenter op een andere poort luistert, moet die poort beschikbaar zijn als uitgaande poort op de collector |
| Collector      | RDP|   | TCP 3389 | U kunt RDP bij de Collector-computer |





## <a name="collector-pre-requisites"></a>Vereisten voor collector

De Collector moet een enkele vereiste controles zodat het kan verbinding maken met de service Azure migreren en de gedetecteerde gegevens uploaden. Dit artikel wordt gekeken naar elk van de vereisten en begrijpen waarom is het vereist.

### <a name="internet-connectivity"></a>Internetconnectiviteit

Het toestel collector moet worden verbonden met internet om de informatie van de gedetecteerde computers te verzenden. U kunt de computer met het internet verbinden in een van de volgende twee manieren.

1. U kunt de Collector als u beschikt over een rechtstreekse internetverbinding wilt configureren.
2. U kunt de Collector verbinding maakt via een proxyserver configureren.
    * Als de proxyserver verificatie vereist, kunt u de gebruikersnaam en wachtwoord opgeven in de verbindingsinstellingen.
    * De IP-adres/FQDN-naam van de proxyserver moet de vorm http://IPaddress of http://FQDN. Alleen http-proxy wordt ondersteund.

> [!NOTE]
> HTTPS gebaseerde proxy-servers worden niet ondersteund door de collector.

#### <a name="whitelisting-urls-for-internet-connection"></a>Whitelisting-URL's voor internet-verbinding

De vereiste controle is geslaagd als de Collector verbinding met internet via de opgegeven instellingen maken kan. De controle van de verbinding is gevalideerd door verbinding te maken met een lijst met URL's die is opgegeven in de volgende tabel. Als u elke URL gebaseerde, firewall-proxy gebruikt om te bepalen uitgaande verbinding, moet aan de lijst met geaccepteerde dat deze URL's vereist:

**URL** | **Doel**  
--- | ---
*.portal.azure.com | Vereist connectiviteit met de Azure-service controleren en valideren tijdsynchronisatie verstrekt.

Bovendien de controle ook probeert de verbinding met de volgende URL's valideren, maar heeft de controle niet mislukken als niet toegankelijk. Goedgekeurde IP-adressen configureren voor de volgende URL's is optioneel, maar u moet uitvoeren, handmatige stappen voor het beperken van de vereiste controle.

**URL** | **Doel**  | **Wat gebeurt er als u goedgekeurde IP-adressen niet**
--- | --- | ---
*.oneget.org:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.
*.windows.net:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.
*.windowsazure.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.
*.powershellgallery.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.
*.msecnd.net:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.
*.visualstudio.com:443 | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module. | PowerCLI installatie mislukt. De module handmatig installeren.

### <a name="time-is-in-sync-with-the-internet-server"></a>Tijd is gesynchroniseerd met de internet-server

De Collector moet worden gesynchroniseerd met de internetserver tijd om te controleren of dat de aanvragen naar de service worden geverifieerd. De portal.azure.com url moet bereikbaar is vanaf de Collector zodat de tijd kan worden gevalideerd. Als de machine niet gesynchroniseerd is, moet u het tijdstip van de klok op de VM-Collector volgens de huidige tijd als volgt wijzigen:

1. Open een opdrachtprompt beheerder op de virtuele machine.
1. Als u wilt controleren in de tijdzone, w32tm /tz worden uitgevoerd.
1. Als u wilt synchroniseren van de tijd, w32tm/resync worden uitgevoerd.

### <a name="collector-service-should-be-running"></a>Collectorservice moet worden uitgevoerd

De service Azure migreren Collector moet worden uitgevoerd op de machine. Deze service wordt automatisch gestart wanneer de computer wordt opgestart. Als de service niet wordt uitgevoerd, start u de *Azure migreren Collector* service via het Configuratiescherm. De Collector-service is verantwoordelijk voor het verbinding maken met de vCenter-server, gegevens van de machine metagegevens en prestaties verzamelen en naar de service te verzenden.

### <a name="vmware-powercli-65"></a>VMware PowerCLI 6.5

De VMware PowerCLI powershell-module moet worden geïnstalleerd zodat de Collector met de vCenter-server en de query voor de details van de machine en de prestatiegegevens communiceren kan. De powershell-module is automatisch gedownload en geïnstalleerd als onderdeel van de vereiste controle. Automatisch downloaden is vereist voor enkele URL-wilt plaatsen, mislukt die u moet ofwel bieden toegang tot door whitelisting ze of handmatig installeren van de module.

Installeer de module handmatig met de volgende stappen uit:

1. Voor het installeren van de PowerCli op Collector zonder internetverbinding, volg de stappen [deze koppeling](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. Als u de PowerShell-module hebt geïnstalleerd op een andere computer die toegang tot internet heeft, de bestanden kopiëren VMware.* uit dat de machine met de Collector-machine.
3. Opnieuw opstarten van de controles van vereisten en Bevestig dat PowerCLI is geïnstalleerd.

## <a name="connecting-to-vcenter-server"></a>Verbinding maken met de vCenter-Server

De Collector moet verbinding met de vCenter-Server en kunnen opvragen voor de virtuele machines, de metagegevens en hun prestatiemeteritems. Deze gegevens worden gebruikt door het project een beoordeling te berekenen.

1. Voor verbinding met de vCenter-Server, kan een alleen-lezen-account met machtigingen die is opgegeven in de volgende tabel worden gebruikt voor het uitvoeren van de detectie.

    |Taak  |Vereiste rol/account  |Machtigingen  |
    |---------|---------|---------|
    |Collector toestel op basis van detectie    | U moet ten minste een alleen-lezen-gebruiker        |Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen         |

2. Alleen deze datacenters die toegankelijk voor het opgegeven vCenter-account zijn toegankelijk zijn voor detectie.
3. U moet de vCenter-FQDN-naam of het IP-adres voor de verbinding met de vCenter-server opgeven. Standaard wordt de verbinding maken via poort 443. Als u de vCenter om te luisteren op een ander poortnummer hebt geconfigureerd, kunt u deze als onderdeel van het adres van de server in de vorm IPAddress:Port_Number of FQDN:Port_Number opgeven.
4. De statistieken-instellingen voor de vCenter-Server moeten worden ingesteld op niveau 3 voordat u implementatie begint. Als het niveau lager dan 3 is, wordt de detectie wordt voltooid, maar prestatiegegevens voor opslag en netwerk won't worden verzameld. De aanbevelingen van de evaluatie-grootte wordt in dit geval gebaseerd op prestatiegegevens voor de CPU en geheugen en configuratiegegevens voor de schijf en netwerkadapters. [Lees meer](./concepts-collector.md) over welke gegevens worden verzameld en hoe dit heeft gevolgen voor de evaluatie.
5. De Collector moet een netwerk onbelemmerd zicht op de vCenter-server hebben.

> [!NOTE]
> Alleen vCenter Server-versies 5.5, 6.0 en 6.5 worden officieel ondersteund.

> [!IMPORTANT]
> U wordt aangeraden dat u algemene optimale (3) voor het niveau van de statistieken zo instellen dat alle items correct zijn verzameld. Als u ingesteld op een lager niveau vCenter hebt, kunnen alleen enkele items volledig worden verkregen met de rest ingesteld op 0. De beoordeling mogelijk onvolledig gegevens vervolgens weer.

### <a name="selecting-the-scope-for-discovery"></a>Het bereik voor detectie selecteren

Wanneer een verbinding met de vCenter-, kunt u een scope om te detecteren. Als u een bereik selecteert, worden alle virtuele machines van het pad van de inventarisatie opgegeven vCenter gedetecteerd.

1. Het bereik mag een datacenter, een map of een ESXi-host.
2. U kunt slechts één scope op een tijdstip selecteren. Als u wilt meer virtuele machines selecteren, kunt u één detectie voltooid en opnieuw opstarten van het detectieproces met een nieuwe scope.
3. U kunt alleen een bereik dat is selecteren *minder dan 1500 virtuele machines*.

## <a name="specify-migration-project"></a>Migratieproject opgeven

Als de lokale vCenter is verbonden en een bereik is opgegeven, kunt u nu de migratie projectdetails die moeten worden gebruikt voor detectie en assessment opgeven. Geef de project-ID en sleutel en verbinding maken.

## <a name="start-discovery-and-view-collection-progress"></a>Start de voortgang van de detectie en weergeven

Zodra de detectie wordt gestart, de vCenter-virtuele machines worden gedetecteerd en hun metagegevens en de prestaties worden verzonden naar de server. De voortgangsstatus van de ook krijgt u de volgende-id:

1. Collector-ID: Een unieke ID die is opgegeven op uw computer verzamelen. Deze ID wordt niet gewijzigd voor een bepaalde computer via verschillende detecties. U kunt deze ID in het geval van fouten bij het melden van het probleem aan Microsoft Support.
2. Sessie-ID: Een unieke ID voor de actieve taak in de verzameling. U kunt verwijzen naar dezelfde Sessionid in de portal wanneer de taak is voltooid. Deze ID wordt gewijzigd voor elke taak. In geval van fouten, kunt u deze ID rapporteren aan Microsoft Support.

### <a name="what-data-is-collected"></a>Welke gegevens worden verzameld?

De verzameling taak detecteert de volgende statische metagegevens over de geselecteerde virtuele machines.

1. VM-weergavenaam (op vCenter)
2. Pad van de inventarisatie van de virtuele machine (host of de map in vCenter)
3. IP-adres
4. MAC-adres
5. Besturingssysteem
5. Het aantal kernen, schijven, NIC 's
6. Grootte van het systeemgeheugen, schijfgrootten
7. En prestatiemeteritems van de VM, schijf en netwerk, zoals vermeld in de onderstaande tabel.

De volgende tabel bevat de prestatiemeteritems die worden verzameld en ook vermeldt de resultaten van de beoordeling die worden beïnvloed als een bepaald item is niet verzameld.

|Teller                                  |Niveau    |Per apparaat niveau  |Evaluatie-impact                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.Average                        | 1       |N.v.t.                |VM-grootte en de kosten van aanbevolen                    |
|Mem.Usage.Average                        | 1       |N.v.t.                |VM-grootte en de kosten van aanbevolen                    |
|virtualDisk.read.average                 | 2       |2                 |Schijfgrootte opslagkosten en VM-grootte         |
|virtualDisk.write.average                | 2       |2                 |Schijfgrootte opslagkosten en VM-grootte         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Schijfgrootte opslagkosten en VM-grootte         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Schijfgrootte opslagkosten en VM-grootte         |
|NET.Received.Average                     | 2       |3                 |VM-grootte en het netwerk kosten                        |
|NET.transmitted.Average                  | 2       |3                 |VM-grootte en het netwerk kosten                        |

> [!WARNING]
> Als u een hoger niveau van de statistieken zojuist hebt geïnstalleerd, zal deze duren per dag voor het genereren van de prestatiemeteritems. Dus is het raadzaam dat u de detectie na één dag uitvoert.

### <a name="time-required-to-complete-the-collection"></a>Tijd nodig voor de verzameling

De Collector wordt alleen gegevens van de machine detecteert en verzendt het naar het project. Het project kan extra tijd voordat de gedetecteerde gegevens op de portal wordt weergegeven en u kunt beginnen met het maken van een beoordeling duren.

Op basis van het aantal virtuele machines in de geselecteerde scope, duurt het maximaal 15 minuten om de metagegevens van de statische aan het project. Nadat de metagegevens van de statische beschikbaar op de portal is, kunt u het overzicht van virtuele machines in de portal en beginnen met het maken van groepen. Een evaluatie kan niet worden gemaakt totdat de taak is voltooid en de gegevens is verwerkt door het project. Eenmaal de verzameling-taak is voltooid op de Collector, duurt het maximaal één uur voor de prestatiegegevens alleen beschikbaar op de portal op basis van het aantal virtuele machines in de geselecteerde scope.

## <a name="locking-down-the-collector-appliance"></a>Vergrendelen van het toestel collector
Het is raadzaam om continue Windows-updates uitgevoerd op het apparaat van de collector. Als een collector voor 45 dagen niet wordt bijgewerkt, gaat de collector automatisch de de machine is afgesloten. Als een detectie wordt uitgevoerd, worden de machine niet uitgeschakeld, zelfs als deze voorbij de 45 dagen. Na de detectietaak is voltooid, de machine wordt uitgeschakeld. Als u van de collector meer dan 45 dagen gebruikmaakt, wordt u aangeraden om de machine is bijgewerkt op alle tijden door actieve Windows update te houden.

We raden u ook aan de volgende stappen voor het beveiligen van uw apparaat
1. Geen delen of beheerderswachtwoorden misplace met niet-gemachtigde gebruikers.
2. Sluit het toestel als deze niet in gebruik.
3. Het apparaat in een beveiligde netwerk plaatsen.
4. Zodra het werk van de migratie voltooid is, verwijdert u het toestel-exemplaar. Zorg ervoor dat de schijf back-upbestanden (VMDKs) ook verwijderen als de schijven vCenter-referenties in cache op deze mogelijk heeft.

## <a name="how-to-upgrade-collector"></a>Het bijwerken van de Collector

U kunt de Collector upgraden naar de nieuwste versie zonder dat de eicellen nogmaals gedownload.

1. Download de meest recente [upgradepakket](https://aka.ms/migrate/col/latestupgrade).
2. Om ervoor te zorgen dat de gedownloade hotfix beveiligd is, opent u het opdrachtvenster Administrator en voer de volgende opdracht voor het genereren van de hash van het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (voorbeeld van syntaxis C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.5.zip SHA256)
3. Kopieer het zip-bestand naar de Azure-collector virtuele machine migreren (collector toestel).
4. Met de rechtermuisknop op het zip-bestand en selecteer Alles uitpakken.
5. Met de rechtermuisknop op Setup.ps1 en klikt u op uitvoeren met PowerShell en volg de instructies op het scherm om de update te installeren.

### <a name="list-of-updates"></a>Lijst met updates

#### <a name="upgrade-to-version-1097"></a>Upgrade uitvoeren naar versie 1.0.9.7

Voor een Upgrade naar versie 1.0.9.7 downloaden [pakket](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

#### <a name="upgrade-to-version-1095"></a>Upgrade uitvoeren naar versie 1.0.9.5

Voor een Upgrade naar versie 1.0.9.5 downloaden [pakket](https://aka.ms/migrate/col/upgrade_9_5)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d969ebf3bdacc3952df0310d8891ffdf
SHA1 | f96cc428eaa49d597eb77e51721dec600af19d53
SHA256 | 07c03abaac686faca1e82aef8b80e8ad8eca39067f1f80b4038967be1dc86fa1

## <a name="next-steps"></a>Volgende stappen

[Instellen van een beoordeling voor lokale virtuele VMware-machines](tutorial-assessment-vmware.md)
