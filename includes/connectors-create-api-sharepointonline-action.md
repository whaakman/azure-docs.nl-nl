Nu dat u een trigger hebt toegevoegd, wordt het tijdstip waarop u iets moet doen met de gegevens die wordt gegenereerd door de trigger interessante. Volg deze stappen om toe te voegen de **SharePoint Online - bestand maken** in te grijpen. Deze actie wordt een bestand worden maken in SharePoint Online telkens wanneer de nieuwe item trigger wordt geactiveerd. 

Voor het configureren van deze actie, moet u de volgende informatie opgeven. Als u deze informatie opgeeft, zult u zien dat het eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor enkele van de eigenschappen voor het nieuwe bestand is:

| Bestandseigenschap maken | Beschrijving |
| --- | --- |
| Site-URL |Dit is de URL van de SharePoint Online-site waar u het nieuwe bestand te maken. Selecteer de site uit de lijst weergegeven. |
| Pad naar de map |Dit is de map (in de URL van de Site die is geselecteerd in de vorige stap) waar het nieuwe bestand wordt geplaatst. Zoeken naar en selecteer de map. |
| Bestandsnaam |Dit is de naam van het bestand wordt gemaakt. |
| Bestandsinhoud |De inhoud die naar het bestand worden geschreven. |

1. Selecteer **+ een nieuwe stap** de actie toevoegen.  
   ![SharePoint online actie afbeelding 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Selecteer de **een actie toevoegen** koppeling. Deze wordt geopend het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld zijn de SharePoint-acties van belang.    
   ![SharePoint online afbeelding 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Voer *sharepoint* om te zoeken naar acties met betrekking tot SharePoint.
4. Selecteer **SharePoint Online - bestand maken** als de actie te ondernemen.   **Opmerking**: u wordt gevraagd voor het autoriseren van uw logische app toegang tot uw SharePoint-account als u hebt een verbinding met SharePoint Online niet eerder hebt gemaakt.    
   ![SharePoint online afbeelding 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. De **Create file** beheren wordt geopend.   
   ![SharePoint online afbeelding 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Selecteer **Site-URL** en blader naar de site waarin u wilt maken van het bestand zoeken.     
   ![SharePoint online afbeelding 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Selecteer **mappad** en blader naar de map waar het nieuwe bestand wordt geplaatst.  
   ![SharePoint online afbeelding 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Selecteer de **bestandsnaam** beheren en voer de naam van het bestand dat u wilt maken. Hier kunt u kunt de bestandsnaam rechtstreeks invoeren of u kunt een van de eigenschappen van de trigger die u eerder hebt gemaakt. U dit doen door de eigenschappen te selecteren in de lijst met **uitvoer wanneer een nieuw item wordt gemaakt**. Deze lijst wordt alleen weergegeven nadat u hebt geselecteerd de **bestandsnaam** besturingselement. In deze walkthough ik ID (ID van het nieuwe lijstitem) geselecteerd als de naam van het bestand wordt gemaakt door de **SharePoint Online - bestand maken** in te grijpen.    
   ![SharePoint online afbeelding 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Selecteer de **inhoud bestand** beheren en voert u de inhoud die wordt geschreven naar het bestand dat wordt gemaakt. U ziet dat u kunt een van de eigenschappen van de trigger die u eerder hebt gemaakt voor de inhoud van het bestand. Selecteer alleen de eigenschappen in de lijst weergegeven. U kunt ook kunt u de **inhoud bestand** tekst rechtstreeks in het besturingselement. In dit voorbeeld ik geselecteerde sommige eigenschappen en spaties en een koppelteken tussen elke eigenschap toegevoegd.        
   ![SharePoint online afbeelding 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Sla de wijzigingen in uw werkstroom  
11. Gefeliciteerd, u hebt nu een volledig functionele logische app die wordt geactiveerd wanneer een nieuw item wordt toegevoegd aan een lijst met SharePoint Online. De app maakt vervolgens een bestand, gebruikmakend van de eigenschappen van het nieuwe lijstitem.  U kunt het nu testen door het maken van een nieuw item in de SharePoint-lijst. 

