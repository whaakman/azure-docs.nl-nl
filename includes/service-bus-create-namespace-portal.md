1. Meld u aan bij de klassieke Azure-portal.

2. Klik in het linkernavigatievenster van de portal op **Service Bus**.

3. Klik in het onderste deelvenster van de portal op **Maken**.

    ![Selecteer Maken][select-create]
   
4. Voer in het dialoogvenster **Een nieuwe naamruimte toevoegen** een naamruimtenaam in. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.

    ![Naam van naamruimte][namespace-name]
  
5. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u het land of regio waarin uw naamruimte moet worden gehost.

6. Laat de standaardwaarden van de andere velden in het dialoogvenster ongewijzigd (**Berichten**- en **Standard**-laag) en klik vervolgens op het vinkje voor OK. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.
 
    ![Is gemaakt][created-successfully]

###De referenties ophalen
1. Klik in het linkernavigatievenster op het knooppunt **Service Bus** om de lijst met beschikbare naamruimten weer te geven:
 
    ![Service Bus selecteren][select-service-bus]
  
2. Selecteer de naamruimte die u zojuist hebt gemaakt in de lijst die wordt weergegeven:
 
    ![Naamruimte selecteren][select-namespace]
 
3. Klik op **Verbindingsgegevens**.

    ![Verbindingsgegevens][connection-information]
  
4. Zoek in het deelvenster **Toegang tot verbindingsgegevens** de verbindingsreeks die de SAS-sleutel en de naam van de sleutel bevat.

    ![Toegang tot verbindingsgegevens][access-connection-information]
  
5. Noteer de sleutel of kopieer de sleutel naar het Klembord.

<!--Image references-->

[select-create]: ./media/service-bus-create-namespace-portal/select-create.png
[namespace-name]: ./media/service-bus-create-namespace-portal/namespace-name.png
[created-successfully]: ./media/service-bus-create-namespace-portal/created-successfully.png
[select-service-bus]: ./media/service-bus-create-namespace-portal/select-service-bus.png
[select-namespace]: ./media/service-bus-create-namespace-portal/select-namespace.png
[connection-information]: ./media/service-bus-create-namespace-portal/connection-information.png
[access-connection-information]: ./media/service-bus-create-namespace-portal/access-connection-information.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[classic-portal]: https://manage.windowsazure.com


<!--HONumber=ago16_HO4-->


