Nu dat u een voorwaarde hebt toegevoegd, wordt het tijdstip waarop u iets moet doen met de gegevens die wordt gegenereerd door de trigger interessante. Volg deze stappen om toe te voegen de **Salesforce - Get-object** in te grijpen. Deze actie worden de gegevens ophalen telkens wanneer die een nieuwe lead wordt gemaakt. Ook voegt u een tweede actie die de gegevens uit het Salesforce - Get een actie van het object voor het verzenden van een e-mailbericht met de Office 365-connector wilt gebruiken.  

Voor het configureren van deze actie, moet u de volgende informatie opgeven. U ziet dat het eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor enkele van de eigenschappen voor het nieuwe bestand is:

| Bestandseigenschap maken | Beschrijving |
| --- | --- |
| Objecttype |Dit is het type u geïnteresseerd bent in Salesforce-object. Voorbeelden zijn Lead, Account, enzovoort. |
| object-ID |Hiermee wordt een id voor het object. |

1. Selecteer **een actie toevoegen** koppeling. Deze wordt geopend het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld zijn de Salesforce-acties van belang.      
   ![SalesForce afbeelding 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Voer *salesforce* om te zoeken naar acties met betrekking tot salesforce.
3. Selecteer **Salesforce - Get-object** als de actie te ondernemen.   **Opmerking**: u wordt gevraagd uw logische app toegang tot uw Salesforce-account als u nog niet gedaan eerder autoriseren.    
   ![SalesForce afbeelding 2](./media/connectors-create-api-salesforce/action-2.png)    
4. De **Get object** beheren wordt geopend.  
5. Selecteer *leiden* als het objecttype.
6. Selecteer de **Object-ID** besturingselement.
7. Selecteer **...**  uitbreiden van de lijst met tokens die worden gebruikt als invoer voor acties.       
   ![SalesForce afbeelding 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Selecteer **Lead-ID** beheren wordt geopend.   
   ![SalesForce afbeelding 4](./media/connectors-create-api-salesforce/action-4.png)     
9. U ziet dat het token ID leiden nu in het besturingselement voor Object-ID die aangeeft is dat de actie Get-object wordt gezocht naar een lead met een ID die gelijk is aan de lead-ID van potentiële klanten die deze logische app heeft geactiveerd.  
   ![SalesForce afbeelding 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Sla uw werk op. Dat is alles, hebt u de actie Get-object toegevoegd aan uw logische app. Het besturingselement met Get-object ziet er als volgt:    
    ![SalesForce afbeelding 6](./media/connectors-create-api-salesforce/action-6.png)  

Nu dat u een actie voor het ophalen van een lead hebt toegevoegd, wilt u mogelijk iets aan de zojuist gemaakte lead interessante doen. Mogelijk wilt u een e-mail sturen naar een distributielijst met de melding dat er een nieuwe lead is gemaakt in een onderneming. Laten we de Office 365-connector gebruiken voor het verzenden van een e-mailbericht met enkele van de relevante informatie uit het nieuwe object lead in Salesforce.  

1. Selecteer **een actie toevoegen** Voer *e* in het besturingselement zoeken. Dit filtert de acties die zijn gerelateerd aan verzenden en ontvangen van e-mail.  
2. Selecteer de **Office 365 Outlook - stuurt u een e-mailadres** item in de lijst. Als u al hebt gemaakt een *verbinding* op uw Office 365-account, wordt u gevraagd de referenties van uw Office 365 om deze map nu maken in te voeren. Nadat u klaar bent, de **e-mailbericht verzenden** beheren wordt geopend.        
   ![SalesForce afbeelding 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Voer het e-mailadres dat u wilt verzenden van e-mail aan in de **naar** besturingselement.
4. In de **onderwerp** beheren, voert u *nieuwe leiden gemaakt* - selecteert u vervolgens de *bedrijf* token. Hiermee wordt weergegeven de *bedrijf* veld van de nieuwe lead gemaakt in Salesforce.  
5. In de **hoofdtekst** beheer, kunt u een van de tokens van het nieuwe lead object selecteren en u kunt ook opgeven welke tekst die u wilt weergeven in de hoofdtekst van het e-mailbericht. Hier volgt een voorbeeld:  
   ![SalesForce afbeelding 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Sla uw werkstroom.  

Dat is alles. Uw logische app is nu voltooid.  

U kunt nu uw logische app testen: in Salesforce, maakt u een nieuwe lead die voldoet aan de voorwaarde die u hebt gemaakt.  Als u deze procedure volledig hebt gevolgd, moet u net een lead maken met een e-mailadres met *amazon.com* erin. Na een paar seconden uw logische app moet worden gestart en de resultaten zien er ongeveer als volgt:  
![SalesForce afbeelding 9](./media/connectors-create-api-salesforce/action-9.png)  

