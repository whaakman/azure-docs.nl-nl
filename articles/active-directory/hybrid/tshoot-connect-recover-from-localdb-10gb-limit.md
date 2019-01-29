---
title: 'Azure AD Connect: Herstellen vanuit LocalDB probleem van de limiet van 10 GB | Microsoft Docs'
description: In dit onderwerp wordt beschreven hoe u Azure AD Connect-synchronisatieservice herstellen wanneer er 10GB LocalDB probleem beperken.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 08e1beb199287db1d030d5ad9357cbd20f1eb859
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168632"
---
# <a name="azure-ad-connect-how-to-recover-from-localdb-10-gb-limit"></a>Azure AD Connect: Probleem met LocalDB met limiet van 10 GB oplossen
Azure AD Connect vereist een SQL Server-database voor het opslaan van identiteitsgegevens. U kunt de standaard SQL Server 2012 Express LocalDB gebruiken die samen met Azure AD Connect is geïnstalleerd, maar ook uw eigen volledige SQL. Voor SQL Server Express geldt een limiet van 10 GB. Wanneer u LocalDB gebruikt en deze limiet is bereikt, kan de Azure AD Connect-synchronisatieservice niet langer starten of goed synchroniseren. Dit artikel bevat de herstelstappen.

## <a name="symptoms"></a>Symptomen
Er zijn twee algemene symptomen:

* Azure AD Connect-synchronisatieservice **wordt uitgevoerd** tevergeefs om te synchroniseren met *"gestopt-database-schijf vol"* fout.

* Azure AD Connect-synchronisatieservice **kan niet worden gestart**. Wanneer u probeert om de service te starten, mislukt met de gebeurtenis 6323 en het volgende foutbericht *"de server een fout opgetreden omdat SQL Server heeft te weinig schijfruimte."*

