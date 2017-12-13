---
title: De schaal van detectie en beoordeling met Azure migreren | Microsoft Docs
description: Beschrijft hoe vast te stellen van grote aantallen lokale machines met de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: e28a2144dd102fcd2ec05531432cac0df250ae01
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detecteren en beoordelen van een grote VMware-omgeving

In dit artikel wordt beschreven hoe vast te stellen van grote aantallen lokale machines met [Azure migreren](migrate-overview.md). Azure migreren beoordeelt machines om te controleren of ze geschikt is voor migratie naar Azure, en biedt sizing en kosten schattingen voor het uitvoeren van de machine in Azure.

## <a name="prerequisites"></a>Vereisten

- **VMware**: U moet ten minste één VMware VM zich bevindt op een ESXi-host of cluster met versie 5.0 of hoger. De host of het cluster moet worden beheerd door een vCenter-server versie 5.5 of 6.0.
- **vCenter account**: U moet een alleen-lezen-account met beheerdersreferenties voor de vCenter-server. Azure migreren gebruikt deze account voor het detecteren van virtuele machines.
- **Machtigingen**: op de vCenter-server, moet u machtigingen voor het maken van een virtuele machine door het importeren van een bestand in. EICELLEN-indeling.
- **Instellingen voor statistieken**: de instellingen van de statistieken voor de vCenter-server moeten worden ingesteld op niveau 2 of hoger, voordat u implementatie begint.

## <a name="plan-azure-migrate-projects"></a>Azure migreren projecten plannen

Een project voor het migreren van Azure kunt beoordelen maximaal 1500 machines. Een enkele detectie in een project kan maximaal 1000 machines detecteren.

- Als u minder dan 1000 computers hebt te detecteren, moet u een project met een enkele detectie.
- Als u tussen 1000 en 1500 machines hebt, moet u een project met twee detecties in het.
- Als u meer dan 1500 machines hebt, moet u meerdere projecten maken en uitvoeren van meerdere detecties volgens uw vereisten. Bijvoorbeeld:
    - Als u 3000 machines hebt, kan u twee projecten met detecties van twee of drie projecten met een enkele detectie instellen.
    - Als u 5000 machines hebt, kunt u vier projecten kan instellen. Twee met een detectie van 1500 machines en één met een detectie van 500 machines. U kunt ook instellen u vijf projecten met een enkele detectie in elk criterium. 
- Wanneer u een detectie in Azure migreren doet, kunt u het detectiebereik ingesteld op een VMware-map, een datacenter of een cluster.
- Hiertoe meer dan één detectie, controleert u of in vCenter de virtuele machines die u wilt detecteren in mappen, datacenters of clusters die de limiet van 1000 machines ondersteunen.
- We adviseren dat voor een evaluatie, u machines met tussen afhankelijkheden binnen het hetzelfde project en de evaluatie. In vCenter, zorg er dus dat afhankelijke machines zich in dezelfde map, datacenter of cluster voor de doeleinden van evaluatie.


## <a name="create-a-project"></a>Een project maken

Maak een project Azure migreren volgens uw vereisten.

1. Klik in de Azure-portal op **maken van een resource**.
2. Zoeken naar **Azure migreren**, en selecteer de service (**Azure migreren (preview)** in de zoekresultaten. Klik vervolgens op **Maken**.
3. Geef de naam van het project en de Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de regio waarin u wilt maken van het project en klik vervolgens op **maken**. Metagegevens die afkomstig zijn van lokale virtuele machines worden opgeslagen in deze regio.

## <a name="set-up-the-collector-appliance"></a>Instellen van het toestel collector

Azure migreren maakt een lokale virtuele machine het toestel collector genoemd. Deze virtuele machine detecteert lokale virtuele VMware-machines en stuurt de metagegevens over deze naar de service Azure migreren. Als u het toestel collector instelt, downloadt u een. EICELLEN bestand en importeer ze in de lokale vCenter-server voor het maken van de virtuele machine.

### <a name="download-the-collector-appliance"></a>Downloaden van het toestel collector

Als u meerdere projecten hebt, hoeft u alleen het toestel collector eenmaal gedownload naar de vCenter-server. Voer deze uit voor elk project nadat hebt gedownload en instellen van het apparaat, en geef de unieke project-ID en sleutel.

1. Klik in het project Azure migreren **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **machines ontdekken**, klikt u op **downloaden**, voor het downloaden van de. EICELLEN-bestand.
3. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. U moet deze bij het configureren van de collector.

   
### <a name="verify-the-collector-appliance"></a>Controleer of het toestel collector

Controleer of de. EICELLEN bestand is beveiligd voordat u deze implementeert.

1. Open een opdrachtvenster beheerder op de machine waarop u het bestand hebt gedownload.
2. Voer de volgende opdracht voor het genereren van de hash voor de eicellen:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Voorbeeld van syntaxis:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen.

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | c283f00f46484bf673dc8383b01d0741 
    SHA1 | 8f49b47f53d051af1780bbc725659ce64e717ed4
    SHA256 | 7aecdbdb2aea712efe99d0c1444503f52d16de5768e783465e226fbbca71501d

## <a name="create-the-collector-vm"></a>De collector VM maken

Het gedownloade bestand importeren in de vCenter-server.

1. Klik in de console van de Client vSphere **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/how-to-scale-assessment/vcenter-wizard.png)

