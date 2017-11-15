---
title: Azure Backup-Server gebruiken voor back-up werkbelastingen naar de klassieke Azure portal | Microsoft Docs
description: Zorg ervoor dat uw omgeving goed is voorbereid op de back-up van werkbelastingen met Azure Backup-Server
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: Azure Backup-server; back-upkluis
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: db19729f652430c80ad0a7af1b630c1c5a1561d3
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Met Azure Backup Server voorbereiden op het maken van back-ups van workloads
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassiek)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassiek)](backup-azure-dpm-introduction-classic.md)
>
>

Dit artikel is over het voorbereiden van uw omgeving back-up werkbelastingen zijn die gebruikmaken van Azure Backup-Server. U kunt de toepassing werkbelastingen zoals Hyper-V-machines, Microsoft SQL Server, SharePoint Server, Microsoft Exchange en Windows-clients vanuit één console beveiligen met Azure Backup-Server.

> [!WARNING]
> Azure Backup-Server neemt de functionaliteit van Data Protection Manager (DPM) voor back-up van de werkbelasting over. Vindt u koppelingen naar DPM-documentatie voor sommige van deze mogelijkheden. Echter Azure Backup-Server biedt geen beveiliging op tape of integreren met System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Windows Server-machine
![step1](./media/backup-azure-microsoft-azure-backup/step1.png)

De eerste stap naar de Azure Backup-Server slag is dat een Windows Server-machine.

