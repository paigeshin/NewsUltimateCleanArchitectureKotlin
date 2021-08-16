# Main Features

- CleanArchitecture
- DiffUtil
- Pagination
- Hilt
- Webview
- Save SecretKey

# Define UseCases

- Get news headlines
- Delete Save News
- Get Saved News
- Get Searched News
- Save Saved News

# Prepare for gradle

### App Dependencies

- Application Level Gradle

```kotlin
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    ext.kotlin_version = "1.4.31"
    ext.nav_version = "2.3.4"
    ext.hilt_version = "2.33-beta"
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.1.3'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "com.google.dagger:hilt-android-gradle-plugin:$hilt_version"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

- App Level Gradle

```kotlin
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'
    id 'dagger.hilt.android.plugin'
    id 'androidx.navigation.safeargs.kotlin'
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.anushka.newsapiclient"
        minSdkVersion 26
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
        buildConfigField("String","API_KEY",MY_KEY)
        buildConfigField("String","BASE_URL",MY_URL)
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures{
        viewBinding = true
    }
}

dependencies {
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
    def lifecycle_version = "2.3.0"
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation 'androidx.core:core-ktx:1.3.2'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.2.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    implementation 'com.google.code.gson:gson:2.8.6'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.2'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.4.2'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:4.9.0'
    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$lifecycle_version"
    // Annotation processor
    //kapt "androidx.lifecycle:lifecycle-compiler:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-common-java8:$lifecycle_version"
    implementation "com.google.dagger:hilt-android:$hilt_version"
    kapt "com.google.dagger:hilt-compiler:$hilt_version"

    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

    implementation 'com.github.bumptech.glide:glide:4.12.0'
    kapt 'com.github.bumptech.glide:compiler:4.12.0'

    def room_version = "2.2.6"

    implementation "androidx.room:room-runtime:$room_version"
    kapt "androidx.room:room-compiler:$room_version"

    // optional - Kotlin Extensions and Coroutines support for Room
    implementation "androidx.room:room-ktx:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    testImplementation 'junit:junit:4.13.1'
    testImplementation 'com.squareup.okhttp3:mockwebserver:4.9.0'
    testImplementation "com.google.truth:truth:1.1"
    androidTestImplementation 'androidx.test.ext:junit:1.1.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'

}
```

### Gradle Properties, add APIKEY and URL

- [gradle.properties](http://gradle.properties)

```kotlin
# Project-wide Gradle settings.
# IDE (e.g. Android Studio) users:
# Gradle settings configured through the IDE *will override*
# any settings specified in this file.
# For more details on how to configure your build environment visit
# http://www.gradle.org/docs/current/userguide/build_environment.html
# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
# org.gradle.parallel=true
# AndroidX package structure to make it clearer which packages are bundled with the
# Android operating system, and which are packaged with your app"s APK
# https://developer.android.com/topic/libraries/support-library/androidx-rn
android.useAndroidX=true
# Automatically convert third-party libraries to use AndroidX
android.enableJetifier=true
# Kotlin code style for this project: "official" or "obsolete":
kotlin.code.style=official
MY_KEY="b7122b5c5f8948eda9715867b6240ce6"
MY_URL="https://newsapi.org"
```

- App Level Gradle

```kotlin
defaultConfig {
        applicationId "com.anushka.newsapiclient"
        minSdkVersion 26
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
        buildConfigField("String","API_KEY",MY_KEY)
        buildConfigField("String","BASE_URL",MY_URL)
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }
```

```kotlin
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'
    id 'dagger.hilt.android.plugin'
    id 'androidx.navigation.safeargs.kotlin'
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.anushka.newsapiclient"
        minSdkVersion 26
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
        buildConfigField("String","API_KEY",MY_KEY)
        buildConfigField("String","BASE_URL",MY_URL)
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures{
        viewBinding = true
    }
}

dependencies {
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
    def lifecycle_version = "2.3.0"
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation 'androidx.core:core-ktx:1.3.2'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.2.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    implementation 'com.google.code.gson:gson:2.8.6'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.2'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.4.2'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:4.9.0'
    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$lifecycle_version"
    // Annotation processor
    //kapt "androidx.lifecycle:lifecycle-compiler:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-common-java8:$lifecycle_version"
    implementation "com.google.dagger:hilt-android:$hilt_version"
    kapt "com.google.dagger:hilt-compiler:$hilt_version"

    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

    implementation 'com.github.bumptech.glide:glide:4.12.0'
    kapt 'com.github.bumptech.glide:compiler:4.12.0'

    def room_version = "2.2.6"

    implementation "androidx.room:room-runtime:$room_version"
    kapt "androidx.room:room-compiler:$room_version"

    // optional - Kotlin Extensions and Coroutines support for Room
    implementation "androidx.room:room-ktx:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    testImplementation 'junit:junit:4.13.1'
    testImplementation 'com.squareup.okhttp3:mockwebserver:4.9.0'
    testImplementation "com.google.truth:truth:1.1"
    androidTestImplementation 'androidx.test.ext:junit:1.1.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'

}
```

- .gitignore

```kotlin
gradle.properties
```

### Release Version Optimization

- App Level Gradle

```kotlin
buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
```

```kotlin
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'
    id 'dagger.hilt.android.plugin'
    id 'androidx.navigation.safeargs.kotlin'
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.anushka.newsapiclient"
        minSdkVersion 26
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
        buildConfigField("String","API_KEY",MY_KEY)
        buildConfigField("String","BASE_URL",MY_URL)
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures{
        viewBinding = true
    }
}

