1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**. Kies **Nieuw maken** en klik vervolgens op **Publiceren**. 

    ![Nieuwe functie-app maken publiceren](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Als u Visual Studio nog niet aan uw Azure-account hebt gekoppeld, klikt u op **Een account toevoegen...**.  

3. In het dialoogvenster **App-service maken** gebruikt u de **hosting**instellingen zoals opgegeven in de volgende tabel: 

    ![Lokale Azure-runtime](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app uniek wordt aangeduid. |
    | **Abonnement** | Kies uw abonnement | Het te gebruiken Azure-abonnement. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Naam van de resourcegroep waarin uw functie-app moet worden gemaakt. |
    | **[App-serviceabonnement](../articles/azure-functions/functions-scale.md)** | Verbruiksabonnement | Zorg dat u het **Verbruik** kiest onder **Grootte** wanneer u een nieuw abonnement maakt.  |
    | **[Opslagaccount](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** | Wereldwijd unieke naam | Gebruik een bestaand opslagaccount of maak een nieuw.   |

4. Klik op **Maken** om in Azure een functie-app met deze instellingen te maken. Noteer nadat het inrichtingsproces is voltooid de **Site-URL**. Dit is het adres van de functie-app in Azure. 

    ![Lokale Azure-runtime](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
