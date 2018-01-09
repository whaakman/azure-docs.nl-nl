---
title: De schaal van detectie en assessment met behulp van Azure migreren | Microsoft Docs
description: Beschrijft hoe vast te stellen van grote aantallen lokale machines met behulp van de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 67661e03e65cde3ec2f1aafd5ef755899cf0c77b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detecteren en beoordelen van een grote VMware-omgeving

In dit artikel wordt beschreven hoe vast te stellen van grote aantallen on-premises virtuele machines (VM's) met behulp van [Azure migreren](migrate-overview.md). Azure migreren beoordeelt machines om te controleren of ze geschikt voor migratie naar Azure zijn. De service biedt sizing en kosten schattingen voor het uitvoeren van de machines in Azure.

## <a name="prerequisites"></a>Vereisten

- **VMware**: de virtuele machines die u wilt migreren, moet worden beheerd door de vCenter-Server versie 5.5, 6.0 of 6.5. Bovendien moet u één ESXi-host actief versie 5.0 of hoger voor het implementeren van de VM-collector.
- **vCenter account**: U moet een alleen-lezen-account voor toegang tot de vCenter-Server. Azure migreren gebruikt deze account voor het detecteren van de lokale virtuele machines.
- **Machtigingen**: In de vCenter-Server, moet u machtigingen voor het maken van een virtuele machine door een bestand in de indeling eicellen te importeren.
- **Instellingen voor statistieken**: de instellingen van de statistieken voor de vCenter-Server moeten worden ingesteld op niveau 3 voordat u implementatie begint. Als het niveau lager dan 3 is, de beoordeling werkt, maar prestatiegegevens voor opslag en netwerk won't worden verzameld. De aanbevelingen voor de grootte wordt in dit geval gebaseerd op prestatiegegevens voor de CPU en geheugen en configuratiegegevens voor de schijf en netwerkadapters.

## <a name="plan-azure-migrate-projects"></a>Azure migreren projecten plannen

Plan uw detecties en beoordelingen op basis van de volgende beperkingen:

| **Entiteit** | **Limiet voor computer** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| Detectie  | 1000              |
| Evaluatie | 400               |

- Als u minder dan 400 machines detecteren en te beoordelen hebt, moet u een project en een enkele detectie. Afhankelijk van uw vereisten kunt u alle machines in een enkel beoordeling te beoordelen of de machines splitsen in meerdere beoordelingen. 
- Als er 400-1000 computers te detecteren, moet u een project met een enkele detectie. Maar moet u meerdere beoordelingen om deze machines vast te stellen omdat een één-evaluatie kan maximaal 400 machines bevatten.
- Als u 1,001-1500 machines hebt, moet u een project met twee detecties erin.
- Als u meer dan 1500 machines hebt, moet u meerdere projecten maken en uitvoeren van meerdere detecties volgens uw vereisten. Bijvoorbeeld:
    - Als u 3000 machines hebt, kunt u twee projecten met detecties van twee of drie projecten met een enkele detectie instellen.
    - Als u 5000 machines hebt, kunt u vier projecten instellen: twee met een detectie van 1500 machines en één met een detectie van 500 machines. U kunt ook vijf projecten met een enkele detectie in elk criterium instellen. 

## <a name="plan-multiple-discoveries"></a>Meerdere detecties plannen

De dezelfde collector voor het migreren van Azure kunt u meerdere detecties op een of meer projecten doen. Houd er rekening mee deze planningsoverwegingen:
 
- Wanneer u een detectie doen met behulp van de collector Azure migreren, kunt u het detectiebereik instellen naar een map van de vCenter-Server, datacenter, cluster of host.
- Controleer hiervoor meer dan één detectie in vCenter-Server die de virtuele machines die u wilt detecteren in mappen, datacenters, clusters of hosts die de limiet van 1000 machines ondersteunen.
- We adviseren dat voor een evaluatie, u machines met afhankelijkheden binnen het hetzelfde project en de evaluatie. Zorg dat afhankelijke machines zich in dezelfde map, datacenter of cluster voor de beoordeling in vCenter-Server.


## <a name="create-a-project"></a>Een project maken

Maak een project Azure migreren volgens uw vereisten:

1. Selecteer in de Azure-portal **maken van een resource**.
2. Zoeken naar **Azure migreren**, en selecteer de service **Azure migreren (preview)** in de zoekresultaten. Selecteer vervolgens **Maken**.
3. Geef de naam van het project en de Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie waarin u wilt maken van het project en selecteer vervolgens **maken**. Houd er rekening mee dat u uw virtuele machines voor een andere doellocatie nog steeds kunt beoordelen. De opgegeven locatie voor het project wordt gebruikt voor het opslaan van de metagegevens die afkomstig zijn van lokale virtuele machines.

## <a name="set-up-the-collector-appliance"></a>Instellen van het toestel collector

Azure migreren maakt een lokale virtuele machine het toestel collector genoemd. Deze virtuele machine detecteert lokale virtuele VMware-machines en stuurt de metagegevens over deze naar de service Azure migreren. Als u het toestel collector instelt, een bestand eicellen downloaden en importeren naar de lokale vCenter Server-exemplaar.

### <a name="download-the-collector-appliance"></a>Downloaden van het toestel collector

Als u meerdere projecten hebt, moet u voor het downloaden van het toestel collector slechts één keer naar de vCenter-Server. Nadat u downloadt en van het toestel instellen, u deze voor elk project uitvoeren en u de unieke project-ID en sleutel opgeven.

1. Selecteer in het project Azure migreren **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **machines ontdekken**, selecteer **downloaden**, het bestand eicellen wilt downloaden.
3. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. U moet deze bij het configureren van de collector.

   
### <a name="verify-the-collector-appliance"></a>Controleer of het toestel collector

Controleer dat het bestand eicellen beveiligd is voordat u deze implementeert:

1. Open een opdrachtvenster beheerder op de machine waarop u het bestand hebt gedownload.
2. Voer de volgende opdracht voor het genereren van de hash voor de eicellen:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Voorbeeld van syntaxis:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Zorg ervoor dat de gegenereerde hash overeenkomt met de volgende instellingen.
 
    Voor eicellen versie 1.0.8.49

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    Voor de versie van de eicellen 1.0.8.40:

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>De collector VM maken

Het gedownloade bestand importeren in de vCenter-Server:

1. Selecteer in de console van de Client vSphere **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/how-to-scale-assessment/vcenter-wizard.png)

