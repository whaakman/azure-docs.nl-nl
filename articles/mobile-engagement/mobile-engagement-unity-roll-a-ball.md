---
title: Een zelfstudie bDe volledige Unity-album
description: Stappen voor het maken van de klassieke Unity draaien een bDe volledige spel dit is een vereiste voor alle zelfstudies over Mobile Engagement Unity
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6392d1f780b1bc2348fee5947550b05e86ea4de2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a id="unity-roll-a-ball"></a>Maak Unity draaien een spel bDe volledige
In deze zelfstudie wordt de belangrijkste stappen voor een enigszins gewijzigd [Unity draaien een zelfstudie bDe volledige](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). In dit voorbeeld van een game bestaat uit een bolvormige player-object dat wordt beheerd door de gebruiker van de app en het doel van het spel is voor het 'verzamelen' Verzamel objecten van het object player met deze Verzamel objecten tegenaan. Hierbij wordt verondersteld enigszins bekend bent met Unity editor-omgeving. Als u problemen ondervindt uitvoert en u naar de volledige zelfstudie verwijzen moet. 

### <a name="setting-up-the-game"></a>Instellen van de game
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Open **Unity-Editor** en klik op **nieuw**. 
   
    ![][51] 
2. Geef een **projectnaam** & **locatie**, selecteer **3D** en klik op **project maken**.
   
    ![][52]
3. Opslaan van de standaard scene zojuist hebt gemaakt als onderdeel van het nieuwe project met de naam van de **MiniGame** binnen een nieuwe  **\_schermen** map onder **activa** map:
   
    ![][53]
4. Maak een **3D-Object -> vlak** als het afspelen veld en de naam van dit object vlak als **compleet**
   
    ![][1]
5. Opnieuw instellen van de transformatie-onderdeel voor dit **compleet** object zodat deze aan de oorsprong. 
   
    ![][3]
6. Schakel het selectievakje **raster weergeven** van **Gizmos menu** voor de **compleet** object.
   
    ![][4]
7. Update de **Scale** component voor de **compleet** object [X = 2, Y = 1, Z = 2]. 
   
    ![][5]
8. Voeg een nieuwe **3D-Object -> bol** naar het project en wijzig de naam van deze bol object in de vorm **Player**. 
   
    ![][6]
9. Selecteer de **Player** object en klikt u op **transformatie opnieuw instellen** lijkt op het vlak-object. 
10. Update **transformatie-positie > -> Y-coördinaat** component voor de Y-speler als 0,5.  
    
    ![][7]
11. Maak een nieuwe map **materialen** in het project waar we de kleur van de speler materiaal zal maken. 
12. Maak een nieuwe **materiaal** aangeroepen **achtergrond** in deze map. 
    
    ![][8]
13. De kleur van het materiaal bijwerken door het bijwerken van de **Albedo** eigenschap ervan. U kunt de RGB-waarden van [0,32,64] selecteren. 
    
    ![][9]
14. Sleep dit materiaal naar de weergave scene om toe te passen die moet worden de **compleet** object. 
    
    ![][10]
15. Ten slotte bijwerken de **transformatie -> rotatie -> Y** 60 op het object gericht licht voor de duidelijkheid. 
    
    ![][12]

### <a name="moving-the-player"></a>Windows media player verplaatsen
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Voeg een **RigidBody** onderdeel aan de **Player** object. 
   
    ![][13]
2. Maak een nieuwe map **Scripts** in het Project. 
3. Klik op **onderdeel toevoegen -> nieuw Script-C# Script >**. Naam **PlayerController**, en klik op **maken en toevoegen**. Dit maakt en een script gekoppeld aan het object Player.  
   
    ![][14]
4. Verplaatsen van dit script onder de **Scripts** map in het project. 
5. Open het script voor het bewerken in uw favoriete scripteditor, de scriptcode bijwerken met de volgende code en sla het. 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. Let op het bovenstaande script gebruikt een **snelheid** eigenschap. In de editor Unity werk u de eigenschap snelheid tot 10.  
   
    ![][15]
7. Klik op **afspelen** in de Unity-Editor. Nu moet u kunnen bepalen de bDe volledige met het toetsenbord en moet draaien en verplaatsen. 

### <a name="moving-the-camera"></a>Het verplaatsen van de camera
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) en wordt deze de **Main Camera** naar de **Player** object. 

1. Update de **Transform.Position** moet X = 0, Y = 10.5, Z =-10.  
2. Update de **Transform.Rotation** moet X = 45, Y = 0, Z = 0.  
   
    ![][16]
3. Toevoegen van een script met de nieuwe naam **CameraController** naar de **MainCamera** en verplaats deze onder de map Scripts. 
   
    ![][17]
4. Open het script voor het bewerken en voeg de volgende code in het:
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. In de omgeving Unity - sleept u de variabele Player in de sleuf speler voor het object Main Camera zo in dat de twee aan elkaar gekoppeld. 
   
    ![][18]
