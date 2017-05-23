1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

1. Klik op **Compute** > **Functie-app**, selecteer uw **Abonnement**. Vervolgens gebruikt u de instellingen voor de functie-app zoals in de tabel wordt vermeld.

    ![Functie-app maken in Azure Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. | 
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. | 
    | **[Hostingplan](../articles/azure-functions/functions-scale.md)** |   Verbruiksabonnement | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. In het standaard **Verbruiksabonnement** worden resources dynamisch toegevoegd zoals door uw functies wordt vereist. U betaalt alleen voor de tijd die uw functies worden uitgevoerd.   |
    | **Locatie** | West-Europa | Kies een locatie in de buurt of in de buurt van andere services die door uw functies worden gebruikt. |
    | **[Opslagaccount](../articles/storage/storage-create-storage-account.md#create-a-storage-account)** |  Wereldwijd unieke naam |  Naam van het nieuwe opslagaccount dat door uw functie-app wordt gebruikt. U kunt ook een bestaand account gebruiken. |

1. Klik op **Maken** om de nieuwe functie-app in te richten en te implementeren.