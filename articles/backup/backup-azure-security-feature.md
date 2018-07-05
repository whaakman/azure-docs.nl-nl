---
title: Beveiligingsfuncties die u helpen hybride back-ups die gebruikmaken van Azure back-up beveiligen
description: Informatie over het gebruik van de beveiligingsfuncties in Azure Backup back-ups om veiliger te maken
services: backup
author: trinadhk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/08/2017
ms.author: trinadhk
ms.openlocfilehash: 714c8fde28be63e5173f89f92d186445f0990214
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447377"
---
# <a name="security-features-to-help-protect-hybrid-backups-that-use-azure-backup"></a>Beveiligingsfuncties die u helpen hybride back-ups die gebruikmaken van Azure back-up beveiligen
Opmerkingen over beveiligingsproblemen, zoals malware, ransomware en inbraakdetectie, verhogen. Deze beveiligingsproblemen met zich mee kunnen erg kostbaar zijn, in termen van gegevens en geld. Als u wilt beveiligen tegen dergelijke aanvallen, biedt Azure Backup nu beveiligingsfuncties ter bescherming van hybride back-ups. In dit artikel wordt uitgelegd hoe u inschakelen en gebruiken van deze functies, met behulp van een Azure Recovery Services-agent en de Azure Backup Server. Deze functies zijn onder andere:

- **Preventie**. Een extra verificatielaag wordt toegevoegd wanneer er een kritieke bewerking zoals het wijzigen van een wachtwoordzin wordt uitgevoerd. Deze validatie is om ervoor te zorgen dat deze bewerkingen kunnen worden uitgevoerd door gebruikers met geldig Azure-referenties.
- **Waarschuwingen**. Een e-mailmelding verzonden naar de beheerder van abonnement telkens wanneer een kritieke bewerking, zoals back-upgegevens verwijderen wordt uitgevoerd. Dit e-mailbericht zorgt ervoor dat de gebruiker snel wordt geïnformeerd over deze acties.
- **Herstel**. Verwijderde back-upgegevens worden bewaard voor een extra veertien dagen vanaf de datum van de verwijdering. Dit zorgt ervoor dat herstel van de gegevens binnen een bepaalde periode, zodat er zonder verlies van gegevens, zelfs als een aanval plaatsvindt. Ook worden een groter aantal minimale herstelpunten bewaard ter bescherming tegen beschadigde gegevens.

> [!NOTE]
> Beveiligingsfuncties mag niet worden ingeschakeld als u infrastructuur als een service (IaaS)-VM back-up. Deze functies nog niet beschikbaar voor IaaS VM backup, zodat dit van invloed is niet om hen in staat hoeven wordt. Beveiligingsfuncties die moeten worden ingeschakeld, alleen als u gebruikmaakt van: <br/>
>  * **Azure backup-agent**. Agentversie 2.0.9052 van de minimale. Nadat u deze functies hebt ingeschakeld, moet u upgraden naar deze versie van agent kritieke bewerkingen uit te voeren. <br/>
>  * **Azure Backup Server**. Minimale Azure Backup-agentversie 2.0.9052 met Azure Backup Server-update 1. <br/>
>  * **System Center Data Protection Manager**. Azure Backup agent minimumversie 2.0.9052 met Data Protection Manager 2012 R2 UR12 of Data Protection Manager 2016 UR2. <br/> 


> [!NOTE]
> Deze functies zijn alleen beschikbaar voor Recovery Services-kluis. Alle gemaakte Recovery Services-kluizen hebben deze functies standaard ingeschakeld. Voor bestaande Recovery Services-kluizen inschakelen gebruikers deze functies met behulp van de stappen in de volgende sectie. Nadat de functies zijn ingeschakeld, ze van toepassing op alle Recovery Services agentcomputers, Azure Backup Server-instanties, en Data Protection Manager-servers geregistreerd bij de kluis. Als u deze instelling inschakelt, is een eenmalige bewerking en kunt u deze functies niet uitschakelen nadat deze is ingeschakeld.
>

## <a name="enable-security-features"></a>Beveiligingsfuncties inschakelen
Als u een Recovery Services-kluis maakt, kunt u de beveiligingsfuncties. Als u met een bestaande kluis werkt, kunt u beveiligingsfuncties inschakelen door de volgende stappen:

