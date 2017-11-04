---
title: Azure AD v2 Android ophalen gestart - instellingen | Microsoft Docs
description: Hoe een Android-App kunt ophalen van een toegangstoken en Microsoft Graph API of API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: a43d7e30a6f4176afba27f0de2c2c116df741080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
## <a name="set-up-your-project"></a>Instellen van uw project

> Voorkeur voor het downloaden van dit voorbeeld Android Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) en doorgaan met de [configuratiestap](#create-an-application-express) voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.


### <a name="create-a-new-project"></a>Een nieuw project maken 
1.  Open Android Studio, gaat u naar:`File` > `New` > `New Project`
2.  Naam van uw toepassing en klik op`Next`
3.  Zorg ervoor dat u selecteert *API 21 of nieuwere (Android 5.0)* en klikt u op`Next`
4.  Laat `Empty Activity`, klikt u op `Next`, klikt u vervolgens`Finish`


### <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>De Microsoft Authentication Library (MSAL) toevoegen aan uw project
1.  In Android Studio, gaat u naar:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Kopieer en plak de volgende code onder `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>Over dit pakket

Het bovenstaande pakket installeert de Microsoft Authentication Library (MSAL). MSAL verwerkt ophalen, opslaan in cache en vernieuwen van gebruikerstokens gebruikt voor toegang tot API's die zijn beveiligd door Azure Active Directory-v2-eindpunt.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>De gebruikersinterface van uw toepassing maken

1.  Open: `activity_main.xml` onder`res` > `layout`
2.  Wijzig de indeling van de activiteit van `android.support.constraint.ConstraintLayout` of andere aan`LinearLayout`
3.  Voeg `android:orientation="vertical"` eigenschap `LinearLayout` knooppunt
4.  Kopieer en plak de volgende code naar de `LinearLayout` knooppunt, vervangen de huidige inhoud:

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