2. In de Wizard OVF sjabloon implementeren > **bron**, geef de locatie van het bestand .ova.
3. In **naam** en **locatie**, Geef een beschrijvende naam voor de VM-collector en de inventaris-object waarmee de virtuele machine wordt gehost.
5. In **Host/Cluster**, geeft u de host of cluster op waarmee de VM-collector wordt uitgevoerd.
7. Geef de opslaglocatie voor de collector VM in opslag.
8. In **schijfindeling**, het schijftype en de grootte opgeven.
9. In **netwerktoewijzing**, geeft u het netwerk waarmee de VM-collector verbinding maakt. Het netwerk moet verbinding met internet, metagegevens verzenden naar Azure. 
10. Controleer en Bevestig de instellingen en klik op **voltooien**.

## <a name="identify-the-key-and-id-for-each-project"></a>De sleutel en de ID voor elk project identificeren

Als er meerdere projecten, zorg ervoor dat u de ID te identificeren en sleutel voor elk adres. U moet de sleutel tijdens het uitvoeren van de collector voor het detecteren van de virtuele machines.

1. Klik in het project op **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. 
    ![Project-ID](./media/how-to-scale-assessment/project-id.png)

## <a name="vcenter-statistics-level-to-collect-the-performance-counters"></a>vCenter statistieken niveau voor het verzamelen van de prestatiemeteritems
Hieronder volgt de lijst met items die worden verzameld tijdens de detectie. De prestatiemeteritems zijn standaard beschikbaar op verschillende niveau in de vCenter-server. Wij raden u algemene optimale (niveau 3) voor het niveau van de statistieken zodat alle items correct zijn verzameld. Als u ingesteld op een lager niveau vCenter hebt, kunnen alleen enkele items volledig ophalen verkregen met de rest ingesteld op 0. De beoordeling mogelijk daarom onvolledige gegevens weer. De onderstaande tabel vermeldt tevens de resultaten van de beoordeling worden beïnvloed als een bepaald item is niet verzameld.

|Prestatiemeteritems                                  |Niveau    |Per apparaatniveau  |Evaluatie-impact                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.Average                        | 1       |N.v.t.                |VM-grootte en de kosten van aanbevolen                    |
|Mem.Usage.Average                        | 1       |N.v.t.                |VM-grootte en de kosten van aanbevolen                    |
|virtualDisk.read.average                 | 2       |2                 |Grootte van de schijf, opslagkosten en de VM-grootte         |
|virtualDisk.write.average                | 2       |2                 |Grootte van de schijf, opslagkosten en de VM-grootte         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Grootte van de schijf, opslagkosten en de VM-grootte         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Grootte van de schijf, opslagkosten en de VM-grootte         |
|NET.Received.Average                     | 2       |3                 |VM-grootte en het netwerk kosten                        |
|NET.transmitted.Average                  | 2       |3                 |VM-grootte en het netwerk kosten                        |

> [!WARNING]
> Als u een hoger niveau van de statistieken zojuist hebt geïnstalleerd, wordt het duurt maximaal een dag voor het genereren van de prestatiemeteritems. Daarom is het aanbevolen dat u de detectie na één dag uitvoert.

## <a name="run-the-collector-to-discover-vms"></a>De collector voor het detecteren van virtuele machines worden uitgevoerd

Voor elke detectieregel die u moet uitvoeren, moet u de collector naar virtuele machines in het vereiste bereik detectie uitvoert. Voer de detecties een na de andere. Gelijktijdige detecties worden niet ondersteund moet, en elke detectieregel een ander bereik.

1. In de console vSphere Client met de rechtermuisknop op de virtuele machine > **Console openen**.
2. Geef de taal, de tijdzone en het wachtwoord voorkeuren voor het apparaat.
3. Klik op het bureaublad op de **uitvoeren collector** snelkoppeling.
4. Open in de Collector Azure migreren **vereisten instellen**.
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang tot internet.
    - Als de virtuele machine toegang heeft tot internet via een proxy, klikt u op **Proxy-instellingen**, en geef de proxy-adres en poort voor luisteren. Geef referenties op als de proxy moet worden geverifieerd.
    - De collector wordt gecontroleerd of de profiler Windows-service wordt uitgevoerd. De service wordt standaard geïnstalleerd op de VM-collector.
    - Download en installeer de VMware PowerCLI.
. In **Machines detecteren**, het volgende doen:
    - Geef de FQDN-naam of IP-adres van de vCenter-server.
    - In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die door de collector wordt gebruikt voor het detecteren van virtuele machines op de vCenter-server.
    - In **verzameling bereik**, selecteert u een bereik voor detectie van de virtuele machine. De collector kan alleen detecteren voor virtuele machines binnen het opgegeven bereik. Bereik kan worden ingesteld op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten. 
    - n **Tag categorie voor groepering**, selecteer **geen**.

        ![Selecteer bereik](./media/how-to-scale-assessment/select-scope.png)

1. In **Project selecteren**, geeft u de ID en sleutel voor het project. Als niet kopiëren, de Azure portal openen van de VM-collector. In het project **overzicht** pagina, klikt u op **Machines detecteren**, en kopieer de waarden.  
In **volledige detectie**, het detectieproces bewaken en controleren van metagegevens verzameld van de virtuele machines zijn binnen het bereik. De collector zorgt voor een geschatte detectie.


### <a name="verify-vms-in-the-portal"></a>Controleer of virtuele machines in de portal

Detectie tijd is afhankelijk van hoeveel virtuele machines die u detectie. Normaal gesproken duurt voor 100 virtuele machines, nadat de collector is uitgevoerd op het om een uur voor de detectie is voltooid. 

1. Klik in het project migratie Planner op **beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal weergegeven.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [maakt u een groep](how-to-create-a-group.md) voor evaluatie.
- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.