dependencies {
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
    def lifecycle_version = "2.3.0"
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation 'androidx.core:core-ktx:1.3.2'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.2.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    implementation 'com.google.code.gson:gson:2.8.6'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.2'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.4.2'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:okhttp:4.9.0'
    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$lifecycle_version"
    // Annotation processor
    //kapt "androidx.lifecycle:lifecycle-compiler:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-common-java8:$lifecycle_version"
    implementation "com.google.dagger:hilt-android:$hilt_version"
    kapt "com.google.dagger:hilt-compiler:$hilt_version"

    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

    implementation 'com.github.bumptech.glide:glide:4.12.0'
    kapt 'com.github.bumptech.glide:compiler:4.12.0'

    def room_version = "2.2.6"

    implementation "androidx.room:room-runtime:$room_version"
    kapt "androidx.room:room-compiler:$room_version"

    // optional - Kotlin Extensions and Coroutines support for Room
    implementation "androidx.room:room-ktx:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    testImplementation 'junit:junit:4.13.1'
    testImplementation 'com.squareup.okhttp3:mockwebserver:4.9.0'
    testImplementation "com.google.truth:truth:1.1"
    androidTestImplementation 'androidx.test.ext:junit:1.1.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'

}
```

---

# 1. Define Pojo

```kotlin
data class APIResponse(
    @SerializedName("articles")
    val articles: List<Article>,
    @SerializedName("status")
    val status: String,
    @SerializedName("totalResults")
    val totalResults: Int
)
```

```kotlin
@Entity(
    tableName = "articles"
)
data class Article(
    @PrimaryKey(autoGenerate = true)
    val id : Int? = null,
    @SerializedName("author")
    val author: String?,
    @SerializedName("content")
    val content: String?,
    @SerializedName("description")
    val description: String?,
    @SerializedName("publishedAt")
    val publishedAt: String?,
    @SerializedName("source")
    val source: Source?,
    @SerializedName("title")
    val title: String?,
    @SerializedName("url")
    val url: String?,
    @SerializedName("urlToImage")
    val urlToImage: String?
):Serializable
```

```kotlin
data class Source(
    @SerializedName("id")
    val id: String,
    @SerializedName("name")
    val name: String
)
```

# 2. Define Repository Interface

- Resource.kt

⇒ Util to handle Network events (Success, Loading, Error)

```kotlin
sealed class Resource<T>(
    val data: T? = null,
    val message: String? = null
) {
    class Success<T>(data: T) : Resource<T>(data)
    class Loading<T>(data: T? = null) : Resource<T>(data)
    class Error<T>(message: String, data: T? = null) : Resource<T>(data, message)
}
```

- NewsRepository

```kotlin
interface NewsRepository{
      suspend fun getNewsHeadlines(country : String, page : Int): Resource<APIResponse>
      suspend fun getSearchedNews(country: String,searchQuery:String,page: Int) : Resource<APIResponse>
      suspend fun saveNews(article: Article)
      suspend fun deleteNews(article: Article)
      fun getSavedNews(): Flow<List<Article>>
}
```

# 3. Define Usecase

```kotlin
class DeleteSavedNewsUseCase(private val newsRepository: NewsRepository) {
    suspend fun execute(article: Article)=newsRepository.deleteNews(article)
}
```

```kotlin
class GetNewsHeadlinesUseCase(private val newsRepository: NewsRepository) {
    suspend fun execute(country : String, page : Int): Resource<APIResponse>{
        return newsRepository.getNewsHeadlines(country,page)
    }
}
```

```kotlin
class GetSavedNewsUseCase(private val newsRepository: NewsRepository) {
    fun execute(): Flow<List<Article>>{
        return newsRepository.getSavedNews()
    }
}
```

```kotlin
class GetSearchedNewsUseCase(private val newsRepository: NewsRepository) {
     suspend fun execute(country:String,searchQuery:String,page:Int): Resource<APIResponse>{
         return newsRepository.getSearchedNews(country,searchQuery,page)
     }
}
```

```kotlin
class SaveNewsUseCase(private val newsRepository: NewsRepository) {
  suspend fun execute(article: Article)=newsRepository.saveNews(article)
}
```

# 4. RemoteDataSource

- Resource.kt

⇒ Util to handle Network events (Success, Loading, Error)

```kotlin
sealed class Resource<T>(
    val data: T? = null,
    val message: String? = null
) {
    class Success<T>(data: T) : Resource<T>(data)
    class Loading<T>(data: T? = null) : Resource<T>(data)
    class Error<T>(message: String, data: T? = null) : Resource<T>(data, message)
}
```

- NewsAPIService.kt

```kotlin
interface NewsAPIService {
  @GET("v2/top-headlines")
  suspend fun getTopHeadlines(
      @Query("country")
      country:String,
      @Query("page")
      page:Int,
      @Query("apiKey")
      apiKey:String = BuildConfig.API_KEY
  ): Response<APIResponse>

