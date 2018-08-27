Nu dat u een voorwaarde hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om toe te voegen de **Salesforce - object ophalen** actie. Deze actie krijgt de gegevens telkens wanneer die een nieuwe lead is gemaakt. Ook voegt u een tweede actie die de gegevens uit het Salesforce - Get een object-actie een e-mail verzenden via de Office 365-connector wordt gebruikt.  

Het configureren van deze actie, moet u de volgende informatie opgeven. U ziet dat het is eenvoudig te gebruiken die worden gegenereerd door de trigger wordt gebruikt als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

| Bestandseigenschap maken | Beschrijving |
| --- | --- |
| Objecttype |Dit is het type Salesforce-object waarin u geïnteresseerd bent. Voorbeelden zijn Lead, Account, enzovoort. |
| Object-id |Hiermee wordt een id voor het object. |

1. Selecteer **een actie toevoegen** koppeling. Deze wordt geopend de zoekvak waarin u voor elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld worden Salesforce-acties van belang zijn.      
   ![SalesForce afbeelding 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Voer *salesforce* om te zoeken naar acties die verband houden met salesforce.
3. Selecteer **Salesforce - object ophalen** als de actie te ondernemen.   **Houd er rekening mee**: u wordt gevraagd om uw logische app toegang krijgt tot uw Salesforce-account als u niet hebt gedaan, eerder te autoriseren.    
   ![Afbeelding 2 SalesForce](./media/connectors-create-api-salesforce/action-2.png)    
4. De **object ophalen** beheren wordt geopend.  
5. Selecteer *leiden* als het objecttype.
6. Selecteer de **Object-ID** besturingselement.
7. Selecteer **...**  om uit te breiden de lijst met tokens die kunnen worden gebruikt als invoer voor acties.       
   ![Afbeelding 3 SalesForce](./media/connectors-create-api-salesforce/action-3.png)    
8. Selecteer **Lead-ID** beheren wordt geopend.   
   ![Afbeelding 4 SalesForce](./media/connectors-create-api-salesforce/action-4.png)     
9. U ziet dat de Lead-ID-token nu in het besturingselement voor Object-ID waarmee wordt aangegeven is dat de actie van Get-object wordt gezocht naar een potentiële klant met een ID die gelijk is aan de potentiële klant-ID van leads die deze logische app geactiveerd.  
   ![SalesForce afbeelding 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Sla uw werk op. Dat is alles, u hebt de Get-object-actie toegevoegd aan uw logische app. De controle-object moet er als volgt:    
    ![Afbeelding 6 SalesForce](./media/connectors-create-api-salesforce/action-6.png)  

Nu dat u een actie voor het ophalen van een lead hebt toegevoegd, kunt u iets interessants met de zojuist gemaakte lead doen. In een onderneming, kunt u een e-mail verzenden naar een distributielijst op op de hoogte stellen dat er een nieuwe lead is gemaakt. Laten we de Office 365-connector gebruiken voor het verzenden van een e-mailbericht met een aantal van de relevante gegevens van het nieuwe object lead in Salesforce.  

1. Selecteer **een actie toevoegen** Voer *e* in het besturingselement voor zoeken. Hiermee worden de acties die zijn gerelateerd aan het verzenden en ontvangen van e-mailbericht gefilterd.  
2. Selecteer de **Office 365 Outlook - een e-mail verzenden** item in de lijst. Als u al hebt gemaakt een *verbinding* naar uw Office 365-account, wordt u gevraagd uw Office 365-referenties voor het maken van deze nu in te voeren. Als u klaar bent, de **een e-mailbericht verzenden** beheren wordt geopend.        
   ![Afbeelding 7 SalesForce](./media/connectors-create-api-salesforce/action-7.png)  
3. Voer het e-mailadres dat u wilt verzenden van e-mailadres op in de **naar** besturingselement.
4. In de **onderwerp** toe, geef *nieuwe Lead is gemaakt* - en selecteer vervolgens de *bedrijf* token. Hiermee wordt weergegeven de *bedrijf* veld van de nieuwe lead in Salesforce wordt gemaakt.  
5. In de **hoofdtekst** besturingselement, kunt u een van de tokens selecteren van het nieuwe potentiële klant-object en u kunt ook opgeven welke tekst die u wilt weergeven in de hoofdtekst van het e-mailbericht. Hier volgt een voorbeeld:  
   ![Afbeelding 8 SalesForce](./media/connectors-create-api-salesforce/action-8.png)   
6. Sla uw werkstroom.  

Dat is alles. Uw logische app is nu voltooid.  

Nu u uw logische app kunt testen: Maak een nieuwe lead die voldoet aan de voorwaarde die u hebt gemaakt in Salesforce,.  Als u deze procedure volledig hebt gevolgd, moet u alleen een potentiële klant maken met een e-mailadres met *amazon.com* erin. Na enkele seconden uw logische app moet worden geactiveerd en de resultaten kunnen er ongeveer als volgt:  
![SalesForce afbeelding 9](./media/connectors-create-api-salesforce/action-9.png)  

