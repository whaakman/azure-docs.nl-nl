## Een Event Hub maken
1. Meld u aan bij [Azure Portal][Azure Portal] en klik op **Nieuw** linksboven in het scherm.
2. Klik op **Gegevens en analyses** en klik vervolgens op **Event Hubs**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)
3. Voer op de blade **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)
4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic of Standard). Kies ook een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. 
5. Klik op **Maken** om de naamruimte te maken.
6. Klik in de lijst met naamruimten van Event Hubs op de zojuist gemaakte naamruimte.      
   
    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)
7. Klik op de blade Naamruimte op **Event Hubs**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)
8. Klik op **Event Hub toevoegen** boven aan de blade.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)
9. Typ een naam voor uw Event Hub en klik vervolgens op **Maken**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)
10. Klik in de lijst met Event Hubs op de naam van de zojuist gemaakte Event Hub. 
    
     ![](./media/event-hubs-create-event-hub/create-event-hub6.png)
11. Klik op de blade Naamruimte (niet de specifieke Event Hub-blade) op **Gedeeld toegangsbeleid** en klik vervolgens op **RootManageSharedAccessKey**.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub7.png)
12. Klik op de knop voor kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord te kopiëren. Sla deze verbindingsreeks op zodat u er verderop in de zelfstudie gebruik van kunt maken.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Uw Event Hub is nu gemaakt en u hebt de verbindingsreeksen die u nodig hebt om gebeurtenissen te kunnen verzenden en ontvangen.

[Azure Portal]: https://portal.azure.com/

<!--HONumber=Sep16_HO3-->