    @GET("v2/top-headlines")
    suspend fun getSearchedTopHeadlines(
        @Query("country")
        country:String,
        @Query("q")
        searchQuery:String,
        @Query("page")
        page:Int,
        @Query("apiKey")
        apiKey:String = BuildConfig.API_KEY
    ): Response<APIResponse>

}
```

- NewsRemoteDataSource.kt

```kotlin
interface NewsRemoteDataSource {
    suspend fun getTopHeadlines(country : String, page : Int):Response<APIResponse>
    suspend fun getSearchedNews(country : String,searchQuery:String, page : Int):Response<APIResponse>
}
```

- NewsRemoteDataSourceImpl.kt

```kotlin
class NewsRemoteDataSourceImpl(
        private val newsAPIService: NewsAPIService
):NewsRemoteDataSource {
    override suspend fun getTopHeadlines(country : String, page : Int): Response<APIResponse> {
          return newsAPIService.getTopHeadlines(country,page)
    }

    override suspend fun getSearchedNews(
        country: String,
        searchQuery: String,
        page: Int
    ): Response<APIResponse> {
        return newsAPIService.getSearchedTopHeadlines(country,searchQuery,page)
    }
}
```

### Test by mocking Webserver

- Prepare for json, `/main/resources/newsresponse.json`

```json
{
  "status": "ok",
  "totalResults": 38,
  "articles": [
    {
      "source": {
        "id": null,
        "name": "CNBC"
      },
      "author": "Saheli Roy Choudhury",
      "title": "Samsung to launch its newest Galaxy smartphones on Jan. 14, earlier than usual - CNBC",
      "description": "Samsung's launch is set to happen in the same week as the digital-only CES 2021.",
      "url": "https://www.cnbc.com/2021/01/04/samsung-galaxy-unpacked-2021.html",
      "urlToImage": "https://image.cnbcfm.com/api/v1/image/106476097-1586158396471gettyimages-1209192651.jpeg?v=1596067547",
      "publishedAt": "2021-01-04T03:25:00Z",
      "content": "SINGAPORE Samsung said Monday its next smartphone announcement will be held on Jan. 14, where it is expected to launch the newest versions of its Galaxy flagship phones.\r\nInvitations sent out by the … [+1687 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "fox8.com"
      },
      "author": "Brittany Rall",
      "title": "Browns to face off against Steelers in first playoff game next Sunday night - WJW FOX 8 News Cleveland",
      "description": "The Cleveland Browns may be enjoying their victory against the Pittsburgh Steelers right now, but the battle will continue next week.",
      "url": "https://fox8.com/news/browns-to-face-off-against-steelers-in-first-playoff-game-next-sunday-night/",
      "urlToImage": "https://fox8.com/wp-content/uploads/sites/12/2021/01/GettyImages-1294317060.jpg?w=1280",
      "publishedAt": "2021-01-04T03:22:00Z",
      "content": "*Watch Baker Mayfield’s interview after Sunday’s win in the video above.*\r\nCLEVELAND (WJW) — The Cleveland Browns are celebrating an exciting victory over the Pittsburgh Steelers but the hard work is… [+697 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "ESPN"
      },
      "author": "Tim McManus",
      "title": "Philadelphia Eagles' Jalen Hurts scores two rushing TDs in first half vs. Washington - ESPN",
      "description": "The rookie becomes the first Philadelphia quarterback with two rushing scores since Mike Vick did it in 2010 against Washington.",
      "url": "https://www.espn.com/nfl/story/_/id/30646925/philadelphia-eagles-jalen-hurts-scores-two-rushing-tds-first-half-vs-washington",
      "urlToImage": "https://a.espncdn.com/combiner/i?img=%2Fphoto%2F2021%2F0104%2Fr797645_1296x729_16%2D9.jpg",
      "publishedAt": "2021-01-04T02:45:52Z",
      "content": "PHILADELPHIA -- Even with a skeleton crew around him, Philadelphia Eagles rookie quarterback Jalen Hurts is still finding a way to be a playmaker.\r\nHurts rushed for a pair of touchdowns on back-to-ba… [+1369 chars]"
    },
    {
      "source": {
        "id": "the-verge",
        "name": "The Verge"
      },
      "author": "Sam Byford",
      "title": "Quibi reportedly in talks to sell its shows to Roku - The Verge",
      "description": "Roku is in talks with Quibi to acquire the failed streaming service’s library of content, according to The Wall Street Journal. A price for the deal wasn’t mentioned.",
      "url": "https://www.theverge.com/2021/1/3/22212447/quibi-roku-content-deal-acquisition-report-wsj",
      "urlToImage": "https://cdn.vox-cdn.com/thumbor/hDpttVmqBOPcAV_XZyxdauP9VCc=/0x215:3000x1786/fit-in/1200x630/cdn.vox-cdn.com/uploads/chorus_asset/file/19870216/gblackmon_200403_3960_quibi_0005.0.jpg",
      "publishedAt": "2021-01-04T02:38:11Z",
      "content": "Could Quibis content find a new home?\r\nIllustration by Grayson Blackmon / The Verge\r\nFailed mobile-first streaming service Quibi is in advanced discussions to sell the rights to its content library t… [+1117 chars]"
    },
    {
      "source": {
        "id": "nfl-news",
        "name": "NFL News"
      },
      "author": null,
      "title": "What's ahead for Los Angeles Rams, Chicago Bears after earning final NFC wild-card spots - NFL.com",
      "description": "The Los Angeles Rams and Chicago Bears locked up the NFC's final two wild-card spots on Sunday. Judy Battista explores what lies ahead for each team in the postseason.",
      "url": "https://www.nfl.com/news/what-s-ahead-for-los-angeles-rams-chicago-bears-after-earning-final-nfc-wild-car",
      "urlToImage": "https://static.www.nfl.com/image/private/t_editorial_landscape_12_desktop/league/puv2pzncddl7cnzqfktd",
      "publishedAt": "2021-01-04T02:35:00Z",
      "content": "After as streaky a season as you'll ever see -- the Bears went 5-1 to start the season, then lost six straight, then won three in a row -- a wild-card spot means that the franchise blowup that felt i… [+2644 chars]"
    },
    {
      "source": {
        "id": "cnn",
        "name": "CNN"
      },
      "author": "Paul LeBlanc, CNN",
      "title": "All 10 living former defense secretaries declare election is over in forceful public letter - CNN",
      "description": "All 10 living former US defense secretaries declared that the US presidential election is over in a forceful public letter published in The Washington Post on Sunday as President Donald Trump continues to deny his election loss to Joe Biden.",
      "url": "https://www.cnn.com/2021/01/03/politics/trump-election-defense-secretaries-public-letter/index.html",
      "urlToImage": "https://cdn.cnn.com/cnnnext/dam/assets/201231163449-trump-final-days-early-white-house-return-election-results-collins-dnt-lead-vpx-00003317-super-tease.jpg",
      "publishedAt": "2021-01-04T02:29:00Z",
      "content": "(CNN)All 10 living former US defense secretaries declared that the US presidential election is over in a forceful public letter published in The Washington Post on Sunday as President Donald Trump co… [+4378 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "CBS Sports"
      },
      "author": "",
      "title": "2021 NFL playoff schedule: Dates, times, bracket and TV for every round of the AFC and NFC postseason - CBS Sports",
      "description": "Here's the schedule for the entire NFL postseason",
      "url": "https://www.cbssports.com/nfl/news/2021-nfl-playoff-schedule-dates-times-bracket-and-tv-for-every-round-of-the-afc-and-nfc-postseason/",
      "urlToImage": "https://sportshub.cbsistatic.com/i/r/2020/12/22/13e8c62f-0171-41b4-a0db-517a149f7538/thumbnail/1200x675/78ab7ccdc48b788b466c1ed43e408124/browns-steelers.jpg",
      "publishedAt": "2021-01-04T02:21:00Z",
      "content": "The NFL playoffs are finally upon us and this year, they're going to have a decidedly different look. Not only will there be 14 teams in the postseason for the first time in league history, but there… [+2886 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "Gizmodo.com"
      },
      "author": "Alyse Stanley",
      "title": "WhatsApp Sets an All-Time Record as Users Welcome in the New Year Virtually - Gizmodo",
      "description": "Facebook’s cross-platform chat app WhatsApp saw a record number of users on New Year’s Eve as some people smartly opted to celebrate the end of 2020 virtually this year. Because even if that cursed year from Hades is finally behind us, the covid-19 pandemic i…",
      "url": "https://gizmodo.com/whatsapp-sets-an-all-time-record-as-users-welcome-in-th-1845981808",
      "urlToImage": "https://i.kinja-img.com/gawker-media/image/upload/c_fill,f_auto,fl_progressive,g_center,h_675,pg_1,q_80,w_1200/zhlnrn1sqdatxnmitf7e.jpg",
      "publishedAt": "2021-01-04T02:12:00Z",
      "content": "Facebooks cross-platform chat app WhatsApp saw a record number of users on New Years Eve as some people smartly opted to celebrate the end of 2020 virtually this year. Because even if that cursed yea… [+1663 chars]"
    },
    {
      "source": {
        "id": "the-hill",
        "name": "The Hill"
      },
      "author": "Justine Coleman",
      "title": "Biden inauguration will include 'presidential escort' to White House, virtual parade | TheHill - The Hill",
      "description": "President-elect Joe Biden’s pared-down inauguration is slated to ...",
      "url": "https://thehill.com/homenews/administration/532465-biden-inauguration-will-include-presidential-escort-to-white-house",
      "urlToImage": "https://thehill.com/sites/default/files/bidenjoe_121920getty_covid.jpg",
      "publishedAt": "2021-01-04T01:50:44Z",
      "content": "President-elect Joe BidenJoe BidenAppeals court dismisses Gohmert's election suit against PenceRomney: Plan to challenge election 'egregious ploy' that 'dangerously threatens' country Pence 'welcomes… [+2505 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "TMZ"
      },
      "author": "TMZ Staff",
      "title": "Bond Girl & 'That '70s Show' Star Tanya Roberts Dead at 65 - TMZ",
      "description": "B-film queen Tanya Roberts has passed away.",
      "url": "https://www.tmz.com/2021/01/03/tanya-roberts-dead-dies-sheena-that-70s-show-bond-girl/",
      "urlToImage": "https://imagez.tmz.com/image/08/16by9/2021/01/04/088f88be36b54144bec39a0c64a1931e_xl.jpg",
      "publishedAt": "2021-01-04T01:49:00Z",
      "content": "Tanya Roberts -- a one-time Bond girl and cult classic '80s star -- has died ... TMZ has learned.\r\nTanya's rep tells TMZ, she was on a walk with her dogs on Christmas Eve and when she returned home s… [+1851 chars]"
    },
    {
      "source": {
        "id": "cnn",
        "name": "CNN"
      },
      "author": "Jasmine Wright, CNN",
      "title": "Harris lambasts Trump call with Georgia officials as 'bold abuse of power' - CNN",
      "description": "Vice President-elect Kamala Harris slammed President Donald Trump and his call pushing Georgia Secretary of State Brad Raffensperger to \"find\" votes to overturn the election results, a \"bald-faced, bold abuse of power,\" in Savannah, Georgia, on Sunday.",
      "url": "https://www.cnn.com/2021/01/03/politics/kamala-harris-trump-georgia/index.html",
      "urlToImage": "https://cdn.cnn.com/cnnnext/dam/assets/210103182736-kamala-harris-trump-georgia-super-tease.jpg",
      "publishedAt": "2021-01-04T01:29:00Z",
      "content": "(CNN)Vice President-elect Kamala Harris slammed President Donald Trump and his call pushing Georgia Secretary of State Brad Raffensperger to \"find\" votes to overturn the election results, a \"bald-fac… [+3653 chars]"
    },
    {
      "source": {
        "id": "the-washington-post",
        "name": "The Washington Post"
      },
      "author": "Cleve Wootson",
      "title": "Sens. Loeffler and Perdue try to steer around Republican infighting as Georgia runoff races draw to a close - The Washington Post",
      "description": "The GOP Senate candidates didn’t vote on whether to override President Trump’s veto of a defense bill, and both sought to avoid a firm answer on their support for an effort to oppose certifying President-elect Biden’s electoral college win.",
      "url": "https://www.washingtonpost.com/politics/loeffler-perdue-georgia-senate-runoffs-gop/2021/01/03/5323071e-4dde-11eb-bda4-615aaefd0555_story.html",
      "urlToImage": "https://www.washingtonpost.com/wp-apps/imrs.php?src=https://arc-anglerfish-washpost-prod-washpost.s3.amazonaws.com/public/RH7KK5SNNQI6XF5WJ247OL7UNM.jpg&w=1440",
      "publishedAt": "2021-01-04T01:17:00Z",
      "content": "Both Loeffler and Perdue aligned themselves with Trump's call for $2,000 stimulus checks to be sent to Americans, even as the idea was blocked by Senate Majority Leader Mitch McConnell (R-Ky.), who l… [+7316 chars]"
    },
    {
      "source": {
        "id": "reuters",
        "name": "Reuters"
      },
      "author": null,
      "title": "Asia shares reach record, Nikkei restrained by lockdown risk - Reuters",
      "description": null,
      "url": "https://www.reuters.com/article/us-global-markets-idUSKBN29901F",
      "urlToImage": null,
      "publishedAt": "2021-01-04T00:35:00Z",
      "content": "FILE PHOTO: Pedestrians wearing facial masks are reflected on an electric board showing stock prices outside a brokerage at a business district in Tokyo, Japan January 30, 2020. REUTERS/Kim Kyung-Hoo… [+4023 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "Billboard"
      },
      "author": "Ashley Iasimone",
      "title": "Playboi Carti Reacts to His First No. 1 Album on the Billboard 200: '!!!!!' - Billboard",
      "description": "Playboi Carti's 'Whole Lotta Red' album tops the Billboard 200 this week, and he seems to be celebrating the good news.",
      "url": "https://www.billboard.com/articles/columns/hip-hop/9506351/playboi-carti-whole-lotta-red-billboard-200-reaction",
      "urlToImage": "https://static.billboard.com/files/2020/04/playboi-carti-live-2019-b-b-billboard-1548-1586966250-1024x677.jpg",
      "publishedAt": "2021-01-04T00:24:48Z",
      "content": "Whole Lotta Red, Playboi Carti's second studio album, started with 100,000 equivalent album units earned in the U.S. in the week ending Dec. 31, 2020, according to Nielsen Music/MRC Data. It follows … [+103 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "DailyFX"
      },
      "author": "Daniel Moss",
      "title": "Bitcoin, Ethereum Outlook: Cryptos Go Parabolic, is a Correction Due? - DailyFX",
      "description": "Bitcoin and Ethereum have stormed higher since the March 2020 nadir, on the back of extraordinary fiscal and monetary support. However, a near-term pullback could be in the offing.",
      "url": "https://www.dailyfx.com/forex/market_alert/2021/01/04/Bitcoin-Ethereum-Outlook-Cryptos-Go-Parabolic-is-a-Correction-Due.html",
      "urlToImage": "https://a.c-dn.net/b/3k4Cfm/headline_iStock-696301332.jpg",
      "publishedAt": "2021-01-04T00:09:21Z",
      "content": "Bitcoin, Ethereum, Cryptocurrency, BTC/USD, ETH/USD Talking Points:\r\n<ul><li>The long-term outlook for both Bitcoin and Ethereum remains skewed to the topside.</li><li>However, both cryptocurrencies … [+4077 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "Daily Mail"
      },
      "author": "Cassie Carpenter",
      "title": "RHOBH star Dorit Kemsley defends Hillary 'Hilaria' Baldwin after Spanish scandal - Daily Mail",
      "description": "The Connecticut-born 44-year-old could relate since she's been blasted for years for affecting an English accent",
      "url": "https://www.dailymail.co.uk/tvshowbiz/article-9109329/RHOBH-star-Dorit-Kemsley-defends-Hillary-Hilaria-Baldwin-Spanish-scandal.html",
      "urlToImage": "https://i.dailymail.co.uk/1s/2021/01/03/23/37558976-0-image-a-135_1609718138809.jpg",
      "publishedAt": "2021-01-03T23:56:00Z",
      "content": "How she met Alec: \r\nHilaria claimed that she told husband Alec from the off that she was from Boston despite his now infamous 2013 Letterman interview in which he claimed she is Spanish. \r\n'My wife i… [+2541 chars]"
    },
    {
      "source": {
        "id": "fox-news",
        "name": "Fox News"
      },
      "author": "Lucas Manfredi",
      "title": "Rodney Davis slams Democrats over 'shameful' plexiglass structure to 'protect' Pelosi votes - Fox News",
      "description": "House Administration Committee ranking member Rodney Davis, R-Ill., slammed Democrats on Sunday for their decision to build a plexiglass structure in the chamber to allow members who were exposed to COVID-19 to vote for speaker of the House.",
      "url": "https://www.foxnews.com/politics/rodney-davis-slams-democrats-over-shameful-plexiglass-structure-to-protect-pelosi-votes",
      "urlToImage": "https://static.foxnews.com/foxnews.com/content/uploads/2021/01/AP21003823780112.jpg",
      "publishedAt": "2021-01-03T23:49:05Z",
      "content": "House Administration Committee ranking member Rodney Davis, R-Ill., slammed Democrats on Sunday for their decision to build a plexiglass structure in the chamber to allow members who were exposed to … [+1618 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "SFist"
      },
      "author": "Matt Charnock",
      "title": "Kaiser Emergency Room in San Jose Center of COVID-19 Outbreak After 43 Staff Members Test Positive - SFist",
      "description": "At least 43 staff members at the Kaiser Permanente San Jose Emergency Department have tested positive for COVID-19 within the past week. The unlikely culprit for the explosive outbreak? An inflatable Christmas costume.",
      "url": "https://sfist.com/2021/01/03/kaiser-emergency-room-in-san-jose-center-of-covid-19-outbreak-after-43-staff-members-test-positive/",
      "urlToImage": "https://img.sfist.com/2021/01/GettyImages-1208953647.jpg",
      "publishedAt": "2021-01-03T21:18:32Z",
      "content": "At least 43 staff members at the Kaiser Permanente San Jose Emergency Department have tested positive for COVID-19 within the past week. The unlikely culprit for the explosive outbreak? An inflatable… [+3038 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "BGR"
      },
      "author": "Chris Smith",
      "title": "‘Ozark’ fans need to watch this Netflix original series ASAP - BGR",
      "description": "Inhuman Resources is a Netflix original released in May 2020, and it’s a family drama similar to Netflix’s hugely popular Ozark. A middle-aged man who was fired from his HR position a few years ago struggles to get by, accepting any job that would help him ma…",
      "url": "https://bgr.com/2021/01/03/netflix-originals-2020-inhuman-resources-review/",
      "urlToImage": "https://bgr.com/wp-content/uploads/2021/01/netflix-inhuman-resources-eric-cantona.jpg?quality=70&strip=all",
      "publishedAt": "2021-01-03T20:24:00Z",
      "content": "<ul><li>Inhuman Resources is a Netflix original released in May 2020, and it’s a family drama similar to Netflix’s hugely popular Ozark.</li><li>A middle-aged man who was fired from his HR position a… [+5785 chars]"
    },
    {
      "source": {
        "id": "engadget",
        "name": "Engadget"
      },
      "author": "",
      "title": "Watch Tesla's Full Self-Driving navigate from SF to LA with (almost) no help - Engadget",
      "description": "A Tesla Model 3 with Full Self-Driving traveled from San Francisco to Los Angeles with virtually no intervention, although it's not truly autonomous yet.",
      "url": "https://www.engadget.com/tesla-self-driving-sf-to-la-trip-191412851.html",
      "urlToImage": "https://o.aolcdn.com/images/dims?resize=1200%2C630&crop=1200%2C630%2C0%2C0&quality=95&image_uri=https%3A%2F%2Fs.yimg.com%2Fos%2Fcreatr-uploaded-images%2F2021-01%2F30794e20-4df3-11eb-97b6-ecf57a2a6461&client=amp-blogside-v2&signature=1bc35f6811f3379e0ed8e10e95802aa7404a4744",
      "publishedAt": "2021-01-03T19:56:32Z",
      "content": "It’s a notable feat for technology that’s still rough around the edges and, despite its name, doesn’t let you take your hands off the wheel. At the same time, it’s also indicative of how far the tech… [+717 chars]"
    }
  ]
}
```

- Test

```kotlin
class NewsAPIServiceTest {

