---
title: Overzicht van Azure SQL Database-beveiliging| Microsoft Docs
description: Meer informatie over de beveiliging van Azure SQL Database en SQL Server, inclusief de verschillen tussen de cloud en SQL Server on-premises als het gaat om verificatie, autorisatie, beveiliging van de verbinding, versleuteling en compatibiliteit.
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 06/09/2016
ms.author: thmullan;jackr
translationtype: Human Translation
ms.sourcegitcommit: 69faa86ddbc43793146653fc8d8dc2bf35c40aa1
ms.openlocfilehash: f3a7bcbc80580232f2704087eb529ee9ec8ead46


---
# <a name="securing-your-sql-database"></a>Uw SQL Database beveiligen

In dit artikel worden de basisbeginselen van het beveiligen van de gegevenslaag van een toepassing met behulp van Azure SQL Database besproken. Dit artikel helpt u in het bijzonder om aan de slag te gaan met resources voor gegevensbeveiliging, toegangsbeheer en proactieve controle. Het volgende diagram illustreert de beveiligingslagen die worden geboden met SQL Database.

![SQL-beveiliging en -naleving](./media/sql-database-security-overview/diagram.png)

Zie het [Beveiligingscentrum voor SQL Server Database Engine en Azure SQL Database](https://msdn.microsoft.com/library/bb510589) voor een volledig overzicht van beveiligingsfuncties die beschikbaar zijn in alle versies van SQL. In het [technische rapport over beveiliging en Azure SQL Database](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF-Bestand) is ook aanvullende informatie beschikbaar.

## <a name="protect-data"></a>Gegevens beveiligen
Uw gegevens worden beveiligd met SQL Database via gegevensversleuteling. Voor gegevens die in beweging zijn, wordt [Transport Layer Security](https://support.microsoft.com/en-us/kb/3135244) gebruikt, voor data-at-rest [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242) en voor gebruikte gegevens [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx). Zie [Data protection and security](sql-database-protect-data.md) (Gegevensbescherming en beveiliging) voor een discussie over het gebruik van deze functies voor gegevensbeveiliging in SQL Database.

> [!IMPORTANT]
>Alle verbindingen met Azure SQL Database vereisen te allen tijde versleuteling (SSL/TLS) wanneer gegevens naar en vanuit de database worden verzonden. In de verbindingsreeks van uw toepassing moet u de parameters voor het versleutelen van de verbinding opgeven en *niet* het servercertificaat vertrouwen (dit doet u als u de verbindingsreeks buiten de klassieke Azure Portal kopieert). Anders verifieert de verbinding niet de identiteit van de server en is deze vatbaar voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET zijn deze verbindingsreeksparameters bijvoorbeeld **Encrypt=True** en **TrustServerCertificate=False**. 

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

* [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
* Als u een Hardware Security Module of centraal beheer van uw versleutelingssleutelhiërarchie nodig heeft, kunt u overwegen om [Azure Key Vault met SQL Server in een virtuele Azure-machine](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx) te gebruiken.

## <a name="control-access"></a>Toegang beheren
Uw gegevens worden beveiligd met SQL Database door de toegang tot uw database te beperken met behulp van firewallregels, verificatiemechanismen die vereisen dat gebruikers hun identiteit bewijzen, en autorisatie voor gegevens via lidmaatschappen en machtigingen op basis van rollen. Ook wordt gebruikgemaakt van beveiliging op rijniveau en dynamische gegevensmaskering. Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.

> [!IMPORTANT]
> Het beheer van databases en logische servers in Azure wordt bepaald door de roltoewijzingen voor uw Portal-gebruikersaccount. Zie [Op rollen gebaseerd toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-configure.md) voor meer informatie over dit onderwerp.
>

### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels
Met firewalls wordt met behulp van [firewallregels](sql-database-firewall-configure.md) alle toegang tot uw databaseserver beperkt om uw gegevens te helpen beveiligen, totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

### <a name="authentication"></a>Authentication
SQL Database-verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

* **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord worden gebruikt. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. 
* **Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory. Deze methode wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory-verificatie (geïntegreerde beveiliging). Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

### <a name="authorization"></a>Autorisatie
Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de databaserollidmaatschappen en objectmachtigingen van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing.

### <a name="row-level-security"></a>Beveiliging op rijniveau
Met beveiliging op rijniveau kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groepslidmaatschap of uitvoeringscontext). Zie [Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) voor meer informatie.

### <a name="data-masking"></a>Gegevensmaskering 
Met dynamische gegevensmaskering in SQL Database wordt de blootstelling van gevoelige gegevens beperkt door deze gegevens te maskeren voor niet-gemachtigde gebruikers. Met dynamische gegevensmaskering worden potentieel gevoelige gegevens in Azure SQL Database automatisch ontdekt. Er worden bruikbare aanbevelingen gedaan om deze velden te maskeren op een manier die de toepassingslaag minimaal beïnvloedt. Dit werkt als volgt: de gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden, worden bedekt, terwijl de gegevens in de database niet worden gewijzigd. Zie [Aan de slag met dynamische gegevensmaskering in SQL Database](sql-database-dynamic-data-masking-get-started.md) voor meer informatie over het beperken van de blootstelling van gevoelige gegevens.

## <a name="proactive-monitoring"></a>Proactieve controle
Uw gegevens worden beveiligd met SQL Database met behulp van mogelijkheden voor controle en detectie van dreigingen. 

### <a name="auditing"></a>Controleren
SQL Database Auditing houdt databaseactiviteiten bij en helpt u bij de naleving van regelgeving door databasegebeurtenissen vast te leggen in een auditlogboek in uw Azure Storage-account. Door te controleren krijgt u informatie over actieve databaseactiviteiten. Het helpt u ook bij het analyseren en onderzoeken van activiteiten uit het verleden. Op deze manier kunt u mogelijke dreigingen, vermoedelijk misbruik of schendingen van de beveiliging identificeren. Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) voor aanvullende informatie.  

### <a name="auditing--threat-detection"></a>Controle en detectie van bedreigingen 
SQL Database Auditing houdt databaseactiviteiten bij en helpt u bij de naleving van regelgeving door databasegebeurtenissen vast te leggen in een auditlogboek in uw Azure Storage-account. Door te controleren krijgt u informatie over actieve databaseactiviteiten. Het helpt u ook bij het analyseren en onderzoeken van activiteiten uit het verleden. Op deze manier kunt u mogelijke dreigingen, vermoedelijk misbruik of schendingen van de beveiliging identificeren. Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) voor aanvullende informatie.  
 
