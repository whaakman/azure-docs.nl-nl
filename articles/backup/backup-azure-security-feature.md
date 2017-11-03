---
title: Beveiligingsfuncties die beschermen hybride back-ups die gebruikmaken van Azure Backup | Microsoft Docs
description: Informatie over het gebruik van beveiligingsfuncties in Azure Backup back-ups om veiliger te maken
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pajosh
ms.openlocfilehash: 8ef9ddc345fb553b93815022dc3e6a796cae8b3a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="security-features-to-help-protect-hybrid-backups-that-use-azure-backup"></a>Beveiligingsfuncties die beschermen hybride back-ups die gebruikmaken van Azure Backup
Opmerkingen over beveiligingsproblemen, zoals malware, ransomware en inbraakdetectie, meer. Deze beveiligingsproblemen kunnen kostbare in termen van gegevens en geld zijn. Azure Backup biedt nu beveiligingsfuncties ter bescherming van back-ups hybride als bescherming tegen dergelijke aanvallen. In dit artikel bevat informatie over het inschakelen en gebruiken van deze functies met behulp van een Azure Recovery Services-agent en de Azure Backup-Server. Deze functies:

- **Preventie**. Een extra verificatielaag wordt toegevoegd wanneer een kritieke bewerking zoals het wijzigen van een wachtwoordzin wordt uitgevoerd. Deze validatie is om ervoor te zorgen dat deze bewerkingen alleen voor gebruikers met geldig Azure-referenties kunnen worden uitgevoerd.
- **Waarschuwingen**. Een e-mailmelding verzonden naar de abonnementsbeheerder telkens wanneer een kritieke bewerking zoals het verwijderen van de back-upgegevens wordt uitgevoerd. Dit e-mailadres zorgt ervoor dat de gebruiker snel wordt geïnformeerd over deze acties.
- **Herstel**. Verwijderde gegevens back-up wordt bewaard voor een extra 14 dagen vanaf de datum van de verwijdering. Hierdoor worden de gegevens kunnen worden hersteld binnen een bepaalde periode, zodat er geen gegevensverlies zelfs als een aanval gebeurt. Ook worden een groter aantal minimale herstelpunten onderhouden ter bescherming tegen beschadigde gegevens.

> [!NOTE]
> Beveiligingsfuncties mag niet worden ingeschakeld als u infrastructuur als een dienst (IaaS) VM-back-up. Deze functies nog niet beschikbaar voor IaaS VM back-up, zodat u deze wilt inschakelen geen invloed heeft. Beveiligingsfuncties moeten alleen worden ingeschakeld als u gebruikmaakt van: <br/>
>  * **Azure backup-agent**. Minimale agentversie 2.0.9052. Nadat u deze functies hebt ingeschakeld, moet u upgraden naar deze agentversie kritieke bewerkingen uit te voeren. <br/>
>  * **Azure Backup-Server**. Azure Backup agent minimumversie 2.0.9052 met Azure Backup-Server bijwerken van 1. <br/>
>  * **System Center Data Protection Manager**. Azure Backup agent minimumversie 2.0.9052 met Data Protection Manager 2012 R2 UR12 of Data Protection Manager 2016 UR2. <br/> 


> [!NOTE]
> Deze functies zijn alleen beschikbaar voor Recovery Services-kluis. Alle gemaakte Recovery Services-kluizen hebben deze functies standaard ingeschakeld. Voor bestaande Recovery Services-kluizen inschakelen gebruikers deze functies met behulp van de stappen die worden vermeld in de volgende sectie. Als de functies zijn ingeschakeld, kunnen ze voor alle Recovery Services agentcomputers, Azure Backup-Server-exemplaren gelden en Data Protection Manager-servers geregistreerd bij de kluis. Inschakelen van deze instelling is een eenmalige bewerking en kunt u deze functies niet uitschakelen nadat deze is ingeschakeld.
>

## <a name="enable-security-features"></a>Beveiligingsfuncties inschakelen
Als u een Recovery Services-kluis maakt, kunt u alle beveiligingsfuncties. Als u met een bestaande kluis werkt, moet u beveiligingsfuncties inschakelen met de volgende stappen:

