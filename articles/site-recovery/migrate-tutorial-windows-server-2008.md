---
title: On-premises Windows Server 2008-servers migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises Windows Server 2008-machines migreren naar Azure met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/23/2018
ms.author: bsiva
ms.openlocfilehash: 6e5946f3f9dcf1c7d941054c844adcf683b485ab
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308640"
---
# <a name="migrate-servers-running-windows-server-2008-to-azure"></a>Servers met Windows Server 2008 naar Azure migreren

Deze zelfstudie leert u hoe u on-premises servers met Windows Server 2008 of 2008 R2 naar Azure migreert met behulp van Azure Site Recovery. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw on-premises omgeving voor de migratie voorbereiden
> * De doelomgeving instellen
> * Een replicatiebeleid instellen
> * Replicatie inschakelen
> * Een testfailover uitvoeren om te controleren of alles goed werkt
> * Failover naar Azure en de migratie voltooien

De beperkingen en bekende problemen sectie, een lijst met enkele beperkingen en tijdelijke oplossingen voor bekende problemen die u kunnen tegenkomen tijdens het migreren van Windows Server 2008-machines naar Azure. 


## <a name="supported-operating-systems-and-environments"></a>Ondersteunde besturingssystemen en omgevingen


|Besturingssysteem  | On-premises omgeving  |
|---------|---------|
|Windows Server 2008 SP2 - 32-bits en 64-bits (IA-32 en x86-64)</br>-Standaard</br>-Enterprise</br>-Datacenter   |     VMware-VM's, Hyper-V-machines en fysieke Servers    |
|WindowsServer 2008 R2 SP1 - 64-bits</br>-Standaard</br>-Enterprise</br>-Datacenter     |     VMware-VM's, Hyper-V-machines en fysieke Servers|

> [!WARNING]
> - Migratie van Server Core-servers wordt niet ondersteund.
> - Zorg ervoor dat u hebt de nieuwste servicepack en de Windows-updates die zijn geïnstalleerd voordat u migreert.


## <a name="prerequisites"></a>Vereisten

Voordat u begint, is het handig om te controleren van de Azure Site Recovery-architectuur voor [migratie van VMware en fysieke servers](vmware-azure-architecture.md) of [migratie van Hyper-V virtuele machines](hyper-v-azure-architecture.md) 

Als u wilt migreren van Hyper-V virtuele machines met Windows Server 2008 of Windows Server 2008 R2, volg de stappen in de [on-premises machines migreren naar Azure](migrate-tutorial-on-premises-azure.md) zelfstudie.

De rest van deze zelfstudie leert u hoe u on-premises virtuele VMware-machines en fysieke servers met Windows Server 2008 of 2008 R2 kunt migreren.


## <a name="limitations-and-known-issues"></a>Beperkingen en bekende problemen

- De configuratieserver, de extra processervers en de mobility-service gebruikt voor het migreren van Windows Server 2008 SP2 servers moeten versie 9.18.0.1 van de Azure Site Recovery-software worden uitgevoerd. De geïntegreerde setup voor versie 9.18.0.1 van de configuratieserver en processerver kan worden gedownload vanaf [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup).

- Een bestaande configuratieserver of processerver kan niet worden gebruikt voor het migreren van servers waarop Windows Server 2008 SP2 wordt uitgevoerd. Een nieuwe configuratie-Server moet worden ingericht met versie 9.18.0.1 van de Azure Site Recovery-software. Deze configuratieserver moeten alleen worden gebruikt voor de migratie van Windows-servers naar Azure.

- Consistente herstelpunten voor toepassing en de functie van de consistentie van meerdere VM's worden niet ondersteund voor de replicatie van servers waarop Windows Server 2008 SP2 wordt uitgevoerd. Windows Server 2008 SP2-servers moeten worden gemigreerd naar een crashconsistent herstelpunt. Crash-consistente herstelpunten worden elke vijf minuten standaard gegenereerd. Met behulp van een replicatiebeleid met de frequentie van de momentopname van een geconfigureerde toepassing zorgt ervoor dat replicatiestatus kritieke vanwege het ontbreken van consistente herstelpunten voor toepassing inschakelen. Fout-positieven te voorkomen, stelt u de App-consistente frequentie in het replicatiebeleid op 'Uit' van de momentopname.

- De servers die wordt gemigreerd, moet .NET Framework 3.5 Service Pack 1 voor de mobility-service om te werken.


