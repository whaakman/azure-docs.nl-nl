
1. In Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project. Selecteer vervolgens **Store** > **App aan de Store koppelen**.

    ![App koppelen met Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Selecteer in de wizard **volgende**. Meld u vervolgens aan met uw Microsoft-account. In **een nieuwe appnaam reserveren**, typ een naam voor uw app en selecteer vervolgens **Reserve**.
3. Nadat de app-registratie is gemaakt, selecteert u de naam van de nieuwe app. Selecteer **volgende**, en selecteer vervolgens **koppelen**. Dit proces worden de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.
4. Herhaal stappen 1 en 3 voor de Windows Phone Store-app-project met behulp van de registratie van de dezelfde die u eerder hebt gemaakt voor de Windows Store-app.  
5. Ga naar de [Windows Dev Center](https://dev.windows.com/en-us/overview), en meld u aan met uw Microsoft-account. In **mijn apps**, selecteert u de nieuwe app-registratie. Vouw vervolgens **Services** > **Pushmeldingen**.
6. Op de **Pushmeldingen** pagina onder **Windows Push Notification Services (WNS) en Microsoft Azure Mobile Apps**, selecteer **Live Services site**.  Noteer de waarden van de **pakket-SID** en de *huidige* waarde in de **Toepassingsgeheim**. 

    ![App-instelling in het ontwikkelaarscentrum](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Het toepassingsgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Niet deel van deze waarden met niemand en distribueer ze met uw app.
   >
   >
