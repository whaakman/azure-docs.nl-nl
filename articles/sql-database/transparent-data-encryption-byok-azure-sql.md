---
title: TDE - Bring Your Own Key (BYOK) - Azure SQL Database | Microsoft Docs
description: Bring Your Own Key (BYOK) ondersteuning voor transparante gegevensversleuteling (TDE) met Azure Key Vault voor SQL-Database en Data Warehouse. TDE met BYOK-overzicht, voordelen, hoe het werkt, overwegingen en aanbevelingen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 474e8d708a335b27899e818dcdba1fb469ad94a6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469233"
---
# <a name="azure-sql-transparent-data-encryption-bring-your-own-key-support"></a>Azure SQL Transparent Data Encryption: Bring Your Own Key-ondersteuning

Bring Your Own Key (BYOK) ondersteuning voor [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) kunt u voor het versleutelen van de Database Gegevensversleutelingsleutel (DEK) met een asymmetrische sleutel met de naam TDE-beveiliging.  De TDE-beveiliging worden opgeslagen onder het besturingselement in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), van het Azure-cloud-gebaseerde externe sleutelbeheersysteem. Azure Key Vault is de eerste service voor sleutelbeheer, waarbij TDE ondersteuning voor BYOK is geïntegreerd. De DEK TDE, die is opgeslagen op de opstartpagina van een database wordt versleuteld en ontsleuteld door de TDE-beveiliging. De TDE-beveiliging worden opgeslagen in Azure Key Vault en nooit verlaat de key vault. Als de server toegang tot de key vault is ingetrokken, kunnen een database kan niet worden ontsleuteld en lezen in het geheugen. Voor Azure SQL Database, de TDE-beveiliging is ingesteld op het niveau van de SQL-Database-server en wordt overgenomen door alle databases die zijn gekoppeld aan die server. Voor Azure SQL Managed Instance, de TDE-beveiliging is ingesteld op het instantieniveau en deze is overgenomen door alle *versleutelde* databases op dat exemplaar. De term *server* verwijst zowel naar de server en het exemplaar in dit document, tenzij anders vermeld.

