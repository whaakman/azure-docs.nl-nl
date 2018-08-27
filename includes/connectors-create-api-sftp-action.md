Nu dat u een trigger hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om toe te voegen een de **SFTP - map uitpakken** actie. Deze actie wordt de inhoud van een bestand uitgepakt als de gedefinieerde voorwaarden wordt voldaan. 

Het configureren van deze actie, moet u de volgende informatie opgeven. U ziet dat het is eenvoudig te gebruiken die worden gegenereerd door de trigger wordt gebruikt als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

| SFTP - mapeigenschap extraheren | Beschrijving |
| --- | --- |
| Het pad naar het bronarchiefbestand |Dit is het pad voor het bestand worden geëxtraheerd. U kunt een van de tokens selecteren uit een eerdere actie of de SFTP-server om te vinden van het pad naar het bladeren. |
| Het pad naar de doelmap |Dit is het pad waar de uitgepakte bestanden wordt geplaatst. U kunt Selecteer een van de tokens in een eerdere actie als wanneer u het doelpad of de SFTP-server bladeren en selecteer een pad. |
| Overschrijven? |Hiermee wordt aangegeven als een bestand met dezelfde naam als het uitgepakte bestand is gevonden in het pad naar de doelmap als het bestaande bestand moet worden overschreven of niet. |

Aan de slag voor het toevoegen van de actie voor het uitpakken van de bestanden als de voorwaarde die u eerder hebt gedefinieerd, resulteert in *waar*. 

1. Selecteer **een actie toevoegen**.        
   ![Afbeelding 6 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-6.png)   
2. Selecteer de **SFTP - map uitpakken** actie      
   ![Afbeelding 7 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-7.png)   
3. Selecteer **archief pad naar het bronbestand**              
   ![Afbeelding 9 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-9.png)   
4. Selecteer de **bestandspad** token. Hiermee wordt aangegeven dat u het bestandspad van het bestand dat de trigger gevonden gebruikt als het bestandspad van de bron-archief.           
   ![Afbeelding 10 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-10.png)   
5. Selecteer **pad naar de doelmap**           
   ![Afbeelding 11 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-11.png)   
6. Selecteer de **bestandspad** token. Hiermee wordt aangegeven dat u gebruikt het bestandspad van het bestand dat de trigger gevonden als het doelpad voor de uitgepakte bestanden.   
7. Voer *\ExtractedFile* in de **pad naar de doelmap** besturingselement. Doe dit alleen na het token voor het pad van bestand in het besturingselement van het doel map pad.         
   ![Afbeelding 12 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-12.png)   
8. Voer *waar* in de **overschrijven?* besturingselement om aan te geven dat bestaande bestanden als ze dezelfde naam als de uitgepakte bestanden hebben moeten worden overschreven.      
   ![Afbeelding 13 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-13.png)   
9. Sla de wijzigingen in uw werkstroom  

