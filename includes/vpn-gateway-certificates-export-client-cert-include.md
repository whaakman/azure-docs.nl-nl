Wanneer u een clientcertificaat genereert, wordt het automatisch geïnstalleerd op de computer die u hebt gebruikt voor het genereren ervan. Als u het clientcertificaat installeren op een andere clientcomputer wilt, moet u het exporteren van het clientcertificaat dat u hebt gegenereerd.

1. Als u een clientcertificaat wilt exporteren, opent u **Gebruikerscertificaten beheren**. De clientcertificaten die u hebt gegenereerd, standaard, staan in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten'. Met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klikt u op **alle taken**, en klik vervolgens op **exporteren** openen de **Wizard Certificaat exporteren**.

  ![Exporteren](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. Klik in de Wizard Certificaat exporteren op **volgende** om door te gaan.

  ![Volgende](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Selecteer **Ja, de persoonlijke sleutel exporteren**, en klik vervolgens op **volgende**.

  ![persoonlijke sleutel exporteren](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. Laat op de pagina **Bestandsindeling voor export** de standaardinstellingen geselecteerd. Zorg ervoor dat **en mogelijk alle certificaten in het certificeringspad opnemen** is geselecteerd. Deze instelling wordt bovendien de certificaatgegevens basiscertificaat dat is vereist voor geslaagde clientverificatie exporteert. Zonder dit mislukt clientverificatie, omdat de client geen het vertrouwde basiscertificaat. Klik op **Volgende**.

  ![bestandsindeling exporteren](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. Op de pagina **Beveiliging** moet u de persoonlijke sleutel beveiligen. Als u ervoor kiest om een wachtwoord te gebruiken, is het belangrijk dat u het wachtwoord voor dit certificaat ergens noteert of onthoudt. Klik op **Volgende**.

  ![security](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.

  ![te exporteren bestand](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Klik op **Voltooien** om het certificaat te exporteren.

  ![voltooien](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)