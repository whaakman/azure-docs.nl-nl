
1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project. Selecteer vervolgens **Store** > **App aan de Store koppelen**.

    ![App aan Windows Store koppelen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Selecteer in de wizard **volgende**. Vervolgens kunt u aanmelden met je Microsoft-account. In **een nieuwe appnaam reserveren**, typ een naam voor uw app en selecteer vervolgens **Reserve**.
3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de nieuwe app. Selecteer **volgende**, en selecteer vervolgens **koppelen**. Dit proces wordt de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.
4. Herhaal stap 1 en 3 voor de Windows Phone Store-app-project met behulp van de registratie van de dezelfde die u eerder hebt gemaakt voor de Windows Store-app.  
5. Ga naar de [Windows-ontwikkelaarscentrum](https://dev.windows.com/en-us/overview), en meld u aan met uw Microsoft-account. In **mijn apps**, selecteert u de registratie van de nieuwe app. Vouw **Services** > **Pushmeldingen**.
6. Op de **Pushmeldingen** pagina onder **Windows Push Notification Services (WNS) en Microsoft Azure Mobile Apps**, selecteer **Live Services site**.  Noteer de waarden van de **pakket-SID** en de *huidige* waarde in **Toepassingsgeheim**. 

    ![App-instelling in het developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Het toepassingsgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Niet deel van deze waarden met niemand en distribueer ze met uw app.
   >
   >
