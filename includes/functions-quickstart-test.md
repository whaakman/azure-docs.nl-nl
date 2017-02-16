
Omdat de Azure Functions-snelstartgidsen functionele code bevatten, kunt u uw nieuwe functie meteen testen.

1. Ga naar het tabblad **Develop** en bekijk het venster **Code**. U zult zien dat deze Node.js-code een HTTP-aanvraag verwacht met een waarde *name* die wordt doorgegeven in de berichttekst of in een queryreeks. Wanneer de functie wordt uitgevoerd, wordt deze waarde in het antwoordbericht geretourneerd.
   
2. Klik op **Testen** om het geïntegreerde deelvenster HTTP-testaanvraag voor de functie te openen.
 
    ![](./media/functions-quickstart-test/function-app-develop-tab-testing.png)

3. Ga naar het tekstvak **Aanvraagtekst**, wijzig de waarde van de eigenschap *naam* in uw naam en klik op **Uitvoeren**. U ziet dat de uitvoering wordt geactiveerd door een HTTP-testaanvraag, dat informatie naar de logboeken wordt geschreven en dat het antwoord 'hallo' wordt weergegeven in de **uitvoer**. 

4. Als u de uitvoering van dezelfde functie wilt activeren vanuit een ander HTTP-testprogramma of via een ander browservenster, kopieert u de waarde **Functie-URL** op het tabblad **Ontwikkelen** en plakt u deze in de adresbalk van het programma of de browser. Voeg de queryreekswaarde `&name=yourname` toe aan de URL en voer de aanvraag uit. Dezelfde informatie wordt naar de logboeken geschreven en dezelfde tekenreeks wordt gebruikt in de hoofdtekst van het antwoordbericht.

    ![](./media/functions-quickstart-test/function-app-browser-testing.png)

<!--HONumber=Feb17_HO1-->


