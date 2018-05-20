---
title: De schaal van detectie en assessment met behulp van Azure migreren | Microsoft Docs
description: Beschrijft hoe vast te stellen van grote aantallen lokale machines met behulp van de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: e0bd62710c47cfdf81535470ef96bad2ab675bb0
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Een grote VMware-omgeving ontdekken en beoordelen

In dit artikel wordt beschreven hoe vast te stellen van grote aantallen on-premises virtuele machines (VM's) met behulp van [Azure migreren](migrate-overview.md). Azure migreren beoordeelt machines om te controleren of ze geschikt voor migratie naar Azure zijn. De service biedt sizing en kosten schattingen voor het uitvoeren van de machines in Azure.

## <a name="prerequisites"></a>Vereisten

- **VMware**: de virtuele machines die u wilt migreren, moet worden beheerd door de vCenter-Server versie 5.5, 6.0 of 6.5. Bovendien moet u één ESXi-host actief versie 5.0 of hoger voor het implementeren van de VM-collector.
- **vCenter account**: U moet een alleen-lezen-account voor toegang tot de vCenter-Server. Azure Migrate gebruikt dit account om de on-premises virtuele machines te detecteren.
- **Machtigingen**: In de vCenter-Server, moet u machtigingen voor het maken van een virtuele machine door een bestand in de indeling eicellen te importeren.
- **Instellingen voor statistieken**: de instellingen van de statistieken voor de vCenter-Server moeten worden ingesteld op niveau 3 voordat u implementatie begint. Als het niveau lager dan 3 is, de beoordeling werkt, maar prestatiegegevens voor opslag en netwerk won't worden verzameld. De aanbevelingen voor de grootte wordt in dit geval gebaseerd op prestatiegegevens voor de CPU en geheugen en configuratiegegevens voor de schijf en netwerkadapters.

## <a name="plan-azure-migrate-projects"></a>Azure migreren projecten plannen

Plan uw detecties en beoordelingen op basis van de volgende beperkingen:

| **Entiteit** | **Limiet voor computer** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Detectie  | 1,500             |
| Evaluatie | 1,500             |

<!-- 
- If you have fewer than 400 machines to discover and assess, you need a single project and a single discovery. Depending on your requirements, you can either assess all the machines in a single assessment or split the machines into multiple assessments. 
- If you have 400 to 1,000 machines to discover, you need a single project with a single discovery. But you will need multiple assessments to assess these machines, because a single assessment can hold up to 400 machines.
- If you have 1,001 to 1,500 machines, you need a single project with two discoveries in it.
- If you have more than 1,500 machines, you need to create multiple projects, and perform multiple discoveries, according to your requirements. For example:
    - If you have 3,000 machines, you can set up two projects with two discoveries, or three projects with a single discovery.
    - If you have 5,000 machines, you can set up four projects: three with a discovery of 1,500 machines, and one with a discovery of 500 machines. Alternatively, you can set up five projects with a single discovery in each one. 
      -->

## <a name="plan-multiple-discoveries"></a>Meerdere detecties plannen

De dezelfde collector voor het migreren van Azure kunt u meerdere detecties op een of meer projecten doen. Houd er rekening mee deze planningsoverwegingen:

- Wanneer u een detectie doen met behulp van de collector Azure migreren, kunt u het detectiebereik instellen naar een map van de vCenter-Server, datacenter, cluster of host.
- Controleer hiervoor meer dan één detectie in vCenter-Server die de virtuele machines die u wilt detecteren in mappen, datacenters, clusters of hosts die ondersteuning bieden voor de beperking van 1500 machines.
- We adviseren dat voor een evaluatie, u machines met afhankelijkheden binnen het hetzelfde project en de evaluatie. Zorg dat afhankelijke machines zich in dezelfde map, datacenter of cluster voor de beoordeling in vCenter-Server.


## <a name="create-a-project"></a>Een project maken

Maak een project Azure migreren volgens uw vereisten:

1. Selecteer in de Azure-portal **maken van een resource**.
2. Zoek naar **Azure Migrate** en selecteer de service **Azure Migrate (preview)** in de zoekresultaten. Selecteer vervolgens **Maken**.
3. Geef de naam van het project en de Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie waarin u wilt maken van het project en selecteer vervolgens **maken**. Houd er rekening mee dat u uw virtuele machines voor een andere doellocatie nog steeds kunt beoordelen. De opgegeven locatie voor het project wordt gebruikt voor het opslaan van de metagegevens die afkomstig zijn van lokale virtuele machines.

## <a name="set-up-the-collector-appliance"></a>Instellen van het toestel collector

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert lokale virtuele VMware-machines en stuurt de metagegevens over deze naar de service Azure migreren. Als u het toestel collector instelt, een bestand eicellen downloaden en importeren naar de lokale vCenter Server-exemplaar.

### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Als u meerdere projecten hebt, moet u voor het downloaden van het toestel collector slechts één keer naar de vCenter-Server. Nadat u downloadt en van het toestel instellen, u deze voor elk project uitvoeren en u de unieke project-ID en sleutel opgeven.

1. Selecteer in het project Azure migreren **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **machines ontdekken**, selecteer **downloaden**, het bestand eicellen wilt downloaden.
3. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. U hebt deze nodig tijdens de configuratie van collector.


### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Controleer dat het bestand eicellen beveiligd is voordat u deze implementeert:

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.

2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Zorg ervoor dat de gegenereerde hash overeenkomt met de volgende instellingen.

    Voor OVA-versie 1.0.9.7

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

    Voor OVA-versie 1.0.9.5

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    Voor OVA-versie 1.0.9.2

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

    Voor OVA-versie 1.0.8.59

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  

    Voor OVA-versie 1.0.8.49

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Voor OVA-versie 1.0.8.40:

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>De collector-VM maken

Het gedownloade bestand importeren in de vCenter-Server:

1. Selecteer in de console van de Client vSphere **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/how-to-scale-assessment/vcenter-wizard.png)

