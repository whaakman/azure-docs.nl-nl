---
title: 'Azure AD Connect: Het herstellen van LocalDB van 10 GB limiet probleem | Microsoft Docs'
description: Dit onderwerp wordt beschreven hoe u Azure AD Connect-synchronisatieservice herstellen wanneer er 10GB LocalDB probleem beperken.
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 08e682c51b12d4506019d2f6b68e1eae0798b990
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-how-to-recover-from-localdb-10-gb-limit"></a>Azure AD Connect: Het herstellen van 10 GB-limiet LocalDB
Azure AD Connect vereist een SQL Server-database voor het opslaan van identiteitsgegevens. U kunt de standaard SQL Server 2012 Express LocalDB gebruiken die samen met Azure AD Connect is geïnstalleerd, maar ook uw eigen volledige SQL. Voor SQL Server Express geldt een limiet van 10 GB. Wanneer u LocalDB gebruikt en deze limiet is bereikt, kan de Azure AD Connect-synchronisatieservice niet langer starten of goed synchroniseren. Dit artikel bevat de herstelstappen.

## <a name="symptoms"></a>Symptomen
Er zijn twee algemene symptomen:

* Azure AD Connect-synchronisatieservice **wordt uitgevoerd** maar niet kan worden gesynchroniseerd met *'gestopt-database-schijf-volledig'* fout.

* Azure AD Connect-synchronisatieservice **kan niet worden gestart**. Wanneer u probeert de service te starten, mislukt met de gebeurtenis 6323 en foutbericht *"de server een fout opgetreden omdat SQL Server geen schijfruimte meer heeft."*

