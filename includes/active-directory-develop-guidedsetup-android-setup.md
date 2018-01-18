
## <a name="set-up-your-project"></a>Instellen van uw project

Wilt u dit voorbeeld Android Studio-project in plaats daarvan downloaden? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip), en ga verder met de [configuratiestap](#create-an-application-express) het codevoorbeeld configureren voordat u deze uitvoert.

### <a name="create-a-new-project"></a>Een nieuw project maken 
1.  Open Android Studio en selecteer vervolgens **bestand** > **nieuw** > **nieuw Project**.
2.  Naam van uw toepassing en selecteer vervolgens **volgende**.
3.  Selecteer **API 21 of nieuwere (Android 5.0)**, en selecteer vervolgens **volgende**.
4.  Laat **lege activiteit** is, selecteert u de **volgende**, en selecteer vervolgens **voltooien**.


### <a name="add-msal-to-your-project"></a>MSAL toevoegen aan uw project
1.  Selecteer in Android Studio **Gradle Scripts** > **build.gradle (Module: app)**.
2.  Onder **afhankelijkheden**, plak de volgende code:

    ```ruby  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Over dit pakket

Het pakket in de bovenstaande code installeert Microsoft Authentication Library. MSAL verwerkt ophalen, opslaan in cache en vernieuwen van gebruikerstokens die worden gebruikt voor toegang tot de API's die zijn beveiligd door het Azure Active Directory-v2-eindpunt.
<!--end-collapse-->

## <a name="create-the-application-ui"></a>De UI-toepassing maken

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