- Als uw server dynamische schijven heeft, merkt u wellicht in bepaalde configuraties, die deze schijven op de mislukte via server offline of weergegeven als externe schijven zijn gemarkeerd. Merkt u wellicht ook dat de status van de gespiegelde instellen voor gespiegelde volumes op de dynamische schijven is gemarkeerd als 'Is mislukt van redundantie'. U kunt dit probleem van diskmgmt.msc oplossen door handmatig importeren van deze schijven en ze opnieuw te activeren.

- De servers die wordt gemigreerd, moeten het stuurprogramma vmstorfl.sys hebben. Failover kan mislukken als het stuurprogramma is niet aanwezig in de server die wordt gemigreerd. 
  > [!TIP]
  >Controleer of het stuurprogramma bij "C:\Windows\system32\drivers\vmstorfl.sys" aanwezig is. Als het stuurprogramma niet wordt gevonden, kunt u tijdelijke oplossing het probleem met het maken van een tijdelijk bestand in plaats. 
  >
  > Open de opdrachtprompt (uitvoeren > cmd) en voer de volgende: "kopiëren nul c:\Windows\system32\drivers\vmstorfl.sys"

- Het is mogelijk dat er geen RDP naar Windows Server 2008 SP2-servers met de 32-bits besturingssysteem onmiddellijk nadat ze failover is uitgevoerd of de test-failover naar Azure. Start de mislukte via virtuele machine van Azure portal en probeer opnieuw verbinding te maken. Als u nog steeds kan geen verbinding maken, moet u controleren als de server is geconfigureerd voor het toestaan van verbindingen met extern bureaublad en zorg ervoor dat er geen firewall-regels of netwerkbeveiligingsgroepen blokkeren van de verbinding. 
  > [!TIP]
  > Een testfailover wordt ten zeerste aangeraden voordat u migreert servers. Zorg ervoor dat u ten minste één test is geslaagd failover hebt uitgevoerd op elke server die u wilt migreren. Als onderdeel van de failovertest uitvoert, verbinding maken met de test is mislukt op de machine en controleer of dingen werkt zoals verwacht.
  >
  >De testfailover is niet-storende en helpt u bij het testen van migraties door u te maken van virtuele machines in een geïsoleerd netwerk van uw keuze. In tegenstelling tot de failoverbewerking tijdens de testfailover blijft gegevensreplicatie lag. U kunt uitvoeren als u wilt voordat u klaar bent om veel testfailovers. 
  >
  >


## <a name="getting-started"></a>Aan de slag

De volgende taken voor het voorbereiden van de Azure-abonnement en on-premises VMware-/ fysieke omgeving uitvoeren:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. On-premises voorbereiden [VMware](vmware-azure-tutorial-prepare-on-premises.md)


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
3. In **naam**, geef de beschrijvende naam **W2K8 migratie**. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep **w2k8migrate**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Als u de kluis snel wilt kunnen openen via het dashboard, klikt u op **Vastmaken aan dashboard** en vervolgens op **Maken**.

   ![Nieuwe kluis](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

De nieuwe kluis wordt toegevoegd op het **Dashboard** onder **Alle resources** en op de hoofdpagina van **Recovery Services-kluizen**.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Uw on-premises omgeving voor de migratie voorbereiden

- Download de configuratieserver-installatieprogramma (geïntegreerde Setup) uit [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup)
- Volg de stappen die hieronder worden beschreven voor het instellen van de bronomgeving met behulp van de installer-bestand in de vorige stap hebt gedownload.

> [!IMPORTANT]
> - Zorg ervoor dat u het setup-bestand gedownload in de eerste stap hierboven gebruiken om te installeren en registreren van de configuratieserver. Het setup-bestand niet downloaden via de Azure-portal. Het setup-bestand beschikbaar op [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup) is de enige versie die ondersteuning biedt voor Windows Server 2008-migratie.
>
> - U kunt een bestaande configuratieserver niet gebruiken voor het migreren van computers met Windows Server 2008. U moet het instellen van een nieuwe configuratie-Server met behulp van de bovenstaande koppeling.
>
> - Volg de stappen hieronder vindt u voor het installeren van de configuratieserver. Probeer niet de procedure op basis van GUI-installatie door het uitvoeren van de geïntegreerde setup rechtstreeks gebruiken. In dat geval wordt de installatie-poging mislukken met een onjuiste foutbericht waarin wordt gemeld dat er geen verbinding met internet is verwijderd.

 
1) Het bestand met kluisreferenties downloaden via de portal: in Azure portal, selecteert u de Recovery Services-kluis in de vorige stap hebt gemaakt. Selecteer in het menu op de pagina kluis **Site Recovery-infrastructuur** > **configuratieservers**. Klik vervolgens op **+ Server**. Selecteer *configuratieserver voor fysieke* in de vervolgkeuzelijst formulier op de pagina die wordt geopend. Klik op de downloadknop bij stap 4 van het bestand met kluisreferenties downloaden.

 ![Kluisregistratiesleutel downloaden](media/migrate-tutorial-windows-server-2008/download-vault-credentials.png) 

