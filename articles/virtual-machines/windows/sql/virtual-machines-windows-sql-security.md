---
title: Veiligheidsoverwegingen voor SQL Server in Azure | Microsoft Docs
description: Dit onderwerp bevat algemene richtlijnen voor het beveiligen van SQL Server in een Azure-machine.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 69b6bd07699d179fc87ac6c5364a7a34b23d14eb
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731713"
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Veiligheidsoverwegingen voor SQL Server in Azure Virtual Machines

Dit onderwerp bevat algemene richtlijnen voor beveiliging die helpt bij het vaststellen van beveiligde toegang tot SQL Server-exemplaren in een Azure-machine (VM).

Azure voldoet aan diverse branche regelgeving en standaarden waarmee u kunt een compatibele oplossing bouwen met SQL Server in een virtuele machine wordt uitgevoerd. Zie voor meer informatie over de naleving van regelgeving met Azure [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>Toegang tot de SQL-VM beheren

Wanneer u uw SQL Server virtuele machine maakt, houd rekening met hoe u zorgvuldig bepalen wie toegang tot de machine en SQL Server heeft. In het algemeen moet u het volgende doen:

- Toegang tot SQL Server beperken tot alleen de toepassingen en clients die ze nodig hebt.
- Volg de aanbevolen procedures voor het beheren van gebruikersaccounts en wachtwoorden.

De volgende secties vindt u suggesties op denken via deze punten.

## <a name="secure-connections"></a>Beveiligde verbindingen

Wanneer u een SQL Server-machine met een afbeelding maken, de **SQL Server-connectiviteit** optie geeft u de keuze van **lokaal (binnen VM)**, **privé (binnen Virtueelnetwerk)**, of **openbaar (Internet)**.

![SQL Server-verbinding](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

Voor de beste beveiliging, kiest u de meest beperkende optie voor uw scenario. Bijvoorbeeld, als u een toepassing die toegang heeft tot SQL Server op dezelfde virtuele machine, klikt u vervolgens **lokale** is de veiligste optie. Als u een Azure-toepassing waarvoor toegang tot de SQL Server, klikt u vervolgens **persoonlijke** beveiligt-communicatie naar SQL Server binnen de opgegeven [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Als u nodig hebt **openbare** (internet) toegang tot de SQL Server-VM en zorg ervoor dat u Volg de aanbevolen procedures in dit onderwerp voor het verminderen van het gebied van de aanval.

De geselecteerde opties in de portal voor gebruik van inkomende beveiligingsregels op de virtuele machines van [netwerkbeveiligingsgroep](../../../virtual-network/security-overview.md) (NSG) wilt toestaan of weigeren van netwerkverkeer naar uw virtuele machine. U kunt wijzigen of maken van nieuwe inkomende NSG-regels voor het toestaan van verkeer naar de SQL Server-poort (standaard 1433). U kunt ook specifieke IP-adressen die zijn toegestaan om te communiceren via deze poort opgeven.

![Regels voor netwerkbeveiligingsgroepen](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Naast de NSG-regels om netwerkverkeer te beperken, kunt u ook de Windows Firewall op de virtuele machine gebruiken.

Als u van eindpunten met het klassieke implementatiemodel gebruikmaakt, verwijdert u alle eindpunten op de virtuele machine als u ze niet gebruikt. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL voor een eindpunt](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint). Dit is niet nodig zijn voor virtuele machines die gebruikmaken van de Resource Manager.

Ten slotte, overweeg in te schakelen versleutelde verbindingen voor het exemplaar van SQL Server Database Engine in uw virtuele machine van Azure. Configureer SQL server-exemplaar met een ondertekend certificaat. Zie voor meer informatie, [versleutelde verbindingen met de Database-Engine inschakelen](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) en [syntaxis verbindingsreeks](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Gebruik van een niet-standaard-poort

Standaard luistert SQL Server op een bekende poort 1433. Voor een betere beveiliging, SQL Server om te luisteren naar een niet-standaard-poort, zoals 1401 te configureren. Als u een installatiekopie van de galerie met SQL Server in Azure portal inricht, kunt u deze poort in de **SQL Server-instellingen** blade.

Als u wilt dit na het inrichten configureren, hebt u twee opties:

- Voor virtuele machines van Resource Manager, kunt u selecteren **SQL Server-configuratie** vanuit de overzichtsblade van de virtuele machine. Dit biedt een optie voor het wijzigen van de poort.

  ![TCP-poort in de portal wijzigen](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Voor klassieke virtuele machines of SQL Server-VM's die niet zijn ingericht via de portal, kunt u handmatig de poort configureren door het op afstand verbinding maken met de virtuele machine. Zie voor de configuratiestappen [configureren van een Server om te luisteren naar een specifieke TCP-poort](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). Als u deze handmatig techniek gebruikt, moet u ook een Windows Firewall-regel voor inkomend verkeer op TCP-poort toevoegen.

> [!IMPORTANT]
> Opgeven van een niet-standaardpoort is een goed idee als uw SQL Server-poort geopend voor het openbare internet-verbindingen is.

Wanneer SQL Server op een niet-standaardpoort luistert, moet u de poort opgeven wanneer u verbinding maakt. Neem bijvoorbeeld een scenario waarbij het IP-adres van de server 13.55.255.255 en SQL Server luistert op poort 1401. Voor verbinding met SQL Server, geeft u `13.55.255.255,1401` in de verbindingsreeks.

## <a name="manage-accounts"></a>Accounts beheren

U wilt niet dat aanvallers te raden eenvoudig accountnamen of wachtwoorden. Gebruik de volgende tips om u te helpen:

- Maken van een unieke lokale administrator-account niet met de naam **beheerder**.

- Complexe sterke wachtwoorden gebruiken voor al uw accounts. Zie voor meer informatie over het maken van een sterk wachtwoord [maken van een sterk wachtwoord](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) artikel.

- Azure selecteert standaard Windows-verificatie tijdens de installatie van SQL Server-VM. Daarom de **SA** aanmelding is uitgeschakeld en een wachtwoord is toegewezen door setup. We raden aan dat de **SA** aanmelding moet niet worden gebruikt of ingeschakeld. Als u een SQL-aanmelding hebben moet, gebruikt u een van de volgende strategieën:

  - Een SQL-account maken met een unieke naam die is **sysadmin** lidmaatschap. U kunt dit doen vanuit de portal door in te schakelen **SQL-verificatie** tijdens het inrichten.

    > [!TIP] 
    > Als u SQL-verificatie niet tijdens het inrichten inschakelt, moet u handmatig de verificatiemodus te wijzigen **SQL Server en Windows-verificatiemodus**. Zie voor meer informatie, [wijziging Server-verificatiemodus](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Als u moet gebruiken de **SA** -aanmelding, het inschakelen van de aanmelding na het inrichten en toewijzen van een sterk wachtwoord.

## <a name="follow-on-premises-best-practices"></a>Volg de aanbevolen procedures voor on-premises

Behalve de procedures in dit onderwerp wordt beschreven, raden wij u bekijken en de beveiligingsprocedures traditionele on-premises implementatie, indien van toepassing. Zie voor meer informatie, [beveiligingsoverwegingen voor een SQL Server-installatie](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Volgende stappen

Als u ook geïnteresseerd in aanbevolen procedures voor prestaties, Zie [aanbevolen procedures voor prestaties voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md). Als u vragen hebt over virtuele machines met SQL Server, raadpleegt u [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