2. In de Wizard OVF sjabloon implementeren > **bron**, geef de locatie van het bestand eicellen.
3. Geef in **Naam** en **Locatie** een beschrijvende naam op voor de collector-VM, en het inventarisobject waarin de virtuele machine wordt gehost.
4. Geef in **Host/Cluster** de host of het cluster op waarop de collector-VM wordt uitgevoerd.
5. Geef in de opslag de opslaglocatie voor de collector-VM op.
6. Geef in **Schijfindeling** het schijftype en de schijfgrootte op.
7. Geef in **Netwerktoewijzing** het netwerk op waarmee de collector-VM verbinding maakt. Het netwerk moet een internetverbinding naar metagegevens wilt verzenden naar Azure. 
8. Controleer en Bevestig de instellingen en selecteer vervolgens **voltooien**.

## <a name="identify-the-id-and-key-for-each-project"></a>Identificeren van de ID en sleutel voor elk project

Als u meerdere projecten hebt, moet u voor het identificeren van de ID en sleutel voor elk criterium. U moet de sleutel tijdens het uitvoeren van de collector voor het detecteren van de virtuele machines.

1. Selecteer in het project **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **project referenties kopiëren**, kopieert u de ID en sleutel voor het project. 
    ![Referenties project kopiëren](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Het niveau van de vCenter-statistieken instellen
Hieronder volgt de lijst met prestatiemeteritems die worden verzameld tijdens de detectie. De prestatiemeteritems zijn standaard beschikbaar op verschillende niveaus in de vCenter-Server. 

U wordt aangeraden dat u algemene optimale (3) voor het niveau van de statistieken zo instellen dat alle items correct zijn verzameld. Als u ingesteld op een lager niveau vCenter hebt, kunnen alleen enkele items volledig worden verkregen met de rest ingesteld op 0. De beoordeling mogelijk onvolledig gegevens vervolgens weer. 

De volgende tabel bevat ook de resultaten van de beoordeling die worden beïnvloed als een bepaald item is niet verzameld.

| Teller                                 | Niveau | Per apparaat niveau | Evaluatie-impact                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| CPU.Usage.Average                       | 1     | N.v.t.               | VM-grootte en de kosten van aanbevolen         |
| Mem.Usage.Average                       | 1     | N.v.t.               | VM-grootte en de kosten van aanbevolen         |
| virtualDisk.read.average                | 2     | 2                | Schijfgrootte opslagkosten en VM-grootte |
| virtualDisk.write.average               | 2     | 2                | Schijfgrootte opslagkosten en VM-grootte |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Schijfgrootte opslagkosten en VM-grootte |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Schijfgrootte opslagkosten en VM-grootte |
| NET.Received.Average                    | 2     | 3                | VM-grootte en het netwerk kosten             |
| NET.transmitted.Average                 | 2     | 3                | VM-grootte en het netwerk kosten             |

> [!WARNING]
> Als u een hoger niveau van de statistieken zojuist hebt geïnstalleerd, zal deze duren per dag voor het genereren van de prestatiemeteritems. Dus is het raadzaam dat u de detectie na één dag uitvoert.

## <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Voor elke detectie dat u wilt uitvoeren, moet u de collector voor het detecteren van virtuele machines in het vereiste bereik uitvoeren. Voer de detecties een na de andere. Gelijktijdige detecties worden niet ondersteund moet, en elke detectieregel een ander bereik.

1.  Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2.  Geef de voorkeursinstellingen voor de taal, de tijdzone en wachtwoorden op voor het apparaat.
3.  Selecteer op het bureaublad de **uitvoeren collector** snelkoppeling.
4.  Open in de collector Azure migreren **vereisten instellen** en vervolgens:

    a. Accepteer de licentievoorwaarden en lees de informatie van derden.

    De collector controleert of de virtuele machine toegang heeft tot internet.

    b. Als de virtuele machine toegang heeft tot internet via een proxyserver, selecteert u **Proxy-instellingen**, en geef de proxy-adres en poort voor luisteren. Geef referenties op als de proxy verificatie nodig heeft.

    De collector controleert of de collector-service wordt uitgevoerd. De service wordt standaard geïnstalleerd op de collector-VM.

    c. Download en installeer de VMware PowerCLI.

5.  Doe het volgende in **vCenter Server-details opgeven**:
    - Geef de FQDN-naam of IP-adres van de vCenter-Server.
    - In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die door de collector wordt gebruikt voor het detecteren van virtuele machines in de vCenter-Server.
    - Selecteer in **Select scope** een bereik voor VM-detectie. De collector kan alleen virtuele machines binnen het opgegeven bereik worden gedetecteerd. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten. 

6.  In **Geef migratieproject**, geeft u de ID en sleutel voor het project. Als u deze niet kopiëren, opent u de Azure-portal van de collector VM. Op het project **overzicht** pagina **Machines detecteren** en kopieer de waarden.  
7.  In **verzameling voortgang**, het detectieproces bewaken en controleren van metagegevens verzameld van de virtuele machines zijn binnen het bereik. De collector geeft aan hoe lang de detectie ongeveer zal duren.


### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

De detectietijd is afhankelijk van het aantal virtuele machines dat u detecteert. Normaal gesproken voor 100 virtuele machines, detectie is voltooid rond een uur nadat de collector is voltooid. 

1. Selecteer in het project migratie Planner **beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal worden weergegeven.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [maakt u een groep](how-to-create-a-group.md) voor evaluatie.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