2) Kopieer het bestand met kluisreferenties gedownload in de vorige stap en de geïntegreerde setup-bestand op het bureaublad van de machine Configuration Server eerder hebt gedownload (de Windows Server 2012 R2 of Windows Server 2016-machine waarop u wilt installeren de configuratie van server-software.)

3) Zorg ervoor dat de configuratieserver verbinding heeft met internet en of de systeemklok en de tijdzone-instellingen op de machine juist zijn geconfigureerd. Download de [MySQL 5.7](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi) installatieprogramma en plaats deze op *C:\Temp\ASRSetup* (Maak de map als deze nog niet bestaat.) 

4) Maken van een bestand met MySQL met de volgende regels en plaats het op het bureaublad op **C:\Users\Administrator\MySQLCreds.txt** . Vervang ' wachtwoord ~ 1 "hieronder met een geschikt en een sterk wachtwoord:

```
[MySQLCredentials]
MySQLRootPassword = "Password~1"
MySQLUserPassword = "Password~1"
```

5) Pak de inhoud van het gedownloade geïntegreerde setup-bestand op het bureaublad met de volgende opdracht:

```
cd C:\Users\Administrator\Desktop

MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Users\Administrator\Desktop\9.18
```
  
6) De configuratie van server-software met behulp van de uitgepakte inhoud door het uitvoeren van de volgende opdrachten installeren:

```
cd C:\Users\Administrator\Desktop\9.18.1

UnifiedSetup.exe /AcceptThirdpartyEULA /ServerMode CS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /MySQLCredsFilePath "C:\Users\Administrator\Desktop\MySQLCreds.txt" /VaultCredsFilePath <vault credentials file path> /EnvType VMWare /SkipSpaceCheck
```

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het Resource Manager-implementatiemodel op.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

1. Klik op om een nieuw replicatiebeleid **infrastructuur voor Site Recovery** > **replicatiebeleid** > **+ replicatiebeleid**.
2. In **replicatiebeleid maken**, een beleidsnaam opgeven.
3. In **RPO-drempelwaarde**, de objective (RPO) herstelpuntlimiet opgeven. Een waarschuwing wordt gegenereerd als de replicatie-RPO deze limiet overschrijdt.
4. In **bewaarperiode voor herstelpunten**, opgeven hoe lang (in uren) de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster. Bewaarperiode van 24 uur maximaal wordt ondersteund voor computers die worden gerepliceerd naar premium storage en 72 uur voor standard-opslag.
5. In **frequentie App-consistente momentopname**, geef **uit**. Klik op **OK** om het beleid te maken.

Het beleid wordt automatisch gekoppeld aan de configuratieserver.

> [!WARNING]
> Zorg ervoor dat u opgeeft **OFF** in de App-consistente momentopname frequentie-instelling van het replicatiebeleid. Alleen crash-consistente herstelpunten worden ondersteund tijdens het repliceren van servers met Windows Server 2008. Het opgeven een andere waarde voor de frequentie van de App-consistente momentopname tot ONWAAR waarschuwingen leiden zal door in te schakelen replicatiestatus van de server kritieke vanwege een gebrek aan App-consistente herstelpunten.

   ![Replicatiebeleid maken](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>Replicatie inschakelen

[Replicatie inschakelen](physical-azure-disaster-recovery.md#enable-replication) voor Windows Server 2008 SP2 / Windows Server 2008 R2 SP1-server moeten worden gemigreerd.
   
   ![Fysieke server toevoegen](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![Replicatie inschakelen](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren

U kunt een test-failover van het repliceren van de servers na de initiële replicatie is voltooid en wordt deze de status van de server uitvoeren **beveiligde**.

Voer een [testfailover](tutorial-dr-drill-azure.md) naar Azure uit om te controleren of alles goed werkt.

   ![Test-failover](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Migreren naar Azure

Een failover uitvoeren voor de machines die u wilt migreren.

1. Klik in **Instellingen** > **Gerepliceerde items** op de machine > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. Selecteer het meest recente herstelpunt.
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery zal proberen de server afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Controleer of de virtuele Azure-machine in Azure wordt weergegeven zoals verwacht.
5. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Migratie voltooien**. Hiermee wordt het migratieproces voltooid, de replicatie voor de VM gestopt en Site Recovery-facturering voor de virtuele machine gestopt.

   ![Migratie voltooien](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.