    private lateinit var service: NewsAPIService
    private lateinit var server: MockWebServer

    @Before
    fun setUp() {
        server = MockWebServer()
        service = Retrofit.Builder()
            .baseUrl(server.url(""))
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(NewsAPIService::class.java)
    }

    private fun enqueueMockResponse(
      fileName:String
    ){
      val inputStream = javaClass.classLoader!!.getResourceAsStream(fileName)
      val source = inputStream.source().buffer()
      val mockResponse = MockResponse()
      mockResponse.setBody(source.readString(Charsets.UTF_8))
      server.enqueue(mockResponse)

    }

    @Test
    fun getTopHeadlines_sentRequest_receivedExpected(){
        runBlocking {
            enqueueMockResponse("newsresponse.json")
            val responseBody = service.getTopHeadlines("us",1).body()
            val request = server.takeRequest()
            assertThat(responseBody).isNotNull()
            assertThat(request.path).isEqualTo("/v2/top-headlines?country=us&page=1&apiKey=b7122b5c5f8948eda9715867b6240ce6")
        }
    }

    @Test
    fun getTopHeadlines_receivedResponse_correctPageSize(){
      runBlocking {
          enqueueMockResponse("newsresponse.json")
          val responseBody = service.getTopHeadlines("us",1).body()
          val articlesList = responseBody!!.articles
          assertThat(articlesList.size).isEqualTo(20)
      }
    }

