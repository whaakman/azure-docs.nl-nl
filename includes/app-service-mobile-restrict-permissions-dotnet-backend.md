
Standaard worden-API's van een back-end van Mobile Apps anoniem aangeroepen. Vervolgens moet u de toegang beperken tot alleen geverifieerde clients.  

* **Node.js-back-end (via de Azure-portal)** :  

    Klik in de instellingen voor mobiele Apps op **gemakkelijk tabellen** en selecteer uw tabel. Klik op **wijzigingsmachtigingen**, selecteer **geverifieerde toegang alleen** voor alle machtigingen en klik vervolgens op **opslaan**.
* **.NET back-end (C#)**:  

    Navigeer in het serverproject naar **domeincontrollers** > **TodoItemController.cs**. Voeg de `[Authorize]` kenmerk de **TodoItemController** klasse als volgt. U kunt ook dit kenmerk alleen op deze methoden in plaats van de klasse om de toegang beperken tot alleen specifieke methoden, toepassen. Het serverproject publiceren.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js-back-end (via Node.js-code)** :  

    Om verificatie vereist voor toegang tot tabellen, moet u de volgende regel toevoegen aan het script Node.js-server:

        table.access = 'authenticated';

    Zie voor meer informatie [procedure: verificatie vereisen voor toegang tot tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zie voor meer informatie over het downloaden van het snelstartproject dat code van uw site, [hoe: downloaden van de Node.js-back-end Quick Start CodeProject met Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
