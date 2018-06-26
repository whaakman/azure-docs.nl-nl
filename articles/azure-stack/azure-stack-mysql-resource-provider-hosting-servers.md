---
title: MySQL die als host fungeert voor Servers op Azure Stack | Microsoft Docs
description: MySQL-exemplaren om in te richten via de Bronprovider van MySQL-Adapter toevoegen
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 5522eb1b8b0398aeb6f1b0dd8578b906880b4e89
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938395"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Hosting-servers toevoegen voor de MySQL-resourceprovider

U kunt een MySQL-exemplaar op een virtuele machine (VM) worden gehost in [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure-Stack-omgeving, zolang de MySQL-resourceprovider verbinding kan maken met het exemplaar.

## <a name="connect-to-a-mysql-hosting-server"></a>Verbinding maken met een MySQL-hostserver

Zorg ervoor dat u hebt de referenties voor een account met beheerdersbevoegdheden system. Als u wilt een hosting-server toevoegt, de volgende stappen uit:

1. Aanmelden bij de Azure-Stack operator-portal als de beheerder van een service.
2. Selecteer **meer services**.
3. Selecteer **SERVERVIRTUALISATIE** > **MySQL-Servers die als host fungeert** > **+ toevoegen**. Hiermee opent u de **toevoegen van een Server die als host fungeert van MySQL** dialoogvenster wordt weergegeven in de volgende schermopname.

   ![Een hosting-server configureren](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Geef de details van de verbinding van uw MySQL-Server-exemplaar.

   * Voor **MySQL Hosting servernaam**, geef de volledig gekwalificeerde domeinnaam (FQDN) of een geldig IPv4-adres. Gebruik geen korte naam van de VM.
   * Een standaardexemplaar MySQL niet is opgegeven, dus u moet opgeven de **grootte van Hosting Server GB**. Voer een grootte die dicht bij de capaciteit van de database-server.
   * Laat de standaardinstelling voor **abonnement**.
   * Voor **resourcegroep**, maak een nieuwe of een bestaande groep.

   > [!NOTE]
   > Als de MySQL-exemplaar kan worden geopend door de tenant en de Azure Resource Manager-beheerder, kunt u het plaatsen onder het beheer van de resourceprovider. Maar het exemplaar MySQL **moet** worden toegewezen aan de resourceprovider.

5. Selecteer **SKU's** openen de **SKU maken** dialoogvenster.

   ![Maken van een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   De SKU **naam** moet overeenkomen met de eigenschappen van de SKU zodat gebruikers hun databases op de juiste SKU implementeren kunnen.

   >[!IMPORTANT]
   >Speciale tekens, inclusief spaties en perioden, worden niet ondersteund **naam** of **laag** wanneer u een SKU maakt voor de MySQL-resourceprovider.

6. Selecteer **OK** voor het maken van de SKU.
7. Onder **toevoegen van een Server die als host fungeert van MySQL**, selecteer **maken**.

Als u servers toevoegt, kunt u ze aan een nieuwe of bestaande SKU om het te onderscheiden van serviceaanbiedingen toewijzen. U kunt bijvoorbeeld een MySQL enterprise-exemplaar dat verbeterde database en automatische back-ups biedt hebben. U kunt deze server met hoge prestaties voor verschillende afdelingen reserveren in uw organisatie.

## <a name="increase-backend-database-capacity"></a>Vergroot de capaciteit van de back-end-database

Door het implementeren van meer MySQL-servers in de Stack van Azure-portal kunt u de capaciteit voor back-end van de database verhogen. Deze servers toevoegen aan een nieuwe of bestaande SKU. Als u een server toevoegen aan een bestaande SKU ervoor zorgen dat de server-kenmerken hetzelfde als de andere servers in de SKU zijn.

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL-database-servers beschikbaar maken voor uw gebruikers

Plannen en aanbiedingen MySQL-database-servers om beschikbaar te maken om gebruikers te maken. De service Microsoft.MySqlAdapter toevoegen aan het plan, en de standaard quotum toevoegen, of een nieuwe Quota maken.

![Plannen en aanbiedingen voor databases maken](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="next-steps"></a>Volgende stappen

[Een MySQL-database maken](azure-stack-mysql-resource-provider-databases.md)