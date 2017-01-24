---
title: Aan de slag met back-up en herstel van Azure SQL-databases voor gegevensbescherming en -herstel | Microsoft Docs
description: In deze zelfstudie leert u hoe u via geautomatiseerde back-ups kunt herstellen naar een eerder tijdstip, geautomatiseerde back-ups kunt opslaan in de Azure Recovery Services-kluis en kunt herstellen vanuit de Azure Recovery Services-kluis
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 7f26cd0f6c5f9c7a2fe692bfcdc6ef60d1b2200f
ms.openlocfilehash: d4ea089ed4b5d29c261b25e95f4d304611f9a857


---
<!------------------
This topic is annotated with TEMPLATE guidelines for TUTORIAL TOPICS.


Metadata guidelines

title
    60 characters or less. Tells users clearly what they will do (deploy an ASP.NET web app to App Service). Not the same as H1. It's 60 characters or fewer including all characters between the quotes and the Microsoft Docs site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015."
------------------>

<!----------------

TEMPLATE GUIDELINES for tutorial topics

The tutorial topic shows users how to solve a problem using a product or service. It includes the prerequisites and steps users need to be successful.  

It is a "solve a problem" topic, not a "learn concepts" topic.

DO include this:
    • What users will do
    • What they will create or accomplish by the end of the tutorial
    • Time estimate
    • Optional but useful: Include a diagram or video. Diagrams help users see the big picture of what they are doing. A video of the steps can be used by customers as an alternative to following the steps in the topic.
    • Prerequisites: Technical expertise and software requirements
    • End-to-end steps. At the end, include next steps to deeper or related tutorials so users can learn more about the service

DON'T include this:
    • Conceptual info about the service. This info is in overview topics that you can link to in the prerequisites section if necessary

------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What will I do in this topic?" Write the H1 heading in conversational language and use search keywords as much as possible. Since this is a "solve a problem" topic, make sure the title indicates that. Use a strong, specific verb like "Deploy."  
        
    Heading must use an industry standard term. If your feature is a proprietary name like "elastic pools", use a synonym. For example: "Learn about elastic pools for multi-tenant databases." In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery"></a>Aan de slag met back-up en herstel voor gegevensbescherming en -herstel

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top keywords that you are using throughout the article.The introduction should be brief and to the point of what users will do and what they will accomplish. 

    In this example:
     

Sentence #1 Explains what the user will do. This is also the metadata description. 
    This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015. 

Sentence #2 Explains what users will learn and the benefit.  
    When you’re finished, you’ll have a simple web application up and running in the cloud.

-------------------->


In deze aan de slag-zelfstudie leert u hoe u Azure Portal kunt gebruiken voor het volgende:

- Bestaande back-ups van een database weergeven
- Een database herstellen naar een eerder tijdstip
- Een langetermijnretentie voor een databaseback-upbestand configureren in de Azure Recovery Services-kluis
- Een database herstellen vanuit de Azure Recovery Services-kluis

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).


## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* U hebt de zelfstudie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio](sql-database-get-started.md) of de equivalente [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie voltooid. Als u dit niet hebt gedaan, voltooit u eerst deze vereiste zelfstudie of voert u het PowerShell-script aan het einde van de [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie uit voordat u doorgaat.


> [!TIP]
> U kunt dezelfde taken uitvoeren in een 'Aan de slag'-zelfstudie met [PowerShell](sql-database-get-started-backup-recovery-powershell.md).


