Hier wordt het gebruik van de **Service Bus - wanneer een bericht wordt ontvangen in een wachtrij** trigger een logic app-werkstroom starten wanneer een nieuw item naar een Service Bus-wachtrij wordt verzonden.  

> [!NOTE]
> U wordt gevraagd zich kunnen aanmelden met uw Service Bus-verbindingsreeks als u een verbinding met Service Bus nog geen hebt gemaakt.  
> 
> 

1. Voer in het zoekvak op de ontwerpfunctie van logic apps, **service bus**. Selecteer vervolgens de **Service Bus - wanneer een bericht wordt ontvangen in een wachtrij** trigger.  
   ![Service Bus triggerafbeelding 1](./media/connectors-create-api-servicebus/trigger-1.png)   
2. De **wanneer een bericht is ontvangen in een wachtrij** in het dialoogvenster wordt weergegeven.  
   ![Service Bus triggerafbeelding 2](./media/connectors-create-api-servicebus/trigger-2.png)   
3. Voer de naam van de Service Bus-wachtrij die u de trigger dat wilt voor het bewaken van.   
   ![Service Bus triggerafbeelding 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Op dit moment is uw logische app geconfigureerd met een trigger. Wanneer een nieuw item is ontvangen in de wachtrij die u hebt geselecteerd, kan de trigger een uitvoering van de andere triggers en acties in de werkstroom wordt gestart.    