| Locatie | Minimale vereisten | Aanvullende instructies |
| --- | --- | --- |
| Azure |Virtuele machine van Azure IaaS<br><br>A2 Standaard: 2 kernen, 3.5GB RAM-geheugen |U kunt beginnen met een eenvoudige afbeelding van Windows Server 2012 R2 Datacenter. [IaaS-werkbelastingen met Azure Backup-Server (DPM) beveiligt](https://technet.microsoft.com/library/jj852163.aspx) veel nuances heeft. Zorg ervoor dat u het artikel volledig gelezen voordat de machine is geïmplementeerd. |
| On-premises |Hyper-V VM<br> VMWare-virtuele machine<br> of een fysieke host.<br><br>2 kernen en 4GB RAM-geheugen |U kunt de DPM-opslag met behulp van Windows Server-Ontdubbeling ontdubbelen. Meer informatie over het [DPM en Ontdubbeling](https://technet.microsoft.com/library/dn891438.aspx) samenwerken als geïmplementeerd in Hyper-V-machines. |

> [!NOTE]
> Het verdient aanbeveling dat Azure Backup-Server worden geïnstalleerd op een computer met Windows Server 2012 R2 Datacenter. Veel van de vereiste onderdelen worden automatisch besproken met de meest recente versie van het Windows-besturingssysteem.
>
>

Als u van plan bent om Azure Backup-Server toevoegen aan een domein, is het raadzaam dat u de fysieke server of de virtuele machine aan het domein toevoegen voordat u de Azure Backup-Server-software installeert. Verplaatsen van een Azure-Server voor back-up naar een nieuw domein na implementatie *niet ondersteund*.

## <a name="2-backup-vault"></a>2. Back-upkluis
![step2](./media/backup-azure-microsoft-azure-backup/step2.png)

Of u back-upgegevens naar Azure verzenden of lokaal houden, moet u de Azure Backup-Server geregistreerd in een kluis. Als u een nieuwe gebruiker in de Azure Backup en Azure Backup-Server wilt gebruiken, raadpleegt u de Azure portal-versie van dit artikel - [voorbereiden op back-up van werkbelastingen met Azure Backup-Server](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen.
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 30 November 2017 zich u niet PowerShell gebruiken voor het maken van de Backup-kluizen. **Per 30 November 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>



## <a name="3-software-package"></a>3. Softwarepakket
![stap 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-the-software-package"></a>Het softwarepakket downloaden
Net als bij de kluis van referenties, kunt u Microsoft Azure Backup voor werkbelastingen van de toepassing van de **pagina snel starten** van de back-upkluis.

1. Klik op **voor Toepassingsworkloads (schijf naar schijf naar Cloud)**. Hiermee gaat u naar de pagina van Downloadcentrum van waar het softwarepakket kan worden gedownload.

    ![Microsoft Azure Backup-welkomstscherm](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Klik op **Downloaden**.

    ![Download center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Selecteer alle bestanden en klik op **volgende**. Alle bestanden die afkomstig zijn van de downloadpagina van Microsoft Azure Backup in downloaden en alle bestanden in dezelfde map te plaatsen.
   ![Download center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Omdat de grootte van het downloadpakket van alle bestanden samen > 3G, klikt u op een 10 Mbps downloaden koppeling het tot 60 minuten voor het downloaden duurt te voltooien.

### <a name="extracting-the-software-package"></a>Het uitpakken van het softwarepakket
Nadat u alle bestanden hebt gedownload, klikt u op **MicrosoftAzureBackupInstaller.exe**. Hiermee start u de **Wizard Setup van Microsoft Azure Backup** uitpakken van de setup-bestanden naar een door u opgegeven locatie. Volg de stappen in de wizard en klik op de **extraheren** knop om te beginnen met de extractie.

> [!WARNING]
> Ten minste 4GB aan vrije ruimte is vereist voor het uitpakken van de setup-bestanden.
>
>

![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Zodra de extractie proces is voltooid, schakel het selectievakje in om te starten na uitgepakte *setup.exe* om te beginnen met de installatie van Microsoft Azure Backup-Server en klik op de **voltooien** knop.

### <a name="installing-the-software-package"></a>Het softwarepakket installeren
1. Klik op **Microsoft Azure Backup** starten van de wizard setup.

    ![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Klik op het welkomstscherm op de **volgende** knop. Hiermee gaat u naar de *vereiste controleert* sectie. In dit scherm, klikt u op de **controleren** knop om te bepalen of de hardware en software-vereisten voor Azure Backup-Server is voldaan. Als alle vereisten zijn voldaan is, ziet u een bericht waarin staat dat de computer voldoet aan de vereisten. Klik op de **volgende** knop.

    ![Azure Backup-Server - welkomstpagina en de vereisten controleren](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup-Server vereist dat SQL Server Standard en het installatiepakket voor de Azure Backup-Server wordt geleverd met de juiste SQL Server-binaire bestanden die nodig zijn. Bij het starten met een nieuwe installatie van de Azure Backup-Server, moet u de optie kiest **nieuw exemplaar van SQL Server installeren met deze installatie** en klik op de **controleren en installeren** knop. Nadat de vereiste onderdelen zijn geïnstalleerd, klikt u op **volgende**.

    ![Azure Backup-Server - controle van SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Als een fout bij de aanbeveling optreedt om de computer opnieuw hebt opgestart, doen en klikt u op **opnieuw controleren**.

   > [!NOTE]
   > Azure Backup-Server komt niet overeen met een extern exemplaar van SQL Server. Het exemplaar dat wordt gebruikt door Azure Backup-Server moet lokaal zijn.
   >
   >

4. Geef een locatie voor de installatie van bestanden van Microsoft Azure Backup-server en klik op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    De tijdelijke locatie is een vereiste voor back-up naar Azure. Zorg ervoor dat de scratchruimte locatie is ten minste 5% van de gegevens die zijn gepland voor een back-up naar de cloud. Voor de beveiliging van schijven moeten afzonderlijke schijven worden geconfigureerd nadat de installatie is voltooid. Zie voor meer informatie over opslaggroepen [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).
5. Geef een sterk wachtwoord voor beperkte lokale gebruikersaccounts en klik op **volgende**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Selecteer of u wilt gebruiken *Microsoft Update* om te controleren op updates en klik op **volgende**.

   > [!NOTE]
   > U wordt aangeraden met Windows Update u naar Microsoft Update beveiligingsupdates en belangrijke updates voor Windows en andere producten, zoals Microsoft Azure Backup-Server biedt.
   >
   >

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Controleer de *samenvatting van instellingen* en klik op **installeren**.

    ![Back-PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. De installatie gebeurt in fasen. In de eerste fase wordt de Microsoft Azure Recovery Services Agent geïnstalleerd op de server. De wizard controleert ook of er verbinding met Internet. Als de verbinding met Internet beschikbaar kunt u doorgaan met installatie, zo niet, moet u proxy-gegevens voor verbinding met het Internet.

    De volgende stap is het configureren van de Microsoft Azure Recovery Services Agent. U moet als onderdeel van de configuratie, geef je de kluisreferenties de machine naar de back-upkluis te registreren. U wordt ook een wachtwoordzin in om te versleutelen/ontsleutelen van de gegevens die worden verzonden tussen Azure en uw bedrijf opgeven. U kunt automatisch genereren van een wachtwoordzin of uw eigen minimaal 16 tekens wachtwoordzin opgeven. Ga door met de wizard totdat de agent is geconfigureerd.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Nadat de registratie van de Microsoft Azure Backup-server is voltooid, gaat de installatiewizard van de algehele door naar de installatie en configuratie van SQL Server en de Azure Backup-Server-onderdelen. Zodra de installatie van de SQL Server-onderdelen is voltooid, wordt de Azure Backup-Server-onderdelen zijn geïnstalleerd.

    ![Azure Backup-server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Wanneer de installatiestap is voltooid, wordt bureaubladpictogrammen van het product gemaakt ook. Dubbelklik op het pictogram voor het starten van het product.

### <a name="add-backup-storage"></a>Back-upopslag toevoegen
De eerste back-up wordt opgeslagen op opslag die is gekoppeld aan de Azure Backup-Server-machine. Zie voor meer informatie over het toevoegen van schijven [Configureer de opslaggroepen en schijfruimte](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> U moet back-upopslag toevoegen, zelfs als u van plan bent om gegevens te verzenden naar Azure. In de huidige architectuur van Azure Backup-Server, de Azure Backup-kluis bevat de *tweede* kopie van de gegevens bij de lokale opslag bevat de eerste (en verplichte) back-up.  
>
>

## <a name="4-network-connectivity"></a>4. Netwerkverbinding
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Azure Backup-Server is vereist voor de verbinding met de Azure Backup-service voor het product werkt met succes. U kunt controleren of de machine de verbinding met Azure heeft, gebruiken de ```Get-DPMCloudConnection``` commandlet in de Azure Backup-Server PowerShell-console. Als de uitvoer van de cmdlet TRUE is en vervolgens de verbinding is, anders er is geen netwerkverbinding.

Op hetzelfde moment moet het Azure-abonnement in een foutloze toestand bevindt. De status van uw abonnement en beheert, meld u aan bij de [abonnement portal](https://account.windowsazure.com/Subscriptions).

Zodra u de status van de Azure-connectiviteit en het Azure-abonnement hebt, kunt u de onderstaande tabel om erachter te komen de gevolgen voor de back-up/herstel functionaliteit beschikbaar wordt gesteld.

| Status connectiviteit | Azure-abonnement | Back-up naar Azure | Back-up op schijf | Herstellen van Azure | Herstellen van de schijf |
| --- | --- | --- | --- | --- | --- |
| Verbonden |Actief |Toegestaan |Toegestaan |Toegestaan |Toegestaan |
| Verbonden |Verlopen |Stopped |Stopped |Toegestaan |Toegestaan |
| Verbonden |Gemaakt |Stopped |Stopped |Gestopt en Azure herstelpunten verwijderd |Stopped |
| Verloren connectiviteit > 15 dagen |Actief |Stopped |Stopped |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Verlopen |Stopped |Stopped |Toegestaan |Toegestaan |
| Verloren connectiviteit > 15 dagen |Gemaakt |Stopped |Stopped |Gestopt en Azure herstelpunten verwijderd |Stopped |

### <a name="recovering-from-loss-of-connectivity"></a>Herstellen van het verlies van verbinding
Als u een firewall of een proxy die is verhinderen van toegang tot Azure hebt, moet u goedgekeurde IP-adressen de volgende adressen domein in de firewall/proxy-profiel:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Zodra de verbinding met Azure met de Azure Backup-Server-machine is hersteld, worden de bewerkingen die kunnen worden uitgevoerd worden bepaald door de status van de Azure-abonnement. De bovenstaande tabel bevat informatie over de bewerkingen die zijn toegestaan als de machine 'verbonden'.

### <a name="handling-subscription-states"></a>Afhandeling van abonnementsstatussen
Het is mogelijk te houden van een Azure-abonnement van een *verlopen* of *Deprovisioned* status naar de *Active* status. Maar dit heeft enkele gevolgen van het gedrag van het product terwijl de status niet is *Active*:

* Een *Deprovisioned* abonnement verliest functionaliteit voor de periode dat deze is gemaakt. In te schakelen *Active*, de functionaliteit van het product van de back-up/herstel wordt weer actief. De back-upgegevens op de lokale schijf kunnen ook worden opgehaald als deze is gehouden met een groot genoeg bewaarperiode. De back-upgegevens in Azure is echter onherstelbaar verloren zodra het abonnement voert de *Deprovisioned* status.
* Een *verlopen* functionaliteit voor het abonnement alleen verliest totdat deze is gemaakt *Active* opnieuw. Alle back-ups voor de periode die het abonnement is gepland *verlopen* kan niet worden uitgevoerd.

## <a name="troubleshooting"></a>Problemen oplossen
Als Microsoft Azure Backup-server is mislukt met fouten tijdens de installatiefase (of backup of restore), raadpleegt u dit [fout codes document](https://support.microsoft.com/kb/3041338) voor meer informatie.
U kunt ook verwijzen naar [Azure Backup gerelateerde Veelgestelde vragen](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Volgende stappen
U kunt gedetailleerde informatie ophalen over [uw omgeving voorbereiden voor DPM](https://technet.microsoft.com/library/hh758176.aspx) op de website Microsoft TechNet. Het bevat ook informatie over ondersteunde configuraties waarop Azure Backup-Server kan worden geïmplementeerd en gebruikt.

Deze artikelen kunt u een beter begrip van de beveiliging van werkbelastingen met behulp van Microsoft Azure Backup-server te krijgen.

* [Back-up van SQL Server](backup-azure-backup-sql.md)
* [Back-up van SharePoint server](backup-azure-backup-sharepoint.md)
* [Alternatieve server back-up](backup-azure-alternate-dpm-server.md)
