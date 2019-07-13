---
title: VMware-VM's beoordelen voor migratie naar Azure met Azure Migrate
description: Hierin wordt beschreven hoe u on-premises VMware-VM's voor migratie naar Azure met behulp van Azure Migrate beoordeelt.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: 5f70037b1e6ce284b55ff5ff0ae38eb50c320122
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868663"
---
# <a name="assess-vmware-vms-with-azure-migrate-server-assessment"></a>Virtuele VMware-machines met Azure beoordelen migreren: Server Assessment

Dit artikel ziet u hoe u on-premises VMware-machines, met behulp van de Azure Migrate beoordeelt: Server Assessment tool.

[Azure Migrate](migrate-services-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV).



Deze zelfstudie is de tweede in een serie die laat zien hoe u om te beoordelen en migreren van virtuele VMware-machines naar Azure. In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Instellen van een Azure Migrate-project.
> * Instellen van een Azure Migrate-apparaat dat on-premises voor het evalueren van virtuele machines wordt uitgevoerd.
> * Continue detectie van on-premises VM's starten. Het apparaat verzendt configuratie-en prestatiegegevens voor gedetecteerde virtuele machines naar Azure.
> * Gedetecteerde virtuele machines groeperen en beoordelen van de VM-groep.
> * Een nieuwe beoordeling.



> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Lees de artikelen met procedures voor gedetailleerde instructies.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

- [Volledige](tutorial-prepare-vmware.md) de eerste zelfstudie in deze reeks. Als u dit niet doet, werkt de instructies in deze zelfstudie niet.
- Dit is wat u moet dat hebt gedaan in de eerste zelfstudie:
    - [Instellen van Azure-machtigingen](tutorial-prepare-vmware.md#prepare-azure) voor Azure Migrate.
    - [VMware voorbereiden](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) voor evaluatie. VMware-instellingen moeten worden geverifieerd en hebt u machtigingen voor het maken van een VMware-VM met een OVA-sjabloon. U hebt ook een account instellen voor VM-detectie. Vereiste poorten beschikbaar moeten zijn en u moet rekening houden met de URL's die nodig zijn voor toegang tot Azure.


## <a name="set-up-an-azure-migrate-project"></a>Instellen van een Azure Migrate-project

Stel als volgt een nieuwe Azure Migrate-project in.

1. In Azure portal > **alle services**, zoeken naar **Azure Migrate**.
2. Onder **Services**, selecteer **Azure Migrate**.
3. In **overzicht**onder **ontdekken, beoordelen en servers migreren**, klikt u op **evalueren en migreren van servers**.

    ![Detecteren en evalueren van servers](./media/tutorial-assess-vmware/assess-migrate.png)

4. In **aan de slag**, klikt u op **toevoegen van extra**.
5. In **Migrate-project**, selecteer uw Azure-abonnement en maak een resourcegroep als u dit niet hebt.     
6. In **projectdetails**, geef de naam van het project en de locatie waarin u wilt maken van het project. Azië, Europa, Verenigd Koninkrijk en de Verenigde Staten worden ondersteund.

    - De geografische locatie van project wordt alleen gebruikt om de metagegevens die zijn verzameld van on-premises VM's worden opgeslagen.
    - U kunt een doelregio selecteren wanneer u een migratie uitvoert.

    ![Een Azure Migrate-project maken](./media/tutorial-assess-vmware/migrate-project.png)


7. Klik op **Volgende**.
8. In **Selecteer assessment tool**, selecteer **Azure Migrate: Server-evaluatie** > **volgende**.

    ![Een Azure Migrate-project maken](./media/tutorial-assess-vmware/assessment-tool.png)

9. In **hulpprogramma voor migratie selecteren**, selecteer **toevoegen van een hulpprogramma voor migratie van voorlopig overslaan** > **volgende**.
10. In **bekijken en toevoegen van extra**, Controleer de instellingen en klik op **hulpprogramma's toevoegen**.
11. Wacht een paar minuten voor de Azure Migrate-project te implementeren. U gaat dan naar de pagina van het project. Als u het project niet ziet, kunt u het openen van **Servers** in het dashboard Azure Migrate.


## <a name="set-up-the-appliance-vm"></a>Het VM-apparaat instellen

Azure Migrate: Server-evaluatie wordt uitgevoerd een lichtgewicht VMware-VM-apparaat.

- Dit apparaat VM-detectie wordt uitgevoerd en verzendt de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure Migrate-Server-evaluatie.
- Instellen van het apparaat u:
    - Een sjabloon voor OVA-bestanden downloaden en importeren naar de vCenter-Server.
    - Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan.
    - Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.
- U kunt meerdere apparaten voor een enkel Azure Migrate-project instellen. In alle apparaten, wordt detectie van maximaal 35.000 machines ondersteund. Een maximum van 10.000 servers kan per apparaat worden gedetecteerd.

### <a name="download-the-ova-template"></a>Het OVA-sjabloon downloaden

1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **ontdekken**.
2. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **Ja, met VMWare vSphere-hypervisor**.
3. Klik op **downloaden** voor het downloaden van de. Sjabloon voor OVA-bestanden.

    ![.OVA-bestand downloaden](./media/tutorial-assess-vmware/download-ova.png)


### <a name="verify-security"></a>Beveiliging controleren

Controleer of het OVA-bestand beveiligd is voordat u deze implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze waarden voor versie 1.19.06.27 is. 

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | 605d208ac5f4173383f616913441144e
  SHA256 | 447d16bd55f20f945164a1189381ef6e98475b573d6d1c694f3e5c172cfc30d4


### <a name="create-the-appliance-vm"></a>De VM-toepassing maken

Importeer het gedownloade bestand en een virtuele machine maken.

1. Klik in de Client vSphere-console op **Bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/tutorial-assess-vmware/deploy-ovf.png)

