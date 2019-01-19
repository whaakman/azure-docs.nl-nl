---
title: Een Recovery Services-kluis voor Azure-abonnementen of naar een andere resourcegroep verplaatsen
description: Instructies voor het verplaatsen van de recovery services-kluis in azure-abonnementen en resourcegroepen.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 1/4/2019
ms.author: sogup
ms.openlocfilehash: 0ab626bffa3520af0ea23314cbaed118d66e280f
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401925"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups-limited-public-preview"></a>Een Recovery Services-kluis verplaatsen tussen Azure-abonnementen en resourcegroepen (beperkte openbare preview-versie)

In dit artikel wordt uitgelegd hoe u een Recovery Services-kluis geconfigureerd voor Azure Backup voor Azure-abonnementen of naar een andere resourcegroep in hetzelfde abonnement verplaatsen. U kunt de Azure portal of PowerShell gebruiken om te verplaatsen van een Recovery Services-kluis.

## <a name="prerequisites-for-moving-a-vault"></a>Vereisten voor het verplaatsen van een kluis

- Bij het verplaatsen tussen resourcegroepen, worden zowel de bron-resourcegroep en de doelresourcegroep vergrendeld tijdens de bewerking. Totdat de verplaatsing is voltooid, schrijven en verwijderen van bewerkingen op de brongroepen zijn geblokkeerd.
- Alleen de beheerder van het abonnement heeft de machtigingen voor het verplaatsen van een kluis.
- Bij het verplaatsen van een kluis voor abonnementen, het doelabonnement moet aanwezig zijn in een status ingeschakeld en moet zich in dezelfde tenant als de bronabonnement.
- U moet gemachtigd om uit te voeren schrijfbewerkingen op de doelresourcegroep.
- U kunt de locatie van de Recovery Services-kluis niet wijzigen. De resourcegroep verplaatsen van de kluis alleen worden gewijzigd. De nieuwe resourcegroep mogelijk in een andere locatie, maar dat de locatie van de kluis niet wijzigen.
- Op dit moment kunt u een Recovery Services-kluis verplaatsen per regio, op een tijdstip.
- Als een virtuele machine niet met de Recovery Services-kluis voor abonnementen, of naar een nieuwe resourcegroep verplaatsen, blijven de huidige VM-herstelpunten behouden in de kluis totdat ze zijn verlopen.
- Of de virtuele machine wordt verplaatst met de kluis of niet, kunt u altijd de virtuele machine herstellen vanuit de bewaarde back-upgeschiedenis in de kluis.
-   De Azure Disk Encryption is vereist dat uw sleutelkluis en de virtuele machines zich in de dezelfde Azure-regio en het abonnement bevinden.
-   Zie voor het verplaatsen van een virtuele machine met beheerde schijven, dit [artikel](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/).
-   De opties voor het verplaatsen van resources die zijn geïmplementeerd via het klassieke model zijn afhankelijk van of u de resources binnen een abonnement of naar een nieuw abonnement worden verplaatst. Zie voor meer informatie dit [artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#classic-deployment-limitations).
-   Back-upbeleid gedefinieerd voor de kluis worden bewaard nadat de kluis tussen abonnementen of naar een nieuwe resourcegroep verplaatst.
-   U kunt met de Azure-bestanden, Azure File Sync of SQL in IaaS-VM's voor abonnementen en resourcegroepen kluizen op dit moment niet verplaatsen. Ondersteuning voor deze scenario's wordt toegevoegd in toekomstige releases.
-   Als u een kluis back-upgegevens van virtuele machine, met verschillende abonnementen, moet u uw virtuele machines verplaatsen naar het hetzelfde abonnement en de dezelfde doelresourcegroep gebruiken om door te gaan van de back-ups.<br>

> [!NOTE]
>
Recovery Services-kluizen geconfigureerd voor gebruik met **Azure Site Recovery** kan niet worden verplaatst, nog. Als u virtuele machines hebt geconfigureerd (Azure IaaS, Hyper-V, VMware) of fysieke machines voor het gebruik van disaster recovery de **Azure Site Recovery**, de verplaatsing worden geblokkeerd. De functie voor het verplaatsen van resources voor Site Recovery-service is nog niet beschikbaar.
>
>

## <a name="register-the-source-subscription-to-move-your-recovery-services-vault"></a>Registratie van het bronabonnement voor het verplaatsen van uw Recovery Services-kluis

Voor het registreren van het abonnement van bron naar **verplaatsen** uw Recovery Services-kluis, de volgende cmdlets uitvoeren vanuit de PowerShell-terminal:

1. Aanmelden bij uw Azure-account

  ```
  Connect-AzureRmAccount
  ```

2.  Selecteer het abonnement dat u wilt registreren

    ```
    Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```
3.  Dit abonnement registreren

  ```
  Register-AzureRmProviderFeature -ProviderNamespace Microsoft.RecoveryServices -FeatureName RecoveryServicesResourceMove
  ```

4. De opdracht uitvoeren

  ```
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
  ```

Wacht 30 minuten voor het abonnement om te worden goedgekeurd voordat u begint met de bewerking voor verplaatsen met behulp van de Azure portal of PowerShell.

## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-different-resource-group"></a>Azure portal gebruiken voor een Recovery Services-kluis naar een andere resourcegroep verplaatsen

Een recovery services-kluis en alle bijbehorende resources naar andere resourcegroep verplaatsen

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Open de lijst van **Recovery Services-kluizen** en selecteer de kluis die u wilt verplaatsen. Wanneer het kluisdashboard wordt geopend, wordt deze weergegeven zoals in de volgende afbeelding.

  ![Open herstellen Services-kluis](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

  Als u niet ziet de **Essentials** informatie voor uw kluis, klikt u op het pictogram van de vervolgkeuzelijst. U ziet nu de Essentials-informatie voor uw kluis.

  ![Tabblad met essentiële informatie](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klik in het menu overzicht-kluis op **wijzigen** naast de **resourcegroep**, om te openen de **resources verplaatsen** blade.

  ![Resourcegroep wijzigen](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. In de **resources verplaatsen** blade voor de geselecteerde kluis het wordt aanbevolen de optionele gerelateerde resources verplaatsen door het selectievakje, zoals wordt weergegeven in de volgende afbeelding.

  ![Abonnement verplaatsen](./media/backup-azure-move-recovery-services/move-resource.png)

5. Toevoegen van de doelresourcegroep in de **resourcegroep** groeperen of klikt u op de vervolgkeuzelijst, selecteer een bestaande resource **Maak een nieuwe groep** optie.

  ![Bron maken](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. Na het toevoegen van de resourcegroep, bevestig **ik begrijp dat hulpmiddelen en scripts die zijn gekoppeld aan verplaatste resources niet werken totdat deze voor het gebruik van nieuwe resource-id's worden bijgewerkt** optie en klik vervolgens op **OK** om te voltooien het verplaatsen van de kluis.

  ![Bevestigingsbericht](./media/backup-azure-move-recovery-services/confirmation-message.png)


## <a name="use-azure-portal-to-move-a-recovery-services-vault-to-a-different-subscription"></a>Azure portal gebruiken voor het verplaatsen van een Recovery Services-kluis naar een ander abonnement

U kunt een Recovery Services-kluis en alle bijbehorende resources verplaatsen naar een ander abonnement

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Open de lijst met Recovery Services-kluizen en selecteer de kluis die u wilt verplaatsen. Wanneer het kluisdashboard wordt geopend, wordt deze weergegeven zoals in de volgende afbeelding wordt weergegeven.

    ![Open herstellen Services-kluis](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Als u niet ziet de **Essentials** informatie voor uw kluis, klikt u op het pictogram van de vervolgkeuzelijst. U ziet nu de Essentials-informatie voor uw kluis.

    ![Tabblad met essentiële informatie](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Klik in het menu overzicht-kluis op **wijzigen** naast **abonnement**, om te openen de **resources verplaatsen** blade.

  ![Abonnement wijzigen](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Selecteer de resources worden verplaatst, hier we raden u aan het gebruik van de **Alles selecteren** alle optionele bronnen in de lijst selecteren.

  ![resource verplaatsen](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Selecteer het doelabonnement uit de **abonnement** vervolgkeuzelijst, waar u de kluis worden verplaatst.
6. Toevoegen van de doelresourcegroep in de **resourcegroep** groeperen of klikt u op de vervolgkeuzelijst, selecteer een bestaande resource **Maak een nieuwe groep** optie.

  ![Abonnement toevoegen](./media/backup-azure-move-recovery-services/add-subscription.png)

7. Klik op **ik begrijp dat hulpmiddelen en scripts die zijn gekoppeld aan verplaatste resources niet werken totdat deze voor het gebruik van nieuwe resource-id's worden bijgewerkt** optie om te bevestigen en klik vervolgens op **OK**.

> [!NOTE]
> Cross-abonnement back-up (RS vault en beveiligde virtuele machines zijn in verschillende abonnementen) is geen ondersteund scenario. Ook opslagredundantie optie van lokaal redundante opslag (LRS) naar globale redundante opslag (GRS) en omgekeerd kan niet worden gewijzigd tijdens de verplaatsing van de kluis.
>
>

## <a name="use-powershell-to-move-a-vault"></a>PowerShell gebruiken voor het verplaatsen van een kluis

Als u een Recovery Services-kluis naar een andere resourcegroep, gebruikt u de `Move-AzureRMResource` cmdlet. `Move-AzureRMResource` vereist de resourcenaam en het type resource. U krijgt van de `Get-AzureRmRecoveryServicesVault` cmdlet.

```
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Om de resources verplaatsen naar een ander abonnement, bevatten de `-DestinationSubscriptionId` parameter.

```
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Na het uitvoeren van de bovenstaande cmdlets, wordt u gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources. Type **Y** om te bevestigen. Na een validatie is geslaagd, is de resource worden verplaatst.

## <a name="use-cli-to-move-a-vault"></a>CLI gebruiken voor het verplaatsen van een kluis

Om een Recovery Services-kluis naar een andere resourcegroep verplaatsen, gebruikt u de volgende cmdlet:

```
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

## <a name="post-migration"></a>Na de migratie

1. U moet de besturingselementen voor toegang voor de resourcegroepen set/controleren.  
2. De back-up reporting en de functie voor toepassingsbewaking moet opnieuw worden geconfigureerd voor de kluis na de verplaatsing is voltooid. De vorige configuratie worden verbroken tijdens de verplaatsing.



## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen.

Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
