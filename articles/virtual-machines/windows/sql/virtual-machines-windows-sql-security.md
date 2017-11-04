---
title: Beveiligingsoverwegingen voor SQL Server in Azure | Microsoft Docs
description: Dit onderwerp bevat algemene richtlijnen voor het beveiligen van SQL Server wordt uitgevoerd in een virtuele Machine van Azure.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2017
ms.author: jroth
ms.openlocfilehash: 4ad9156e481eac0bae32bca35a2b126363e5d8b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Veiligheidsoverwegingen voor SQL Server in Azure Virtual Machines

Dit onderwerp bevat algemene richtlijnen voor beveiliging die helpt bij het vaststellen van beveiligde toegang tot SQL Server-exemplaren in Azure een virtuele machine (VM).

Azure voldoet aan de verschillende regelgeving vanuit de sector en normen waarmee u een compatibele oplossing bouwen met SQL Server in een virtuele machine wordt uitgevoerd. Zie voor meer informatie over de naleving van regelgeving met Azure [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>Toegang tot de SQL VM beheren

Wanneer u uw virtuele machine van SQL Server maakt, Overweeg zorgvuldig beheren wie heeft toegang tot de machine en SQL Server. In het algemeen moet u het volgende doen:

- Toegang tot SQL Server beperken tot alleen de toepassingen en clients die u nodig hebt.
- Volg de aanbevolen procedures voor het beheren van gebruikersaccounts en wachtwoorden.

De volgende secties geven suggesties over denken via deze punten.

## <a name="secure-connections"></a>Beveiligde verbindingen

Wanneer u een virtuele machine van SQL Server met een afbeelding maakt, de **SQL Server-verbindingen** optie biedt u de keuze van **lokale (binnen VM)**, **privé (binnen virtueel netwerk)**, of **openbaar (Internet)**.

![SQL Server-verbinding](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

Kies voor de beste beveiliging, de meest beperkende optie voor uw scenario. Bijvoorbeeld, als u een toepassing die toegang heeft tot SQL Server op dezelfde virtuele machine vervolgens **lokale** is de veiligste optie. Als u werkt met een Azure-toepassing waarvoor toegang tot de SQL Server, klikt u vervolgens **persoonlijke** beveiligt communicatie naar SQL Server binnen de opgegeven [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Als u nodig hebt **openbare** (internest) toegang tot de SQL Server VM, Controleer of andere aanbevelingen in dit onderwerp om te beperken van de aanval surface area volgen.

De geselecteerde opties in de portal gebruiken inkomende beveiligingsregels voor verbindingen op de virtuele machines [Netwerkbeveiligingsgroep](../../../virtual-network/virtual-networks-nsg.md) (NSG) wilt toestaan of weigeren van netwerkverkeer naar de virtuele machine. U kunt wijzigen of nieuwe regels voor binnenkomende NSG zodat verkeer naar de SQL Server-poort (standaard 1433) te maken. U kunt ook bepaalde IP-adressen die zijn toegestaan om te communiceren via deze poort opgeven.

![Netwerkbeveiligingsgroepen](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Naast het NSG-regels om te beperken van netwerkverkeer, kunt u ook de Windows Firewall op de virtuele machine gebruiken.

Als u van eindpunten met het klassieke implementatiemodel gebruikmaakt, verwijdert u alle eindpunten op de virtuele machine als u deze niet gebruikt. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL voor een eindpunt](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint). Dit is niet nodig is voor virtuele machines die gebruikmaken van de Resource Manager.

Ten slotte kunt u de versleutelde verbindingen voor het exemplaar van SQL Server Database Engine in uw virtuele machine van Azure inschakelen. Configureer SQL server-exemplaar met een ondertekend certificaat. Zie voor meer informatie [versleutelde verbindingen inschakelen met de Database-Engine](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) en [syntaxis verbindingsreeks](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Gebruik van een niet-standaard poort

Standaard luistert SQL Server op een bekende poort 1433. Voor een betere beveiliging, SQL Server om te luisteren op een niet-standaard-poort, zoals 1401 te configureren. Als u een SQL Server afbeelding in de Azure portal inricht, kunt u deze poort in de **SQL Server-instellingen** blade.

Voor het configureren van dit na het inrichten, hebt u twee opties:

- Voor virtuele machines van Resource Manager, kunt u **SQL Server-configuratiebestand** van de VM-overzichtsblade. Dit biedt een optie voor het wijzigen van de poort.

  ![TCP-poort wijzigen in portal](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Voor klassieke virtuele machines of SQL Server-VM's die niet zijn ingericht met de portal, kunt u handmatig de poort configureren door op afstand verbinding te maken met de virtuele machine. Zie voor de configuratiestappen [configureren van een Server om te luisteren naar een specifieke TCP-poort](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). Als u deze handmatig techniek gebruikt, moet u ook een Windows Firewall-regel voor inkomend verkeer op TCP-poort toevoegen.

> [!IMPORTANT]
> Het opgeven van een niet-standaardpoort is een goed idee als uw SQL Server-poort geopend voor het openbare internet-verbindingen is.

Wanneer SQL Server naar een niet-standaardpoort luistert, moet u de poort opgeven wanneer u verbinding maakt. Neem bijvoorbeeld een scenario waarbij de IP-adres van de server 13.55.255.255 en SQL Server luistert op poort 1401. Voor verbinding met SQL Server, geeft u `13.55.255.255,1401` in de verbindingstekenreeks.

## <a name="manage-accounts"></a>Accounts beheren

U wilt niet dat aanvallers gemakkelijk kunnen raden accountnamen of wachtwoorden. Gebruik de volgende tips om u te helpen:

- Maak een unieke lokale administrator-account met heeft geen naam **beheerder**.

- Complexe sterke wachtwoorden gebruiken voor alle accounts. Zie voor meer informatie over het maken van een sterk wachtwoord [maken van een sterk wachtwoord](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) artikel.

- Azure selecteert standaard Windows-verificatie tijdens de installatie van de virtuele Machine van SQL Server. Daarom de **SA** aanmelding is uitgeschakeld en een wachtwoord is toegewezen door setup. We raden aan dat de **SA** aanmelding moet niet worden gebruikt of ingeschakeld. Als u een SQL-aanmelding hebben moet, een van de volgende strategieën gebruiken:

  - Een SQL-account maken met een unieke naam met **sysadmin** lidmaatschap. U kunt dit doen via de portal door in te schakelen **SQL-verificatie** tijdens het inrichten.

    > [!TIP] 
    > Als u SQL-verificatie niet tijdens het inrichten inschakelt, moet u handmatig de verificatiemodus voor wijzigen **SQL Server en Windows-verificatiemodus**. Zie voor meer informatie [wijziging Server-verificatiemodus](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Als u moet de **SA** aanmelding, aanmelding bij de inschakelen na het inrichten en een sterk wachtwoord toe te wijzen.

## <a name="follow-on-premises-best-practices"></a>Volg de aanbevolen procedures voor lokale

Naast de procedures in dit onderwerp beschreven, wordt aangeraden dat u bekijken en implementeren van de beveiligingsprocedures traditionele on-premises, indien van toepassing. Zie voor meer informatie [beveiligingsoverwegingen voor een SQL Server-installatie](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Volgende stappen

Als u ook geïnteresseerd in aanbevolen procedures om de prestaties, Zie [Best Practices prestaties for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