Controle wordt aangevuld door detectie van dreigingen, door middel van een extra beveiligingslaag die is ingebouwd in de Azure SQL Database-service. Dit is een doorlopend proces waarbij afwijkende databaseactiviteiten worden gedetecteerd, geprofileerd en geanalyseerd. U wordt gewaarschuwd bij verdachte activiteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang. U kunt reageren op waarschuwingen door de opgegeven informatieve en bruikbare instructies te volgen. Zie [Aan de slag met SQL Database Threat Detection](sql-database-threat-detection-get-started.md) voor meer informatie.  
 
### <a name="data-masking"></a>Gegevensmaskering 
Met dynamische gegevensmaskering in SQL Database wordt de blootstelling van gevoelige gegevens beperkt door deze gegevens te maskeren voor niet-gemachtigde gebruikers. Met dynamische gegevensmaskering worden potentieel gevoelige gegevens in Azure SQL Database automatisch ontdekt. Er worden bruikbare aanbevelingen gedaan om deze velden te maskeren op een manier die de toepassingslaag minimaal beïnvloedt. Dit werkt als volgt: de gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden, worden bedekt, terwijl de gegevens in de database niet worden gewijzigd. Zie [Aan de slag met SQL Database dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) voor meer informatie.
 
## <a name="compliance"></a>Naleving
Naast de bovenstaande functies en functionaliteit waarmee uw toepassing kan voldoen aan verschillende nalevingsvereisten met betrekking tot beveiliging, neemt Azure SQL Database ook deel aan reguliere controles en is het gecertificeerd voor een aantal nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://azure.microsoft.com/support/trust-center/services/) vindt, voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [Data protection and security](sql-database-protect-data.md) (Gegevensbescherming en beveiliging) voor een discussie over het gebruik van de functies voor gegevensbeveiliging in SQL Database.
- Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.
- Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) en [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Aan de slag met SQL Database Threat Detection) voor een discussie over proactieve controle.



<!--HONumber=Jan17_HO2-->


