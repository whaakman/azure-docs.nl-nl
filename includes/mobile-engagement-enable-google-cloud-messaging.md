
###Met API-sleutel een Google Cloud Messaging-project maken

>[AZURE.NOTE] Voor deze procedure moet u een Google-account hebben met een bijbehorend e-mailadres dat is geverifieerd. Als u een nieuw Google-account wilt maken, gaat u naar <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.

1. Navigeer naar [Google Cloud Console](https://console.developers.google.com/project) en meld u aan met de referenties voor uw Google-account.

2. Ga naar **Alle projecten** en klik vervolgens op **Project maken**.

3. Voer een **Projectnaam** in en klik op **Maken**

4. Wanneer het project is gemaakt, moet u ervoor zorgen dat u het **Projectnummer** noteert. Dit is een lange numerieke waarde. U vindt dit nummer onder het gedeelte **IAM & Admin** (IAM en beheer) in de **Settings** (Instellingen) van uw project. U hebt dit nummer later nodig. 
 
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)

5. We maken nu een sleutel voor het Google Cloud Messaging Platform. De sleutel wordt gebruikt door ons platform om meldingen naar de Android-apparaten te verzenden. Ga naar het gedeelte **API Manager** (API-beheer) en klik op **Google Cloud Messaging** onder **Mobile API's** (Mobiele API’s). 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/gcm.png)

6. Klik op de volgende pagina op de knop **Enable** (Inschakelen). In het dashboard wordt u gevraagd referenties te maken. Klik dus op de knop **Go to Credentials** (Ga naar referenties). 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

6. Selecteer **Google Cloud Messaging** in de eerste vervolgkeuzelijst en **Web server** (Webserver) in de tweede waarde, en klik vervolgens op **What credentials do I need?** (Welke referenties heb ik nodig?)

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

7. Op de pagina **Referenties aan uw project toevoegen** klikt u op **API-sleutel maken**.

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)

8. Noteer de waarde van de **API-SLEUTEL**. Deze API-sleutelwaarde gaat u later gebruiken voor de configuratie in de sectie Native pushbericht. Klik nu op **Gereed**.



<!--HONumber=Aug16_HO4-->