2. In de Wizard OVF sjabloon implementeren > **bron**, geef de locatie van het bestand eicellen.
3. In **naam** en **locatie**, Geef een beschrijvende naam voor de VM-collector en de inventaris-object waarmee de virtuele machine wordt gehost.
5. In **Host/Cluster**, geeft u de host of cluster op waarmee de VM-collector wordt uitgevoerd.
7. Geef de opslaglocatie voor de collector VM in opslag.
8. In **schijfindeling**, het schijftype en de grootte opgeven.
9. In **netwerktoewijzing**, geeft u het netwerk waarmee de VM-collector verbinding maakt. Het netwerk moet een internetverbinding naar metagegevens wilt verzenden naar Azure. 
10. Controleer en Bevestig de instellingen en selecteer vervolgens **voltooien**.

## <a name="identify-the-id-and-key-for-each-project"></a>Identificeren van de ID en sleutel voor elk project

Als u meerdere projecten hebt, moet u voor het identificeren van de ID en sleutel voor elk criterium. U moet de sleutel tijdens het uitvoeren van de collector voor het detecteren van de virtuele machines.

1. Selecteer in het project **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. 
    ![Referenties project kopiëren](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Het niveau van de vCenter-statistieken instellen
Hieronder volgt de lijst met prestatiemeteritems die worden verzameld tijdens de detectie. De prestatiemeteritems zijn standaard beschikbaar op verschillende niveaus in de vCenter-Server. 

U wordt aangeraden dat u algemene optimale (3) voor het niveau van de statistieken zo instellen dat alle items correct zijn verzameld. Als u ingesteld op een lager niveau vCenter hebt, kunnen alleen enkele items volledig worden verkregen met de rest ingesteld op 0. De beoordeling mogelijk onvolledig gegevens vervolgens weer. 

De volgende tabel bevat ook de resultaten van de beoordeling die worden beïnvloed als een bepaald item is niet verzameld.

|Prestatiemeteritems                                  |Niveau    |Per apparaat niveau  |Evaluatie-impact                               |
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

## <a name="run-the-collector-to-discover-vms"></a>De collector voor het detecteren van virtuele machines worden uitgevoerd

Voor elke detectie dat u wilt uitvoeren, moet u de collector voor het detecteren van virtuele machines in het vereiste bereik uitvoeren. Voer de detecties een na de andere. Gelijktijdige detecties worden niet ondersteund moet, en elke detectieregel een ander bereik.

1. In de console vSphere Client met de rechtermuisknop op de virtuele machine > **Console openen**.
2. Geef de taal, de tijdzone en het wachtwoord voorkeuren voor het apparaat.
3. Selecteer op het bureaublad de **uitvoeren collector** snelkoppeling.
4. Open in de collector Azure migreren **vereisten instellen** en vervolgens:

   a. Accepteer de licentievoorwaarden en lees de informatie van derden.

   De collector controleert of de virtuele machine toegang tot internet.
   
   b. Als de virtuele machine toegang heeft tot internet via een proxyserver, selecteert u **Proxy-instellingen**, en geef de proxy-adres en poort voor luisteren. Geef referenties op als de proxy moet worden geverifieerd.

   De collector wordt gecontroleerd of de collectorservice wordt uitgevoerd. De service wordt standaard geïnstalleerd op de VM-collector.

   c. Download en installeer de VMware PowerCLI.

5. In **gegevens vCenter-Server opgeven**, het volgende doen:
    - Geef de FQDN-naam of IP-adres van de vCenter-Server.
    - In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die door de collector wordt gebruikt voor het detecteren van virtuele machines in de vCenter-Server.
    - In **Selecteer bereik**, selecteert u een bereik voor detectie van de virtuele machine. De collector kan alleen virtuele machines binnen het opgegeven bereik worden gedetecteerd. Bereik kan worden ingesteld op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten. 

6. In **Geef migratieproject**, geeft u de ID en sleutel voor het project. Als u deze niet kopiëren, opent u de Azure-portal van de collector VM. Op het project **overzicht** pagina **Machines detecteren** en kopieer de waarden.  
7. In **verzameling voortgang**, het detectieproces bewaken en controleren van metagegevens verzameld van de virtuele machines zijn binnen het bereik. De collector zorgt voor een geschatte detectie.


### <a name="verify-vms-in-the-portal"></a>Controleer of virtuele machines in de portal

Detectie tijd is afhankelijk van hoeveel virtuele machines die u detectie. Normaal gesproken voor 100 virtuele machines, detectie is voltooid rond een uur nadat de collector is voltooid. 

1. Selecteer in het project migratie Planner **beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal weergegeven.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [maakt u een groep](how-to-create-a-group.md) voor evaluatie.
- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.
