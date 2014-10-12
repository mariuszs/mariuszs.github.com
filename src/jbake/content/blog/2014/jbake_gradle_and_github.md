title=Java Geek Blog - jBake, Gradle and GitHub
date=2014-10-12
type=post
tags=blog, jbake, gradle, thymeleaf, markdown, github
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
    
IntelliJ IDEA Community Edition w zupełności wystarczy do naszych potrzeb. Warto doinstalować sobie wtyczkę [idea-markdown](https://github.com/nicoulaj/idea-markdown) oraz powiązać pliki z rozszerzeniem `.thyme` z edytorem HTML.

![alt text](/idea_file_types.png ".thyme file type")   

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
    rm -rf src/jbake/content/blog/*    
    git add --all
    git commit -m "jbake structure"

    
Konfigurujemy gradla
----
        
Zawartość `build.gradle`
    
    buildscript {
        dependencies {
            classpath 'org.pegdown:pegdown:1.4.2'
            classpath 'org.thymeleaf:thymeleaf:2.1.3.RELEASE'
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

Pierwszy post
---

Przygotowujemy katalog na posty z roku 2014

    mkdir -p src/jbake/content/blog/2014
    
Tworzymy pierwszwego posta, nazwa jest dowolna np. testujemy_bloga.md

    title=Testujemy bloga
    date=2014-10-12
    type=post
    tags=blog
    status=published
    ~~~~~~
    
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque vel diam purus. Curabitur ut nisi lacus.

![alt text](/jbake_structure.png "Project structure")

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
    
Dodajemy możliwość komentowania na blogu
----

Zakładamy sobie konto w serwisie [disqus](https://disqus.com/). Nazwa która wybierzemyjako site identity shortname będzie nam potrzebna w konfiguracji bloga. Należy ją zapisać w zmiennej `disqus.username`.

W tym celu do build.gradle dodajemy kolejny wpis

    configuration['disqus.username'] = 'myuser'

W pliku szablonu `post.thyme` szukamy użycia `content.body` i umieszczamy poniżej panel do komentowania:

    <div id="disqus_thread"></div>
    <script type="text/javascript" th:inline="javascript">
        var disqus_shortname = [[${config.disqus_username}]];
        var disqus_identifier = [[${content.uri}]];
        var disqus_title = [[${content.title}]];
        var disqus_url = [[${config.site_host} + ${content.uri}]];
    
        /* * * DON'T EDIT BELOW THIS LINE * * */
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    
W pliku szablonu `index.thyme` zmieniamy urla do postów na:
    
    <a th:href="${post.uri} + '#disqus_thread'"  th:attr='data-disqus-identifier=${post.uri}'><h1 th:text='${post.title}'>title</h1></a>
    
W pliku szablonu `footer.thyme` dodajemy przed `</body>`:
     
    <script type="text/javascript" th:inline="javascript">
        var disqus_shortname = [[${config.disqus_username}]];
    
        /* * * DON'T EDIT BELOW THIS LINE * * */
        (function () {
            var s = document.createElement('script'); s.async = true;
            s.type = 'text/javascript';
            s.src = '//' + disqus_shortname + '.disqus.com/count.js';
            (document.getElementsByTagName('HEAD')[0] || document.getElementsByTagName('BODY')[0]).appendChild(s);
        }());
    </script>     
    
Więcej
------

* [JBake Documentation](http://jbake.org/docs/2.3.2/)
* [Tutorial: Using Thymeleaf](http://www.thymeleaf.org/doc/usingthymeleaf.html#introducing-thymeleaf)
* [Markdown Here Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet)
* [Authoring your blog on GitHub with JBake and Gradle](http://melix.github.io/blog/2014/02/hosting-jbake-github.html)
* [Blogguer sous GitHub avec JBake](http://blog.ackx.net/blogguer-sous-github-avec-jbake.html)
