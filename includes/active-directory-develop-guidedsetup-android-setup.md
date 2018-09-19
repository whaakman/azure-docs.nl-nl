---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 2e23a15a6bff81f0b48b703e516de8a1a1820972
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293730"
---
## <a name="set-up-your-project"></a>Instellen van uw project

Wilt u dit voorbeeld van Android Studio-project in plaats daarvan downloaden? [Een project hebt gedownload](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip), en gaat u naar de [configuratiestap](#register-your-application) het codevoorbeeld configureren voordat u deze uitvoert.

### <a name="create-a-new-project"></a>Een nieuw project maken 
1.  Open Android Studio en selecteer vervolgens **bestand** > **nieuw** > **nieuw Project**.
2.  Geef uw toepassing en selecteer vervolgens **volgende**.
3.  Selecteer **API 21 of nieuwere (Android 5.0)**, en selecteer vervolgens **volgende**.
4.  Laat **lege activiteit** is, selecteert u **volgende**, en selecteer vervolgens **voltooien**.


### <a name="add-msal-to-your-project"></a>MSAL toevoegen aan uw project
1.  Selecteer in Android Studio **Gradle-Scripts** > **build.gradle (Module: app)**.
2.  Onder **afhankelijkheden**, plak de volgende code:

    ```gradle  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Over dit pakket

Het pakket in de bovenstaande code installeert Microsoft Authentication Library. MSAL verwerkt alle token bewerkingen zoals ophalen, caching, vernieuwen en verwijderen.  De tokens zijn nodig voor toegang tot de API's beveiligd door de Microsoft identity-platform.
<!--end-collapse-->

## <a name="create-the-apps-ui"></a>Maken van de app-gebruikersinterface

1. Ga naar **res** > **lay-out**, en open vervolgens **activity_main.xml**. 
2. Wijzig de indeling van de activiteit van `android.support.constraint.ConstraintLayout` of andere aan `LinearLayout`.
3. Voeg de `android:orientation="vertical"` eigenschap in op de `LinearLayout` knooppunt.
4. Plak de volgende code in de `LinearLayout` knooppunt, vervangen de huidige inhoud:

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