    @Test
    fun getTopHeadlines_receivedResponse_correctContent(){
        runBlocking {
            enqueueMockResponse("newsresponse.json")
            val responseBody = service.getTopHeadlines("us",1).body()
            val articlesList = responseBody!!.articles
            val article = articlesList[0]
            assertThat(article.author).isEqualTo("Saheli Roy Choudhury")
            assertThat(article.url).isEqualTo("https://www.cnbc.com/2021/01/04/samsung-galaxy-unpacked-2021.html")
            assertThat(article.publishedAt).isEqualTo("2021-01-04T03:25:00Z")
        }
    }

    @After
    fun tearDown() {
       server.shutdown()
    }
}
```

# 5. LocalDataSource

- ArticleDatabase.kt

```kotlin
@Database(
    entities = [Article::class],
    version = 1,
    exportSchema = false
)
@TypeConverters(Converters::class)
abstract  class ArticleDatabase : RoomDatabase(){
    abstract fun getArticleDAO():ArticleDAO
}
```

⇒ This will be initialized with `DI`

- ArticleDao.kt

```kotlin
@Dao
interface ArticleDAO {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(article: Article)

    @Query("SELECT * FROM articles")
    fun getAllArticles(): Flow<List<Article>>

    @Delete
    suspend fun deleteArticle(article: Article)

}
```

- Converters.kt

```kotlin

class Converters {
    @TypeConverter
    fun fromSource(source: Source):String{
           return source.name
    }
    @TypeConverter
    fun toSource(name:String):Source{
        return Source(name,name)
    }
}

```

- NewsLocalDataSource.kt

```kotlin
interface NewsLocalDataSource {
    suspend fun saveArticleToDB(article: Article)
    fun getSavedArticles(): Flow<List<Article>>
    suspend fun deleteArticlesFromDB(article: Article)
}
```

- NewsLocalDataSourceImpl

```kotlin
class NewsLocalDataSourceImpl(
    private val articleDAO: ArticleDAO
) : NewsLocalDataSource {
    override suspend fun saveArticleToDB(article: Article) {
        articleDAO.insert(article)
    }

    override fun getSavedArticles(): Flow<List<Article>> {
        return articleDAO.getAllArticles()
    }

    override suspend fun deleteArticlesFromDB(article: Article) {
        articleDAO.deleteArticle(article)
    }
}
```

# 6. NewsRespositoryImpl

- All combined together

```kotlin
class NewsRepositoryImpl(
        private val newsRemoteDataSource: NewsRemoteDataSource,
        private val newsLocalDataSource: NewsLocalDataSource
):NewsRepository {
    override suspend fun getNewsHeadlines(country : String, page : Int): Resource<APIResponse> {
        return responseToResource(newsRemoteDataSource.getTopHeadlines(country,page))
    }

    override suspend fun getSearchedNews(
        country: String,
        searchQuery: String,
        page: Int
    ): Resource<APIResponse> {
        return responseToResource(
            newsRemoteDataSource.getSearchedNews(country,searchQuery,page)
        )
    }

    private fun responseToResource(response:Response<APIResponse>):Resource<APIResponse>{
        if(response.isSuccessful){
            response.body()?.let {result->
                return Resource.Success(result)
            }
        }
        return Resource.Error(response.message())
    }


    override suspend fun saveNews(article: Article) {
        newsLocalDataSource.saveArticleToDB(article)
    }

    override suspend fun deleteNews(article: Article) {
        newsLocalDataSource.deleteArticlesFromDB(article)
    }

    override fun getSavedNews(): Flow<List<Article>> {
        return newsLocalDataSource.getSavedArticles()
    }
}
```

---

# 0. Hilt

```kotlin
@HiltAndroidApp
class NewsApp : Application()
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class AdapterModule {
   @Singleton
   @Provides
   fun provideNewsAdapter():NewsAdapter{
       return NewsAdapter()
   }
}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class DataBaseModule {
    @Singleton
    @Provides
    fun provideNewsDatabase(app: Application): ArticleDatabase {
        return Room.databaseBuilder(app, ArticleDatabase::class.java, "news_db")
            .fallbackToDestructiveMigration()
            .build()
    }

    @Singleton
    @Provides
    fun provideNewsDao(articleDatabase: ArticleDatabase): ArticleDAO {
        return articleDatabase.getArticleDAO()
    }

}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class FactoryModule {
    @Singleton
    @Provides
  fun provideNewsViewModelFactory(
     application: Application,
     getNewsHeadlinesUseCase: GetNewsHeadlinesUseCase,
     getSearchedNewsUseCase: GetSearchedNewsUseCase,
     saveNewsUseCase: SaveNewsUseCase,
     getSavedNewsUseCase: GetSavedNewsUseCase,
     deleteSavedNewsUseCase: DeleteSavedNewsUseCase
  ):NewsViewModelFactory{
      return NewsViewModelFactory(
          application,
          getNewsHeadlinesUseCase,
          getSearchedNewsUseCase,
          saveNewsUseCase,
          getSavedNewsUseCase,
          deleteSavedNewsUseCase
      )
  }

}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class LocalDataModule {
    @Singleton
    @Provides
    fun provideLocalDataSource(articleDAO: ArticleDAO):NewsLocalDataSource{
      return NewsLocalDataSourceImpl(articleDAO)
    }

}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class NetModule {
    @Singleton
    @Provides
    fun provideRetrofit(): Retrofit {
         return Retrofit.Builder()
             .addConverterFactory(GsonConverterFactory.create())
             .baseUrl(BuildConfig.BASE_URL)
             .build()
    }

    @Singleton
    @Provides
    fun provideNewsAPIService(retrofit: Retrofit):NewsAPIService{
        return retrofit.create(NewsAPIService::class.java)
    }

}
```

```kotlin
@InstallIn(SingletonComponent::class)
@Module
class RemoteDataModule {