1. Meld u bij Azure portal met behulp van uw Azure-referenties.
2. Selecteer **Bladeren**, en het type **herstelservices**.

    ![Schermafbeelding van de Azure portal bladeroptie](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven. In deze lijst, selecteer een kluis. Het geselecteerde kluisdashboard wordt geopend.
3. In de lijst met items dat wordt weergegeven op de kluis onder **instellingen**, klikt u op **eigenschappen**.

    ![Schermafbeelding van de Recovery Services-kluis opties](./media/backup-azure-security-feature/vault-list-properties.png)
4. Onder **beveiligingsinstellingen**, klikt u op **Update**.

    ![Schermafbeelding van de Recovery Services-kluis eigenschappen](./media/backup-azure-security-feature/security-settings-update.png)

    Hiermee opent u de koppeling voor het bijwerken de **beveiligingsinstellingen** blade die bevat een samenvatting van de functies en kunt u ze inschakelen.
5. In de lijst vervolgkeuzelijst **hebt u Azure multi-factor Authentication?**, selecteert u een waarde om te bevestigen of u hebt ingeschakeld [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md). Als deze is ingeschakeld, wordt u gevraagd om u te verifiëren vanaf een ander apparaat (bijvoorbeeld een mobiele telefoon) tijdens het aanmelden bij de Azure-portal.

   Wanneer u kritieke bewerkingen in de back-up uitvoert, hebt u een beveiligingspincode invoeren, beschikbaar via Azure portal. Azure multi-factor Authentication inschakelen, voegt een beveiligingslaag toe. Alleen gebruikers met geldig Azure-referenties geautoriseerde, en is geverifieerd vanaf een tweede apparaat, hebben toegang tot de Azure-portal.
6. Selecteren om op te slaan beveiligingsinstellingen, **inschakelen** en klikt u op **opslaan**. U kunt selecteren **inschakelen** pas nadat u een waarde van de **hebt u Azure multi-factor Authentication?** lijst in de vorige stap.

    ![Schermafbeelding van de beveiligingsinstellingen](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Verwijderde back-upgegevens herstellen
Back-up verwijderde back-upgegevens worden bewaard een extra 14 dagen en wordt niet verwijderd onmiddellijk als de **back-up stoppen met de back-upgegevens verwijderen** bewerking wordt uitgevoerd. Als u deze gegevens herstellen in de periode van 14 dagen, de volgende stappen, afhankelijk van wat u gebruikt:

Voor **Azure Recovery Services-agent** gebruikers:

1. Als de computer waar de back-ups zijn gebeurt nog steeds beschikbaar is, gebruikt u [gegevens op dezelfde computer herstellen](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) in de Azure Recovery Services, om van de oude herstelpunten te herstellen.
2. Als deze computer niet beschikbaar is, gebruikt u [herstellen naar een alternatieve machine](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) aan het gebruik van een andere Azure Recovery Services-computer om deze gegevens.

Voor **Azure Backup Server** gebruikers:

1. Als de server waar de back-ups zijn gebeurt nog steeds beschikbaar is, de verwijderde gegevensbronnen opnieuw te beveiligen en gebruiken de **gegevens herstellen** functie om van de oude herstelpunten te herstellen.
2. Als deze server niet beschikbaar is, gebruikt u [gegevens herstellen vanaf een andere Azure Backup Server](backup-azure-alternate-dpm-server.md) naar een ander exemplaar van Azure Backup Server gebruiken deze gegevens op te halen.

Voor **Data Protection Manager** gebruikers:

1. Als de server waar de back-ups zijn gebeurt nog steeds beschikbaar is, de verwijderde gegevensbronnen opnieuw te beveiligen en gebruiken de **gegevens herstellen** functie om van de oude herstelpunten te herstellen.
2. Als deze server niet beschikbaar is, gebruikt u [externe DPM toevoegen](backup-azure-alternate-dpm-server.md) naar een andere Data Protection Manager-server gebruikt deze gegevens op te halen.

## <a name="prevent-attacks"></a>Aanvallen te voorkomen
Controles zijn toegevoegd om ervoor te zorgen dat alleen geldige gebruikers verschillende bewerkingen kunnen uitvoeren. Deze omvatten een extra verificatielaag toe te voegen en onderhouden van een minimale bewaartermijn voor hersteldoeleinden.

### <a name="authentication-to-perform-critical-operations"></a>Verificatie kritieke bewerkingen uit te voeren
Als onderdeel van het toevoegen van een extra verificatielaag voor essentiële bewerkingen u wordt gevraagd een beveiligingspincode invoeren bij het uitvoeren van **beveiliging stoppen en verwijder gegevens** en **wijziging wachtwoordzin** bewerkingen.

Voor het ontvangen van deze PINCODE:

1. Meld u aan bij Azure Portal.
2. Blader naar **Recovery Services-kluis** > **instellingen** > **eigenschappen**.
3. Onder **BEVEILIGINGSPINCODE**, klikt u op **genereren**. Hiermee opent u een blade met de PINCODE moet worden ingevoerd in de gebruikersinterface van de Azure Recovery Services-agent.
    Deze PINCODE slechts vijf minuten geldig is en deze automatisch wordt gegenereerd na die periode.

### <a name="maintain-a-minimum-retention-range"></a>Een minimale bewaartermijn onderhouden
Om ervoor te zorgen dat er altijd een geldig aantal herstelpunten beschikbaar zijn, zijn de volgende controles toegevoegd:

- Voor het bewaren van dagelijkse, een minimum van **zeven** dagen retentie moeten worden uitgevoerd.
- Voor het bewaren van wekelijks, een minimum van **vier** weken na de bewaarperiode moeten worden uitgevoerd.
- Voor het bewaren van maandelijkse, een minimum van **drie** retentieperiode van maanden moeten worden uitgevoerd.
- Voor het bewaren van jaarlijkse, een minimum van **één** jaar bewaard moet worden uitgevoerd.

## <a name="notifications-for-critical-operations"></a>Meldingen voor essentiële bewerkingen
Wanneer een kritieke bewerking wordt uitgevoerd, wordt de beheerder van abonnement normaal gesproken een e-mailmelding met informatie over de bewerking verzonden. U kunt aanvullende e-mailontvangers voor deze meldingen configureren met behulp van de Azure-portal.

De beveiligingsfuncties die worden vermeld in dit artikel biedt defense methoden op basis van gerichte aanvallen. Nog belangrijker, als een aanval gebeurt, bieden deze functies u de mogelijkheid om uw gegevens te herstellen.

## <a name="troubleshooting-errors"></a>Fouten oplossen
| Bewerking | Foutdetails | Oplossing |
| --- | --- | --- |
| Beleid wijzigen |Het back-upbeleid kan niet worden gewijzigd. Fout: De huidige bewerking is mislukt vanwege een interne servicefout [0x29834]. Probeer het later opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |**Oorzaak:**<br/>Deze fout ontstaat wanneer beveiligingsinstellingen zijn ingeschakeld en u probeert te verminderen bewaartermijn hieronder de hierboven opgegeven minimumwaarden u van niet-ondersteunde versie gebruikmaakt (de ondersteunde versies zijn opgegeven in de eerste notitie van dit artikel). <br/>**Aanbevolen actie:**<br/> In dit geval moet u de bewaarperiode boven de minimale bewaartermijn opgegeven tijdsperiode (zeven dagen voor dagelijks, vier weken voor wekelijkse, drie weken voor maandelijks of één jaar voor jaarlijkse) instellen om door te gaan met het beleid gerelateerde updates. (Optioneel) de aanpak van voorkeur om bij te werken van de backup-agent, Azure Backup-Server en/of DPM UR gebruikmaken van alle beveiligingsupdates zou zijn. |
| Wachtwoordzin wijzigen |De ingevoerde beveiligingspincode is onjuist. (ID: 100130) Geef de BEVEILIGINGSPINCODE op om deze bewerking te voltooien. |**Oorzaak:**<br/> Deze fout wordt geleverd bij het invoeren van ongeldige of verlopen BEVEILIGINGSPINCODE tijdens het uitvoeren van kritieke bewerking (zoals wachtwoordzin wijzigen). <br/>**Aanbevolen actie:**<br/> Als u wilt de bewerking is voltooid, moet u geldige BEVEILIGINGSPINCODE invoeren. Als de PINCODE, meld u aan bij Azure portal en navigeer naar de Recovery Services-kluis > Instellingen > Eigenschappen > BEVEILIGINGSPINCODE genereren. Gebruik deze PINCODE te wijzigen van de wachtwoordzin. |
| Wachtwoordzin wijzigen |Bewerking mislukt. ID: 120002 |**Oorzaak:**<br/>Deze fout ontstaat wanneer beveiligingsinstellingen zijn ingeschakeld, u probeert te wachtwoordzin wijzigen en u op niet-ondersteunde versie (geldige versies die zijn opgegeven in de eerste notitie van dit artikel bent).<br/>**Aanbevolen actie:**<br/> Als u wilt wijzigen wachtwoordzin, moet u eerst backup-agent bijwerken naar minimaal versie 2.0.9052 van minimale, Azure back-upserver voor minimale update 1, en/of DPM naar ten minste DPM 2012 R2 UR12 of DPM 2016 UR2 (downloadkoppelingen hieronder), en voer vervolgens geldig BEVEILIGINGSPINCODE. Als de PINCODE, meld u aan bij Azure portal en navigeer naar de Recovery Services-kluis > Instellingen > Eigenschappen > BEVEILIGINGSPINCODE genereren. Gebruik deze PINCODE te wijzigen van de wachtwoordzin. |

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Recovery Services-kluis](backup-azure-vms-first-look-arm.md) om in te schakelen van deze functies.
* [Download de nieuwste Azure Recovery Services-agent](http://aka.ms/azurebackup_agent) om u te helpen bij het beveiligen van Windows-computers en bescherming van uw back-upgegevens tegen aanvallen.
* [Download de meest recente Azure Backup Server](https://aka.ms/latest_azurebackupserver) om u te helpen bij het beveiligen van workloads en bescherming van uw back-upgegevens tegen aanvallen.
* [Download UR12 voor System Center 2012 R2 Data Protection Manager](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) of [UR2 voor System Center 2016 Data Protection Manager downloaden](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) om u te helpen bij het beveiligen van workloads en bescherming van uw back-upgegevens tegen aanvallen.
