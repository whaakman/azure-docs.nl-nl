---
title: Hoe u een in-place upgrade van de editie van SQL Server op virtuele Azure-machine | Microsoft Docs
description: Informatie over het wijzigen van de editie van SQL Server-VM in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 05d2170fe9e6055179bf49d803d4739ddc05be89
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607544"
---
# <a name="how-to-perform-an-in-place-upgrade-of-sql-server-edition-on-azure-vm"></a>Het uitvoeren van een in-place upgrade van de editie van SQL Server op Azure VM

In dit artikel wordt beschreven hoe u de editie van SQL Server wijzigen voor een bestaande SQL Server op een Windows-machine in Azure. 

De editie van SQL Server wordt bepaald door de productcode, en wordt opgegeven met het installatieproces. De editie bepaalt wat [functies](/sql/sql-server/editions-and-components-of-sql-server-2017) zijn beschikbaar in de SQL Server-product. U kunt de editie van SQL Server met de installatiemedia en een downgrade van uw kosten verlagen of een upgrade uitvoeren om in te schakelen van meer functies kunt wijzigen.

Als u de editie van SQL Server met behulp van de installatiemedia na de registratie bij de resourceprovider van de SQL-VM hebt bijgewerkt, klikt u vervolgens voor het bijwerken van de Azure billing dienovereenkomstig u moet de eigenschap SQL Server-editie van de SQL-VM-resource als volgt instellen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com). 
1. Navigeer naar de bron van de virtuele machine van uw SQL Server. 
1. Onder **instellingen**, selecteer **configureren** en selecteer vervolgens de gewenste editie van SQL Server in de vervolgkeuzelijst onder **Edition**. 

   ![Metagegevens van de editie wijzigen](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Bekijk de waarschuwing om te informeren dat u eerst SQL Server-editie wijzigen moet en die eigenschap editie moet overeenkomen met de editie van SQL Server. 
1. Selecteer **toepassen** de editie metagegevens wijzigingen worden toegepast. 


## <a name="prerequisites"></a>Vereisten

Als u wilt een wijziging in plaats van de editie van SQL Server doet nodig u het volgende: 

- Een [Azure-abonnement](https://azure.microsoft.com/free/).
- Een Windows [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) geregistreerd bij de [SQL-VM-resourceprovider](virtual-machines-windows-sql-register-with-resource-provider.md).
- Media met de gewenste editie van SQL Server Setup. Klanten die hebben [software assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) kunt verkrijgen van de installatiemedia van de [Volume Licensing Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Klanten hebben geen software assurance het installatiemedium van een marketplace-installatiekopie voor SQL Server-VM met de gewenste editie kunt gebruiken.


## <a name="upgrade-edition"></a>Editie bijwerken

  > [!WARNING]
  > - **Upgraden van de editie van SQL Server, wordt de service voor SQL Server, evenals alle gekoppelde services, zoals Analysis Services en R Services opnieuw.** 

Als u de editie van SQL Server bijwerken, verkrijgen van de SQL Server setup-media voor de gewenste editie van SQL Server en vervolgens het volgende doen:

1. Setup.exe starten vanaf de installatiemedia van SQL Server. 
1. Navigeer naar **onderhoud** en kies de **editie-Upgrade** optie. 

   ![Upgrade van de editie van SQL Server](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Selecteer **volgende** totdat de **gereed voor upgrade edition** pagina en selecteer vervolgens **Upgrade**. Het instellingenvenster loopt over een paar minuten terwijl de wijziging effect duurt, en vervolgens u ziet een **voltooid** pagina bevestiging dat uw editie-upgrade voltooid is. 

Na de upgrade van SQL Server-editie, moet u de eigenschap 'Edition' van de Sql-Machine in Azure portal wijzigen zoals hierboven; Hiermee worden de metagegevens en facturering die zijn gekoppeld aan deze virtuele machine bijgewerkt.

## <a name="downgrade-edition"></a>Downgrade edition

  > [!WARNING]
  > - **Het downgraden van de editie van SQL Server vereist volledig verwijderen van SQL Server, die meer downtime in rekening worden gebracht**. 


Als u wilt downgraden van de editie van SQL Server, moet u volledig SQL-Server verwijderen en opnieuw installeren met de gewenste editie-installatiemedium. 

U kunt de editie van SQL Server downgraden door de volgende stappen:

1. Maak een back-up van alle databases, met inbegrip van de systeemdatabases. 
1. Systeemdatabases (master, model en msdb) naar een nieuwe locatie verplaatsen. 
1. Verwijder SQL Server en alle bijbehorende services. 
1. Start de virtuele machine. 
1. Installeer SQL Server met behulp van de media met de gewenste editie van SQL Server.
1. Installeer de nieuwste servicepacks zijn en de cumulatieve updates.  
1. Vervangt het nieuwe systeemdatabases die zijn gemaakt tijdens de installatie met het systeemdatabases die u eerder hebt verplaatst naar een andere locatie. 

Wanneer de Downgrade van de editie van SQL Server wordt uitgevoerd, moet u de eigenschap 'Edition' van de virtuele SQL-machine in Azure portal wijzigen zoals hierboven; Hiermee worden de metagegevens en facturering die zijn gekoppeld aan deze virtuele machine bijgewerkt.

## <a name="remarks"></a>Opmerkingen

 - De editie-eigenschap voor de SQL Server-VM moet overeenkomen met de versie van de SQL-Server geïnstalleerd op de virtuele machine voor alle SQL virtuele Machines met inbegrip van zowel betalen per gebruik en BYOL licentietypen.
 - Als u uw SQL Server-VM-resource, gaat u terug naar de instelling van de vastgelegde editie van de installatiekopie.
  - De mogelijkheid om te wijzigen van de editie is een functie van de SQL-VM-resourceprovider. Automatisch implementeren van een marketplace-installatiekopie via de Azure-portal, wordt een virtuele machine van SQL Server geregistreerd bij de resourceprovider. Klanten die SQL Server zelf installeert wordt moet echter handmatig [registreren hun SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md).
- Een SQL Server-VM toevoegen aan een beschikbaarheidsset is vereist dat de virtuele machine opnieuw wordt gemaakt. Als dergelijke ziet u alle virtuele machines die zijn toegevoegd aan een beschikbaarheidsset set wordt gaat u terug naar de standaard-editie en de editie moet opnieuw worden gewijzigd.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server op een Windows VM-Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server op een Windows-VM prijsinformatie](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server op een Windows VM release-opmerkingen](virtual-machines-windows-sql-server-iaas-release-notes.md)