Met BYOK-ondersteuning, gebruikers essentiële beheertaken, met inbegrip van sleutelrotaties kunnen beheren, key vault-machtigingen, verwijderen van sleutels en Schakel controle/rapportage over alle TDE beveiligingstoepassingen met behulp van Azure Key Vault-functionaliteit. Key Vault biedt centraal beheer, maakt gebruik van nauw bewaakte hardware security modules (HSM's), en kunt scheiding van functies tussen het beheer van sleutels en gegevens om te voldoen aan regelgeving.  

TDE met BYOK biedt de volgende voordelen:

- Transparantie en nauwkeurig beheer mogelijk verhoogd met de mogelijkheid om zelf de TDE-beveiliging beheren
- Centraal beheer van TDE beveiligingstoepassingen (samen met andere sleutels en geheimen die worden gebruikt in andere Azure-services) door deze als host fungeert in Key Vault
- Scheiding van verantwoordelijkheden van de management sleutel en gegevens binnen de organisatie, ter ondersteuning van scheiding van functies
- Meer vertrouwen van uw eigen clients, omdat de Key Vault zo ontworpen is dat Microsoft niet kan zien of versleutelingssleutels extraheren.
- Ondersteuning voor sleutelroulatie

> [!IMPORTANT]
> Degenen die met behulp van de service beheerde TDE die u wilt beginnen met Key Vault, blijft de TDE ingeschakeld tijdens het proces van schakelen naar een TDE-beveiliging in Key Vault. Er is geen downtime of opnieuw te versleutelen van bestanden van de database. Overschakelen van een service beheerde sleutel naar een Key Vault-sleutel vereist opnieuw te versleutelen van de databaseversleutelingssleutel (DEK), die een snelle en online-bewerking is alleen.

## <a name="how-does-tde-with-byok-support-work"></a>Hoe ondersteunt TDE met BYOK werk

![Verificatie van de Server naar de Key Vault](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

Wanneer TDE is eerst geconfigureerd voor gebruik van een TDE-beveiliging uit Key Vault, verzendt de server de DEK van elke database TDE is ingeschakeld voor Key Vault voor de aanvraag voor een wrap. Key Vault geeft als resultaat van de versleutelingssleutel versleutelde database die is opgeslagen in de database.  

> [!IMPORTANT]
> Het is belangrijk te weten dat **wanneer een TDE-beveiliging zijn opgeslagen in Azure Key Vault, de Azure Key Vault nooit verlaat**. Kan de server alleen sleutelbewerking aanvragen verzenden naar de TDE-beveiliging-sleutelmateriaal in Key Vault en **nooit toegang heeft tot of de TDE-beveiliging in de cache opslaat**. De Key Vault-beheerder heeft het recht om in te trekken van Key Vault-machtigingen van de server op elk gewenst moment, in welk geval alle verbindingen met de server worden afgekapt.

## <a name="guidelines-for-configuring-tde-with-byok"></a>Richtlijnen voor het configureren van TDE met BYOK

### <a name="general-guidelines"></a>Algemene richtlijnen

- Zorg ervoor dat Azure Key Vault en Azure SQL Database/MI gebruikt gaan worden in dezelfde tenant.  Cross-tenant key vault en server interacties **worden niet ondersteund**.
- Bepaal welke abonnementen zijn die moet worden gebruikt voor de vereiste resources: het verplaatsen van dat de server voor abonnementen later een nieuwe installatie van TDE met BYOKs vereist. Meer informatie over [verplaatsen van resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- Bij het configureren van TDE met BYOK, is het belangrijk om te overwegen de belasting die door operations herhaalde verpakken/uitpakken van de key vault geplaatst. Bijvoorbeeld, omdat alle databases die zijn gekoppeld aan een SQL Database-server de dezelfde TDE-beveiliging, wordt een failover van die server geactiveerd als er veel belangrijke bewerkingen op de kluis omdat er databases op de server zijn. Op basis van onze ervaring en gedocumenteerd [key vault-Servicelimieten](https://docs.microsoft.com/azure/key-vault/key-vault-service-limits), het is raadzaam het koppelen van maximaal 500 Standard / algemeen gebruik of 200 Premium / bedrijfskritieke databases met een Azure Key Vault in één abonnement om ervoor te zorgen consistent hoge beschikbaarheid bij toegang tot de TDE-beveiliging in de kluis.
- Aanbevolen: Bewaar een kopie van de TDE-beveiliging on-premises.  Hiervoor hebt u een HSM-apparaat om te maken van lokaal een TDE-beveiliging en een systeem sleutelbewaring door derden voor het opslaan van een lokale kopie van de TDE-beveiliging.  Informatie over [over te dragen van een sleutel uit een lokale HSM naar Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys).

### <a name="guidelines-for-configuring-azure-key-vault"></a>Richtlijnen voor het configureren van Azure Key Vault

- Maak een key vault met [voorlopig verwijderen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) ingeschakeld om te beschermen tegen gegevensverlies in geval van per ongeluk sleutel – of sleutelkluis – verwijderen van gegevens.  U moet gebruiken [PowerShell om in te schakelen van de eigenschap 'voorlopig verwijderen'](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) voor de sleutelkluis (deze optie is niet beschikbaar is via de Portal voor Azure Sleutelkluis nog –, maar vereist voor Azure SQL):  
  - Voorlopig verwijderde bronnen worden gedurende een ingestelde periode van 90 dagen tijd bewaard, tenzij ze zijn hersteld of verwijderd.
  - De **herstellen** en **opschonen** acties hebben hun eigen machtigingen die zijn gekoppeld in een toegangsbeleid voor key vault.
- Stel een resourcevergrendeling voor de sleutelkluis om te bepalen wie kan deze kritieke resource verwijdert en helpen om te voorkomen dat per ongeluk of niet-geautoriseerde wordt verwijderd.  [Meer informatie over de resource wordt vergrendeld](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)

- De SQL-Database-servertoegang verlenen tot de key vault met behulp van de identiteit van de Azure Active Directory (Azure AD).  Wanneer u de Portal-gebruikersinterface, de Azure AD-identiteit automatisch wordt gemaakt en de key vault-machtigingen zijn verleend aan de server.  Met behulp van PowerShell TDE met BYOK te configureren, de Azure AD-identiteit moet worden gemaakt en voltooiing moet worden gecontroleerd. Zie [TDE configureren met BYOK](transparent-data-encryption-byok-azure-sql-configure.md) en [TDE configureren met BYOK voor beheerd exemplaar](http://aka.ms/sqlmibyoktdepowershell) voor gedetailleerde stapsgewijze instructies bij het gebruik van PowerShell.

  > [!NOTE]
  > Als de Azure AD Identity **per ongeluk is verwijderd of de machtigingen van de server worden ingetrokken** met behulp van de key vault-toegangsbeleid, de server verliest de toegang tot de key vault en TDE versleuteld databases binnen 24 uur zijn verwijderd.

- Als u firewalls en virtuele netwerken voor Azure Key Vault, moet u het volgende configureren: - de vertrouwde Microsoft-services om over te slaan van de firewall toestaan: Ja gekozen

 > [!NOTE]
 > Als TDE versleuteld SQL-databases geen toegang meer tot de key vault omdat ze de firewall niet overslaan, de databases binnen 24 uur zijn verwijderd.

- Controle en rapportage van alle versleutelingssleutels inschakelen: Key Vault biedt de logboeken die gemakkelijk zijn te injecteren in andere informatie over beveiliging en event management (SIEM)-hulpprogramma's. Operations Management Suite (OMS) [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) is een voorbeeld van een service die al is geïntegreerd.
- Om ervoor te zorgen hoge beschikbaarheid van de versleutelde databases, configureert u elke SQL-Database-server met twee Sleutelkluizen van Azure die zich in verschillende regio's bevinden.

### <a name="guidelines-for-configuring-the-tde-protector-asymmetric-key"></a>Richtlijnen voor het configureren van de TDE-beveiliging (asymmetrische sleutel)

- De versleutelingssleutel lokaal op een lokale HSM-apparaat maken. Zorg ervoor dat dit een asymmetrische sleutel met RSA 2048 is daarom is het indeling die kan worden in Azure Key Vault.
- Zodat de sleutel wordt bewaard in een systeem sleutelbewaring door derden.  
- Bestand met de versleutelingssleutel (.pfx, .byok of .backup) importeren in Azure Key Vault.

   > [!NOTE]
   > Voor testdoeleinden, is het mogelijk om te maken van een sleutel met Azure Key Vault, maar deze sleutel kan niet worden verwekt, omdat de persoonlijke sleutel nooit de key vault verlaten.  Altijd een back-up en zodat sleutels die worden gebruikt voor het versleutelen van productiegegevens, als het verlies van gegevens van de sleutel wordt bewaard (per ongeluk verwijderen in de sleutel kluis, verloop enz.) resulteert in permanente gegevens verloren gaan.

- Gebruik van een sleutel zonder een vervaldatum- en nooit een vervaldatum instellen voor een sleutel al in gebruik: **zodra de sleutel is verlopen, de versleutelde databases op hun TDE-beveiliging voor de toegang kwijtraakt en binnen 24 uur zijn verwijderd**.
- Zorg ervoor dat de sleutel is ingeschakeld en is gemachtigd om uit te voeren *ophalen*, *sleutel inpakken*, en *sleutel uitpakken* bewerkingen.
- Een sleutel back-up voor Azure Key Vault maken voordat u met behulp van de sleutel voor de eerste keer in Azure Key Vault. Meer informatie over de [back-up-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1) opdracht.
- Een nieuwe back-up maken wanneer er wijzigingen worden aangebracht aan de sleutel (bijvoorbeeld: ACL's, toevoegen tags toevoegen, belangrijke kenmerken toevoegen).
- **Vorige versies behouden** van de sleutel in de key vault wanneer u sleutels, zodat oudere back-ups kunnen worden hersteld. Wanneer de TDE-beveiliging voor een database, de oude back-ups van de database wordt gewijzigd **worden niet bijgewerkt** gebruik van de meest recente TDE-beveiliging.  Elke back-up moet de TDE-beveiliging is gemaakt met tijdens het terugzetten. Sleutelrotaties kunnen worden uitgevoerd op de instructies op [draaien de Transparent Data Encryption Protector met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Houd alle eerder gebruikte sleutels in Azure Key Vault na het wijzigen van terug naar de service beheerde sleutels.  Dit zorgt ervoor databaseback-ups kunnen worden hersteld met de TDE-beveiligingstoepassingen die zijn opgeslagen in Azure Key Vault.  TDE beveiligingstoepassingen gemaakt met Azure Key Vault moeten worden onderhouden totdat alle opgeslagen back-ups zijn gemaakt met de service beheerde sleutels.  
- Herstelbare back-ups van deze sleutels met behulp van [back-up-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1).
- Als u wilt verwijderen een potentieel aangetast sleutel tijdens een beveiligingsincident zonder het risico van gegevensverlies, volg de stappen in [een potentieel aangetast sleutel verwijderen](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md).

## <a name="high-availability-geo-replication-and-backup--restore"></a>Hoge beschikbaarheid, Geo-replicatie en back-up / herstellen

### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen

Hoge beschikbaarheid configureren met Azure Key Vault, is afhankelijk van de configuratie van uw database en SQL Database-server en hier vindt u de aanbevolen configuraties voor twee afzonderlijke gevallen.  Het eerste geval is een zelfstandige database of SQL Database-server met geen geconfigureerde geo-redundantie.  Het tweede geval is een database of SQL Database-server is geconfigureerd met failover-groepen of geografische redundantie op, waarbij moet worden gezorgd dat elke geografisch redundante kopie heeft een lokale Azure Key Vault binnen de failovergroep om te zorgen dat werk geo-failover.

In het eerste geval, als u nodig hebt met hoge beschikbaarheid van een database en SQL Database-server met geen geconfigureerde geo-redundantie, is het raadzaam om de server voor het gebruik van twee verschillende sleutelkluizen in twee verschillende regio's met de dezelfde sleutelmateriaal te configureren. Dit kan worden bewerkstelligd door het maken van een TDE-beveiliging met behulp van de primaire Key Vault in dezelfde regio als de SQL Database-server geplaatst en de primaire klonen van de sleutel in een key vault in een andere Azure-regio, zodat de server toegang tot een tweede key vault heeft moet sleutelkluis zich een storing, terwijl de database is actief en werkend. Gebruik de cmdlet Backup-AzureKeyVaultKey ophalen van de sleutel in een versleutelde indeling van de primaire key vault en gebruik de cmdlet Restore-AzureKeyVaultKey en een key vault in de tweede regio opgeven.

![Single-Server HA en geen geo-dr](./media/transparent-data-encryption-byok-azure-sql/SingleServer_HA_Config.PNG)

## <a name="how-to-configure-geo-dr-with-azure-key-vault"></a>Geo-DR configureren met Azure Key Vault

Als u wilt behouden hoge beschikbaarheid van TDE beveiligingstoepassingen voor versleutelde databases, is het vereist voor redundante Sleutelkluizen van Azure op basis van de bestaande of de gewenste SQL-Database failover-groepen of exemplaren van actieve geo-replicatie configureren.  Elke server geo-replicatie vereist een afzonderlijke key vault, moet dezelfde locatie bevindt als de server in dezelfde Azure-regio. Moet een primaire database ontoegankelijk worden om een storing in één regio en een failover wordt geactiveerd, kan de secundaire database om over te nemen met behulp van de secundaire key vault.

De volgende Azure Key Vault-configuratie is vereist voor Geo-Replicated Azure SQL-databases:

- Een primaire database in een key vault in de regio en een secundaire database met een key vault in de regio.
- Ten minste één secundaire is vereist, er worden maximaal vier secundaire databases ondersteund.
- Secundaire replica's van de secundaire replica's (koppeling) worden niet ondersteund.

De volgende sectie gaat over de installatie- en configuratiestappen in meer detail.

### <a name="azure-key-vault-configuration-steps"></a>Stappen voor het configureren van Azure Key Vault

- Installeer [PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-5.6.0)
- Maak twee Azure-Sleutelkluizen in twee verschillende regio's met behulp van [PowerShell om in te schakelen van de eigenschap 'voorlopig verwijderen'](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) in de sleutelkluizen (deze optie is niet beschikbaar is via de Portal voor Azure Sleutelkluis nog –, maar vereist voor SQL).
- Beide Sleutelkluizen van Azure moet zich bevinden in de twee regio's beschikbaar zijn in het hetzelfde geografische gebied voor Azure in volgorde voor back-up en herstel van sleutels om te werken.  Als u de twee sleutelkluizen zich bevinden in verschillende geografische gebieden om te voldoen aan de SQL-Geo-DR-behoeften, volgt u de [BYOK proces](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) die ondersteuning biedt voor sleutels moeten worden geïmporteerd uit een on-premises HSM.
- Maak een nieuwe sleutel in de eerste key vault:  
  - RSA/RSA-HSA 2048 key
  - Er is geen vervaldatum
  - Sleutel is ingeschakeld en machtigingen heeft voor het uitvoeren van get, sleutel Inpakken en uitpakken sleutelbewerkingen
- Back-up van de primaire sleutel en de sleutel herstellen naar de tweede key vault.  Zie [BackupAzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-5.1.1) en [Restore-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-5.5.0).

### <a name="azure-sql-database-configuration-steps"></a>Configuratiestappen voor Azure SQL Database

De volgende configuratiestappen uit verschillen of beginnen met een nieuwe SQL-implementatie of als u werkt met een bestaande SQL-Geo-DR-implementatie.  We beschrijven de configuratiestappen voor een nieuwe implementatie eerst en vervolgens wordt uitgelegd hoe u om toe te wijzen TDE beveiligingstoepassingen die zijn opgeslagen in Azure Key Vault aan een bestaande implementatie waarvoor al een Geo-DR-koppeling tot stand gebracht.

**Stappen voor een nieuwe implementatie**:

- De twee SQL-Database-servers in dezelfde twee regio's als de eerder gemaakte sleutelkluizen maken.
- Selecteer het deelvenster TDE met SQL Database-server en voor elke SQL-Database-server:  
  - Selecteer de AKV in dezelfde regio
  - Selecteer de sleutel te gebruiken als TDE-beveiliging: de lokale kopie van de TDE-beveiliging wordt gebruikt door elke server.
  - Dit doen in de Portal maakt een [AppID](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) voor de SQL-Database-server, die wordt gebruikt voor het toewijzen van de SQL-Database-servermachtigingen voor toegang tot de key vault – deze identiteit niet verwijderen. Toegang kan worden ingetrokken door het verwijderen van de machtigingen in Azure Key Vault in plaats daarvan voor de SQL-Database-server, die wordt gebruikt voor het toewijzen van de SQL-Database-servermachtigingen voor toegang tot de sleutelkluis.
- Maken van de primaire database.
- Ga als volgt de [actieve geo-replicatie richtlijnen](sql-database-geo-replication-overview.md) voor het voltooien van het scenario, deze stap maakt u de secundaire database.

![Failover-groepen en geo-dr](./media/transparent-data-encryption-byok-azure-sql/Geo_DR_Config.PNG)

> [!NOTE]
> Het is belangrijk om ervoor te zorgen dat de dezelfde TDE-beveiligingstoepassingen aanwezig in beide sleutelkluizen, voordat u doorgaat zijn met de geo-koppeling tussen de databases maken.

**Stappen voor een bestaande SQL-database met Geo-DR-implementatie**:

Omdat de SQL Database-servers bestaan al en primaire en secundaire databases al zijn toegewezen, moeten de stappen voor het configureren van Azure Key Vault worden uitgevoerd in de volgende volgorde:

- Beginnen met de SQL-Database-server die als host fungeert voor de secundaire database:
  - Toewijzen van de key vault zich in dezelfde regio
  - Toewijzen van de TDE-beveiliging
- Nu gaat u naar de SQL Database-server die als host fungeert voor de primaire database:
  - Selecteer de dezelfde TDE-beveiliging als gebruikt voor de secundaire database

![Failover-groepen en geo-dr](./media/transparent-data-encryption-byok-azure-sql/geo_DR_ex_config.PNG)

>[!NOTE]
>Bij het toewijzen van de key vault met de server, is het belangrijk om te beginnen met de secundaire server.  In het toewijzen van de tweede stap de sleutel kluis naar de primaire server en bijwerken van de TDE-beveiliging, blijven de Geo-DR-koppeling werken omdat de TDE-beveiliging die wordt gebruikt door de gerepliceerde database nu beschikbaar voor beide servers is.

Voordat u TDE inschakelen met de klant beheerde sleutels in Azure Key Vault voor SQL Database-Geo-herstel na noodgevallen, is het belangrijk om te maken en beheren van twee Sleutelkluizen van Azure met identieke inhoud in dezelfde regio's die worden gebruikt voor SQL-Database geo-replicatie.  "Identieke inhoud" betekent specifiek dat beide sleutelkluizen kopieën van de dezelfde TDE Protector(s) bevatten moeten, zodat beide servers toegang tot de TDE-beveiligingstoepassingen-gebruik door alle databases hebben.  Voortaan kunt is vereist dat beide sleutelkluizen synchroon, dat betekent dat ze moeten de dezelfde kopieën van TDE beveiligingstoepassingen bevatten na het rouleren van de sleutel, oude versies van sleutels die worden gebruikt voor de logboekbestanden onderhouden of back-ups, TDE beveiligingstoepassingen dezelfde sleuteleigenschappen en de sleutel moeten onderhouden kluizen moeten dezelfde toegangsmachtigingen voor SQL behouden.  

Volg de stappen in [overzicht Actieve geo-replicatie](sql-database-geo-replication-overview.md) om te testen en een failover wordt geactiveerd, die moet worden uitgevoerd op een geregelde om te bevestigen van de toegangsmachtigingen voor SQL tot beide key vaults is behouden.

### <a name="backup-and-restore"></a>Back-up en herstel

Zodra een database is versleuteld met TDE met behulp van een sleutel uit Key Vault, worden eventuele gegenereerde back-ups met de dezelfde TDE-beveiliging eveneens versleuteld.

Zorg ervoor dat het sleutelmateriaal nog steeds in de oorspronkelijke kluis onder de naam van de oorspronkelijke sleutel is voor het herstellen van een back-up die is versleuteld met een TDE-beveiliging uit Key Vault. Wanneer de TDE-beveiliging voor een database, de oude back-ups van de database wordt gewijzigd **niet** bijgewerkt voor het gebruik van de meest recente TDE-beveiliging. Daarom raden wij aan alle oude versies van de TDE-beveiliging te houden in Key Vault, zodat de databaseback-ups kunnen worden hersteld.

Als een sleutel die nodig zijn voor het herstellen van een back-up niet langer in de oorspronkelijke sleutelkluis is, wordt het volgende foutbericht geretourneerd: "Doelserver `<Servername>` heeft geen toegang tot alle AKV URI's die worden gemaakt tussen < tijdstempel #1 > en < tijdstempel #2 >. Probeer opnieuw na het herstellen van alle AKV-URI's."

Als oplossing hiervoor kunt uitvoeren de [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) cmdlet om te retourneren van de lijst met sleutels uit Key Vault die zijn toegevoegd aan de server (tenzij ze zijn verwijderd door een gebruiker). Zorg ervoor dat de doelserver voor de back-up heeft toegang tot alle van deze sleutels, zodat de dat alle back-ups kunnen worden hersteld.

```powershell
Get-AzureRmSqlServerKeyVaultKey `
  -ServerName <LogicalServerName> `
  -ResourceGroup <SQLDatabaseResourceGroupName>
```

Zie voor meer informatie over het herstellen van back-up voor SQL-Database, [herstellen van een Azure SQL database](sql-database-recovery-using-backups.md). Zie voor meer informatie over het herstellen van back-up voor SQL Data Warehouse, [herstellen van een Azure SQL Data Warehouse](../sql-data-warehouse/backup-and-restore.md).

Extra aandacht voor back-ups van logboekbestanden: Back-ups van logboekbestanden bestanden versleuteld met de oorspronkelijke TDE Encryptor, blijven zelfs als de TDE-beveiliging is gedraaid en de database is nu met behulp van een nieuwe TDE-beveiliging.  Tijdens het terugzetten beide sleutels nodig om de database te herstellen.  Als het logboekbestand opgeslagen in Azure Key Vault TDE-beveiliging gebruikt is, wordt deze sleutel tijdens het terugzetten nodig, zelfs als de database is gewijzigd voor het gebruik van service beheerde TDE in de tussentijd.
