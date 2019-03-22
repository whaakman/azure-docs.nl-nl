---
title: Maak een back-up van App Service in Azure Stack | Microsoft Docs
description: Gedetailleerde richtlijnen voor het back-up van Azure Stack App Service.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: jeffgilb
ms.reviewer: apwestgarth
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: 66f1f1389a7e4ceebc38544f2eb9836fad2bd96a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340548"
---
# <a name="back-up-app-service-on-azure-stack"></a>Back-up van App Service in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*  

Dit document bevat instructies over het back-up van App Service in Azure Stack.

> [!IMPORTANT]
> App Service in Azure Stack is geen back-ups als onderdeel van [back-up van Azure Stack-infrastructuur](azure-stack-backup-infrastructure-backup.md). Als een Azure Stack-operators, moet u de stappen om ervoor te zorgen dat App Service met succes kunnen worden hersteld indien nodig uitvoeren.

Azure App Service in Azure Stack bestaat uit vier belangrijke onderdelen om te overwegen bij het plannen voor herstel na noodgevallen:
1. De resource provider infrastructuur; Server-rollen, werkrolniveaus, enzovoort. 
2. De App Service-geheimen
3. De App Service SQL Server die als host fungeert en betaling naar databases
4. De App Service werkbelasting voor gebruikers inhoud opgeslagen in de App Service-bestandsshare   

## <a name="back-up-app-service-secrets"></a>Back-up van App Service-geheimen
Tijdens het herstellen van App Service vanuit back-up, moet u de App Service-sleutels die worden gebruikt door de eerste implementatie opgeven. Deze gegevens moet worden opgeslagen als App Service met succes is geïmplementeerd en die zijn opgeslagen in een veilige locatie. De configuratie van de resource provider-infrastructuur wordt opnieuw vanuit back-up gemaakt tijdens het herstel met behulp van App Service recovery PowerShell-cmdlets.

Gebruik de beheerportal voor back-up van app service-geheimen door de volgende stappen: 

1. Meld u aan de Azure Stack-beheerportal als de service-beheerder.

2. Blader naar **App Service** -> **geheimen**. 

3. Selecteer **geheimen downloaden**.

   ![Geheimen downloaden](./media/app-service-back-up/download-secrets.png)

4. Wanneer de geheimen zijn gereed voor het downloaden, klikt u op **opslaan** en op te slaan van de App Service-geheimen (**SystemSecrets.JSON**)-bestand op een veilige locatie. 

   ![Geheimen opslaan](./media/app-service-back-up/save-secrets.png)

> [!NOTE]
> Herhaal deze stappen telkens wanneer de App Service-geheimen worden gedraaid.

## <a name="back-up-the-app-service-databases"></a>Back-up van de App Service-databases
Voor het herstellen van App Service, moet u de **Appservice_hosting** en **Appservice_metering** databaseback-ups. We adviseren onderhoudsplannen voor SQL Server of Azure Backup Server gebruiken om te controleren of deze databases worden back-ups en veilig opgeslagen op gezette tijden. Een methode om ervoor te zorgen regelmatig SQL-back-ups worden echter gemaakt kan worden gebruikt.

Als u wilt handmatig back-up van deze databases terwijl u bent aangemeld bij de SQL-Server, kunt u de volgende PowerShell-opdrachten:

  ```powershell
  $s = "<SQL Server computer name>"
  $u = "<SQL Server login>" 
  $p = read-host "Provide the SQL admin password"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_hosting TO DISK = '<path>\hosting.bak'"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_metering TO DISK = '<path>\metering.bak'"
  ```

> [!NOTE]
> Als u back-up van SQL AlwaysOn-databases wilt, voert u de [deze instructies](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server?view=sql-server-2017). 

Nadat alle databases is back-ups zijn, moet u de .bak-bestanden kopiëren naar een veilige locatie, samen met de gegevens van de App Service-geheimen.

## <a name="back-up-the-app-service-file-share"></a>Back-up van de App Service-bestandsshare
App Service-winkels tenant app-gegevens in de bestandsshare. Dit moet een back-up op een geregelde samen met de App Service-databases zodat zo weinig mogelijk gegevens verloren gaan als herstel vereist is. 

Als u wilt back-up van de App Service-bestandsshare die inhoud, dat kunt u Azure Backup Server of een andere methode naar de bestandsshare regelmatig te kopiëren naar de locatie van inhoud u hebt opgeslagen alle vorige herstelgegevens. 

Volg deze stappen kunt u bijvoorbeeld robocopy vanuit een sessie van de console Windows PowerShell (niet PowerShell ISE) gebruiken:

```powershell
$source = "<file share location>"
$destination = "<remote backup storage share location>"
net use $destination /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy $source $destination
net use $destination /delete
```

## <a name="next-steps"></a>Volgende stappen
[Herstellen van App Service op Azure Stack](app-service-recover.md)