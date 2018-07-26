---
title: Zelfstudie voor MongoDB, Angular en Node voor Azure - deel 6 | Microsoft Docs
description: Deel 6 van de serie zelfstudies voor het maken van een MongoDB-app met Angular en Node op Azure Cosmos DB, waarbij gebruik wordt gemaakt van dezelfde API's als voor MongoDB
services: cosmos-db
author: johnpapa
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/17/2018
ms.author: john.papa
ms.custom: mvc
ms.openlocfilehash: bceb984a3f309ce1220204abf5f703cc166ddf82
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248145"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-6-add-post-put-and-delete-functions-to-the-app"></a>Een MongoDB-app maken met Angular en Node voor Azure Cosmos DB - deel 6: de functies Post, Put en Delete aan de app toevoegen

In deze meerdelige zelfstudie leert u hoe u een nieuwe [MongoDB API](mongodb-introduction.md)-app maakt in Node.js met Express en Angular, waarna u de app verbinding laat maken met uw Azure Cosmos DB-database.

Deel 6 van de zelfstudie bouwt voort op [deel 5](tutorial-develop-mongodb-nodejs-part5.md) en beschrijft de volgende taken:

> [!div class="checklist"]
> * De functies Post, Put en Delete voor de hero-service maken
> * De app uitvoeren

