title=Java Geek Blog - jBake, Gradle and GitHub
date=2014-10-12
type=post
tags=blog
status=published
~~~~~~

Wymagania
====

* [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowsze, ale wersja 1.7 też jest będzie dobra
* [Gradle](http://www.gradle.org/) 2.1 lub nowszy
* [JBake](http://jbake.org/) 2.3.2 lub nowszy
* [IntelliJ IDEA 13.1](http://www.jetbrains.com/idea/) - opcjonalnie

Warto aby polecenia `gradle` oraz `jbake` były dostępne z linii poleceń. W przypadku korzystania powłoki fisha wystarczy dodać do `~/.config/fish/config.fish`

    set -x JBAKE_HOME ~/.local/tools/jbake
    set -x GRADLE_HOME ~/.local/tools/gradle
    
    set -x PATH $PATH $JBAKE_HOME/bin $GRADLE_HOME/bin

Rozpoczynamy
===

Tworzymy nowy publiczny projekt na [githubie](https://github.com/)


Tworzymy nowy katalog na projekt bloga

    mkdir ~/git/foo
    cd ~/git/foo
    git init
    git remote add origin git@github.com:USER/foo.git    
    mkdir -p src/jbake

Przygotowyjemy repozytorium gita    
    
    gradle init    
    echo -e ".idea\n*.iml\nbuild\n.gradle" > .gitignore
    git add --all
    git commit -m "gradle structure"

Inicjalizujemy bloga z szablonem thymeleafa

    cd src/jbake
    jbake -i thymeleaf
    cd ../..
    git add --all
    git commit -m "jbake structure"

    
Konfigurujemy gradla
----
        
Zawartość `build.gradle`
    
    buildscript {
        dependencies {
            classpath 'org.pegdown:pegdown:1.4.2'
            classpath 'org.thymeleaf:thymeleaf:2.1.3.RELEASE'
            classpath 'org.asciidoctor:asciidoctor-java-integration:0.1.4'
        }
    }
    
    plugins {
        id "me.champeau.jbake" version "0.2"
    }

Do `build.gradle` dodajemy konfigurację bloga:

    jbake {
        configuration['site.host'] = 'http://jbake.org'
        configuration['render.tags'] = false
        configuration['render.sitemap'] = true
        configuration['template.archive.file'] = 'archive.thyme'
        configuration['template.index.file'] = 'index.thyme'
        configuration['template.tag.file'] = 'tags.thyme'
        configuration['template.sitemap.file'] = 'sitemap.thyme'
        configuration['template.post.file'] = 'post.thyme'
        configuration['template.page.file'] = 'page.thyme'
        configuration['template.feed.file'] = 'feed.thyme'
    }

i usuwamy plik properties

    rm src/jbake/jbake.properties        

 Sprawdzamy czy blog kompiluje się bez błędów
 
     gradle jbake
     
Uruchamiamy lokalny serwer bloga
     
     jbake -s build/jbake/
     
Blog można obejrzeć pod adresem http://localhost:8820/     
     
Zapisujemy zmiany

    git add --all
    git commit -m "init blog"

Publikowanie bloga na githubie
----

Tworzymy plik `publish.gradle` z zawartością:

    plugins {
        id "org.ajoberstar.github-pages" version "0.11.1"
    }
    
    githubPages {
        repoUri = 'git@github.com:USER/foo.git'
        pages {
            from(file('build/jbake')) {
                into '.'
            }
        }
    }

Do `build.gradle` dodajemy:

    task publish(type: GradleBuild) {
        buildFile = 'publish.gradle'
        tasks = ['publishGhPages']
    }

Aby opublikować skompilowanego bloga należy na stronie githuba utworzyć branch o nazwie `gh-pages` i uruchomić:

    gradle publish
    
Możemy również utowrzyć branch `gh-pages` z linii poleceń:
    
    git checkout -b gh-pages
    git push origin gh-pages
    
Całość oczywiście zapisujemy
    
    git add --all
    git commit -m "add publish settings"

Jeśli chcemy zachować źródła naszego bloga na githubie to należy wykonać

    git push -u origin master
    
Więcej
------

* http://melix.github.io/blog/2014/02/hosting-jbake-github.html[Authoring your blog on GitHub with JBake and Gradle]
* http://blog.ackx.net/blogguer-sous-github-avec-jbake.html[Blogguer sous GitHub avec JBake]