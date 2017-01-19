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
- Ùltima versão do Android SDK Tools.


## Arquitetura

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

## Distribuição

Este projeto pode ser distribuido tanto pelo [Crashlytics](http://support.crashlytics.com/knowledgebase/articles/388925-beta-distributions-with-gradle) quanto pela [Google Play Store](https://github.com/Triple-T/gradle-play-publisher).

### Play Store

Nós usamos o __Gradle Play Publisher__ plugin. Uma vez configurado corretamente, você será capaz de enviar novas builds para os canais
Alpha, Beta or produção da seguinte forma:

```
./gradlew publishApkRelease
```
Leia [plugin documentation](https://github.com/Triple-T/gradle-play-publisher) para mais informações.

### Crashlytics

Você também pode usar o Fabric's Crashlytics para distribuir beta releases. Lembre de adicionar sua conta do fabric
para `app/src/fabric.properties`.

Para enviar uma build de release para o Crashlytics execute:

```
./gradlew assembleRelease crashlyticsUploadDistributionRelease
```

## New project setup

Para começar rápidamente um novo projeto apartir deste boilerplate siga os seguintes passos:

* Baixe deste [repositório como zip](https://github.com/ribot/android-boilerplate/archive/master.zip).
* Alterar o nome do pacote.
  * Renomear os pacotes na main using Android Studio.
  * Nos arquivos `app/build.gradle`, `packageName` e `testInstrumentationRunner`.
  * Também `src/main/AndroidManifest.xml` e `src/debug/AndroidManifest.xml`.
* Criar um novo repositório.
* Alterar todos os códigos de exemplo com o seu código seguindo a arquitetura.
* No `app/build.gradle` adicionar as configurações de signin para liberar versões de release.
* Adicionar a API Key e a secret Key do Fabric no fabric.properties e descomentar o Fabric plugin no `app/build.gradle`
* Atualizar `proguard-rules.pro` para manter o modelo (ler os TODO no arquivo) e adicionar regras extras caso necessário.
* Atualizar o README com informações relevantes para o novo projeto.
* Atualizar a licença de acordo com os requirementos do novo projeto.

## Licença

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
