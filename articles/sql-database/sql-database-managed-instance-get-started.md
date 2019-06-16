---
title: 'Azure Portal: Een beheerd exemplaar van SQL-Database maken | Microsoft Docs'
description: Maak een SQL-Database beheerd exemplaar, de netwerkomgeving en de client VM om toegang te krijgen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: 27439758c9e2b909bb7179b69c7aade81429f502
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070497"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Quickstart: Een beheerd exemplaar van Azure SQL Database maken

In deze quickstart begeleidt u bij het maken van een Azure SQL Database [beheerd exemplaar](sql-database-managed-instance.md) in Azure portal.

> [!IMPORTANT]
> Zie voor beperkingen, [ondersteunde regio's](sql-database-managed-instance-resource-limits.md#supported-regions) en [abonnementstypen ondersteund](sql-database-managed-instance-resource-limits.md#supported-subscription-types).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Als u nog geen abonnement op Azure hebt, [maak dan een gratis account](https://azure.microsoft.com/free/).

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

In de volgende stappen wordt uitgelegd hoe u een beheerd exemplaar maakt.

1. Selecteer **een resource maken** in de linkerbovenhoek van Azure portal.
2. Zoek **beheerd exemplaar**, en selecteer vervolgens **Azure SQL Managed Instance**.
3. Selecteer **Maken**.

   ![Een beheerd exemplaar maken](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Vul de **SQL beheerd exemplaar** formulier met de aangevraagde informatie met behulp van de informatie in de volgende tabel.

   | Instelling| Voorgestelde waarde | Description |
   | ------ | --------------- | ----------- |
   | **Abonnement** | Uw abonnement. | Een abonnement waarmee u beschikt over machtigingen voor het maken van nieuwe resources. |
   |**Naam van het beheerde exemplaar**|Een geldige naam.|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Beheerdersaanmeldgegevens voor het beheerde exemplaar**|Een geldige gebruikersnaam.|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen. Gebruik niet 'serverbeheerder' omdat dit een gereserveerde niveau van de server-rol.|
   |**Wachtwoord**|Een geldig wachtwoord.|Het wachtwoord moet minstens 16 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Tijdzone**|De tijdzone die moet worden gevolgd door uw beheerde exemplaar.|Zie voor meer informatie, [tijdzones](sql-database-managed-instance-timezone.md).|
   |**Sortering**|De sortering die u wilt gebruiken voor uw beheerde exemplaar.|Als u databases van SQL Server migreert, controleert u de sortering van de gegevensbron met behulp van `SELECT SERVERPROPERTY(N'Collation')` en die waarde gebruiken. Zie voor meer informatie over sorteringen [instellen of wijzigen van de server-sortering](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|
   |**Location**|De locatie waarin u wilt maken van het beheerde exemplaar.|Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's.|
   |**Virtueel netwerk**|Selecteer **Nieuw virtueel netwerk maken** of een geldig virtueel netwerk en subnet.| Als een netwerk of subnet niet beschikbaar is, moet dit [gewijzigd om te voldoen aan de netwerkvereisten](sql-database-managed-instance-configure-vnet-subnet.md) voordat u deze als een doel voor de nieuwe beheerde exemplaar selecteren. Zie voor meer informatie over de vereisten voor het configureren van de netwerkomgeving voor een beheerd exemplaar [configureren van een virtueel netwerk voor een beheerd exemplaar](sql-database-managed-instance-connectivity-architecture.md). |
   |**Openbare eindpunt inschakelen**   |Schakel deze optie in op het openbare eindpunt inschakelen   |Voor het beheerde exemplaar moeten toegankelijk zijn via het eindpunt openbare gegevens **openbare eindpunt inschakelen** moet worden gecontroleerd.| 
   |**Toegang toestaan**   |Selecteer een van de opties: <ul> <li>**Azure-services**</li> <li>**Internet**</li> <li>**Geen toegang**</li></ul>   |Portal-ervaring kunt configureren beveiligingsgroep met openbaar eindpunt. </br> </br> Op basis van uw scenario, selecteer een van de volgende opties: </br> <ul> <li>Azure-services - aanbevolen bij het verbinden met Power BI of andere service met meerdere tenants. </li> <li> Internet - gebruik voor test-doeleinden als u wilt tuig snel een beheerd exemplaar. Het wordt niet aanbevolen voor gebruik in productieomgevingen. </li> <li> Geen toegang: deze optie maakt een regel voor weigeren. U moet deze regel wijzigen om het beheerde exemplaar toegankelijk zijn via openbare eindpunt maken. </li> </ul> </br> Zie voor meer informatie over de beveiliging van de openbare eindpunt [met behulp van Azure SQL-Database beheerd exemplaar veilig met een openbaar eindpunt](sql-database-managed-instance-public-endpoint-securely.md).|
   |**Verbindingstype**|Kiezen tussen een Proxy- en een omleidings-verbindingstype.|Zie voor meer informatie over verbindingstypen [Azure SQL Database-verbindingsbeleid](sql-database-connectivity-architecture.md#connection-policy).|
   |**Resourcegroep**|een nieuwe of bestaande resourcegroep.|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen.|

   ![formulier beheerd exemplaar](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Voor gebruik van het beheerde exemplaar als secundaire exemplaarfailovergroep schakelt u het selectievakje in en geeft u het beheerde exemplaar DnsAzurePartner op. Deze functie is beschikbaar als preview en wordt niet weergegeven in de volgende schermafbeelding.
6. Selecteer **prijscategorie** omvang reken- en opslagresources en bekijk de opties voor Prijscategorieën. De prijscategorie voor algemeen gebruik met 32 GB geheugen en 16 vCores is de standaardwaarde.
7. Gebruik de schuifregelaars of tekstvakken om de hoeveelheid opslagruimte en het aantal virtuele kernen op te geven.
8. Wanneer u klaar bent, selecteert u **toepassen** om op te slaan, uw selectie. 
9. Selecteer **Maken** om het beheerde exemplaar te implementeren.
10. Selecteer de **meldingen** pictogram om de status van de implementatie weer te geven.

    ![voortgang van de implementatie beheerd exemplaar](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Selecteer **Implementatie wordt uitgevoerd** om het venster Beheerd exemplaar te openen. Hier kunt u details van de voortgang van de implementatie controleren. 

> [!IMPORTANT]
> Het eerste exemplaar in een subnet kent doorgaans een veel langere implementatietijd dan volgende exemplaren. Annuleer de implementatiebewerking niet omdat deze langer duurt dan verwacht.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Resources controleren en de volledig gekwalificeerde servernaam ophalen

Wanneer de implementatie geslaagd is, bekijk de resources die zijn gemaakt en de volledig gekwalificeerde servernaam voor gebruik in latere snelstartgidsen ophalen.

1. Open de resourcegroep voor uw beheerde exemplaar. Weergeven van de resources die zijn gemaakt voor u in de [een beheerd exemplaar maken](#create-a-managed-instance) Quick Start.

   ![Resources voor beheerd exemplaar](./media/sql-database-managed-instance-get-started/resources.png)

2. Selecteer de routetabel om te controleren van de gebruiker gedefinieerde route (UDR) tabel die voor u is gemaakt.

   ![Routetabel](./media/sql-database-managed-instance-get-started/route-table.png)

3. Bekijk de vermeldingen in de routetabel om verkeer vanaf en binnen het virtuele netwerk van het beheerde exemplaar door te sturen. Als u maakt of uw routetabel handmatig configureren, moet u ervoor dat u deze vermeldingen maken in de routetabel zijn.

   ![Vermelding voor een beheerd exemplaar-subnet aan lokale](./media/sql-database-managed-instance-get-started/udr.png)

4. Ga terug naar de resourcegroep en selecteer de netwerkbeveiligingsgroep om te controleren van de beveiligingsregels.

   ![Netwerkbeveiligingsgroep](./media/sql-database-managed-instance-get-started/network-security-group.png)

5. Bekijk de inkomende en uitgaande beveiligingsregels. Als u openbare eindpunten zijn geconfigureerd voor uw beheerde exemplaar, Zie het artikel [openbare eindpunt configureren](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group) voor meer informatie.

   ![Beveiligingsregels](./media/sql-database-managed-instance-get-started/security-rules.png)

6. Terug naar de resourcegroep en selecteer uw beheerde exemplaar.

   ![Beheerd exemplaar](./media/sql-database-managed-instance-get-started/managed-instance.png)

7. Op de **overzicht** tabblad, Ga naar de **Host** eigenschap. Kopieer het adres van de volledig gekwalificeerde host voor het beheerde exemplaar voor gebruik in de volgende snelstartgids.

   ![Hostnaam](./media/sql-database-managed-instance-get-started/host-name.png)

   De naam ziet er als volgt uit: **naam_van_uw_machine.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over hoe u verbinding maken met een beheerd exemplaar:
  - Zie voor een overzicht van de opties voor de verbinding voor toepassingen, [verbinding maken met uw toepassingen naar een beheerd exemplaar](sql-database-managed-instance-connect-app.md).
  - Zie voor een snelstartgids die laat zien hoe u verbinding maken met een beheerd exemplaar van een virtuele machine van Azure, [een virtuele machine van Azure-verbinding configureren](sql-database-managed-instance-configure-vm.md).
  - Zie voor een snelstartgids die laat hoe u verbinding maken met een beheerd exemplaar van een on-premises clientcomputer zien met behulp van een punt-naar-site-verbinding, [een punt-naar-site-verbinding configureren](sql-database-managed-instance-configure-p2s.md).
- Een bestaande SQL Server-database herstellen van on-premises naar een beheerd exemplaar: 
    - Gebruik de [Azure Database Migration Service (DMS) voor de migratie](../dms/tutorial-sql-server-to-managed-instance.md) om terug te zetten vanuit een back-upbestand van de database. 
    - Gebruik de [T-SQL terugzetten opdracht](sql-database-managed-instance-get-started-restore.md) om terug te zetten vanuit een back-upbestand van de database.
- Voor geavanceerde bewaking van prestaties van de database beheerde instanties met ingebouwde intelligentie voor het oplossen van problemen, Zie [Azure SQL Database controleren met behulp van Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).
