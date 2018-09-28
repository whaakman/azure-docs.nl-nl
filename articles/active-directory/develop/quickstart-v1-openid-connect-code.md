---
title: Een Node.js Express-web-app bouwen voor aanmelden en afmelden met Azure Active Directory | Microsoft Docs
description: Meer informatie over het bouwen van een Node.js Express MVC-web-app die is geïntegreerd met Azure AD voor aanmelden.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 19563f76c261fda1fca53babcb553f2dceeaa345
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990090"
---
# <a name="quickstart-build-a-nodejs-express-web-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Snelstart: Een Node.js Express-web-app bouwen voor aanmelden en afmelden met Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Passport is verificatiemiddleware voor Node.js. Passport is flexibel en modulair, en u kunt het onopvallend installeren in elke Express- of Restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie die gebruikmaakt van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. Voor Azure Active Directory (Azure AD) installeert u deze module en voegt u de Azure AD-invoegtoepassing `passport-azure-ad` toe.

In deze snelstart leert u hoe u Passpart gebruikt om het volgende te doen:

* De gebruiker aanmelden bij de app met behulp van Azure AD.
* Meer informatie weergeven over de gebruiker.
* De gebruiker afmelden voor de app.

Om de volledige, werkende toepassing te bouwen, moet u het volgende doen:

1. Een app registreren.
2. De app instellen om de strategie `passport-azure-ad` te gebruiken.
3. U gebruikt Passport om aan- en afmeldingsaanvragen te verzenden naar Azure AD.
4. Gegevens over de gebruiker afdrukken.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u dat u aan deze vereisten voldoet:

* [het raamwerk van de app downloaden als een ZIP-bestand](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip)
  
    * U kloont het raamwerk als volgt:

        ```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

    De code voor deze snelstart wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS). De voltooide toepassing wordt eveneens verstrekt aan het einde van deze snelstart.

* Zorg voor een Azure AD-tenant waarin u gebruikers kunt maken en een toepassing kunt registreren. Als u nog geen tenant hebt, vindt u [hier informatie over het verkrijgen van een tenant](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-app"></a>Stap 1: Een app registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account in het menu bovenaan de pagina. Kies onder **Directory** de Active Directory-tenant waar u uw toepassing wilt registreren.
1. Selecteer **Alle services** in het menu aan de linkerkant van het scherm en selecteer vervolgens **Azure Active Directory**.
1. Selecteer **App-registraties** en kies **Toevoegen**.
1. Volg de aanwijzingen voor het maken van een **Webtoepassing** en/of **WebAPI**.

    * De **Naam** van de toepassing beschrijft de toepassing voor gebruikers.
    * De **Aanmeldings-URL** is de basis-URL van uw app. De standaardwaarde van het raamwerk is `http://localhost:3000/auth/openid/return`.

1. Na de registratie wijst Azure AD een unieke toepassings-id toe aan uw app. U hebt deze waarde nodig in de volgende secties. Kopieer deze waarde daarom vanaf de toepassingspagina.
1. Op de pagina **Instellingen > Eigenschappen** van uw toepassing werkt u de URI voor de app-id bij. 
    
    De **URI voor de app-id** is een unieke id voor uw toepassing. De overeengekomen opmaak is `https://<tenant-domain>/<app-name>`, bijvoorbeeld: `https://contoso.onmicrosoft.com/my-first-aad-app`.

