1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**. Kies **maken van nieuwe** en vervolgens **publiceren**. 

    ![Nieuwe functie-app maken publiceren](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Als u dit nog niet hebt Visual Studio al met uw Azure-account verbonden, selecteert u **account toevoegen...** .  

3. In het dialoogvenster **App-service maken** gebruikt u de **hosting**instellingen zoals opgegeven in de volgende tabel: 

    ![Lokale Azure-runtime](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app uniek wordt aangeduid. |
    | **Abonnement** | Kies uw abonnement | Het te gebruiken Azure-abonnement. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Naam van de resourcegroep waarin uw functie-app moet worden gemaakt. Kies **nieuw** om een nieuwe resourcegroep te maken.|
    | **[App-serviceabonnement](../articles/azure-functions/functions-scale.md)** | Verbruiksabonnement | Zorg dat u kiest de **verbruik** onder **grootte** nadat u op **nieuw** om een nieuw plan te maken. Ook kiezen een **locatie** in een [regio](https://azure.microsoft.com/regions/) in de buurt of in de buurt van andere services uw functies toegang hebben tot.  |

    >[!NOTE]
    >Een Azure storage-account is vereist door de runtime functies. Als gevolg hiervan een nieuw Azure Storage-account voor u gemaakt wanneer u een functie-app maakt.

4. Klik op **maken** voor het maken van een functie-app en verwante resources in Azure met deze instellingen en implementeren van uw project functiecode. 

5. Nadat de implementatie voltooid is, noteert u de **Site-URL** waarde, dit het adres van de functie-app in Azure is.

    ![Lokale Azure-runtime](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
