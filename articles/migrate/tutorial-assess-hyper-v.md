---
title: Hyper-V-machines evalueren voor migratie naar Azure met Azure Migrate | Microsoft Docs
description: Hierin wordt beschreven hoe u on-premises Hyper-V VM's voor migratie naar Azure met behulp van Azure Migrate beoordeelt.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 83567a45980b29931f9b68bd6d60df0d427b09de
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813021"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Evalueren van Hyper-V-machines met Azure Migrate Server-evaluatie

Dit artikel ziet u hoe u on-premises Hyper-V-machines, met behulp van de Azure Migrate beoordeelt: Server Assessment tool.

[Azure Migrate](migrate-services-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV).



Deze zelfstudie is de tweede in een serie die laat zien hoe u om te beoordelen en Hyper-V-machines migreren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van een Azure Migrate-project.
> * Instellen en registreer een apparaat Azure Migrate.
> * Continue detectie van on-premises VM's starten.
> * Gedetecteerde virtuele machines groeperen en beoordelen van de groep.
> * Een nieuwe beoordeling.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Lees de artikelen met procedures voor gedetailleerde instructies.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

- [Volledige](tutorial-prepare-hyper-v.md) de eerste zelfstudie in deze reeks. Als u dit niet doet, werkt de instructies in deze zelfstudie niet.
- Dit is wat u moet dat hebt gedaan in de eerste zelfstudie:
    - [Instellen van Azure-machtigingen](tutorial-prepare-hyper-v.md#prepare-azure) voor Azure Migrate.
    - [Hyper-V voorbereiden](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-assessment) clusters, hosts en virtuele machines voor evaluatie.

## <a name="set-up-an-azure-migrate-project"></a>Instellen van een Azure Migrate-project

1. In Azure portal > **alle services**, zoeken naar **Azure Migrate**.
2. Selecteer in de lijst met zoekresultaten **Azure Migrate**.
3. In **overzicht**onder **ontdekken, beoordelen en servers migreren**, klikt u op **evalueren en migreren van servers**.

    ![Detecteren en evalueren van servers](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. In **aan de slag**, klikt u op **toevoegen van extra**.
5. In de **Migrate-project** tabblad, selecteert u uw Azure-abonnement en maak een resourcegroep als u dit niet hebt.
6. In **projectdetails**, geef de naam van het project en de regio waarin u wilt maken van het project.


    ![Een Azure Migrate-project maken](./media/tutorial-assess-hyper-v/migrate-project.png)

    U kunt een Azure Migrate-project maken in deze regio's.

    **Geografie** | **Regio**
    --- | ---
    Azië  | Azië - zuidoost
    Europa | Europa - noord of Europa - west
    Verenigd Koninkrijk |  UK-Zuid of UK-West
    Verenigde Staten | VS-Oost, VS-West 2 en West-Centraal VS

    - De regio van het project wordt alleen gebruikt om de metagegevens die zijn verzameld van on-premises VM's worden opgeslagen.
    - U kunt een andere Azure-doelregio selecteren wanneer u de virtuele machines migreert. Alle Azure-regio's worden ondersteund voor migratiedoel.

7. Klik op **Volgende**.
8. In **Selecteer assessment tool**, selecteer **Azure Migrate: Server-evaluatie** > **volgende**.

    ![Een Azure Migrate-project maken](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. In **hulpprogramma voor migratie selecteren**, selecteer **toevoegen van een hulpprogramma voor migratie van voorlopig overslaan** > **volgende**.
10. In **bekijken en toevoegen van extra**, Controleer de instellingen en klik op **hulpprogramma's toevoegen**.
11. Wacht een paar minuten voor de Azure Migrate-project te implementeren. U gaat dan naar de pagina van het project. Als u het project niet ziet, kunt u het openen van **Servers** in het dashboard Azure Migrate.




## <a name="set-up-the-appliance-vm"></a>Het VM-apparaat instellen

Azure Migrate-Server-evaluatie wordt uitgevoerd een lichtgewicht Hyper-V-VM-apparaat.

- Dit apparaat VM-detectie wordt uitgevoerd en verzendt de gegevens van de metagegevens en prestaties van de virtuele machine voor het migreren van Azure: Server-evaluatie.
- Instellen van het apparaat u:
    - Download een gecomprimeerde Hyper-V-VHD vanuit Azure portal.
    - Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan.
    - Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.

### <a name="download-the-vhd"></a>Downloaden van de VHD

Download de ingepakte VHD-sjabloon voor het apparaat.

1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **ontdekken**.
2. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **Ja, met Hyper-V**.
3. Klik op **downloaden** om de VHD-bestand te downloaden.

    ![Virtuele machine downloaden](./media/tutorial-assess-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Beveiliging controleren

Controleer of het ZIP-bestand beveiligd, is voordat u deze implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Voer de volgende opdracht uit voor het genereren van de hash voor de VHD
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  Voor versie 1.19.06.27 van het toestel, de gegenereerde hash moet overeenkomen met deze instellingen.

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | 3681f745fa2b0a0a6910707d85161ec5
  SHA256 | e6ca109afab9657bdcfb291c343b3e3abced9a273d25273059171f9954d25832



### <a name="create-the-appliance-vm"></a>De VM-toepassing maken

Importeer het gedownloade bestand en de virtuele machine maken.

1. Pak het gezipte VHD-bestand naar een map op de Hyper-V-host die als voor de virtuele machine van het apparaat host fungeert. Drie mappen worden geëxtraheerd.
2. Open Hyper-V-beheer. In **acties**, klikt u op **virtuele Machine importeren**.

    ![VHD implementeren](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. In de Wizard virtuele Machine importeren > **voordat u begint met**, klikt u op **volgende**.
3. In **vinden map**, geeft u de map met de uitgepakte VHD. Klik op **Volgende**.
1. In **virtuele Machine selecteren**, klikt u op **volgende**.
2. In **importtype Kies**, klikt u op **de virtuele machine kopiëren (Maak een nieuwe unieke ID)** . Klik op **Volgende**.
3. In **doel kiezen**, laat de standaardinstelling. Klik op **Volgende**.
4. In **opslagmappen**, laat de standaardinstelling. Klik op **Volgende**.
5. In **netwerk kiezen**, geef de virtuele switch die de virtuele machine wilt gebruiken. De switch moet verbinding met internet om gegevens te verzenden naar Azure.
6. In **samenvatting**, Controleer de instellingen. Klik vervolgens op **voltooien**.
7. In Hyper-V-beheer > **virtuele Machines**, start de virtuele machine.


### <a name="verify-appliance-access-to-azure"></a>Apparaat toegang tot Azure controleren

Zorg ervoor dat het apparaat VM verbinding met maken kan [Azure-URL's](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access).

### <a name="configure-the-appliance"></a>Configureer het apparaat

Het apparaat voor de eerste keer instellen.

1. In Hyper-V-beheer > **virtuele Machines**, met de rechtermuisknop op de virtuele machine > **Connect**.
2. Geef de taal, de tijdzone en het wachtwoord voor het apparaat.
3. Open een browser op elke computer die u kunt verbinding maken met de virtuele machine en open de URL van de web-app van het apparaat: **https://*apparaatnaam of IP-adres*: 44368**.

   U kunt ook de app openen vanaf het bureaublad apparaat door te klikken op de snelkoppeling naar de app.
1. In de web-app > **vereisten instellen**, doet u het volgende:
    - **Licentie**: Accepteer de licentievoorwaarden en lees de informatie van derden.
    - **Connectiviteit**: De app controleert of de virtuele machine toegang heeft tot internet heeft. Als de virtuele machine gebruikmaakt van een proxy:
        - Klik op **Proxy-instellingen**, en geeft u het proxyadres en de luisterende poorten in het formulier http://ProxyIPAddress of http://ProxyFQDN.
        - Geef referenties op als de proxy verificatie nodig heeft.
        - Alleen HTTP-proxy wordt ondersteund.
    - **Tijd synchronisatie**: Tijd is geverifieerd. De tijd op het apparaat moet worden gesynchroniseerd met de tijd voor internet voor VM-detectie goed te laten werken.
    - **Updates installeren**: Azure Migrate-Server-evaluatie controleert of het apparaat de meest recente updates zijn geïnstalleerd.

### <a name="register-the-appliance-with-azure-migrate"></a>Het apparaat registreren met Azure Migrate

1. Klik op **aanmelden**. Als deze niet wordt weergegeven, zorg er dan voor dat u uitgeschakeld hebt met het pop-upblokkering in de browser.
2. Meld u aan met uw Azure-referenties op het nieuwe tabblad.
    - Meld u aan met uw gebruikersnaam en wachtwoord.
    - Aanmelden met een PINCODE wordt niet ondersteund.
3. Wanneer is aangemeld, gaat u terug naar de web-app.
4. Selecteer het abonnement waarin de Azure Migrate-project is gemaakt. Selecteer vervolgens het project.
5. Geef een naam voor het apparaat. De naam moet alfanumeriek met 14 tekens of minder.
6. Klik op **registreren**.


### <a name="delegate-credentials-for-smb-vhds"></a>Gemachtigde referenties voor SMB-VHD 's

Als u VHD's op midden-en kleinbedrijf uitvoert, moet u de overdracht van referenties van het apparaat naar de Hyper-V-hosts inschakelen. Als u dit niet doet op elke host in de [vorige zelfstudie](tutorial-prepare-hyper-v.md#enable-credssp-on-hosts), kan deze nu van het apparaat:

1. Op het apparaat VM, moet u deze opdracht uitvoeren. HyperVHost1/HyperVHost2 zijn voorbeeldnamen host.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. U kunt ook dit doen in de Editor voor lokaal groepsbeleid op het apparaat:
    - In **lokaal computerbeleid** > **Computerconfiguratie**, klikt u op **Beheersjablonen** > **System**  >  **Delegatie referenties**.
    - Dubbelklik op **delegeren van nieuwe referenties toestaan**, en selecteer **ingeschakeld**.
    - In **opties**, klikt u op **weergeven**, en Voeg elke Hyper-V-host die u detecteren aan de lijst wilt met **wsman /** als voorvoegsel.
    - In **referenties**, dubbelklikt u op **toestaan delegeren van nieuwe referenties met alleen NTLM-server-verificatie**. Opnieuw toevoegen van elke Hyper-V-host die u detecteren aan de lijst wilt met **wsman /** als voorvoegsel.

## <a name="start-continuous-discovery"></a>Continue detectie starten

Verbinding maken tussen het apparaat en Hyper-V-hosts of clusters, en start de VM-detectie.

1. In **gebruikersnaam** en **wachtwoord**, geef de accountreferenties die het apparaat wordt gebruikt voor het detecteren van virtuele machines. Geef een beschrijvende naam voor de referenties op en klikt u op **details opslaan**.
2. Klik op **toevoegen host**, en geef de details van Hyper-V-host/het cluster.
3. Klik op **valideren**. Na de validatie, wordt het aantal VM's die kunnen worden gedetecteerd op elke host/het cluster weergegeven.
    - Als validatie voor een host mislukt, controleert u de fout door de muiswijzer op het pictogram in de **Status** kolom. Los problemen op en Valideer opnieuw.
    - Als u wilt verwijderen van hosts of clusters, selecteer > **verwijderen**.
    - U kunt een specifieke host niet verwijderen uit een cluster. U kunt alleen het hele cluster verwijderen.
    - U kunt een cluster kunt toevoegen, zelfs als er problemen met specifieke hosts in het cluster zijn.
4. Na de validatie, klikt u op **opslaan en detectie starten** om de detectieproces te starten.

Hiermee start u detectie. Het duurt ongeveer 15 minuten voor metagegevens van de gedetecteerde virtuele machines in Azure portal worden weergegeven.

### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Wanneer de detectie is voltooid, kunt u controleren of de virtuele machines worden weergegeven in de portal.

1. Open het dashboard Azure Migrate.
2. In **Azure Migrate - Servers** > **Azure Migrate: Server-evaluatie** pagina, klikt u op het pictogram met het aantal voor **ontdekte servers**.

## <a name="set-up-an-assessment"></a>Instellen van een evaluatie

Er zijn twee soorten evaluaties die u kunt uitvoeren met behulp van Azure Migrate-Server-evaluatie.

**Evaluatie** | **Details** | **Gegevens**
--- | --- | ---
**Performance-based** | Evaluaties op basis van verzamelde prestatiegegevens | **VM-grootte aanbevolen**: Op basis van CPU en geheugen gegevens over het gebruik.<br/><br/> **Schijftype (standard of premium beheerde schijven) aanbevolen**: Op basis van de IOPS en doorvoer van de on-premises-schijven.
**As on-premises** | Evaluaties op basis van on-premises formaat wijzigen. | **VM-grootte aanbevolen**: Op basis van de on-premises VM-grootte<br/><br> **Aanbevolen schijftype**: Op basis van de instelling van het type opslag die u voor de evaluatie selecteert.



### <a name="run-an-assessment"></a>Een evaluatie uitvoeren

Voer een evaluatie als volgt uit:

1. Controleer de [aanbevolen procedures](best-practices-assessment.md) voor het maken van evaluaties.
2. In **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **evalueren**.

    ![Evalueren](./media/tutorial-assess-hyper-v/assess.png)

3. In **beoordelen Servers**, Geef een naam voor de evaluatie.
4. Klik op **Alles weergeven** om de evaluatie-eigenschappen te controleren.

    ![Evaluatie-eigenschappen](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. In **Selecteer of maak een groep**, selecteer **nieuw** en geef een groepsnaam op. Een groep worden verzameld van een of meer virtuele machines voor evaluatie.
4. In **computers toevoegen aan de groep**, VM's om toe te voegen aan de groep selecteren.
5. Klik op **evaluatie maken** aan de groep maken en uitvoeren van de evaluatie.

    ![Een evaluatie maken](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Nadat de evaluatie is gemaakt, weer te geven in **Servers** > **Azure Migrate: Server-evaluatie**.
7. Klik op **Evaluatie exporteren** om deze te downloaden als een Excel-bestand.


## <a name="review-an-assessment"></a>Een evaluatie bekijken

Een evaluatie worden beschreven:

- **Azure-gereedheid**: Of de VM's zijn geschikt voor migratie naar Azure.
- **Schatting van de maandelijkse kosten**: De geschatte maandelijkse berekenings- en opslagkosten voor het uitvoeren van de virtuele machines in Azure.
- **Maandelijkse opslag raming van kosten**: Geschatte kosten voor schijfopslag na de migratie.


### <a name="view-an-assessment"></a>Een evaluatie weergeven

1. In **migratie doelstellingen** >  **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **evaluaties**.
2. In **evaluaties**, klikt u op een evaluatie om dit te openen.

    ![Evaluatie-overzicht](./media/tutorial-assess-hyper-v/assessment-summary.png)


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

![Betrouwbaarheidsclassificatie](./media/tutorial-assess-hyper-v/confidence-rating.png)

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

Verder met de derde zelfstudie in de reeks, voor informatie over het migreren van Hyper-V-machines naar Azure met Azure Migrate-servermigratie.

> [!div class="nextstepaction"]
> [Hyper-V-machines migreren](./tutorial-migrate-hyper-v.md)
