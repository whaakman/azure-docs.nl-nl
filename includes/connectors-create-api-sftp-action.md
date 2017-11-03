Nu dat u een trigger hebt toegevoegd, wordt het tijdstip waarop u iets moet doen met de gegevens die wordt gegenereerd door de trigger interessante. Volg deze stappen om toe te voegen een de **SFTP - extract map** in te grijpen. Deze actie wordt de inhoud van een bestand uitgepakt als aan de gedefinieerde voorwaarden wordt voldaan. 

Voor het configureren van deze actie, moet u de volgende informatie opgeven. U ziet dat het eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor enkele van de eigenschappen voor het nieuwe bestand is:

| SFTP - extract map eigenschap | Beschrijving |
| --- | --- |
| Bronbestandspad archief |Dit is het pad voor het bestand uitgepakt. U kunt een van de tokens selecteren uit een eerdere actie of de SFTP-server naar het pad naar het bladeren. |
| Pad naar de doelmap |Dit is het pad waar de uitgepakte bestanden wordt geplaatst. U kunt een van de tokens selecteren uit een eerdere actie als wanneer u het doelpad of bladeren door de SFTP-server en selecteer een pad. |
| Overschrijven? |Hiermee wordt aangegeven als een bestand met dezelfde naam als het uitgepakte bestand is gevonden in het doelpad van de map als het bestaande bestand moet worden overschreven of niet. |

Laten we beginnen met het toevoegen van de actie voor het uitpakken van de bestanden als de voorwaarde gedefinieerd eerder resulteert in *True*. 

1. Selecteer **een actie toevoegen**.        
   ![Afbeelding 6 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-6.png)   
2. Selecteer de **SFTP - Extract map** actie      
   ![Afbeelding 7 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-7.png)   
3. Selecteer **bronbestandspad archiveren**              
   ![Afbeelding 9 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-9.png)   
4. Selecteer de **bestandspad** token. Dit geeft aan dat u het bestandspad van het bestand dat de trigger gevonden gebruikt als het bronpad van het archief.           
   ![Afbeelding 10 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-10.png)   
5. Selecteer **pad naar de doelmap**           
   ![Afbeelding 11 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-11.png)   
6. Selecteer de **bestandspad** token. Dit geeft aan dat u gebruikt het bestandspad van het bestand dat de trigger gevonden als het doelpad voor de uitgepakte bestanden.   
7. Voer *\ExtractedFile* in de **pad naar de doelmap** besturingselement. Doe dit direct na het bestand pad-token in het besturingselement van het doel map pad.         
   ![Afbeelding 12 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-12.png)   
8. Voer *True* in de **overschrijven?* besturingselement om aan te geven dat bestaande bestanden die dezelfde naam hebben als de uitgepakte bestanden moeten worden overschreven.      
   ![Afbeelding 13 voorwaarde SFTP](./media/connectors-create-api-sftp/condition-13.png)   
9. Sla de wijzigingen in uw werkstroom  