    @Singleton
    @Provides
    fun provideNewsRemoteDataSource(
        newsAPIService: NewsAPIService
    ):NewsRemoteDataSource{
       return NewsRemoteDataSourceImpl(newsAPIService)
    }

}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class RepositoryModule {

    @Singleton
    @Provides
    fun provideNewsRepository(
        newsRemoteDataSource: NewsRemoteDataSource,
        newsLocalDataSource: NewsLocalDataSource
    ): NewsRepository {
        return NewsRepositoryImpl(
            newsRemoteDataSource,
            newsLocalDataSource
        )
    }

}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
class UseCaseModule {
   @Singleton
   @Provides
   fun provideGetNewsheadLinesUseCase(
       newsRepository: NewsRepository
   ):GetNewsHeadlinesUseCase{
      return GetNewsHeadlinesUseCase(newsRepository)
   }

   @Singleton
   @Provides
   fun provideGetSearchedNewsUseCase(
      newsRepository: NewsRepository
   ):GetSearchedNewsUseCase{
      return GetSearchedNewsUseCase(newsRepository)
   }

   @Singleton
   @Provides
   fun provideSaveNewsUseCase(
      newsRepository: NewsRepository
   ):SaveNewsUseCase{
      return SaveNewsUseCase(newsRepository)
   }

   @Singleton
   @Provides
   fun provideGetSavedNewsUseCase(
      newsRepository: NewsRepository
   ):GetSavedNewsUseCase{
      return GetSavedNewsUseCase(newsRepository)
   }

   @Singleton
   @Provides
   fun provideDeleteSavedNewsUseCase(
      newsRepository: NewsRepository
   ):DeleteSavedNewsUseCase{
      return DeleteSavedNewsUseCase(newsRepository)
   }
}
```

# 1. Presentation NavGraph

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/newsFragment">

    <fragment
        android:id="@+id/newsFragment"
        android:name="com.anushka.newsapiclient.NewsFragment"
        android:label="fragment_news"
        tools:layout="@layout/fragment_news" >
        <action
            android:id="@+id/action_newsFragment_to_infoFragment"
            app:destination="@id/infoFragment" />
    </fragment>
    <fragment
        android:id="@+id/savedFragment"
        android:name="com.anushka.newsapiclient.SavedFragment"
        android:label="fragment_saved"
        tools:layout="@layout/fragment_saved" >
        <action
            android:id="@+id/action_savedFragment_to_infoFragment"
            app:destination="@id/infoFragment" />
    </fragment>
    <fragment
        android:id="@+id/infoFragment"
        android:name="com.anushka.newsapiclient.InfoFragment"
        android:label="fragment_info"
        tools:layout="@layout/fragment_info" >
        <argument
            android:name="selected_article"
            app:argType="com.anushka.newsapiclient.data.model.Article" />
    </fragment>
</navigation>
```

# 2. Hosting Activity

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject
    lateinit var factory: NewsViewModelFactory
    @Inject
    lateinit var newsAdapter: NewsAdapter
    lateinit var viewModel: NewsViewModel
    private lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.fragment) as NavHostFragment
        val navController = navHostFragment.navController

        binding.bnvNews.setupWithNavController(
           navController
        )
        viewModel = ViewModelProvider(this,factory)
            .get(NewsViewModel::class.java)
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="7"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bnv_news"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="@color/search_background"
        app:itemTextColor="@color/list_text"
        app:itemIconTint="@color/list_text"
        app:menu="@menu/bottom_menu" />
