# Android Boilerplate
Este projeto usa como referência o modelo usado pela [ribot](http://ribot.co.uk). Serve para iniciar com uma arquitetura, as ferramentas e guidelines a serem utilizadas para desenvolver um aplicativo Android.

Bibliotecas e ferramentas utilizadas:

- Support libraries
- RecyclerViews e CardViews
- [RxJava](https://github.com/ReactiveX/RxJava) and [RxAndroid](https://github.com/ReactiveX/RxAndroid)
- [Retrofit 2](http://square.github.io/retrofit/)
- [Dagger 2](http://google.github.io/dagger/)
- [SqlBrite](https://github.com/square/sqlbrite)
- [Butterknife](https://github.com/JakeWharton/butterknife)
- [Timber](https://github.com/JakeWharton/timber)
- [Glide](https://github.com/bumptech/glide)
- [AutoValue](https://github.com/google/auto/tree/master/value) with extensions [AutoValueParcel](https://github.com/rharter/auto-value-parcel) and [AutoValueGson](https://github.com/rharter/auto-value-gson)
- [Checkstyle](http://checkstyle.sourceforge.net/), [PMD](https://pmd.github.io/) and [Findbugs](http://findbugs.sourceforge.net/) for code analysis

## Requerimentos

- JDK 1.8
- [Android SDK](http://developer.android.com/sdk/index.html).
- Android N [(API 25) ](http://developer.android.com/tools/revisions/platforms.html).
- Latest Android SDK Tools and build tools.


## Architecture

Esse projeto segue a arquitetura baseada em [MVP (Model View Presenter)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter). Para saber mais leia [here](https://github.com/ribot/android-guidelines/blob/master/architecture_guidelines/android_architecture.md).

![](https://github.com/ribot/android-guidelines/raw/master/architecture_guidelines/architecture_diagram.png)

### Como implementar uma nova tela seguindo o MVP

Imagine que você tem que implementar uma tela de sign.

1. Criar um novo pacote dentro de `ui` chamado `signin`.
2. Criar uma nova Activity chamda `ActivitySignIn`. Também pode ser usado um fragment.
3. Definir uma interface para a view que a Activity irá implementar. Neste caso crie uma interface chamada `SignInMvpView` que extende `MvpView`. Adicione métodos que você considera necessário. Ex.: `showSignInSuccessful()`
4. Criar uma classe `SignInPresenter` que extende `BasePresenter<SignInMvpView>`
5. Implementar um método em `SignInPresenter` que sua Activity precisa para realizar alguma ação necessária. Ex.:`signIn(String email)`. Assim que a ação de sign in for finalizada você deve chamar `getMvpView().showSignInSuccessful()`.
6. Faça sua `ActivitySignIn` implementar `SignInMvpView` e implementar os métodos necessários `showSignInSuccessful()`
8. Na sua activity, injetar uma nova instância de `SignInPresenter` e chamar `presenter.attachView(this)` no `onCreate` e `presenter.detachView()` no `onDestroy()`. Também configurar um listener no para chamar `presenter.signIn(email)`.

## Qualidade de código

Esse projeto tem integração com ferramentas de análise de código. É fortemente recomendado utilizar os scripts de análise antes de realizar o commit para o repositório. Evitando sempre realizar commits com alertas e erros sendo detectados pelos scripts.

### Ferramentas de análise de código

As seguintes ferramentas de análise de código que estão configuradas para este projeto:

* [PMD](https://pmd.github.io/): Localiza as falhas mais simples como variáveis não utilizadas, blocos de Try/Catch vazios, criação desnecessária de objetos e assim por diante. Veja [this project's PMD ruleset](config/quality/pmd/pmd-ruleset.xml).

```
./gradlew pmd
```

* [Findbugs](http://findbugs.sourceforge.net/): Esta ferramenta usa uma análise estática para acar bugs no código Java. Ao contrário do PMD, ela usa bytecodes compilados do Java ao invés do código fonte.

```
./gradlew findbugs
```

* [Checkstyle](http://checkstyle.sourceforge.net/): Garante que o estilo do cógigo siga o padrão [our Android code guidelines](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md#2-code-guidelines). See our [checkstyle config file](config/quality/checkstyle/checkstyle-config.xml).

```
./gradlew checkstyle
```

### The check task
Para garantir que o seu código é válido e estável use:

```
./gradlew check
```

This will run all the code analysis tools and unit tests in the following order:

![Check Diagram](images/check-task-diagram.png)

## Distribution

The project can be distributed using either [Crashlytics](http://support.crashlytics.com/knowledgebase/articles/388925-beta-distributions-with-gradle) or the [Google Play Store](https://github.com/Triple-T/gradle-play-publisher).

### Play Store

We use the __Gradle Play Publisher__ plugin. Once set up correctly, you will be able to push new builds to
the Alpha, Beta or production channels like this

```
./gradlew publishApkRelease
```
Read [plugin documentation](https://github.com/Triple-T/gradle-play-publisher) for more info.

### Crashlytics

You can also use Fabric's Crashlytics for distributing beta releases. Remember to add your fabric
account details to `app/src/fabric.properties`.

To upload a release build to Crashlytics run:

```
./gradlew assembleRelease crashlyticsUploadDistributionRelease
```

## New project setup

To quickly start a new project from this boilerplate follow the next steps:

* Download this [repository as a zip](https://github.com/ribot/android-boilerplate/archive/master.zip).
* Change the package name.
  * Rename packages in main, androidTest and test using Android Studio.
  * In `app/build.gradle` file, `packageName` and `testInstrumentationRunner`.
  * In `src/main/AndroidManifest.xml` and `src/debug/AndroidManifest.xml`.
* Create a new git repository, [see GitHub tutorial](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).
* Replace the example code with your app code following the same architecture.
* In `app/build.gradle` add the signing config to enable release versions.
* Add Fabric API key and secret to fabric.properties and uncomment Fabric plugin set up in `app/build.gradle`
* Update `proguard-rules.pro` to keep models (see TODO in file) and add extra rules to file if needed.
* Update README with information relevant to the new project.
* Update LICENSE to match the requirements of the new project.

## License

```
    Copyright 2015 Ribot Ltd.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
```