## <a name="short-term-recovery-steps"></a>Korte termijn herstelstappen
Deze sectie bevat de stappen voor het vrijmaken van DB vereiste ruimte voor Azure AD Connect-synchronisatieservice bewerking hervatten. De stappen omvatten:
1. [De status van de synchronisatieservice bepalen](#determine-the-synchronization-service-status)
2. [De database te verkleinen](#shrink-the-database)
3. [Uitvoeren van de gegevens van geschiedenis verwijderen](#delete-run-history-data)
4. [Geef een kortere bewaarperiode voor gegevens van de geschiedenis uitvoeren](#shorten-retention-period-for-run-history-data)

### <a name="determine-the-synchronization-service-status"></a>De status van de synchronisatieservice bepalen
Bepaal eerst of de synchronisatie-Service nog wordt uitgevoerd of niet:

1. Meld u aan bij uw Azure AD Connect-server als beheerder.

2. Ga naar **Service Control Manager**.

3. Controleer de status van **Microsoft Azure AD Sync**.


4. Als deze wordt uitgevoerd, niet stoppen en start de service. Overslaan [verkleinen van de database](#shrink-the-database) stap en gaat u naar [verwijderen uitvoeren geschiedenisgegevens](#delete-run-history-data) stap.

5. Als deze niet wordt uitgevoerd, probeert u de service te starten. Als de service is gestart, gaat u verder [verkleinen van de database](#shrink-the-database) stap en gaat u naar [verwijderen uitvoeren geschiedenisgegevens](#delete-run-history-data) stap. Ga anders verder met [verkleinen van de database](#shrink-the-database) stap.

### <a name="shrink-the-database"></a>De database te verkleinen
Gebruik de verkleiningsbewerking voldoende ruimte DB starten van de synchronisatieservice vrijmaken. Het vrijgemaakt DB-ruimte door het verwijderen van spaties in de database. Deze stap is best-effort als is er geen garantie dat u altijd ruimte kunt herstellen. Lees dit artikel voor meer informatie over de verkleiningsbewerking [verkleinen van een database](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Deze stap overslaan als u krijgt de synchronisatieservice om uit te voeren. Dit wordt niet aanbevolen voor het verkleinen van de SQL-database zoals tot slechte prestaties vanwege de fragmentatie van de toegenomen leiden kan.

De naam van de database gemaakt voor Azure AD Connect is **ADSync**. Als u wilt een verkleiningsbewerking uitvoeren, moet u zich aanmeldt als de rol sysadmin of de DBO van de database. Tijdens de installatie van Azure AD Connect, worden de volgende accounts sysadmin-rechten verleend:
* Lokale beheerders
* Het gebruikersaccount dat is gebruikt voor het uitvoeren van Azure AD Connect-installatie.
* De synchronisatie-serviceaccount die wordt gebruikt als de context van Azure AD Connect-synchronisatieservice.
* De lokale groep ADSyncAdmins die is gemaakt tijdens de installatie.

1. Maak een back-up van de database door te kopiëren **ADSync.mdf** en **ADSync_log.ldf** bestanden onder `%ProgramFiles%\program files\Microsoft Azure AD Sync\Data` naar een veilige locatie.

2. Start een nieuwe PowerShell-sessie.

3. Navigeer naar de map `%ProgramFiles%\Program Files\Microsoft SQL Server\110\Tools\Binn`.

4. Start **sqlcmd** hulpprogramma met de opdracht `./SQLCMD.EXE -S “(localdb)\.\ADSync” -U <Username> -P <Password>`, met de referenties van een sysadmin of de DBO-database.

5. Voor het verkleinen van de database, op het sqlcmd-opdrachtprompt (1 >), voer `DBCC Shrinkdatabase(ADSync,1);`, gevolgd door `GO` in de volgende regel.

6. Als de bewerking voltooid is, probeert u de synchronisatie-Service opnieuw starten. Als u de Synchronization Service starten kunt, gaat u naar [verwijderen uitvoeren geschiedenisgegevens](#delete-run-history-data) stap. Als dat niet het geval is, moet u contact op met ondersteuning.

### <a name="delete-run-history-data"></a>Uitvoeren van de gegevens van geschiedenis verwijderen
Standaard behoudt Azure AD Connect up aan zeven dagen aan gegevens uitvoeringsgeschiedenis. In deze stap wordt de uitvoeringsgeschiedenis-gegevens om de ingenomen ruimte DB zodat Azure AD Connect-synchronisatieservice kunt starten synchroniseren opnieuw te verwijderen.

1.  Start **Synchronization Service Manager** gaat u naar START → Synchronization Service.

2.  Ga naar de **Operations** tabblad.

3.  Onder **acties**, selecteer **wissen wordt uitgevoerd**...

4.  U kunt kiezen **wissen van alle sessies** of **wissen wordt uitgevoerd voordat er... <date>**  optie. Het wordt aanbevolen dat u start door het wissen van gegevens van geschiedenis die ouder dan twee dagen zijn worden uitgevoerd. Als u DB grootte probleem ondervindt doorgaat, kiest u de **wissen van alle sessies** optie.

### <a name="shorten-retention-period-for-run-history-data"></a>Geef een kortere bewaarperiode voor gegevens van de geschiedenis uitvoeren
Deze stap is de kans na meerdere synchronisatiecycli die worden uitgevoerd in de limiet van 10 GB-probleem.

1. Open een nieuw PowerShell-sessie.

2. Voer `Get-ADSyncScheduler` en noteer de eigenschap PurgeRunHistoryInterval, waarmee de huidige bewaarperiode.

3. Voer `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` in te stellen de bewaarperiode op twee dagen. Pas de bewaarperiode naar gelang van toepassing.

## <a name="long-term-solution--migrate-to-full-sql"></a>Op lange termijn oplossing: migreren naar een volledige SQL
In het algemeen is het probleem indicatief 10 GB-databasegrootte is niet meer voldoende voor Azure AD Connect te synchroniseren van uw lokale Active Directory naar Azure AD. Het is raadzaam dat u overschakelt naar de volledige versie van SQL server. U kunt de LocalDB van een bestaande Azure AD Connect-implementatie niet rechtstreeks vervangen door de database van de volledige versie van SQL. In plaats daarvan implementeert u een nieuwe Azure AD Connect-server met de volledige SQL. U doet er verstandig aan een swingmigratie uit te voeren, waarbij de nieuwe Azure AD Connect-server (met SQL-database) wordt geïmplementeerd als testserver, naast de bestaande Azure AD Connect-server (met LocalDB). 
* Zie het artikel [Aangepaste installatie van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom) voor instructies over het configureren van externe SQL met Azure AD Connect.
* Zie het artikel [Azure AD Connect: Upgrade from a previous version to the latest](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration) (Azure AD Connect: upgraden van een vorige naar de meest recente versie) voor instructies over swingmigratie voor een Azure AD Connect-upgrade.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
