---
title: MySQL-Servers in Azure Stack Hosting | Microsoft Docs
description: MySQL-instanties om in te richten via de Resourceprovider van MySQL-Adapter toevoegen
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 8cffcc938a247a2b08ff53b128560e1ab5e1653a
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499774"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Hosting-servers voor de MySQL-resourceprovider toevoegen

U kunt een MySQL server-exemplaar op een virtuele machine (VM) die als host fungeert hosten [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure Stack-omgeving, zolang de MySQL-resourceprovider kunt verbinding maken met het exemplaar.

> [!NOTE]
> De MySQL-resourceprovider moet worden gemaakt in het abonnement van de provider standaard terwijl hosting MySQL-servers moeten worden gemaakt in factureerbare, gebruikersabonnementen. De server van de resource provider moet niet worden gebruikt om host gebruikersdatabases.

MySQL-versie 5.6, 5.7 en 8.0 kunnen worden gebruikt voor de hosting-servers. De RP MySQL biedt geen ondersteuning voor caching_sha2_password-authenticatie. die wordt toegevoegd in de volgende release. MySQL 8.0 servers moeten worden geconfigureerd voor het gebruik van mysql_native_password. MariaDB wordt ook ondersteund.

## <a name="connect-to-a-mysql-hosting-server"></a>Verbinding maken met een MySQL-server voor hosting

Zorg ervoor dat u hebt de referenties voor een account met systeembeheerdersbevoegdheden. Als u wilt toevoegen een hosting-server, de volgende stappen uit:

1. Aanmelden bij de portal voor Azure Stack-operator als de beheerder van een service.
2. Selecteer **Alle services**.
3. Onder de **BEHEERDERSRESOURCES** categorie selecteren **MySQL-Servers die als host fungeert** > **+ toevoegen**. Hiermee opent u de **toevoegen van een MySQL-Server die als host fungeert** dialoogvenster wordt weergegeven in de volgende schermopname.

   ![Een host-server configureren](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Geef de details van de verbinding van uw MySQL-Server-exemplaar.

   * Voor **naam van MySQL-Server die als host fungeert**, geef de volledig gekwalificeerde domeinnaam (FQDN) of een geldig IPv4-adres. Gebruik niet de korte naam van de virtuele machine.
   * De beheerder van de standaard **gebruikersnaam** voor u de Bitnami MySQL installatiekopieën beschikbaar in de Azure Stack marketplace is *hoofdmap*. 
   * Als u niet dat de hoofdmap weet **wachtwoord**, Zie de [Bitnami documentatie](https://docs.bitnami.com/azure/faq/#how-to-find-application-credentials) voor informatie over het downloaden. 
   * Een standaardexemplaar van de MySQL is niet opgegeven, dus u hoeft op te geven de **grootte van de host-Server in GB**. Voer een grootte die zich in de buurt van de capaciteit van de database-server.
   * Laat de standaardinstelling voor **abonnement**.
   * Voor **resourcegroep**, een nieuwe maken of een bestaande groep gebruiken.

   > [!NOTE]
   > Als de MySQL-exemplaar kan worden geopend door de tenant en de Azure Resource Manager-beheerder, kunt u deze plaatsen onder het beheer van de resourceprovider. Maar het MySQL-exemplaar **moet** worden toegewezen aan de resourceprovider.

5. Selecteer **SKU's** openen de **SKU maken** dialoogvenster.

   ![Maken van een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   De SKU **naam** te relateren aan de eigenschappen van de SKU, zodat gebruikers hun databases op de juiste-SKU implementeren kunnen.

6. Selecteer **OK** te maken van de SKU.
   > [!NOTE]
   > SKU's kunnen een uur duren om te worden weergegeven in de portal. U kunt een database kan niet maken, totdat de SKU geïmplementeerd en wordt uitgevoerd is.

7. Onder **toevoegen van een MySQL-Server die als host fungeert**, selecteer **maken**.

Als u servers toevoegt, deze toewijzen aan een nieuwe of bestaande SKU om het te onderscheiden van de service-aanbiedingen. U kunt bijvoorbeeld een MySQL-enterprise-instantie die voorziet in verbeterde database en automatische back-ups hebben. U kunt deze server met hoge prestaties voor verschillende afdelingen in uw organisatie reserveren.

## <a name="security-considerations-for-mysql"></a>Beveiligingsoverwegingen voor MySQL

De volgende informatie is van toepassing op de RP- en MySQL-servers die als host fungeert:

* Zorg ervoor dat alle hosting-servers zijn geconfigureerd voor communicatie met behulp van TLS 1.2. Zie [MySQL voor het gebruik van versleutelde verbindingen configureren](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html).
* Gebruikmaken van [Transparent Data Encryption](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html).
* De RP MySQL biedt geen ondersteuning voor caching_sha2_password verificatie.

## <a name="increase-backend-database-capacity"></a>Vergroot de capaciteit van de back-end-database

U kunt capaciteit voor back-end van de database vergroten door meer MySQL-servers in de portal voor Azure Stack implementeren. Deze servers toevoegen aan een nieuwe of bestaande SKU. Als u een server toevoegen aan een bestaande SKU, ervoor zorgen dat de kenmerken van de server hetzelfde als de andere servers in de SKU zijn.

## <a name="sku-notes"></a>Opmerkingen bij de SKU
Gebruik een SKU-naam die de mogelijkheden van de servers in de SKU, zoals de capaciteit en prestaties beschrijft. De naam fungeert als hulpmiddel om gebruikers hun databases implementeren op de juiste SKU te helpen. Bijvoorbeeld, kunt u de namen van de SKU serviceaanbiedingen onderscheiden door de volgende kenmerken:
  
* hoge capaciteit
* hoge prestaties
* hoge beschikbaarheid

Als een best practice, moeten de host-servers in een SKU dezelfde resource- en kenmerken hebben.

SKU's kunnen niet worden toegewezen aan specifieke gebruikers of groepen.

Als u wilt een SKU bewerken, gaat u naar **alle services** > **MySQL Adapter** > **SKU's**. Selecteer de SKU wijzigen, breng eventueel benodigde wijzigingen aan en klikt u op **opslaan** wijzigingen op te slaan. 

Als u wilt verwijderen een SKU die niet meer nodig is, gaat u naar **alle services** > **MySQL Adapter** > **SKU's**. Met de rechtermuisknop op de SKU-naam en selecteer **verwijderen** om deze te verwijderen.

> [!IMPORTANT]
> Het kan een uur duren voor de nieuwe SKU's worden weergegeven in de gebruikersportal.

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL-database-servers beschikbaar maken voor uw gebruikers

Maak plannen en aanbiedingen om MySQL-database-servers beschikbaar stellen aan gebruikers. De service Microsoft.MySqlAdapter toevoegen aan het plan en maak een nieuw quotum. MySQL is niet toegestaan voor het beperken van de grootte van databases.

> [!IMPORTANT]
> Duurt maximaal twee uur nieuwe quota zijn beschikbaar in de gebruikersportal of voordat een gewijzigde quotum wordt afgedwongen.

## <a name="next-steps"></a>Volgende stappen

[Een MySQL-database maken](azure-stack-mysql-resource-provider-databases.md)