6. Nu als u op afspelen te in de Unity-editor drukken en het object Player bDe volledige draaien ziet vervolgens u de volgende tekst in het verkeer Camera.  

### <a name="setting-up-the-play-area"></a>Instellen van het gebied Play
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). We gaan de wanden rond de grond zodat het object Player bDe volledige niet uit de play gebied in de verplaatsing verwijderen maken. 

1. Klik op **Create-maken leeg >-Game Object >** en noem deze **wanden**
   
    ![][19]
2. Maak een nieuwe onder dit object wanden - **3D-Object-kubus >** en noem deze 'West wanden'. 
   
    ![][20]
3. Update de **transformatie -> positie** en **transformatie -> Scale** voor dit object West wanden. 
   
    ![][21]
4. Dubbele van de wanden West maken een **Oost wanden** transformeren met de bijgewerkte positie en de schaal. 
   
    ![][22]
5. Dubbele van de wanden Oost maken een **Noord wanden** transformeren met de bijgewerkte positie & schaal. 
   
    ![][23]
6. De wanden Noord dupliceren en maak een **Zuid wanden** transformeren met de bijgewerkte positie & schaal. 
   
    ![][24]

### <a name="creating-collectible-objects"></a>Verzamel objecten maken
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Sommige aantrekkelijk ogende objecten die de set Verzamel objecten die het object Player bDe volledige verzamelen moet door tegenaan ermee wordt vormen kunt maken. 

1. Maak een nieuwe **3D-kubus object** en noem deze ophalen. 
2. Pas de **transformatie -> rotatie** & **transformatie -> Scale** van het object ophalen. 
   
    ![][25]
3. Maak en voeg een **nieuwe C# Script** aangeroepen **Rotator** op het object ophalen. Zorg ervoor dat het script plaatsen onder de map Scripts. 
   
    ![][26]
4. Open dit script voor het bewerken en deze als volgt bijwerken: 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. Klik op nu worden de Play-modus in de Unity-Editor en het weergeven van uw ophalen-object draaien op de as.
6. Maak een nieuwe map **Prefabs** 
   
    ![][27]
7. Sleep de **ophalen** object en plaats deze in de map Prefabs.
   
    ![][28]
8. Maak een nieuwe **leeg Game object** aangeroepen **overdracht op**. Opnieuw instellen van de positie in de oorsprong en sleep het object ophalen onder dit spel object.  
   
    ![][29]
9. Dubbele de **ophalen** object en deze verspreiden zich op de **compleet** object rond de **Player** object door het bijwerken van de **van Transform.Position X & Z** waarden op de juiste wijze. 
   
    ![][30]
10. Maak een **nieuw materiaal** aangeroepen **ophalen** bij te werken als rood in kleur door bij te werken de **Albedo eigenschap** vergelijkbaar met wat we hebben gedaan voor het bijwerken van het object compleet. 
    
    ![][31]
11. Het materiaal van toepassing op alle 4 ophalen objecten.
    
    ![][32]

### <a name="collecting-the-pickup-objects"></a>Verzamelen van de objecten ophalen
De onderstaande stappen zijn van de [Unity-zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Windows Media Player wordt bijgewerkt zodat deze ' verzamelen ' de ophalen-objecten door leesbewerkingen en ze. 

1. Open de **PlayerController** script gekoppeld aan het object Player bewerken en bijwerken naar het volgende:  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. Maak een nieuwe **Tag** aangeroepen **Kies Up** (moet overeenkomen met wat is er in het script)  
   
    ![][33]
   
    ![][34]
3. Deze toepassen **Tag** op het object Prefab ophalen. 
   
    ![][35]
4. Schakel **IsTrigger** selectievakje voor het object Prefab.
   
    ![][36]
5. Een Rigid instantie aan ophalen Prefab object toevoegen. Voor de optimalisatie van de prestaties wordt de statische collider zoals bijgewerkt naar een dynamische collider. 
   
    ![][37]
6. Controleer ten slotte de **IsKinematic** eigenschap voor het prefab object. 
   
    ![][38]
7. Bereikt **afspelen** in de Unity-editor en u kunnen worden afgespeeld **draaien geen** game door het object Player met behulp van de toetsen voor invoer van de richting te verplaatsen. 

### <a name="updating-the-game-for-mobile-play"></a>Bijwerken van de game voor mobiele play
De bovenstaande secties gesloten de basic zelfstudie uit Unity. Er wordt nu het spel zodat het mobiele apparaat beschrijvende wijzigen. Houd er rekening mee dat we toetsenbordinvoer voor het spel tot nu toe voor het testen gebruikt. Nu we dit zodanig aanpassen zullen dat we de speler beheren kunt via de beweging van het telefoonnummer dat wil zeggen versnellingsmeter gebruiken als invoer. 

Open de **PlayerController** voor het bewerken van een script en werk de **FixedUpdate** methode met de beweging van de versnellingsmeter te verplaatsen van het object Player. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Deze zelfstudie concludeert de domeincontroller een basic game maken met Unity en kunt u dit implementeren op een apparaat van uw keuze om te spelen. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













