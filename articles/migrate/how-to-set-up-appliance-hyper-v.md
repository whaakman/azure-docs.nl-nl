---
title: Instellen van een apparaat voor Azure Migrate evaluatie/servermigratie voor Hyper-V-machines | Microsoft Docs
description: Beschrijft hoe u voor het instellen van een apparaat voor detectie, beoordeling en zonder agents migratie van Hyper-V-machines met behulp van Azure Migrate evaluatie/servermigratie.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: c531fe49ebff6c021547c2d1c2f382bcd6c9caef
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811763"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Een apparaat instellen voor Hyper-V-machines

Dit artikel wordt beschreven hoe u het apparaat Azure Migrate instelt als u beoordeling van de Hyper-V-machines met het hulpprogramma Azure Migrate-evaluatie-Server of VMware-VM's migreren naar Azure met behulp van het hulpprogramma voor migratie van Azure-Server migreren.

De Hyper-V-VM-apparaat is een lichtgewicht apparaat die wordt gebruikt door Azure Migrate evaluatie/servermigratie het volgende doen:

- On-premises Hyper-V virtuele machines detecteren.
- Metagegevens-en prestatiegegevens voor gedetecteerde virtuele machines naar Azure migreren evaluatie/servermigratie verzenden.

[Meer informatie](migrate-appliance.md) over het apparaat Azure Migrate.


## <a name="appliance-deployment-steps"></a>Stappen voor de implementatie van apparaat

Instellen van het apparaat u:
- Download een gecomprimeerde Hyper-V-VHD vanuit Azure portal.
- Maken van het apparaat en controleer dat deze verbinding met Azure Migrate-Server-evaluatie maken kan. 
- Het apparaat voor het eerst configureren en registreren bij de Azure Migrate-project.

## <a name="download-the-vhd"></a>Downloaden van de VHD

Download de ingepakte VHD-sjabloon voor het apparaat.

1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Server-evaluatie**, klikt u op **ontdekken**.
2. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **Ja, met Hyper-V**.
3. Klik op **downloaden** om de VHD-bestand te downloaden.

    ![Virtuele machine downloaden](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Beveiliging controleren

Controleer of het ZIP-bestand beveiligd, is voordat u deze implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Voer de volgende opdracht uit voor het genereren van de hash voor de VHD
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  Voor versie 1.19.05.10 van het toestel, de gegenereerde hash moet overeenkomen met deze instellingen.

  **Algoritme** | **Hash-waarde**
  --- | ---
  SHA256 | 598d2e286f9c972bb7f7382885e79e768eddedfe8a3d3460d6b8a775af7d7f79


  
## <a name="create-the-appliance-vm"></a>De VM-toepassing maken

Importeer het gedownloade bestand en de virtuele machine maken.

1. Pak het gezipte VHD-bestand naar een map op de Hyper-V-host die als voor de virtuele machine van het apparaat host fungeert. Drie mappen worden geëxtraheerd.
2. Open Hyper-V-beheer. In **acties**, klikt u op **virtuele Machine importeren**.

    ![VHD implementeren](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

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

## <a name="configure-the-appliance"></a>Configureer het apparaat

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

Als u VHD's op midden-en kleinbedrijf uitvoert, moet u de overdracht van referenties van het apparaat naar de Hyper-V-hosts inschakelen. Om dit te doen met het apparaat:

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

## <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Wanneer de detectie is voltooid, kunt u controleren of de virtuele machines worden weergegeven in de portal.

1. Open het dashboard Azure Migrate.
2. In **Azure Migrate - Servers** > **Azure Migrate: Server-evaluatie** pagina, klikt u op het pictogram met het aantal voor **ontdekte servers**. 


## <a name="next-steps"></a>Volgende stappen

Probeer [evaluatie van de Hyper-V](tutorial-assess-hyper-v.md) met Azure Migrate-Server-evaluatie.