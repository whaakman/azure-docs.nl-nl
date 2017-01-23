---
title: Overzicht van Azure SQL Database Protection | Microsoft Docs
description: Meer informatie over het beveiligen van gegevens in een Azure SQL-database.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 12/21/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 67b32b224d86e8d0ed06d8248c3a2c5f5569c5cb


---
# <a name="protecting-data-within-your-sql-database"></a>Beveiligen van gegevens in SQL Database

Met SQL Database kunt u uw gegevens beveiligen met behulp van versleuteling.  

## <a name="overview"></a>Overzicht

Uw gegevens worden beveiligd met SQL Database via gegevensversleuteling. Voor gegevens die in beweging zijn, wordt [Transport Layer Security](https://support.microsoft.com/en-us/kb/3135244) gebruikt, voor data-at-rest [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242) en voor gebruikte gegevens [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx). Zie [Data protection and security](sql-database-protect-data.md) (Gegevensbescherming en beveiliging) voor een discussie over het gebruik van deze functies voor gegevensbeveiliging in SQL Database.

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

* [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
* Als u een Hardware Security Module of centraal beheer van uw versleutelingssleutelhiërarchie nodig heeft, kunt u overwegen om [Azure Key Vault met SQL Server in een virtuele Azure-machine](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx) te gebruiken.


Uw gegevens worden beveiligd met SQL Database met behulp van mogelijkheden voor controle en detectie van dreigingen. 

### <a name="auditing"></a>Controleren
SQL Database Auditing houdt databaseactiviteiten bij en helpt u bij de naleving van regelgeving door databasegebeurtenissen vast te leggen in een auditlogboek in uw Azure Storage-account. Door middel van controle krijgt u informatie over actieve databaseactiviteiten. Het helpt u ook bij het analyseren en onderzoeken van activiteiten uit het verleden. Op deze manier kunt u mogelijke dreigingen, vermoedelijk misbruik of schendingen van de beveiliging identificeren. Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) voor aanvullende informatie.  

### <a name="threat-detection"></a>Detectie van bedreigingen
Controle wordt aangevuld door detectie van dreigingen, door middel van een extra beveiligingslaag die is ingebouwd in de Azure SQL Database-service. Dit is een doorlopend proces waarbij afwijkende databaseactiviteiten worden gedetecteerd, geprofileerd en geanalyseerd. U wordt gewaarschuwd bij verdachte activiteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende patronen voor databasetoegang. U kunt reageren op waarschuwingen door de opgegeven informatieve en bruikbare instructies te volgen. Zie [Aan de slag met SQL Database Threat Detection](sql-database-threat-detection-get-started.md) voor meer informatie.  

## <a name="connection-security"></a>Verbindingsbeveiliging
Verbindingsbeveiliging verwijst naar de manier waarop u verbindingen met uw database beperkt en beveiligt met behulp van firewallregels en verbindingsversleuteling.

Firewallregels worden zowel door de server als de database gebruikt om verbindingspogingen van IP-adressen die niet expliciet zijn goedgekeurd te weigeren. Als u uw toepassing of het openbare IP-adres van de clientcomputer wilt toestaan om verbinding te maken met een nieuwe database, moet u eerst een firewallregel op serverniveau maken met behulp van de klassieke Azure Portal, REST-API of PowerShell. Als best practice moet u het IP-adresbereik dat is toegestaan door uw serverfirewall zoveel mogelijk beperken. Zie [Azure SQL Database-firewall](https://msdn.microsoft.com/library/ee621782) voor meer informatie.

Alle verbindingen met Azure SQL Database vereisen te allen tijde versleuteling (SSL/TLS) wanneer gegevens naar en vanuit de database worden verzonden. In de verbindingsreeks van de toepassing moet u de parameters voor het versleutelen van de verbinding opgeven en *niet* het servercertificaat vertrouwen (dit doet u als u de verbindingsreeks buiten de klassieke Azure-portal kopieert). Anders kan de verbinding de identiteit van de server niet controleren en is deze vatbaar voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET zijn deze verbindingsreeksparameters bijvoorbeeld **Encrypt=True** en **TrustServerCertificate=False**. Zie [Versleuteling en certificaatvalidatie van Azure SQL Database-verbindingen](https://msdn.microsoft.com/library/azure/ff394108#encryption) voor meer informatie.

## <a name="authentication"></a>Authentication
Verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:

* **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord worden gebruikt
* **Azure Active Directory-verificatie**, waarbij gebruik wordt gemaakt van identiteiten die worden beheerd door Azure Active Directory en die wordt ondersteund voor beheerde en geïntegreerd domeinen

Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

Als best practice moet uw toepassing een ander account gebruiken om te verifiëren - op deze manier kunt u de machtigingen die aan de toepassing worden verleend, beperken en het risico op schadelijke activiteiten in het geval waarin uw toepassingscode kwetsbaar is voor een SQL-injectieaanvallen verminderen. U wordt aanbevolen om een [ingesloten databasegebruiker](https://msdn.microsoft.com/library/ff929188) te maken, zodat uw app zich rechtstreeks bij de database kan verifiëren. U kunt een ingesloten databasegebruiker maken die SQL-verificatie gebruikt door de volgende T-SQL-opdracht uit te voeren terwijl u als serverbeheerder verbinding hebt met uw gebruikersdatabase:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Als u een Azure AD-beheerder hebt gemaakt, kunt u een ingesloten databasegebruiker maken die Azure Active Directory-verificatie gebruikt door de volgende T-SQL-opdracht uit te voeren terwijl u als Azure AD-beheerder verbinding hebt met uw gebruikersdatabase:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beide gevallen moeten deze aanmeldingsgegevens in de verbindingsreeks van uw toepassing zijn opgegeven, en niet de serverbeheerder, om verbinding met de database te maken.

Zie [Databases en aanmeldingen beheren in Azure SQL Database](sql-database-manage-logins.md) voor meer informatie over verificatie bij een SQL Database.

## <a name="authorization"></a>Autorisatie
Autorisatie verwijst naar wat u binnen een Azure SQL Database kunt doen, en dit wordt bepaald door de rollidmaatschappen en machtigingen van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Dit is in Azure SQL Database eenvoudig te beheren met rollen in T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing.

Er zijn manieren om verder te beperken wat een gebruiker met Azure SQL Database kan doen:

* [Databaserollen](https://msdn.microsoft.com/library/ms189121) anders dan db_datareader en db_datawriter kunnen worden gebruikt om krachtigere toepassingsgebruikersaccounts of minder krachtige beheeraccounts te maken.
* Met gedetailleerde [machtigingen](https://msdn.microsoft.com/library/ms191291) kunt u beheren welke bewerkingen u op afzonderlijke kolommen, tabellen, weergaven, procedures en andere objecten in de database kunt uitvoeren.
* [Imitatie](https://msdn.microsoft.com/library/vstudio/bb669087) en [module-ondertekening](https://msdn.microsoft.com/library/bb669102) kunnen worden gebruikt om machtigingen tijdelijk veilig te verhogen.
* [Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) kan worden gebruikt om de rijen waartoe een gebruiker toegang heeft te beperken.
* [Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om de weergave van gevoelige gegevens te beperken.
* [Opgeslagen procedures](https://msdn.microsoft.com/library/ms190782) kunnen worden gebruikt om de acties die op de database kunnen worden uitgevoerd te beperken.

Het beheren van databases en logische servers via de klassieke Azure Portal of met behulp van de Azure Resource Manager-API wordt bepaald door de roltoewijzingen van uw portalgebruikersaccount. Zie [Op rollen gebaseerd toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-configure.md) voor meer informatie over dit onderwerp.

## <a name="encryption"></a>Versleuteling
Azure SQL Database helpt uw gegevens te beschermen door uw gegevens te versleutelen wanneer deze 'in rust' of opgeslagen zijn in de databasebestanden en back-ups, met behulp van [Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242). Als u uw database wilt versleutelen, maakt u verbinding als een database-eigenaar en voert u de volgende opdracht uit:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Voor andere manieren om uw gegevens versleutelen, kunt u het volgende overwegen:

* [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
* Als u een Hardware Security Module of centraal beheer van uw versleutelingssleutelhiërarchie nodig heeft, kunt u overwegen om [Azure Key Vault met SQL Server in een virtuele Azure-machine](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx) te gebruiken.
* [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) (in preview) maakt versleuteling transparant voor toepassingen en maakt het clients mogelijk gevoelige gegevens binnen clienttoepassingen te versleutelen zonder de versleutelingssleutels met SQL Database te delen.

## <a name="auditing"></a>Controleren
Door databasegebeurtenissen te controleren en bij te houden, kunt u regelgeving naleven en verdachte activiteiten identificeren. Met SQL Database Auditing worden gebeurtenissen in uw database geschreven naar een controlelogboek in uw Azure Storage-account. SQL Database Auditing kan ook worden geïntegreerd met Microsoft Power BI om het inzoomen op rapporten en analyses mogelijk te maken. Zie [Aan de slag met SQL Database Auditing](sql-database-auditing-get-started.md) voor meer informatie.

SQL Database Threat Detection biedt een extra beveiligingslaag boven op Auditing. Hiermee kunt u op bedreigingen reageert zodra deze optreden doordat er beveiligingswaarschuwingen vanwege afwijkende activiteiten worden gegeven. Zie [Aan de slag met SQL Database Threat Detection](sql-database-threat-detection-get-started.md) voor meer informatie.  

## <a name="compliance"></a>Naleving
Naast de bovenstaande functies en functionaliteit waarmee uw toepassing kan voldoen aan verschillende nalevingsvereisten met betrekking tot beveiliging, neemt Azure SQL Database ook deel aan reguliere controles en is het gecertificeerd voor een aantal nalevingsstandaarden. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://azure.microsoft.com/support/trust-center/services/) vindt, voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de SQL Database-beveiligingsfuncties.
- Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.
- Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) en [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Aan de slag met SQL Database Threat Detection) voor een discussie over proactieve controle.




<!--HONumber=Dec16_HO4-->


