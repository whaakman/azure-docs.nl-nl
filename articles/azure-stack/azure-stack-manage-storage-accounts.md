---
title: Azure Stack-opslagaccounts beheren | Microsoft Docs
description: Meer informatie over het zoeken, beheren, herstellen en Azure Stack-storage-accounts vrijmaken
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 8eed0b4f2d14d22bdd9eddac9dbb4c0e02404975
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302711"
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Storage-accounts in Azure Stack beheren
Informatie over het beheren van storage-accounts in Azure Stack om te zoeken, herstellen en het vrijmaken van opslagcapaciteit op basis van bedrijfsbehoeften.

## <a name="find"></a>Een storage-account zoeken
De lijst met opslagaccounts in de regio kan worden weergegeven in Azure Stack door:

1. Aanmelden bij de [-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **alle services** > **regiobeheer** onder **beheer**.

3. Selecteer **opslag** uit de **Resourceproviders** lijst.
   
   ![Opslagresourceprovider](media/azure-stack-manage-storage-accounts/image1.png)

5. Selecteer **opslagaccounts** in **opslag**.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
   
   De blade wordt weergegeven de lijst met opslagaccounts in deze regio.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

De eerste 10 accounts worden standaard weergegeven. U kunt kiezen voor het ophalen van informatie door te klikken op de **meer laden** koppelen aan de onderkant van de lijst.

OF

Als u geïnteresseerd in een bepaalde storage-account bent, kunt u **filteren en op te halen van de relevante accounts** alleen.


**Om te filteren voor accounts:**

1. Selecteer **Filter** aan de bovenkant van het deelvenster.
2. Op het filterdeelvenster Hiermee kunt u om op te geven **accountnaam**, ** abonnements-ID, of **status** voor het afstemmen van de lijst met opslagaccounts moet worden weergegeven. Gebruik deze waar nodig.
3. Selecteer **Update**. De lijst moet dienovereenkomstig vernieuwen.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Het filter opnieuw: Selecteer **Filter**, wist u de selecties en bijwerken.

Het zoekvak (boven aan het deelvenster van de lijst in de storage-accounts) kunt u de geselecteerde tekst in de lijst van accounts markeren. U kunt dit gebruiken wanneer de volledige naam of ID niet eenvoudig beschikbaar is.

U kunt hier vrije tekst gebruiken om te zoeken naar het account waarin u geïnteresseerd bent.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Bekijk de details
Nadat u de accounts die u geïnteresseerd bent in de weergave hebt gevonden, kunt u het specifieke account om bepaalde gegevens weer te geven. Een nieuw deelvenster geopend met de accountdetails zoals: het type van het account, het maken, locatie, enzovoort.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Een verwijderd account herstellen
Het is mogelijk in een situatie waarin u wilt een verwijderd account herstellen.

Er is een eenvoudige manier om dat te doen in Azure Stack:

1. Blader naar de lijst met storage-accounts. Zie [vinden van een storage-account](#find) in dit artikel voor meer informatie.
2. Zoek dat specifieke account in de lijst. U wilt filteren.
3. Controleer de *status* van het account. Deze melding **verwijderde**.
4. Selecteer het account waarmee het detailvenster van het account wordt geopend.
5. Boven op dit deelvenster, zoek de **herstellen** knop en selecteer deze.
6. Selecteer **Ja** om te bevestigen.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Het herstel wordt nu *verwerken... Wacht* voor een indicatie dat deze geslaagd is.
   U kunt ook selecteren in de map ' belpictogram ' aan de bovenkant van de portal om voortgang vermeldingen weer te geven.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Zodra het herstelde account wordt gesynchroniseerd, kunnen deze kan worden gebruikt.

### <a name="some-gotchas"></a>Sommige gotcha 's
* Uw account verwijderd toont de status als **buiten de bewaarperiode**.
  
  Buiten de bewaarperiode manier zijn dat de verwijderde account de bewaarperiode is overschreden en kan niet worden hersteld.
* Uw account verwijderd, wordt niet weergegeven in de lijst met accounts.
  
  Uw account kan niet in de lijst met accounts weergegeven wanneer de verwijderde account is al verwijderd. In dit geval wordt worden deze niet hersteld. Zie [vrijmaken capaciteit](#reclaim) in dit artikel.

## <a name="set-the-retention-period"></a>De bewaarperiode instellen
De bewaartermijn instellen kunt een cloud-operator om op te geven van een bepaalde periode in dagen (tussen 0 en 9999 dagen) gedurende welke een verwijderd account kan mogelijk worden hersteld. De gebruikelijke bewaarperiode is ingesteld op 0 dagen. Als de waarde '0' betekent dat alle verwijderde account onmiddellijk buiten de bewaarperiode valt en gemarkeerd voor periodieke garbagecollection.

**De bewaarperiode wijzigen:**

1. Aanmelden bij de [-beheerportal](https://adminportal.local.azurestack.external).
2. Selecteer **alle services** > **regiobeheer** onder **beheer**.
3. Selecteer **opslag** uit de **Resourceproviders** lijst.
4. Selecteer **instellingen** aan de bovenkant om de instelling deelvenster te openen.
5. Selecteer **configuratie** bewerkt u vervolgens de waarde voor de retentie-periode.

   Het aantal dagen instellen en vervolgens op te slaan.
   
   Deze waarde wordt onmiddellijk van kracht en is ingesteld voor uw hele regio.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Capaciteit vrijmaken
Een van de neveneffecten van met een bewaarperiode is dat een verwijderd account blijft totdat het buiten de bewaarperiode gaat de capaciteit verbruiken. Als een cloud-operator moet u een manier om de verwijderde account ruimte vrijmaken, zelfs als de bewaarperiode is nog niet verlopen.

U kunt vrijmaken capaciteit die gebruikmaakt van de portal of PowerShell.

**Het vrijmaken van capaciteit die gebruikmaakt van de portal:**
1. Ga naar het deelvenster met storage-accounts. Zie [vinden van een storage-account](#find).
2. Selecteer **ruimte vrijmaken** aan de bovenkant van het deelvenster.
3. Lees het bericht en selecteer vervolgens **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Wachten op een geslaagde melding Zie het belpictogram op de portal.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Vernieuw de pagina Storage-accounts. De verwijderde accounts worden niet meer weergegeven in de lijst omdat ze zijn verwijderd.

U kunt ook PowerShell gebruiken om op te heffen expliciet de bewaarperiode en onmiddellijk vrijmaken capaciteit.

**Capaciteit vrij te maken met behulp van PowerShell:**   

1. Bevestig dat u hebt Azure PowerShell is geïnstalleerd en geconfigureerd. Als dat niet het geval is, gebruik de volgende instructies: 
   * Zie voor de meest recente versie van Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement, [hoe u Azure PowerShell installeren en configureren](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Zie voor meer informatie over Azure Resource Manager-cmdlets, [met behulp van Azure PowerShell met Azure Resource Manager](https://go.microsoft.com/fwlink/?LinkId=394767)
2. Voer de volgende cmdlets:

> [!NOTE]  
> Als u deze cmdlets uitvoert, verwijdert u permanent het account en de inhoud ervan. Het kan niet worden hersteld. Gebruik deze optie met zorg.

```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
````

Zie voor meer informatie, [documentatie over Azure Stack PowerShell](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
 

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het beheren van machtigingen [Manage Role-Based Access Control](azure-stack-manage-permissions.md).
 - Zie voor informatie over het beheren van opslagcapaciteit voor Azure Stack, [beheren opslagcapaciteit voor Azure Stack](azure-stack-manage-storage-shares.md).