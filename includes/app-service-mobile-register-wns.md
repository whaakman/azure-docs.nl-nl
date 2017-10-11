
1. In Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project en klik op **Store** > **App aan de Store koppelen**.

    ![App aan Windows Store koppelen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Klik in de wizard op **volgende**, en meld u aan met uw Microsoft-account. Typ een naam voor uw app in **een nieuwe appnaam reserveren**, en klik vervolgens op **Reserve**.
3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de nieuwe app, klikt u op **volgende**, en klik vervolgens op **koppelen**. Hierdoor worden de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.
4. Herhaal stap 1 en 3 voor de Windows Phone Store-app-project met behulp van de registratie van de dezelfde die u eerder hebt gemaakt voor de Windows Store-app.  
5. Blader naar de [Windows-ontwikkelaarscentrum](https://dev.windows.com/en-us/overview), en meld u aan met uw Microsoft-account. Klik op de nieuwe app-registratie in **mijn apps**, en vouw vervolgens **Services** > **Pushmeldingen**.
6. Op de **Pushmeldingen** pagina, klikt u op **Live Services site** onder **Windows Push Notification Services (WNS) en Microsoft Azure Mobile Apps**. Noteer de waarden van de **pakket-SID** en de *huidige* waarde in **Toepassingsgeheim**. 

    ![App-instelling in het developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Het toepassingsgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Deel deze waarden met niemand en distribueer ze niet met uw app.
   >
   >