> [!VIDEO https://www.youtube.com/embed/Y5mdAlFGZjc]

## <a name="prerequisites"></a>Vereisten

Voltooi de stappen in [deel 5](tutorial-develop-mongodb-nodejs-part5.md) van de zelfstudie voordat u aan dit deel begint.

> [!TIP]
> Aan de hand van deze zelfstudie wordt u stapsgewijs begeleid bij het maken van de toepassing. Als u het voltooide project wilt downloaden, kunt u de voltooide app vinden in de [angular-cosmosdb-opslagplaats](https://github.com/Azure-Samples/angular-cosmosdb) op GitHub.

## <a name="add-a-post-function-to-the-hero-service"></a>Een Post-functie aan de hero-service toevoegen

1. In Visual Studio Code opent u **routes.js** en **hero.service.js** naast elkaar door op de knop **Split Editor** ![knop Split Editor in Visual Studio](./media/tutorial-develop-mongodb-nodejs-part6/split-editor-button.png) te drukken.

    U ziet dat regel 7 van routes.js de functie `getHeroes` aanroept in regel 5 van **hero.service.js**.  Voor de functies Post, Put en Delete moet dezelfde koppeling worden gemaakt. 

    ![routes.js en hero.service.js in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/routes-heroservicejs.png)
    
    We beginnen met het invoeren van code in de hero-service. 

2. Kopieer na de functie `getHeroes` en vóór `module.exports` de volgende code in **hero.service.js**. Deze code:  
   * Maakt gebruik van een hero-model voor het posten van een nieuwe hero.
   * Controleert de responsen om te kijken of er fouten zijn, en retourneert de statuswaarde 500.

   ```javascript
   function postHero(req, res) {
     const originalHero = { uid: req.body.uid, name: req.body.name, saying: req.body.saying };
     const hero = new Hero(originalHero);
     hero.save(error => {
       if (checkServerError(res, error)) return;
       res.status(201).json(hero);
       console.log('Hero created successfully!');
     });
   }

   function checkServerError(res, error) {
     if (error) {
       res.status(500).send(error);
       return error;
     }
   }
   ```

3. Werk in **hero.service.js** de `module.exports` bij zodat de nieuwe functie `postHero` is opgenomen. 

    ```javascript
    module.exports = {
      getHeroes,
      postHero
    };
    ```

4. Voeg in **routes.js** na de `get`-router een router toe voor de functie `post`. Met deze router wordt één hero per keer gepost. Door het routerbestand op deze manier te structureren, kunt u duidelijk alle beschikbare API-eindpunten bekijken, terwijl het echte werk aan het bestand **hero.service.js** wordt overgelaten.

    ```javascript
    router.post('/hero', (req, res) => {
      heroService.postHero(req, res);
    });
    ```

5. Controleer of alles werkt door de app uit te voeren. Sla uw wijzigingen op in Visual Studio Code, klik aan de linkerkant op de knop **Debug** ![pictogram Debug in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/debug-button.png) en klik vervolgens op de knop **Start Debugging** ![pictogram Start Debugging in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/start-debugging-button.png).

6. Ga terug naar de internetbrowser en open het tabblad Netwerk in de hulpmiddelen voor ontwikkelaars door op F12 te drukken (op de meeste computers). Ga naar [http://localhost:3000](http://localhost:3000) om de aanroepen te bekijken die via het netwerk zijn gedaan.

    ![Tabblad Netwerk in Chrome met een overzicht van de netwerkactiviteit](./media/tutorial-develop-mongodb-nodejs-part6/add-new-hero.png)

7. Voeg een nieuwe hero toe door op de knop **Add New Hero** te klikken. Voer de id '999' in, de naam 'Fred' en de uitspraak 'Hello'. Klik vervolgens op **Save**. Op het tabblad Netwerk zou u een POST-aanvraag voor een nieuwe hero moeten zien. 

    ![Tabblad Netwerk in Chrome met de netwerkactiviteit voor de functies Get en Post](./media/tutorial-develop-mongodb-nodejs-part6/post-new-hero.png)

    We gaan terug en voegen de functies Put en Delete aan de app toe.

## <a name="add-the-put-and-delete-functions"></a>De functies Put en Delete toevoegen

1. Voer in **routes.js** na de post-router de `put`- en `delete`-router toe.

    ```javascript
    router.put('/hero/:uid', (req, res) => {
      heroService.putHero(req, res);
    });

    router.delete('/hero/:uid', (req, res) => {
      heroService.deleteHero(req, res);
    });
    ```

2. Kopieer na de functie `checkServerError` de volgende code in **hero.service.js**. Deze code:
   * Maakt de functies `put` en `delete`
   * Controleert of de hero is gevonden
   * Handelt fouten af 

   ```javascript
   function putHero(req, res) {
     const originalHero = {
       uid: parseInt(req.params.uid, 10),
       name: req.body.name,
       saying: req.body.saying
     };
     Hero.findOne({ uid: originalHero.uid }, (error, hero) => {
       if (checkServerError(res, error)) return;
       if (!checkFound(res, hero)) return;

      hero.name = originalHero.name;
       hero.saying = originalHero.saying;
       hero.save(error => {
         if (checkServerError(res, error)) return;
         res.status(200).json(hero);
         console.log('Hero updated successfully!');
       });
     });
   }

   function deleteHero(req, res) {
     const uid = parseInt(req.params.uid, 10);
     Hero.findOneAndRemove({ uid: uid })
       .then(hero => {
         if (!checkFound(res, hero)) return;
         res.status(200).json(hero);
         console.log('Hero deleted successfully!');
       })
       .catch(error => {
         if (checkServerError(res, error)) return;
       });
   }

   function checkFound(res, hero) {
     if (!hero) {
       res.status(404).send('Hero not found.');
       return;
     }
     return hero;
   }
   ```

3. Exporteer de nieuwe modules in **hero.service.js**:

   ```javascript
    module.exports = {
      getHeroes,
      postHero,
      putHero,
      deleteHero
    };
    ```

4. De code is nu bijgewerkt. Klik op de knop **Restart** ![knop Restart in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part6/restart-debugger-button.png) in Visual Studio Code.

5. Vernieuw de pagina in uw internetbrowser en klik op de knop **Add New Hero**. Voeg een nieuwe hero toe met de id '9', de naam 'Starlord' en de uitspraak 'Hi’. Klik op de knop **Save** om de nieuwe hero op te slaan.

6. Selecteer de hero **Starlord** en wijzig de uitspraak 'Hi' in 'Bye'. Klik vervolgens op de knop **Save**. 

    U kunt nu de id selecteren op het tabblad Network om de payload weer te geven. In de payload kunt u zien dat de uitspraak nu is ingesteld op 'Bye'.

    ![De app Heroes en het tabblad Netwerk met de payload](./media/tutorial-develop-mongodb-nodejs-part6/put-hero-function.png) 

    U kunt ook een van de hero's in de gebruikersinterface verwijderen en zien hoe lang het duurt om de bewerking Verwijderen te voltooien. Probeer dit uit door op de knop Verwijderen voor de hero met de naam Fred te klikken.

    ![De app Heroes en het tabblad Netwerk met de tijdsduur voor het voltooien van de functies](./media/tutorial-develop-mongodb-nodejs-part6/times.png) 

    Als u de pagina vernieuwt, wordt op het tabblad Netwerk weergegeven hoelang het duurt voor de hero's zijn opgehaald. Deze tijden zijn kort, maar het hangt ervan af waar ter wereld uw gegevens zich bevinden en tevens van de mogelijkheid hierop geo-replicatie uit te voeren in een gebied dat in de buurt van uw gebruikers is. Meer informatie over geo-replicatie vindt u in de volgende zelfstudie, die binnenkort wordt uitgebracht.

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * De functies Post, Put en Delete aan de app toegevoegd 

Kom hier snel terug voor aanvullende video's in deze serie zelfstudies.

