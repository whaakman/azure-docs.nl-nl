Nu dat u een trigger hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om toe te voegen de **SharePoint Online - bestand maken** actie. Deze actie maakt een bestand in SharePoint Online telkens wanneer de nieuwe item-trigger wordt geactiveerd. 

Het configureren van deze actie, moet u de volgende informatie opgeven. Als u deze informatie verstrekt, zult u merken dat het is eenvoudig te gebruiken die worden gegenereerd door de trigger wordt gebruikt als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

| Bestandseigenschap maken | Beschrijving |
| --- | --- |
| URL van de site |Dit is de URL van de SharePoint Online-site waar u het nieuwe bestand wilt maken. Selecteer de site in de lijst weergegeven. |
| Mappad |Dit is de map (in de URL van de Site in de vorige stap hebt geselecteerd) waar het nieuwe bestand wordt geplaatst. Zoek en selecteer de map. |
| Bestandsnaam |Dit is de naam van het bestand wordt gemaakt. |
| Bestandsinhoud |De inhoud die naar het bestand worden geschreven. |

1. Selecteer **+ nieuwe stap** om toe te voegen van de actie.  
   ![SharePoint online afbeelding 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Selecteer de **een actie toevoegen** koppeling. Deze wordt geopend de zoekvak waarin u voor elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld worden SharePoint-acties van belang zijn.    
   ![SharePoint online afbeelding 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Voer *sharepoint* om te zoeken naar acties die verband houden met SharePoint.
4. Selecteer **SharePoint Online - bestand maken** als de actie te ondernemen.   **Houd er rekening mee**: u wordt gevraagd om uw logische app toegang krijgt tot uw account voor SharePoint als u een verbinding met SharePoint Online eerder hebt gemaakt te autoriseren.    
   ![SharePoint online afbeelding 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. De **bestand maken** beheren wordt geopend.   
   ![SharePoint online afbeelding 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Selecteer **Site-URL** en blader naar de site waar u wilt maken van het bestand zoeken.     
   ![SharePoint online afbeelding 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Selecteer **mappad** en Ga naar de map waar het nieuwe bestand wordt geplaatst.  
   ![SharePoint online afbeelding 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Selecteer de **bestandsnaam** beheren en voer de naam van het bestand dat u wilt maken. Hier kunt u kunt de bestandsnaam rechtstreeks invoeren of u kunt een van de eigenschappen van de trigger die u eerder hebt gemaakt. U dit doen door de eigenschappen te selecteren in de lijst van **uitvoer wanneer een nieuw item wordt gemaakt**. Deze lijst wordt alleen weergegeven nadat u hebt geselecteerd de **bestandsnaam** besturingselement. In deze walkthough ik ID (de ID van het nieuwe lijstitem) hebt geselecteerd als de naam van het bestand wordt gemaakt door de **SharePoint Online - bestand maken** actie.    
   ![SharePoint online afbeelding 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Selecteer de **bestandsinhoud** beheren en voert u de inhoud die wordt geschreven naar het bestand dat wordt gemaakt. U ziet dat u een van de eigenschappen van de trigger die u eerder hebt gemaakt, kunt gebruiken voor de inhoud van het bestand. Selecteer alleen de eigenschappen in de lijst weergegeven. U kunt ook opgeven de **bestandsinhoud** tekst rechtstreeks in het besturingselement. In dit voorbeeld ik geselecteerde sommige eigenschappen en spaties en een koppelteken tussen elke eigenschap toegevoegd.        
   ![SharePoint online afbeelding 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Sla de wijzigingen in uw werkstroom  
11. Gefeliciteerd, u hebt nu een volledig functionele logische app die wordt geactiveerd wanneer een nieuw item wordt toegevoegd aan een SharePoint Online-lijst. De app maakt vervolgens een bestand, met enkele van de eigenschappen van het nieuwe lijstitem.  U kunt deze nu testen door het maken van een nieuw item in de SharePoint-lijst. 