2. In de Wizard OVF-sjabloon implementeren > **bron**, geef de locatie van het OVA-bestand.
3. In **naam** en **locatie**, Geef een beschrijvende naam voor de virtuele machine. Selecteer de inventaris-object waarin de virtuele machine wordt gehost.
5. In **Host/Cluster**, geeft u de host of cluster op waarmee de virtuele machine wordt uitgevoerd.
6. In **opslag**, geef de opslaglocatie voor de virtuele machine.
7. Geef in **Schijfindeling** het schijftype en de schijfgrootte op.
8. In **netwerktoewijzing**, waarmee de virtuele machine verbinding maken via het netwerk opgeven. Het netwerk moet verbinding met internet, metagegevens verzenden naar Azure Migrate-Server-evaluatie.
9. Controleer en bevestig de instellingen en klik op **Voltooien**.


### <a name="verify-appliance-access-to-azure"></a>Apparaat toegang tot Azure controleren

Zorg ervoor dat het apparaat VM verbinding met maken kan [Azure-URL's](migrate-support-matrix-vmware.md#assessment-url-access-requirements).


### <a name="configure-the-appliance"></a>Configureer het apparaat

Instellen van het apparaat met behulp van de volgende stappen uit.

1. Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2. Geef de taal, de tijdzone en het wachtwoord voor het apparaat.
3. Open een browser op elke computer die u kunt verbinding maken met de virtuele machine en open de URL van de web-app van het apparaat: **https://*apparaatnaam of IP-adres*: 44368**.

   U kunt ook de app openen vanaf het bureaublad apparaat door te klikken op de snelkoppeling naar de app.
4. In de web-app > **vereisten instellen**, doet u het volgende:
    - **Licentie**: Accepteer de licentievoorwaarden en lees de informatie van derden.
    - **Connectiviteit**: De app controleert of de virtuele machine toegang heeft tot internet heeft. Als de virtuele machine gebruikmaakt van een proxy:
        - Klik op **Proxy-instellingen**, en geeft u het proxyadres en de luisterende poorten in het formulier http://ProxyIPAddress of http://ProxyFQDN.
        - Geef referenties op als de proxy verificatie nodig heeft.
        - Alleen HTTP-proxy wordt ondersteund.
    - **Tijd synchronisatie**: de tijd op het apparaat moet worden gesynchroniseerd met de tijd voor de detectie van goed te laten werken.
    - **Updates installeren**: Het apparaat zorgt ervoor dat de meest recente updates zijn geïnstalleerd.
    - **Installeer VDDK**: Het apparaat controleert waarop VMWare vSphere virtuele schijf Development Kit (VDDK) is geïnstalleerd.
        - Azure Migrate: Migratie-server maakt gebruik van de VDDK op virtuele machines tijdens de migratie naar Azure repliceren.
        - Download VDDK 6.7 van VMware en pak het gedownloade zip-inhoud naar de opgegeven locatie op het apparaat.

### <a name="register-the-appliance-with-azure-migrate"></a>Het apparaat registreren met Azure Migrate

1. Klik op **aanmelden**. Als deze niet wordt weergegeven, zorg er dan voor dat u uitgeschakeld hebt met het pop-upblokkering in de browser.
2. Meld u aan met uw Azure-referenties op het nieuwe tabblad.
    - Meld u aan met uw gebruikersnaam en wachtwoord.
    - Meld u aan met een PINCODE wordt niet ondersteund.
3. Wanneer is aangemeld, gaat u terug naar de web-app.
2. Selecteer het abonnement waarin de Azure Migrate-project is gemaakt, en selecteer vervolgens het project.
3. Geef een naam voor het apparaat. De naam moet alfanumeriek met 14 tekens of minder.
4. Klik op **registreren**.


## <a name="start-continuous-discovery"></a>Continue detectie starten

Nu verbinding maken tussen het apparaat en vCenter-Server en start de VM-detectie.

1. In **opgeven van de vCenter-Serverdetails**, geef de FQDN-naam of IP-adres van de vCenter-Server. U kunt laat de standaardpoort, of een aangepaste poort op waarop de vCenter-Server luistert.
2. In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die het apparaat wordt gebruikt voor het detecteren van virtuele machines op de vCenter-server. Zorg ervoor dat het account heeft de [vereiste machtigingen voor de detectie van](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions). U kunt het bereik van de detectie door het beperken van toegang tot de vCenter-account. meer informatie over het bereik van detectie [hier](tutorial-assess-vmware.md#scoping-discovery).
3. Klik op **-verbinding valideren** om ervoor te zorgen dat het apparaat verbinding met vCenter-Server maken kan.
4. Nadat de verbinding tot stand is gebracht, klikt u op **opslaan en detectie starten**.

Hiermee start u detectie. Het duurt ongeveer 15 minuten voor metagegevens van de gedetecteerde virtuele machines in de portal worden weergegeven.

### <a name="scoping-discovery"></a>Bereik van detectie

Detectie kan worden gericht op door het beperken van toegang van de vCenter-account gebruikt voor de detectie. U kunt het bereik instellen op de vCenter-Server datacenters, clusters, map van clusters, hosts, de map van de hosts of afzonderlijke virtuele machines. 

> [!NOTE]
> Server-evaluatie is vandaag de dag niet kunnen VM's detecteren als de vCenter-account toegang verleend op het niveau van vCenter-map voor virtuele machine heeft. Als u het bereik van uw detectie door de mappen van de virtuele machine wilt, kunt u doen zodat door ervoor te zorgen dat de vCenter-account alleen-lezentoegang toegewezen op het niveau van een virtuele machine heeft.  Hieronder vindt u instructies over hoe u dit kunt doen:
>
> 1. Alleen-lezen machtigingen op alle virtuele machines in de VM-mappen die u als bereik voor de detectie wilt toewijzen. 
> 2. Alleen-lezen toegang verlenen tot alle bovenliggende objecten waar de virtuele machines worden gehost. Alle bovenliggende objecten - host, de map van hosts, cluster, map van clusters - in de hiërarchie tot aan het datacenter zijn om op te nemen. U hoeft niet de machtigingen op alle onderliggende objecten doorgeven.
> 3. Gebruik de referenties voor detectie selecteren datacenter als *Collection Scope*. Het instellen van RBAC zorgt ervoor dat de bijbehorende vCenter-gebruiker toegang tot alleen tenant-specifieke virtuele machines heeft.
>
> Houd er rekening mee dat map van hosts en clusters worden ondersteund.

### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Na de detectie, kunt u controleren of de virtuele machines worden weergegeven in de Azure-portal.

1. Open het dashboard Azure Migrate.
2. In **Azure Migrate - Servers** > **Azure Migrate: Server-evaluatie** pagina, klikt u op het pictogram met het aantal voor **ontdekte servers**.

## <a name="set-up-an-assessment"></a>Instellen van een evaluatie

Er zijn twee soorten evaluaties die u kunt maken met Azure Migrate: Server-evaluatie.

**Evaluatie** | **Details** | **Gegevens**
--- | --- | ---
**Performance-based** | Evaluaties op basis van verzamelde prestatiegegevens | **VM-grootte aanbevolen**: Op basis van CPU en geheugen gegevens over het gebruik.<br/><br/> **Schijftype (standard of premium beheerde schijven) aanbevolen**: Op basis van de IOPS en doorvoer van de on-premises-schijven.
**As on-premises** | Evaluaties op basis van on-premises formaat wijzigen. | **VM-grootte aanbevolen**: Op basis van de on-premises VM-grootte<br/><br> **Aanbevolen schijftype**: Op basis van de instelling van het type opslag die u voor de evaluatie selecteert.


### <a name="run-an-assessment"></a>Een evaluatie uitvoeren

Voer een evaluatie als volgt uit:

1. Controleer de [aanbevolen procedures](best-practices-assessment.md) voor het maken van evaluaties.
2. In de **Servers** tabblad, in **Azure Migrate: Server-evaluatie** tegel, klikt u op **evalueren**.

    ![Evalueren](./media/tutorial-assess-vmware/assess.png)

2. In **beoordelen servers**, Geef een naam voor de evaluatie.
3. Klik op **Alles weergeven** om de evaluatie-eigenschappen te controleren.

    ![Evaluatie-eigenschappen](./media/tutorial-assess-vmware/view-all.png)

3. In **Selecteer of maak een groep**, selecteer **nieuw**, en geef een groepsnaam op. Een groep worden verzameld van een of meer virtuele machines voor evaluatie.
4. In **computers toevoegen aan de groep**, VM's om toe te voegen aan de groep selecteren.
5. Klik op **evaluatie maken** aan de groep maken en uitvoeren van de evaluatie.

    ![Een evaluatie maken](./media/tutorial-assess-vmware/assessment-create.png)

6. Nadat de evaluatie is gemaakt, weer te geven in **Servers** > **Azure Migrate: Server-evaluatie** > **evaluaties**.
7. Klik op **Evaluatie exporteren** om deze te downloaden als een Excel-bestand.



## <a name="review-an-assessment"></a>Een evaluatie bekijken

Een evaluatie worden beschreven:

- **Azure-gereedheid**: Of de VM's zijn geschikt voor migratie naar Azure.
- **Schatting van de maandelijkse kosten**: De geschatte maandelijkse berekenings- en opslagkosten voor het uitvoeren van de virtuele machines in Azure.
- **Maandelijkse opslag raming van kosten**: Geschatte kosten voor schijfopslag na de migratie.

### <a name="view-an-assessment"></a>Een evaluatie weergeven

1. In **migratie doelstellingen** >  **Servers**, klikt u op **evaluaties** in **Azure Migrate: Server-evaluatie**.
2. In **evaluaties**, klikt u op een evaluatie om dit te openen.

    ![Evaluatie-overzicht](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure-gereedheid controleren

1. In **Azure-gereedheid**, controleren of virtuele machines gereed zijn voor migratie naar Azure.
2. Controleer de status van de virtuele machine:
    - **Gereed voor Azure**: Azure Migrate adviseert om een VM-grootte en kosten schattingen voor virtuele machines in de evaluatie.
    - **Gereed met voorwaarden**: Problemen en voorgestelde oplossing bevat.
    - **Niet gereed voor Azure**: Problemen en voorgestelde oplossing bevat.
    - **Gereedheid onbekend**: Wanneer Azure Migrate kan geen beoordeling van gereedheid vanwege problemen met de beschikbaarheid van gegevens gebruikt.

2. Klik op een **Azure-gereedheid** status. U kunt details van de gereedheid van de virtuele machine weergeven en inzoomen op Zie VM details, met inbegrip van compute, opslag- en netwerkinstellingen.



### <a name="review-cost-details"></a>Details van toegangsbeoordeling kosten

Deze weergave toont de geschatte kosten voor berekeningen en opslag van het uitvoeren van virtuele machines in Azure.

1. Bekijk de maandelijkse kosten voor berekeningen en opslag. Kosten worden samengevoegd voor alle virtuele machines in de geëvalueerde groep.

    - De geschatte kosten zijn gebaseerd op de aanbevelingen voor de grootte voor een machine en de schijven en eigenschappen.
    - Geschatte maandelijkse kosten voor rekenen en opslag worden weergegeven.
    - Er is de schatting van de kosten voor het uitvoeren van de on-premises VM's als IaaS-VM's. Azure Migrate-Server-evaluatie, geen rekening gehouden met PaaS of SaaS-kosten.

2. U kunt per maand opslag kostenramingen bekijken. Deze weergave toont de samengevoegde kosten voor de geëvalueerde groep splitsen in op verschillende soorten schijven voor opslag.
3. U kunt inzoomen op Bekijk de details voor specifieke virtuele machines.


### <a name="review-confidence-rating"></a>Betrouwbaarheidsclassificatie controleren

Wanneer u evaluaties op basis van prestaties uitvoert, wordt een betrouwbaarheidsclassificatie is toegewezen aan de evaluatie.

![Betrouwbaarheidsclassificatie](./media/tutorial-assess-vmware/confidence-rating.png)

- Een score van 1 ster (laagste) tot 5 sterren (hoogste) is toegewezen.
- De betrouwbaarheidsclassificatie kunt u een schatting maken van de betrouwbaarheid van de aanbevelingen voor de grootte geleverd door de evaluatie.
- De betrouwbaarheidsclassificatie is gebaseerd op de beschikbaarheid van gegevenspunten die nodig zijn voor het berekenen van de evaluatie.

Vertrouwen van de classificaties voor een evaluatie zijn als volgt.

**Beschikbaarheid van gegevenspunt** | **Betrouwbaarheidsclassificatie**
--- | ---
0%-20% | 1 ster
21%-40% | 2 sterren
41%-60% | 3 sterren
61%-80% | 4 sterren
81%-100% | 5 sterren

[Meer informatie](best-practices-assessment.md#best-practices-for-confidence-ratings) over aanbevolen procedures voor het vertrouwen van classificaties.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Instellen van een apparaat Azure Migrate
> * Gemaakt en een evaluatie gecontroleerd

Verder met de derde zelfstudie in de reeks, voor informatie over het migreren van virtuele VMware-machines naar Azure met Azure Migrate-servermigratie.

> [!div class="nextstepaction"]
> [Virtuele VMware-machines migreren](./tutorial-migrate-vmware.md)