1. Meld u aan bij de Azure portal met behulp van uw Azure-referenties.
2. Selecteer **Bladeren**, en het type **Recovery Services**.

    ![Schermopname van Azure portal optie voor bladeren](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer een kluis in deze lijst. Het geselecteerde kluisdashboard wordt geopend.
3. In de lijst met items die wordt weergegeven onder de kluis onder **instellingen**, klikt u op **eigenschappen**.

    ![Schermopname van Recovery Services-kluis opties](./media/backup-azure-security-feature/vault-list-properties.png)
4. Onder **beveiligingsinstellingen**, klikt u op **Update**.

    ![Schermopname van Recovery Services-kluis eigenschappen](./media/backup-azure-security-feature/security-settings-update.png)

    Hiermee opent u de koppeling voor het bijwerken de **beveiligingsinstellingen** blade die bevat een samenvatting van de functies en kunt u ze inschakelen.
5. In de lijst vervolgkeuzelijst **hebt geconfigureerd Azure multi-factor Authentication?**, selecteert u een waarde om te bevestigen of u hebt ingeschakeld [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md). Als deze is ingeschakeld, wordt u gevraagd om te verifiëren vanaf een ander apparaat (bijvoorbeeld een mobiele telefoon) tijdens het aanmelden bij de Azure-portal.

   Wanneer u kritieke bewerkingen in back-up uitvoert, hebt u een PINCODE, beschikbaar is op de Azure portal-beveiliging opgeven. Azure multi-factor Authentication inschakelt, wordt een beveiligingslaag toegevoegd. Alleen gebruikers met een geldige Azure-referenties geautoriseerde en van een tweede apparaat is geverifieerd, toegang tot de Azure-portal.
6. Selecteren om op te slaan beveiligingsinstellingen, **inschakelen** en klik op **opslaan**. U kunt selecteren **inschakelen** pas nadat u een waarde van de **hebt geconfigureerd Azure multi-factor Authentication?** lijst in de vorige stap.

    ![Schermopname van beveiligingsinstellingen](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Verwijderde back-upgegevens herstellen
Back-up verwijderde back-upgegevens voor een extra 14 dagen behouden en niet wordt verwijderd als onmiddellijk de **Stop back-up met back-upgegevens verwijderen** bewerking wordt uitgevoerd. Voor het herstellen van deze gegevens in de 14 dagen, voert u de volgende stappen, afhankelijk van wat u gebruikt:

Voor **Azure Recovery Services agent** gebruikers:

1. Als de computer waar de back-ups zijn gebeurt er nog steeds beschikbaar is, gebruikt u [herstellen van gegevens op de machine](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) in Azure Recovery Services van de oude herstelpunten wilt herstellen.
2. Als deze computer niet beschikbaar is, gebruikt u [herstellen naar een alternatieve machine](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) naar een andere Azure Recovery Services-computer gebruiken om deze gegevens te verkrijgen.

Voor **Azure Backup-Server** gebruikers:

1. Als de server waar de back-ups zijn gebeurt er nog steeds beschikbaar is, opnieuw de verwijderde gegevensbronnen te beveiligen en gebruiken de **gegevens herstellen** functie van de oude herstelpunten wilt herstellen.
2. Als deze server niet beschikbaar is, gebruikt u [gegevens herstellen vanaf een andere back-upserver van Azure](backup-azure-alternate-dpm-server.md) een ander exemplaar van Azure Backup-Server gebruiken voor deze gegevens.

Voor **Data Protection Manager** gebruikers:

1. Als de server waar de back-ups zijn gebeurt er nog steeds beschikbaar is, opnieuw de verwijderde gegevensbronnen te beveiligen en gebruiken de **gegevens herstellen** functie van de oude herstelpunten wilt herstellen.
2. Als deze server niet beschikbaar is, gebruikt u [externe DPM toevoegen](backup-azure-alternate-dpm-server.md) naar een andere Data Protection Manager-server gebruikt deze gegevens ophalen.

## <a name="prevent-attacks"></a>Aanvallen te voorkomen
Controles zijn toegevoegd om ervoor te zorgen dat alleen geldige gebruikers verschillende bewerkingen kunnen uitvoeren. Het gaat hierbij om een extra verificatielaag toe te voegen en een minimale bewaartermijn voor hersteldoeleinden te bewaren.

### <a name="authentication-to-perform-critical-operations"></a>Verificatie kritieke bewerkingen uit te voeren
Als onderdeel van het toevoegen van een extra verificatielaag voor kritieke bewerkingen u wordt gevraagd een beveiligingsbeleid voor PINCODE invoeren wanneer u uitvoert **beveiliging stoppen met het verwijderen van gegevens** en **wijziging wachtwoordzin** bewerkingen.

Deze PINCODE ontvangen:

1. Meld u aan bij Azure Portal.
2. Blader naar **Recovery Services-kluis** > **instellingen** > **eigenschappen**.
3. Onder **BEVEILIGINGSCODE**, klikt u op **genereren**. Hiermee opent u een blade met de PINCODE moet worden ingevoerd in de gebruikersinterface van de Azure Recovery Services-agent.
    Deze PINCODE slechts vijf minuten geldig is en deze automatisch wordt gegenereerd na die periode.

### <a name="maintain-a-minimum-retention-range"></a>Een minimale bewaartermijn onderhouden
Om ervoor te zorgen dat er altijd een geldig aantal herstelpunten beschikbaar zijn, zijn de volgende controles toegevoegd:

- Voor het dagelijkse bewaren van een minimum van **zeven** dagen retentieperiode moeten worden gedaan.
- Voor het bewaren van wekelijks, een minimum van **vier** weken retentieperiode moeten worden gedaan.
- Voor het maandelijkse bewaren van een minimum van **drie** retentieperiode van maanden moeten worden gedaan.
- Voor het bewaren van jaarlijkse, een minimum van **één** jaar retentieperiode moet worden gedaan.

## <a name="notifications-for-critical-operations"></a>Meldingen voor kritieke bewerkingen
Wanneer een kritieke bewerking wordt uitgevoerd, wordt de abonnementsbeheerder normaal gesproken een e-mailbericht met informatie over het opnieuw verzonden. U kunt extra e-mailontvangers voor deze meldingen configureren met behulp van de Azure-portal.

De beveiligingsfuncties in dit artikel genoemde bieden verdediging mechanismen tegen gerichte aanvallen. Belangrijker is, als een aanval gebeurt, bieden deze functies u de mogelijkheid om uw gegevens te herstellen.

## <a name="troubleshooting-errors"></a>Het oplossen van problemen
| Bewerking | Details van fouten | Oplossing |
| --- | --- | --- |
| Beleid wijzigen |Het back-upbeleid kan niet worden gewijzigd. Fout: De huidige bewerking is mislukt vanwege een interne servicefout [0x29834]. Probeer het later opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |**Oorzaak:**<br/>Deze fout wordt geleverd wanneer beveiligingsinstellingen zijn ingeschakeld, u probeert te verkort de bewaartermijn lager dan de minimale waarden die hierboven is opgegeven en u zich op een niet-ondersteunde versie (ondersteunde versies zijn opgegeven in de eerste notitie van dit artikel). <br/>**Aanbevolen actie:**<br/> In dit geval moet u de bewaarperiode boven de minimale periode opgegeven bewaarperiode (zeven dagen voor dagelijks, vier weken wekelijks, drie weken voor maandelijks of één jaar voor per jaar) instellen om door te gaan met het beleid betrekking updates. Gewenste aanpak kan eventueel worden back-upagent, Azure Backup-Server en/of DPM UR gebruikmaken van alle beveiligingsupdates bijwerken. |
| Wachtwoordzin wijzigen |Beveiliging opgegeven PINCODE is onjuist. (ID: 100130) Geef de juiste beveiliging PINCODE om deze bewerking te voltooien. |**Oorzaak:**<br/> Deze fout wordt geleverd wanneer u ongeldige of verlopen BEVEILIGINGSCODE invoeren tijdens het uitvoeren van kritieke bewerking (zoals wachtwoordzin wijzigen). <br/>**Aanbevolen actie:**<br/> Om de bewerking niet voltooien, moet u geldige BEVEILIGINGSCODE invoeren. Als u de PINCODE, aanmelden bij Azure portal en navigeer naar de Recovery Services-kluis > Instellingen > Eigenschappen > beveiliging-PINCODE genereren. Gebruik deze PINCODE wijzigen wachtwoordzin. |
| Wachtwoordzin wijzigen |Bewerking is mislukt. ID: 120002 |**Oorzaak:**<br/>Deze fout wordt geleverd wanneer beveiligingsinstellingen zijn ingeschakeld, u probeert te wijzigen wachtwoordzin en u zich op een niet-ondersteunde versie (geldige versies die zijn opgegeven in de eerste notitie van dit artikel).<br/>**Aanbevolen actie:**<br/> Als u wilt wijzigen wachtwoordzin, moet u eerst backup-agent bijwerken naar minimumversie minimale 2.0.9052, Azure back-upserver voor minimale update 1, en/of minimale DPM 2012 R2 UR12 met DPM of DPM 2016 UR2 (downloadkoppelingen hieronder), voer dan geldig BEVEILIGINGSCODE. Als u de PINCODE, aanmelden bij Azure portal en navigeer naar de Recovery Services-kluis > Instellingen > Eigenschappen > beveiliging-PINCODE genereren. Gebruik deze PINCODE wijzigen wachtwoordzin. |

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Recovery Services-kluis](backup-azure-vms-first-look-arm.md) inschakelen van deze functies.
* [Download de nieuwste Azure Recovery Services agent](http://aka.ms/azurebackup_agent) voor het beveiligen van Windows-computers en bescherming van uw back-upgegevens tegen aanvallen.
* [Download de meest recente Azure Backup-Server](https://aka.ms/latest_azurebackupserver) voor het beveiligen van workloads en bescherming van uw back-upgegevens tegen aanvallen.
* [UR12 voor System Center 2012 R2 Data Protection Manager downloaden](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) of [UR2 voor System Center 2016 Data Protection Manager downloaden](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) voor het beveiligen van workloads en bescherming van uw back-upgegevens tegen aanvallen.