</LinearLayout>
```

# 3. Presentation, get News

### NewsViewModel

```kotlin
class NewsViewModel(
    private val app:Application,
    private val getNewsHeadlinesUseCase: GetNewsHeadlinesUseCase,
    private val getSearchedNewsUseCase: GetSearchedNewsUseCase,
    private val saveNewsUseCase: SaveNewsUseCase,
    private val getSavedNewsUseCase: GetSavedNewsUseCase,
    private val deleteSavedNewsUseCase: DeleteSavedNewsUseCase
) : AndroidViewModel(app) {
    val newsHeadLines: MutableLiveData<Resource<APIResponse>> = MutableLiveData()

    fun getNewsHeadLines(country: String, page: Int) = viewModelScope.launch(Dispatchers.IO) {
        newsHeadLines.postValue(Resource.Loading())
        try{
      if(isNetworkAvailable(app)) {

          val apiResult = getNewsHeadlinesUseCase.execute(country, page)
          newsHeadLines.postValue(apiResult)
      }else{
          newsHeadLines.postValue(Resource.Error("Internet is not available"))
      }

        }catch (e:Exception){
            newsHeadLines.postValue(Resource.Error(e.message.toString()))
        }

    }

    private fun isNetworkAvailable(context: Context?):Boolean{
        if (context == null) return false
        val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
            val capabilities = connectivityManager.getNetworkCapabilities(connectivityManager.activeNetwork)
            if (capabilities != null) {
                when {
                    capabilities.hasTransport(NetworkCapabilities.TRANSPORT_CELLULAR) -> {
                        return true
                    }
                    capabilities.hasTransport(NetworkCapabilities.TRANSPORT_WIFI) -> {
                        return true
                    }
                    capabilities.hasTransport(NetworkCapabilities.TRANSPORT_ETHERNET) -> {
                        return true
                    }
                }
            }
        } else {
            val activeNetworkInfo = connectivityManager.activeNetworkInfo
            if (activeNetworkInfo != null && activeNetworkInfo.isConnected) {
                return true
            }
        }
        return false

    }

    //search
    val searchedNews : MutableLiveData<Resource<APIResponse>> = MutableLiveData()

    fun searchNews(
        country: String,
        searchQuery : String,
        page: Int
    ) = viewModelScope.launch {
       searchedNews.postValue(Resource.Loading())
        try {
            if (isNetworkAvailable(app)) {
                val response = getSearchedNewsUseCase.execute(
                    country,
                    searchQuery,
                    page
                )
                searchedNews.postValue(response)
            } else {
                searchedNews.postValue(Resource.Error("No internet connection"))
            }
        }catch(e:Exception){
            searchedNews.postValue(Resource.Error(e.message.toString()))
        }
    }

    //local data
    fun saveArticle(article: Article) = viewModelScope.launch {
        saveNewsUseCase.execute(article)
    }

    fun getSavedNews() = liveData{
        getSavedNewsUseCase.execute().collect {
            emit(it)
        }
    }

    fun deleteArticle(article: Article) = viewModelScope.launch {
        deleteSavedNewsUseCase.execute(article)
    }

}
```

### NewsViewModelFactory

```kotlin
class NewsViewModelFactory(
    private val app:Application,
    private val getNewsHeadlinesUseCase: GetNewsHeadlinesUseCase,
    private val getSearchedNewsUseCase: GetSearchedNewsUseCase,
    private val saveNewsUseCase: SaveNewsUseCase,
    private val getSavedNewsUseCase: GetSavedNewsUseCase,
    private val deleteSavedNewsUseCase: DeleteSavedNewsUseCase
):ViewModelProvider.Factory {
    override fun <T : ViewModel?> create(modelClass: Class<T>): T {
        return NewsViewModel(
            app,
            getNewsHeadlinesUseCase,
            getSearchedNewsUseCase,
            saveNewsUseCase,
            getSavedNewsUseCase,
            deleteSavedNewsUseCase
        ) as T
    }
}
```

### fragment_news.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/layout_background"
    tools:context=".NewsFragment">

    <SearchView
        android:id="@+id/svNews"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/search_background" />

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rvNews"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="5dp">
    </androidx.recyclerview.widget.RecyclerView>
    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyle"
        android:visibility="invisible"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

### NewsFragment.kt

```kotlin
class NewsFragment : Fragment() {
    private  lateinit var viewModel: NewsViewModel
    private lateinit var newsAdapter: NewsAdapter
    private lateinit var fragmentNewsBinding: FragmentNewsBinding
    private var country = "us"
    private var page = 1
    private var isScrolling = false
    private var isLoading = false
    private var isLastPage = false
    private var pages = 0
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_news, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        fragmentNewsBinding = FragmentNewsBinding.bind(view)
        viewModel= (activity as MainActivity).viewModel
        newsAdapter= (activity as MainActivity).newsAdapter
        newsAdapter.setOnItemClickListener {
          val bundle = Bundle().apply {
             putSerializable("selected_article",it)
          }
          findNavController().navigate(
              R.id.action_newsFragment_to_infoFragment,
              bundle
          )
        }
        initRecyclerView()
        viewNewsList()
        setSearchView()
    }

    private fun viewNewsList() {

        viewModel.getNewsHeadLines(country,page)
        viewModel.newsHeadLines.observe(viewLifecycleOwner,{response->
            when(response){
               is Resource.Success->{

                     hideProgressBar()
                     response.data?.let {
                         Log.i("MYTAG","came here ${it.articles.toList().size}")
                         newsAdapter.differ.submitList(it.articles.toList())
                         if(it.totalResults%20 == 0) {
                              pages = it.totalResults / 20
                         }else{
                             pages = it.totalResults/20+1
                         }
                         isLastPage = page == pages
                     }
               }
                is Resource.Error->{
                   hideProgressBar()
                   response.message?.let {
                       Toast.makeText(activity,"An error occurred : $it", Toast.LENGTH_LONG).show()
                   }
                }

                is Resource.Loading->{
                    showProgressBar()
                }

            }
        })
    }

    private fun initRecyclerView() {
       // newsAdapter = NewsAdapter()
        fragmentNewsBinding.rvNews.apply {
            adapter = newsAdapter
            layoutManager = LinearLayoutManager(activity)
            addOnScrollListener(this@NewsFragment.onScrollListener)
        }

    }

    private fun showProgressBar(){
        isLoading = true
        fragmentNewsBinding.progressBar.visibility = View.VISIBLE
    }

    private fun hideProgressBar(){
        isLoading = false
        fragmentNewsBinding.progressBar.visibility = View.INVISIBLE
    }

   private val onScrollListener = object : RecyclerView.OnScrollListener(){
       override fun onScrollStateChanged(recyclerView: RecyclerView, newState: Int) {
           super.onScrollStateChanged(recyclerView, newState)
           if(newState == AbsListView.OnScrollListener.SCROLL_STATE_TOUCH_SCROLL){
               isScrolling = true
           }

       }

       override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
           super.onScrolled(recyclerView, dx, dy)
           val layoutManager = fragmentNewsBinding.rvNews.layoutManager as LinearLayoutManager
           val sizeOfTheCurrentList = layoutManager.itemCount
           val visibleItems = layoutManager.childCount
           val topPosition = layoutManager.findFirstVisibleItemPosition()

           val hasReachedToEnd = topPosition+visibleItems >= sizeOfTheCurrentList
           val shouldPaginate = !isLoading && !isLastPage && hasReachedToEnd && isScrolling
           if(shouldPaginate){
               page++
               viewModel.getNewsHeadLines(country,page)
               isScrolling = false

           }

       }
   }

   //search
   private fun setSearchView(){
     fragmentNewsBinding.svNews.setOnQueryTextListener(
         object : SearchView.OnQueryTextListener{
             override fun onQueryTextSubmit(p0: String?): Boolean {
                 viewModel.searchNews("us",p0.toString(),page)
                 viewSearchedNews()
                 return false
             }

             override fun onQueryTextChange(p0: String?): Boolean {
                 MainScope().launch {
                     delay(2000)
                     viewModel.searchNews("us", p0.toString(), page)
                     viewSearchedNews()
                 }
                 return false
             }

         })

         fragmentNewsBinding.svNews.setOnCloseListener(
             object :SearchView.OnCloseListener{
                 override fun onClose(): Boolean {
                     initRecyclerView()
                     viewNewsList()
                     return false
                 }

             })
   }

   fun viewSearchedNews(){
       viewModel.searchedNews.observe(viewLifecycleOwner,{response->
           when(response){
               is Success->{

                   hideProgressBar()
                   response.data?.let {
                       Log.i("MYTAG","came here ${it.articles.toList().size}")
                       newsAdapter.differ.submitList(it.articles.toList())
                       if(it.totalResults%20 == 0) {
                           pages = it.totalResults / 20
                       }else{
                           pages = it.totalResults/20+1
                       }
                       isLastPage = page == pages
                   }
               }
               is Error->{
                   hideProgressBar()
                   response.message?.let {
                       Toast.makeText(activity,"An error occurred : $it", Toast.LENGTH_LONG).show()
                   }
               }

               is Loading->{
                   showProgressBar()
               }

           }
       })
   }

}
```

### news_list_item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:background="@color/list_background"
    android:layout_marginTop="10dp"
    >
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:ellipsize="end"
        android:maxLines="3"
        android:textColor="@color/list_text"
        android:textSize="15sp"
        android:textStyle="bold"
        android:layout_marginBottom="10sp"
        />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        >
        <ImageView
            android:id="@+id/ivArticleImage"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="2"
            />

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="3"
            android:orientation="vertical"
            android:layout_marginStart="10dp"
            >
            <TextView
                android:id="@+id/tvDescription"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:maxLines="5"
                android:text="DESCRIPTION"
                android:textColor="@color/list_text"
                android:textSize="15sp"
                android:layout_weight="3"
                />
            <TextView
                android:id="@+id/tvPublishedAt"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="@color/list_text"
                />

            <TextView
                android:id="@+id/tvSource"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textColor="@color/list_text"
                />

        </LinearLayout>

    </LinearLayout>

</LinearLayout>
```

### NewsAdapter