1. Op de pagina **Instellingen > Antwoord-URL's** voor uw toepassing voegt u de URL toe die is toegevoegd in de aanmeldings-URL uit stap 5 en selecteert u **Opslaan**.
1. Om een geheime sleutel te maken, volgt u stap 4 in [Toepassingsreferenties of -machtigingen toevoegen voor toegang tot web-API's](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis).

   > [!IMPORTANT]
   > Kopieer de waarde van de toepassingssleutel. Dit is de waarde voor het `clientSecret`, die u nodig hebt voor **stap 3** hieronder. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>Stap 2: Vereisten aan de directory toevoegen

1. Wijzig vanaf de opdrachtregel de directory's in de hoofdmap, als u hier nog niet bent en voer daarna de volgende opdrachten uit:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

1. U hebt ook `passport-azure-ad` nodig. Voer daarom de volgende opdracht uit:

    * `npm install passport-azure-ad`

Hiermee worden de bibliotheken geïnstalleerd waarvan `passport-azure-ad` afhankelijk is.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Stap 3: Uw app instellen voor gebruik van de strategie Passport-Node.js

Hier configureert u Express voor gebruik van het OpenID Connect-verificatieprotocol. Passport wordt gebruikt voor verschillende zaken, waaronder de uitgifte van aan- en afmeldingsaanvragen, het beheer van de gebruikerssessie en het verkrijgen van informatie over de gebruiker.

1. Open het bestand `config.js` in de hoofdmap van het project en voer de configuratiewaarden van de app in de sectie `exports.creds` in.

    * De `clientID` is de **Toepassings-id** die in de registratieportal is toegewezen aan uw app.
    * De `returnURL` is de **Antwoord-URL** die in de portal is ingevoerd.
    * Het `clientSecret` is het geheim dat u in de portal hebt gegenereerd.

1. Open daarna het bestand `app.js` in de hoofdmap van het project. Voeg vervolgens de volgende aanroep toe om de strategie `OIDCStrategy` aan te roepen die is meegeleverd met `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

1. Gebruik daarna de strategie waarnaar we zojuist hebben verwezen om aanmeldingsaanvragen af te handelen.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
   In Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort) waaraan alle schrijvers van strategieën zich houden. Wanneer u de strategie bekijkt, ziet u dat er een functie aan wordt doorgegeven die een token en een 'done' als parameters heeft. De strategie komt weer bij ons terug nadat deze zijn werk heeft gedaan. Daarna willen we de gebruiker en het token opslaan zodat we daar niet opnieuw om hoeven te vragen.

   > [!IMPORTANT]
   > In de voorgaande code wordt elke gebruiker gebruikt die zich bij de server probeert te verifiëren. Dit wordt automatische registratie genoemd. Het is raadzaam dat iedereen die zich bij een productieserver wil verifiëren zich eerst moet registreren via een proces dat u bepaalt. Dit is doorgaans het patroon dat u ziet in consumenten-apps waarbij u zich kunt registreren via Facebook, maar waarvoor u vervolgens aanvullende informatie moet verstrekken. Als dit geen voorbeeldtoepassing zou zijn, zouden we het e-mailadres van de gebruiker kunnen extraheren uit het tokenobject dat wordt geretourneerd en vervolgens de gebruiker vragen om aanvullende informatie in te vullen. Omdat dit een testserver is, voegen we deze toe aan de database in het geheugen.

1. Voeg de methoden toe waarmee we de aangemelde gebruikers kunnen bijhouden, zoals is vereist door Passport. Deze methoden omvatten het serialiseren en deserialiseren van de informatie over de gebruiker.

    ```JavaScript
    // Passport session setup. (Section 2)

    //   To support persistent sign-in sessions, Passport needs to be able to
    //   serialize users into the session and deserialize them out of the session. Typically,
    //   this is done simply by storing the user ID when serializing and finding
    //   the user by ID when deserializing.
    passport.serializeUser(function(user, done) {
        done(null, user.email);
    });

    passport.deserializeUser(function(id, done) {
        findByEmail(id, function (err, user) {
            done(err, user);
        });
    });

    // array to hold signed-in users
    var users = [];

    var findByEmail = function(email, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            log.info('we are using user: ', user);
            if (user.email === email) {
                return fn(null, user);
            }
        }
        return fn(null, null);
    };
    ```

1. Voeg de code voor het laden van de Express-engine toe. Hier gebruiken we het standaardpatroon /views en /routes van Express.

    ```JavaScript
    // configure Express (section 2)

      var app = express();
      app.configure(function() {
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.logger());
      app.use(express.methodOverride());
      app.use(cookieParser());
      app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
      app.use(bodyParser.urlencoded({ extended : true }));
      // Initialize Passport!  Also use passport.session() middleware, to support
      // persistent login sessions (recommended).
      app.use(passport.initialize());
      app.use(passport.session());
      app.use(app.router);
      app.use(express.static(__dirname + '/../../public'));
    });
    ```

1. Voeg tot slot de routes toe waarmee de daadwerkelijke aanmeldingsaanvragen worden afgeleverd bij de `passport-azure-ad`-engine:

    ```JavaScript
    // Our Auth routes (section 3)

    // GET /auth/openid
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. The first step in OpenID authentication involves redirecting
    //   the user to their OpenID provider. After authenticating, the OpenID
    //   provider redirects the user back to this application at
    //   /auth/openid/return.
    app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
        log.info('Authentication was called in the Sample');
        res.redirect('/');
    });

    // GET /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.get('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });

    // POST /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.post('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });
    ```

## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Stap 4: Passport gebruiken om aan- en afmeldingsaanvragen te verzenden naar Azure AD

Uw app is nu correct geconfigureerd voor communicatie met het eindpunt met behulp van het OpenID Connect-verificatieprotocol. `passport-azure-ad` heeft gezorgd voor alle details van het samenstellen van verificatieberichten, het valideren van tokens van Azure AD en het beheren van gebruikerssessies. U hoeft uw gebruikers alleen nog een manier te bieden waarop ze zich kunnen aan- en afmelden, en aanvullende informatie te verzamelen over de aangemelde gebruikers.

1. Voeg de standaardmethode en de aanmeldings-, account- en afmeldingsmethoden toe aan uw `app.js`-bestand:

    ```JavaScript
    //Routes (section 4)

    app.get('/', function(req, res){
      res.render('index', { user: req.user });
    });

    app.get('/account', ensureAuthenticated, function(req, res){
      res.render('account', { user: req.user });
    });

    app.get('/login',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('Login was called in the Sample');
        res.redirect('/');
    });

    app.get('/logout', function(req, res){
      req.logout();
      res.redirect('/');
    });
    ```

1. Bekijk deze uitvoerig:

    * De route `/` leidt om naar de weergave index.ejs, waarbij de gebruiker in de aanvraag (indien aanwezig) wordt doorgegeven.
    * De route `/account` *zorgt er eerst voor dat we worden geverifieerd* (we implementeren dit in het volgende voorbeeld), en geeft vervolgens de gebruiker uit de aanvraag door, zodat we meer informatie over de gebruiker kunnen krijgen.
    * De route `/login` roept onze azuread-openidconnect-authenticator aan vanuit `passport-azuread`. Als dit niet lukt, wordt de gebruiker opnieuw omgeleid naar /login.
    * De route `/logout` roept gewoon de logout.ejs (en route) aan, waarmee cookies worden gewist, en retourneert de gebruiker weer terug naar index.ejs.

1. Voor het laatste deel van `app.js` voegt u de methode **EnsureAuthenticated** toe die in `/account` is gebruikt, zoals eerder is getoond.

    ```JavaScript
    // Simple route middleware to ensure user is authenticated. (section 4)

    //   Use this route middleware on any resource that needs to be protected. If
    //   the request is authenticated (typically via a persistent sign-in session),
    //   the request proceeds. Otherwise, the user is redirected to the
    //   sign-in page.
    function ensureAuthenticated(req, res, next) {
      if (req.isAuthenticated()) { return next(); }
      res.redirect('/login')
    }
    ```

1. Tot slot maakt u de server zelf in `app.js`:

    ```JavaScript
    app.listen(3000);
    ```

## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Stap 5: De weergaven en routes maken in Express om de gebruiker weer te geven in de website

Nu is `app.js` voltooid. U hoeft alleen de routes en weergaven toe te voegen die de verkregen gegevens aan de gebruiker tonen, en de routes `/logout` en `/login` afhandelen die we hebben gemaakt.

1. Maak de route `/routes/index.js` onder de hoofddirectory.

    ```JavaScript
    /*
     * GET home page.
     */

    exports.index = function(req, res){
      res.render('index', { title: 'Express' });
    };
    ```

1. Maak de route `/routes/user.js` onder de hoofddirectory.

    ```JavaScript
    /*
     * GET users listing.
     */

    exports.list = function(req, res){
      res.send("respond with a resource");
    };
    ```

    Deze geven de aanvraag door aan onze weergaven, met inbegrip van de gebruiker, indien aanwezig.

1. Maak de weergave `/views/index.ejs` onder de hoofddirectory. Dit is een eenvoudige pagina die onze aan- en afmeldingsmethoden aanroept en ons de gelegenheid geeft om accountgegevens te verzamelen. U ziet dat u de voorwaardelijke `if (!user)` kunt gebruiken omdat de gebruiker die in de aanvraag wordt doorgegeven, het bewijs is dat er een aangemelde gebruiker is.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Maak de weergave `/views/account.ejs` onder de hoofddirectory, zodat u aanvullende informatie kunt weergeven die `passport-azure-ad` in de gebruikersaanvraag heeft geplaatst.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
    <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Voeg een indeling toe om de weergave van de pagina te verbeteren. Maak de weergave `/views/layout.ejs` onder de hoofddirectory.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

## <a name="step-6-build-and-run-your-app"></a>Stap 6: De app bouwen en uitvoeren

1. Voer `node app.js` uit en ga naar `http://localhost:3000`.
1. Meld u aan met een persoonlijk Microsoft-account of een werk- of schoolaccount.

    U ziet hoe de identiteit van de gebruiker wordt weergegeven in de /account-lijst. U hebt nu een web-app die is beveiligd met protocollen volgens de industrienorm die gebruikers kunnen verifiëren met zowel hun persoonlijke als hun werk-/schoolaccounts.

    Voor naslag is het voltooide voorbeeld (zonder uw configuratiewaarden) [geleverd als zip-bestand](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/master.zip). U kunt deze ook klonen vanuit GitHub:

    ```git clone --branch master https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

## <a name="next-steps"></a>Volgende stappen

Nu kunt u andere scenario's gaan proberen:

> [!div class="nextstepaction"]
> [Een Web API beveiligen met Azure AD](quickstart-v1-nodejs-webapi.md)