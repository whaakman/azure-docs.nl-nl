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
ms.date: 07/02/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 60f1978b4173f169ad14deb67b075a61f9e7f149
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344135"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Hosting-servers voor de MySQL-resourceprovider toevoegen

U kunt een MySQL-exemplaar op een virtuele machine (VM) hosten [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure Stack-omgeving, zolang de MySQL-resourceprovider kunt verbinding maken met het exemplaar.

MySQL-versie 5.6, 5.7 en 8.0 kunnen worden gebruikt voor de hosting-servers. De RP MySQL biedt geen ondersteuning voor caching_sha2_password-authenticatie. die wordt toegevoegd in de volgende release. MySQL 8.0 servers moeten worden geconfigureerd voor het gebruik van mysql_native_password. MariaDB wordt ook ondersteund.

## <a name="connect-to-a-mysql-hosting-server"></a>Verbinding maken met een MySQL-server voor hosting

Zorg ervoor dat u hebt de referenties voor een account met systeembeheerdersbevoegdheden. Als u wilt toevoegen een hosting-server, de volgende stappen uit:

1. Aanmelden bij de portal voor Azure Stack-operator als de beheerder van een service.
2. Selecteer **meer services**.
3. Selecteer **BEHEERDERSRESOURCES** > **MySQL-Servers die als host fungeert** > **+ toevoegen**. Hiermee opent u de **toevoegen van een MySQL-Server die als host fungeert** dialoogvenster wordt weergegeven in de volgende schermopname.

   ![Een host-server configureren](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Geef de details van de verbinding van uw MySQL-Server-exemplaar.

   * Voor **naam van MySQL-Server die als host fungeert**, geef de volledig gekwalificeerde domeinnaam (FQDN) of een geldig IPv4-adres. Gebruik niet de korte naam van de virtuele machine.
   * Een standaardexemplaar van de MySQL is niet opgegeven, dus u hoeft op te geven de **grootte van de host-Server in GB**. Voer een grootte die zich in de buurt van de capaciteit van de database-server.
   * Laat de standaardinstelling voor **abonnement**.
   * Voor **resourcegroep**, een nieuwe maken of een bestaande groep gebruiken.

   > [!NOTE]
   > Als de MySQL-exemplaar kan worden geopend door de tenant en de Azure Resource Manager-beheerder, kunt u deze plaatsen onder het beheer van de resourceprovider. Maar het MySQL-exemplaar **moet** worden toegewezen aan de resourceprovider.

5. Selecteer **SKU's** openen de **SKU maken** dialoogvenster.

   ![Maken van een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   De SKU **naam** te relateren aan de eigenschappen van de SKU, zodat gebruikers hun databases op de juiste-SKU implementeren kunnen.

   >[!IMPORTANT]
   >Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in **naam** of **laag** wanneer maakt u een SKU voor de MySQL-resourceprovider.

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

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL-database-servers beschikbaar maken voor uw gebruikers

Maak plannen en aanbiedingen om MySQL-database-servers beschikbaar stellen aan gebruikers. De service Microsoft.MySqlAdapter toevoegen aan het plan en maak een nieuw quotum. MySQL is niet toegestaan voor het beperken van de grootte van databases.

## <a name="next-steps"></a>Volgende stappen

[Een MySQL-database maken](azure-stack-mysql-resource-provider-databases.md)