## <a name="sign-in-by-using-your-existing-account"></a>Aanmelden met uw bestaande account
Gebruik uw [bestaande abonnement](https://account.windowsazure.com/Home/Index) en volg deze stappen om verbinding te maken met Azure Portal.

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Het oudste herstelpunt van de door de service gegenereerde back-ups van een database weergeven

In deze sectie van de zelfstudie kunt u informatie bekijken over het oudste herstelpunt van de [door de service gegenereerde automatische back-ups](sql-database-automated-backups.md) van uw database. 

1. Open de **SQL-database**-blade voor uw database, **sqldbtutorialdb**.

    ![blade nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-blade.png)

2. Klik op de werkbalk op **Herstellen**.

    ![werkbalk herstellen](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. Controleer het oudste herstelpunt op de blade Herstellen.

    ![oudste herstelpunt](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip

In deze sectie van de zelfstudie herstelt u de database naar een nieuwe database vanaf een bepaald punt in de tijd.

1. Controleer op de blade **Herstellen** voor de database de standaardnaam voor de nieuwe database waarnaar u uw database naar een eerder tijdstip wilt herstellen (de naam is de bestaande databasenaam gevolgd door een tijdstempel). Deze naam wordt aangepast in overeenstemming met het tijdstip dat u in de volgende stappen opgeeft.

    ![naam van herstelde database](./media/sql-database-get-started-backup-recovery/restored-database-name.png)

2. Klik op het **kalender**pictogram in het invoervak **Herstelpunt (UTC)**.

    ![herstelpunt](./media/sql-database-get-started-backup-recovery/restore-point.png)

2. Selecteer op de kalender een datum binnen de bewaartermijn

    ![datum herstelpunt](./media/sql-database-get-started-backup-recovery/restore-point-date.png)

3. Geef in het invoervak **Herstelpunt (UTC)** de tijd op de geselecteerde datum op waarnaar u de gegevens in de database vanuit de automatische databaseback-ups wilt herstellen.

    ![tijd herstelpunt](./media/sql-database-get-started-backup-recovery/restore-point-time.png)

    >[!NOTE]
    >U ziet dat de databasenaam is gewijzigd in overeenstemming met de datum en tijd die u hebt geselecteerd. U ziet ook dat u de server waarop u naar een bepaald punt in de tijd wilt herstellen, niet kunt wijzigen. Als u wilt herstellen naar een andere server, gebruikt u [Geo-herstel](sql-database-disaster-recovery.md#recover-using-geo-restore). Ten slotte ziet u dat u kunt herstellen naar een [elastische pool](sql-database-elastic-jobs-overview.md) of naar een andere prijscategorie. 
    >

4. Klik op **OK** om de database naar een eerder tijdstip te herstellen naar de nieuwe database.

5. Klik op de werkbalk op het meldingspictogram om de status van de hersteltaak weer te geven.

    ![voortgang hersteltaak](./media/sql-database-get-started-backup-recovery/restore-job-progress.png)

6. Als de hersteltaak is voltooid, opent u de blade **SQL-databases** om de herstelde database weer te geven.

    ![herstelde database](./media/sql-database-get-started-backup-recovery/restored-database.png)

   > [!NOTE]
   > Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).
   >

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Langetermijnretentie van automatische back-ups configureren in een Azure Recovery Services-kluis 

In deze sectie van de zelfstudie configureert u [een Azure Recovery Services-kluis voor de retentie van automatische back-ups](sql-database-long-term-retention.md) gedurende een periode die langer is dan de retentietermijn van uw servicecategorie. 


> [!TIP]
> Zie [Delete long-term retention backups](sql-database-long-term-retention-delete.md) (Back-ups voor langetermijnretentie verwijderen) als u back-ups wilt verwijderen.


1. Open de **SQL Server**-blade voor uw server, **sqldbtutorialserver**.

    ![blade sql-server](./media/sql-database-get-started/sql-server-blade.png)

2. Klik op **Langetermijnretentie**.

   ![koppeling langetermijnretentie](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Lees en accepteer op de blade **sqldbtutorial - Langetermijnretentie** de voorwaarden voor het weergeven van een voorbeeld (tenzij u dit al hebt gedaan of deze functie zich niet langer in de voorbeeldmodus bevindt).

   ![voorwaarden voor weergeven van een voorbeeld accepteren](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Als u de langetermijnretentie wilt configureren voor de database sqldbtutorialdb, selecteert u die database in het raster en klikt u vervolgens op **Configureren** op de werkbalk.

   ![database selecteren voor langetermijnretentie](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Klik op de blade **Configureren** op **Vereiste instellingen configureren** onder **Recovery Service-kluis**.

   ![koppeling kluis](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Selecteer op de blade **Recovery Services-kluis** een bestaande kluis, indien aanwezig. Als er voor uw abonnement geen Recovery Services-kluis is gevonden, klikt u om de werkstroom af te sluiten en een Recovery Services-kluis te maken.

   ![koppeling nieuwe kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Klik op de blade **Recovery Services-kluizen** op **Toevoegen**.

   ![koppeling nieuwe kluis toevoegen](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Geef op de blade **Recovery Services-kluis** een geldige naam op voor de nieuwe Recovery Services-kluis.

   ![naam nieuwe kluis](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Selecteer uw abonnement en resourcegroep. Selecteer vervolgens de locatie voor de kluis. Klik op **Maken** als u klaar bent.

   ![nieuwe kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > De kluis moet zich bevinden in dezelfde regio als de logische Azure SQL-server en moet dezelfde resourcegroep gebruiken als de logische server.
   >

10. Voer na het maken van de nieuwe kluis de benodigde stappen uit om terug te keren naar de blade **Recovery Services-kluis**.

11. Klik op de blade **Recovery Services-kluis** op de kluis en klik vervolgens op **Selecteren**.

   ![bestaande kluis selecteren](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Geef op de blade **Configureren** een geldige naam op voor het nieuwe retentiebeleid, pas het standaardretentiebeleid desgewenst aan en klik op **OK**.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. Klik op de blade **sqldbtutorial - Langetermijnretentie** op **Opslaan** en klik vervolgens op **OK** om het langetermijnretentiebeleid voor back-ups toe te passen op alle geselecteerde databases.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klik op **Opslaan** om langetermijnretentie voor back-ups in te schakelen met dit nieuwe beleid voor de Azure Recovery Services-kluis die u hebt geconfigureerd.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

15. Nadat de langetermijnretentie voor back-ups is ingeschakeld, opent u de blade **sqldbtutorialvault** (ga naar **Alle resources** en selecteer deze in de lijst met resources voor uw abonnement).

   ![recovery services-kluis weergeven](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault.png)

   > [!IMPORTANT]
   > Na de configuratie worden back-ups in de kluis binnen de komende zeven dagen weergegeven. Ga pas verder met deze zelfstudie als er back-ups in de kluis worden weergegeven.
   >

## <a name="view-backups-in-long-term-retention"></a>Back-ups met langetermijnretentie weergeven

In deze sectie van de zelfstudie kunt u informatie bekijken over uw databaseback-ups met [langetermijnretentie](sql-database-long-term-retention.md). 

1. Open de blade **sqldbtutorialvault** (ga naar **Alle resources** en selecteer deze in de lijst met resources voor uw abonnement) om de hoeveelheid opslag die door uw back-ups in de kluis wordt gebruikt, te bekijken.

   ![recovery services-kluis weergeven met back-ups](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Open de **SQL-database**-blade voor uw database, **sqldbtutorialdb**.

    ![blade nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-blade.png)

3. Klik op de werkbalk op **Herstellen**.

    ![werkbalk herstellen](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Klik op de blade Herstellen op **Lange termijn**.

5. Klik onder de back-ups van de Azure kluis op **Een back-up selecteren** om de beschikbare databaseback-ups met langetermijnretentie weer te geven.

    ![back-ups in kluis](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Een database herstellen vanuit een back-up met langetermijnretentie

In deze sectie van de zelfstudie herstelt u de database naar een nieuwe database vanuit een back-up in de Azure Recovery Services-kluis.

1. Klik op de blade **Back-ups Azure kluis** op de back-up die u wilt herstellen en klik vervolgens op **Selecteren**.

    ![back-up in kluis selecteren](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. Geef in het tekstvak **Databasenaam** de naam voor de herstelde database op.

    ![nieuwe databasenaam](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klik op **OK** om de database vanuit de back-up in de kluis naar de nieuwe database te herstellen.

4. Klik op de werkbalk op het meldingspictogram om de status van de hersteltaak weer te geven.

    ![taakvoortgang herstellen vanuit kluis](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Als de hersteltaak is voltooid, opent u de blade **SQL-databases** om de herstelde database weer te geven.

    ![uit kluis herstelde database](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

   > [!NOTE]
   > Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).
   >


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
- Zie [Herstellen vanuit back-up](sql-database-recovery-using-backups.md) voor meer informatie over het herstellen van back-ups.



<!--HONumber=Dec16_HO4-->


