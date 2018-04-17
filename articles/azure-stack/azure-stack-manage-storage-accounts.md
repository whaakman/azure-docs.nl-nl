---
title: Stack Azure storage-accounts beheren | Microsoft Docs
description: Meer informatie over het zoeken, beheren, herstellen en vrijmaken Stack Azure storage-accounts
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: anirudha
ms.openlocfilehash: ce61dec785d6207e2d2df21884525f76a6778f4a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Storage-accounts in Azure-Stack beheren
Informatie over het beheren van de storage-accounts in Azure-Stack om te zoeken, herstellen en vrijmaken opslagcapaciteit op basis van zakelijke behoeften.

## <a name="find"></a>Een opslagaccount vinden
De lijst met opslagaccounts in het gebied kan worden weergegeven in de Azure-Stack door:

1. In een webbrowser en navigeer naar https://adminportal.local.azurestack.external.
2. Aanmelden bij de Azure-Stack-beheerportal als een cloud-operator (met behulp van de referenties die u hebt opgegeven tijdens de implementatie)
3. Zoek op het standaarddashboard – de **regio management** lijst en klik op de regio die u verkennen wilt, bijvoorbeeld **(lokale**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Selecteer **opslag** van de **Resourceproviders** lijst.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. In het deelvenster van de beheerder in de storage Resource Provider – Schuif vervolgens omlaag naar de **opslagaccounts** tabblad en klik erop.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   De resulterende pagina is de lijst met storage-accounts in deze regio.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

De eerste 10 accounts worden standaard weergegeven. U kunt meer ophalen door te klikken op de **laden meer** koppeling onder aan de lijst.

OF

Als u geïnteresseerd in een bepaalde opslagaccounts bent – kunt u **filteren en ophalen van de relevante accounts** alleen.


**Filteren voor accounts:**

1. Klik op **Filter** aan de bovenkant van het deelvenster.
2. In het deelvenster Filter kunt u opgeven **accountnaam**, ** abonnements-ID of **status** af te stemmen van de lijst met opslagaccounts moet worden weergegeven. Deze zo nodig gebruiken.
3. Klik op **Update**. De lijst moet dienovereenkomstig vernieuwen.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Het filter opnieuw: klik op **Filter**, wis de selecties en bijwerken.

Het zoekvak (boven aan het deelvenster van de lijst in de storage-accounts) kunt u de geselecteerde tekst in de lijst met accounts markeren. U kunt dit gebruiken als de volledige naam of ID niet toegankelijk is.

U kunt hier vrije tekst gebruiken om te zoeken naar het account dat u geïnteresseerd bent in.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Bekijk de details
Zodra u de accounts die u geïnteresseerd bent in de weergave hebt gevonden, kunt u het specifieke account om bepaalde details te bekijken. Een nieuw deelvenster wordt geopend met de accountdetails, zoals: het type van de account, de tijd voor het maken, de locatie, enzovoort.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Een verwijderde account herstellen
Het is mogelijk in een situatie waarin u wilt herstellen, een verwijderde account.

Er is een eenvoudige manier dat in Azure-Stack:

1. Blader naar de lijst van de storage-accounts. Zie [vinden van een opslagaccount](#find) in dit onderwerp voor meer informatie.
2. Dat bepaalde account niet vinden in de lijst. U wilt filteren.
3. Controleer de *status* van het account. Deze melding **verwijderd**.
4. Klik op het account waarmee het detailvenster van het account wordt geopend.
5. Naast dit deelvenster, zoek de **herstellen** knop en klik erop.
6. Klik op **Ja** om te bevestigen.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Het herstel wordt nu *verwerken... Wacht* voor een indicatie dat geslaagd is.
   U kunt ook klikken op het ' belpictogram ' aan de bovenkant van de portal om vermeldingen van de voortgang weer te geven.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Als de herstelde account wordt gesynchroniseerd, kunnen deze kan worden gebruikt.

### <a name="some-gotchas"></a>Sommige Gotchas
* Uw account verwijderd toont de status als **buiten de bewaarperiode**.
  
  Buiten de bewaarperiode manier zijn dat de verwijderde account is groter dan de bewaarperiode en kan niet worden hersteld.
* Uw account verwijderd, wordt niet weergegeven in de accountlijst.
  
  Je account kan niet in de lijst met accounts weergegeven wanneer het verwijderde account is al garbage collector zijn verzameld. In dit geval wordt worden deze niet hersteld. Zie [vrijmaken capaciteit](#reclaim) in dit onderwerp.

## <a name="set-the-retention-period"></a>De bewaarperiode instellen
De bewaartermijn instellen kunt een cloud-operator om op te geven van een bepaalde periode in dagen (tussen 0 en 9999 dagen) waarover mogelijk alle verwijderde accounts kan worden hersteld. De bewaartermijn is ingesteld op 0 dagen. Als u de waarde instelt op '0' of een verwijderde account onmiddellijk buiten de bewaarperiode valt en gemarkeerd voor periodieke garbagecollection.

**De bewaarperiode wijzigen:**

1. In een webbrowser en navigeer naar https://adminportal.local.azurestack.external.
2. Aanmelden bij de Azure-Stack-beheerportal als een cloud-operator (met behulp van de referenties die u hebt opgegeven tijdens de implementatie)
3. Zoek op het standaarddashboard – de **regio management** lijst en klik op de regio die u wilt verkennen bijvoorbeeld **(lokale**).
4. Selecteer **opslag** van de **Resourceproviders** lijst.
5. Klik op **instellingen** boven om de instelling deelvenster te openen.
6. Klik op **configuratie** bewerkt u vervolgens de waarde voor de bewaarperiode-periode.

   Het aantal dagen instellen en vervolgens opslaan.
   
   Deze waarde wordt onmiddellijk van kracht en is ingesteld voor uw hele regio.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Capaciteit vrijmaken
Een van de neveneffecten van met een bewaarperiode is dat een verwijderde account blijft totdat deze buiten de bewaarperiode wordt de capaciteit verbruiken. Als een cloud-operator moet u mogelijk een manier om de verwijderde account ruimte vrijmaken, zelfs als de bewaarperiode is nog niet verlopen.

U kunt vrijmaken met de portal of PowerShell capaciteit.

**Vrijmaken capaciteit via de portal:**
1. Ga naar het deelvenster storage-accounts. Zie [vinden van een opslagaccount](#find).
2. Klik op **ruimte vrijmaken** aan de bovenkant van het deelvenster.
3. Het bericht gelezen en klik vervolgens op **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Wachten op een geslaagde melding, Zie het belpictogram op de portal.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Vernieuw de pagina Storage-accounts. De verwijderde accounts worden niet meer in de lijst weergegeven omdat ze zijn verwijderd.

U kunt ook PowerShell gebruiken voor het onderdrukken van expliciet de bewaarperiode en onmiddellijk vrijmaken capaciteit.

**Om capaciteit vrij te maken met behulp van PowerShell:**   

1. Bevestigen dat u Azure PowerShell installeren en configureren. Als dat niet het geval is, gebruik de volgende instructies: 
   * Als u wilt de nieuwste versie van Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement, Zie [installeren en configureren van Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Zie voor meer informatie over cmdlets van Azure Resource Manager [Azure PowerShell gebruiken met Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Voer de volgende cmdlet uit:

> [!NOTE]
> Als u deze cmdlet uitvoert, verwijdert u permanent het account en de inhoud ervan. Het is niet hersteld. Gebruik deze zorgvuldig.


        Clear-ACSStorageAccount -ResourceGroupName system.local -FarmName <farm ID>


Zie voor meer informatie [Stack Azure powershell-documentatie.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="migrate-a-container"></a>Migreren van een container
Vanwege het opslaggebruik van de ongelijke door tenants, een operator cloud wellicht een of meer onderliggende tenant deelt met meer ruimte dan andere. Als dit het geval is, worden de cloud-operator kunt proberen te Maak ruimte vrij op de share extreme door een aantal blob-containers handmatig te migreren naar een andere share. 

U moet PowerShell gebruiken voor het migreren van containers.
> [!NOTE]
>Migratie van BLOB-container biedt geen ondersteuning voor livemigratie en is momenteel een bewerking offline. Tijdens de migratie en totdat deze voltooid is in de onderliggende blobs in de container kunnen niet worden gebruikt en 'offline' zijn. 

**Migreren met behulp van PowerShell containers:**

1. Bevestigen dat u Azure PowerShell installeren en configureren. Als dat niet het geval is, gebruik de volgende instructies:
    * Als u wilt de nieuwste versie van Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement, Zie [installeren en configureren van Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Zie voor meer informatie over cmdlets van Azure Resource Manager [Azure PowerShell gebruiken met Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Naam van de farm ophalen: 
      
      `$farm = Get-ACSFarm -ResourceGroupName system.local`
3. Haal de shares: 

   `$shares = Get-ACSShare -ResourceGroupName system.local -FarmName $farm.FarmName`

4. De containers voor een bepaalde share niet ophalen. Houd er rekening mee dat telling en het doel optionele parameters zijn:
            
   `$containers = Get-ACSContainer -ResourceGroupName system.local -FarmName $farm.FarmName -ShareName $shares[0].ShareName -Count 4 -Intent Migration`  

   Controleer $containers:

   `$containers`

    ![](media/azure-stack-manage-storage-accounts/image13.png)
5. Haal de beste bestemming shares voor de migratie van de container:

    `$destinationshares= Get-ACSSharesForMigration  -ResourceGroupName system.local -FarmName $farm.farmname -SourceShareName $shares[0].ShareName`

    Controleer $destinationshares:

    `$destinationshares`

    ![](media/azure-stack-manage-storage-accounts/image14.png)
6. Ere van migratie voor een container, zoals u ziet dat dit is een async-implementatie, zodat een kan alle containers in een share in een lus en bijhouden van de status van de geretourneerde taak-ID.

    `$jobId = Start-ACSContainerMigration -ResourceGroupName system.local -FarmName $farm.farmname -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares.UncPath`

    Controleer $jobId:

   ```
   $jobId
   d1d5277f-6b8d-4923-9db3-8bb00fa61b65
   ```
7. Controleer de status van de migratietaak door de taak-ID. Wanneer de migratie van de container is voltooid, is MigrationStatus ingesteld op 'Voltooid'.

    `Get-ACSContainerMigrationStatus -ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image15.png)

8. U kunt een migratietaak in voortgang annuleren. Dit opnieuw is een asynchrone bewerking en kan worden bijgehouden met $jobid:

    `Stop-ACSContainerMigration-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId-Verbose`

    ![](media/azure-stack-manage-storage-accounts/image16.png)

    U kunt controleren dat de statussen van de migratie annuleren opnieuw:

    `Get-ACSContainerMigrationStatus-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image17.png)




  
  