## <a name="short-term-recovery-steps"></a>Op korte termijn herstelstappen
Deze sectie bevat de stappen voor het vrijmaken van DB vereiste ruimte voor Azure AD Connect-synchronisatieservice om te hervatten. De stappen omvatten:
1. [De status van de synchronisatie bepalen](#determine-the-synchronization-service-status)
2. [De database te verkleinen](#shrink-the-database)
3. [Uitvoeringsgeschiedenisgegevens verwijderen](#delete-run-history-data)
4. [Verkort de bewaarperiode voor gegevens van uitvoeringsgeschiedenis](#shorten-retention-period-for-run-history-data)

### <a name="determine-the-synchronization-service-status"></a>De status van de synchronisatie bepalen
Bepaal eerst of de synchronisatie-Service nog steeds wordt uitgevoerd of niet:

1. Meld u aan bij uw Azure AD Connect-server aan als beheerder.

2. Ga naar **Service Control Manager**.

3. Controleer de status van **Microsoft Azure AD Sync**.


4. Als deze wordt uitgevoerd, geen stop en start de service opnieuw op. Overslaan [verkleinen van de database](#shrink-the-database) stap en gaat u naar [verwijderen uitvoeringsgeschiedenisgegevens](#delete-run-history-data) stap.

5. Als deze niet wordt uitgevoerd, probeert u de service te starten. Als de service is gestart, gaat u verder [verkleinen van de database](#shrink-the-database) stap en gaat u naar [verwijderen uitvoeringsgeschiedenisgegevens](#delete-run-history-data) stap. Anders gaat u door met [verkleinen van de database](#shrink-the-database) stap.

### <a name="shrink-the-database"></a>De database te verkleinen
Gebruik de verkleiningsbewerking opnieuw om vrij te maken genoeg ruimte is DB voor de synchronisatieservice niet starten. Het vrijgemaakt DB-ruimte door het verwijderen van alleen uit spaties bestaan in de database. Deze stap is best-effort, is er geen garantie dat u altijd ruimte kunt herstellen. Lees dit artikel voor meer informatie over de verkleiningsbewerking [verkleinen van een database](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Deze stap overslaan als u de Synchronization Service om uit te voeren. Het wordt niet aanbevolen voor het verkleinen van de SQL-database, kan dit leiden tot slechte prestaties vanwege toegenomen fragmentatie.

De naam van de database hebt gemaakt voor Azure AD Connect is **ADSync**. Als u wilt een verkleiningsbewerking opnieuw uitvoeren, moet u zich aanmeldt als de rol sysadmin of de DBO van de database. Tijdens de installatie van Azure AD Connect, worden de volgende accounts sysadmin-rechten verleend:
* Lokale beheerders
* Het gebruikersaccount dat is gebruikt voor het uitvoeren van Azure AD Connect-installatie.
* De Sync-Service-account dat wordt gebruikt als de context van Azure AD Connect-synchronisatieservice.
* De lokale groep ADSyncAdmins die is gemaakt tijdens de installatie.

1. Maak een back-up van de database door te kopiëren **ADSync.mdf** en **ADSync_log.ldf** bestanden die zich onder `%ProgramFiles%\Microsoft Azure AD Sync\Data` op een veilige locatie.

2. Een nieuwe PowerShell-sessie starten.

3. Navigeer naar de map `%ProgramFiles%\Microsoft SQL Server\110\Tools\Binn`.

4. Start **sqlcmd** hulpprogramma met de opdracht `./SQLCMD.EXE -S "(localdb)\.\ADSync" -U <Username> -P <Password>`, met behulp van de referentie van een sysadmin of de DBO-database.

5. Voor het verkleinen van de database, klikt u op de sqlcmd-opdrachtprompt (1 >), voer `DBCC Shrinkdatabase(ADSync,1);`, gevolgd door `GO` in de volgende regel.

6. Als de bewerking geslaagd is, probeert u de Synchronization Service opnieuw starten. Als u de Synchronization Service starten kunt, gaat u naar [verwijderen uitvoeringsgeschiedenisgegevens](#delete-run-history-data) stap. Als dat niet het geval is, moet u contact op met ondersteuning.

### <a name="delete-run-history-data"></a>Uitvoeringsgeschiedenisgegevens verwijderen
Standaard behoudt Azure AD Connect omhoog naar zeven dagen aan gegevens van de uitvoeringsgeschiedenis. In deze stap verwijderen we de gegevens van uitvoeringsgeschiedenis DB om vrij te maken zodat Azure AD Connect-synchronisatieservice synchroniseren opnieuw kunt starten.

1.  Start **Synchronization Service Manager** door te gaan naar de synchronisatieservice START →.

2.  Ga naar de **Operations** tabblad.

3.  Onder **acties**, selecteer **wissen wordt uitgevoerd**...

4.  U kunt kiezen **wissen van alle uitvoeringen** of **wissen wordt uitgevoerd voordat er... <date>**  optie. Het is raadzaam dat u start door uitvoeringsgeschiedenisgegevens die ouder dan twee dagen zijn uit te schakelen. Als u DB grootte probleem ondervindt doorgaat, kiest u de **wissen van alle uitvoeringen** optie.

### <a name="shorten-retention-period-for-run-history-data"></a>Verkort de bewaarperiode voor gegevens van uitvoeringsgeschiedenis
Deze stap is het minder kans op wordt uitgevoerd in de limiet van 10 GB-probleem na meerdere synchronisatiecycli.

1. Open een nieuwe PowerShell-sessie.

2. Voer `Get-ADSyncScheduler` en noteer de eigenschap PurgeRunHistoryInterval, die Hiermee geeft u de huidige bewaarperiode.

3. Voer `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` de bewaarperiode instellen op twee dagen. De bewaarperiode waar nodig aanpassen.

## <a name="long-term-solution--migrate-to-full-sql"></a>Oplossing op lange termijn: migreren naar volledige SQL
In het algemeen is het probleem op duiden dat de grootte van 10 GB-database niet meer voldoende is voor Azure AD Connect om te synchroniseren van uw on-premises Active Directory naar Azure AD. Het is raadzaam dat u overschakelt naar de volledige versie van SQL server. U kunt de LocalDB van een bestaande Azure AD Connect-implementatie niet rechtstreeks vervangen door de database van de volledige versie van SQL. In plaats daarvan implementeert u een nieuwe Azure AD Connect-server met de volledige SQL. U doet er verstandig aan een swingmigratie uit te voeren, waarbij de nieuwe Azure AD Connect-server (met SQL-database) wordt geïmplementeerd als testserver, naast de bestaande Azure AD Connect-server (met LocalDB). 
* Zie het artikel [Aangepaste installatie van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom) voor instructies over het configureren van externe SQL met Azure AD Connect.
* Voor instructies over swingmigratie voor Azure AD Connect-upgrade, Raadpleeg het artikel [Azure AD Connect: Een upgrade uitvoeren van een eerdere versie naar de meest recente](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration).

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