```kotlin
class NewsAdapter:RecyclerView.Adapter<NewsAdapter.NewsViewHolder>() {

    private val callback = object : DiffUtil.ItemCallback<Article>(){
        override fun areItemsTheSame(oldItem: Article, newItem: Article): Boolean {
            return oldItem.url == newItem.url
        }

        override fun areContentsTheSame(oldItem: Article, newItem: Article): Boolean {
           return oldItem == newItem
        }

    }

    val differ = AsyncListDiffer(this,callback)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): NewsViewHolder {
        val binding = NewsListItemBinding
            .inflate(LayoutInflater.from(parent.context),parent,false)
        return NewsViewHolder(binding)
    }

    override fun onBindViewHolder(holder: NewsViewHolder, position: Int) {
        val article = differ.currentList[position]
        holder.bind(article)
    }

    override fun getItemCount(): Int {
        return differ.currentList.size
    }

    inner class NewsViewHolder(
        val binding:NewsListItemBinding):
        RecyclerView.ViewHolder(binding.root){
           fun bind(article: Article){
               Log.i("MYTAG","came here ${article.title}")
               binding.tvTitle.text = article.title
               binding.tvDescription.text = article.description
               binding.tvPublishedAt.text = article.publishedAt
               binding.tvSource.text = article.source?.name

               Glide.with(binding.ivArticleImage.context).
               load(article.urlToImage).
               into(binding.ivArticleImage)

               binding.root.setOnClickListener {
                  onItemClickListener?.let {
                        it(article)
                  }
               }
           }
        }

        private var onItemClickListener :((Article)->Unit)?=null

        fun setOnItemClickListener(listener : (Article)->Unit){
            onItemClickListener = listener
        }

}
```

# 4. Presentation - Info (webview)

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".InfoFragment">

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab_save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:clickable="true"
        android:src="@drawable/ic_baseline_save_24"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <WebView
        android:id="@+id/wv_info"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </WebView>
</androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
class InfoFragment : Fragment() {
    private lateinit var fragmentInfoBinding: FragmentInfoBinding
    private lateinit var viewModel : NewsViewModel

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_info, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        fragmentInfoBinding = FragmentInfoBinding.bind(view)
        val args : InfoFragmentArgs by navArgs()
        val article = args.selectedArticle

        viewModel=(activity as MainActivity).viewModel

        fragmentInfoBinding.wvInfo.apply {
          webViewClient = WebViewClient()
          if(article.url!=null) {
              loadUrl(article.url)
          }

        }
        fragmentInfoBinding.fabSave.setOnClickListener {
           viewModel.saveArticle(article)
           Snackbar.make(view,"Saved Successfully!",Snackbar.LENGTH_LONG).show()
        }
    }
}
```

# 5. Presentation - Info (webview)

### fragment_saved.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/layout_background"
    tools:context=".SavedFragment">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rvSaved"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="5dp">
    </androidx.recyclerview.widget.RecyclerView>

</LinearLayout>
```

### SavedFragment.kt

```kotlin
class SavedFragment : Fragment() {
    private lateinit var fragmentSavedBinding: FragmentSavedBinding
    private lateinit var viewModel: NewsViewModel
    private lateinit var newsAdapter: NewsAdapter

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_saved, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        fragmentSavedBinding = FragmentSavedBinding.bind(view)
        viewModel = (activity as MainActivity).viewModel
        newsAdapter = (activity as MainActivity).newsAdapter
        newsAdapter.setOnItemClickListener {
            val bundle = Bundle().apply {
                putSerializable("selected_article",it)
            }
            findNavController().navigate(
                R.id.action_newsFragment_to_infoFragment,
                bundle
            )
        }
        initRecyclerView()
        viewModel.getSavedNews().observe(viewLifecycleOwner,{
            newsAdapter.differ.submitList(it)
        })

        val itemTouchHelperCallback = object : ItemTouchHelper.SimpleCallback(
          ItemTouchHelper.UP or ItemTouchHelper.DOWN,
            ItemTouchHelper.LEFT or ItemTouchHelper.RIGHT
        ){
            override fun onMove(
                recyclerView: RecyclerView,
                viewHolder: RecyclerView.ViewHolder,
                target: RecyclerView.ViewHolder
            ): Boolean {
                return true
            }

            override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
                val position = viewHolder.adapterPosition
                val article = newsAdapter.differ.currentList[position]
                viewModel.deleteArticle(article)
                Snackbar.make(view,"Deleted Successfully",Snackbar.LENGTH_LONG)
                    .apply {
                        setAction("Undo"){
                            viewModel.saveArticle(article)
                        }
                        show()
                    }

            }

        }

        ItemTouchHelper(itemTouchHelperCallback).apply {
            attachToRecyclerView(fragmentSavedBinding.rvSaved)
        }

    }

    private fun initRecyclerView(){
      fragmentSavedBinding.rvSaved.apply {
          adapter = newsAdapter
          layoutManager = LinearLayoutManager(activity)
      }
    }

}
```

### news_list_item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:background="@color/list_background"
    android:layout_marginTop="10dp"
    >
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:ellipsize="end"
        android:maxLines="3"
        android:textColor="@color/list_text"
        android:textSize="15sp"
        android:textStyle="bold"
        android:layout_marginBottom="10sp"
        />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        >
        <ImageView
            android:id="@+id/ivArticleImage"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="2"
            />

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="3"
            android:orientation="vertical"
            android:layout_marginStart="10dp"
            >
            <TextView
                android:id="@+id/tvDescription"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:maxLines="5"
                android:text="DESCRIPTION"
                android:textColor="@color/list_text"
                android:textSize="15sp"
                android:layout_weight="3"
                />
            <TextView
                android:id="@+id/tvPublishedAt"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="@color/list_text"
                />

            <TextView
                android:id="@+id/tvSource"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textColor="@color/list_text"
                />

        </LinearLayout>

    </LinearLayout>

</LinearLayout>
```

### NewsAdapter

```kotlin
class NewsAdapter:RecyclerView.Adapter<NewsAdapter.NewsViewHolder>() {

    private val callback = object : DiffUtil.ItemCallback<Article>(){
        override fun areItemsTheSame(oldItem: Article, newItem: Article): Boolean {
            return oldItem.url == newItem.url
        }

        override fun areContentsTheSame(oldItem: Article, newItem: Article): Boolean {
           return oldItem == newItem
        }

    }

    val differ = AsyncListDiffer(this,callback)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): NewsViewHolder {
        val binding = NewsListItemBinding
            .inflate(LayoutInflater.from(parent.context),parent,false)
        return NewsViewHolder(binding)
    }

    override fun onBindViewHolder(holder: NewsViewHolder, position: Int) {
        val article = differ.currentList[position]
        holder.bind(article)
    }

    override fun getItemCount(): Int {
        return differ.currentList.size
    }

    inner class NewsViewHolder(
        val binding:NewsListItemBinding):
        RecyclerView.ViewHolder(binding.root){
           fun bind(article: Article){
               Log.i("MYTAG","came here ${article.title}")
               binding.tvTitle.text = article.title
               binding.tvDescription.text = article.description
               binding.tvPublishedAt.text = article.publishedAt
               binding.tvSource.text = article.source?.name

               Glide.with(binding.ivArticleImage.context).
               load(article.urlToImage).
               into(binding.ivArticleImage)

               binding.root.setOnClickListener {
                  onItemClickListener?.let {
                        it(article)
                  }
               }
           }
        }

        private var onItemClickListener :((Article)->Unit)?=null

        fun setOnItemClickListener(listener : (Article)->Unit){
            onItemClickListener = listener
        }

}
```
