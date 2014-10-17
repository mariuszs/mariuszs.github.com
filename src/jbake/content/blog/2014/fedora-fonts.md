title=Czcionki w Fedorze
date=2014-10-11
type=post
tags=blog
status=published
~~~~~~

Czcionki
----

Silnik renderowania czcionek z włączonym renderowaniem subpikselowym - możemy go legalnie używać, ponieważ nie obowiązują w naszym kraju patenty na oprogramowanie.

    sudo yum install freetype-freeworld
    
Nie polecam korzystania z [Infinality](http://www.infinality.net/blog/) projekt najwyraźniej nie jest już dłużej rozwijany (ostatnia wydanie jest z 2013-05-15). Pojawił się za to nowy projekt http://bohoomil.com/, aktualnie bez wsparcia dla Fedory.    
       
[Google noto fonts](https://code.google.com/p/noto/)
    
    sudo yum install google-noto-sans-fonts google-noto-serif-fonts google-noto-sans-symbols-fonts  
    
[Google Droid fonts](https://android.googlesource.com/)
        
    sudo yum install google-droid-serif-fonts google-droid-sans-mono-fonts google-droid-sans-fonts  

[Inconsolata](http://www.levien.com/type/myfonts/inconsolata.html)

    sudo yum install levien-inconsolata-fonts
                      
[Adobe Source Code Pro](http://adobe-fonts.github.io/source-code-pro/)
                      
    sudo yum install adobe-source-sans-pro-fonts                      

[Adobe Source Sans Pro](http://adobe-fonts.github.io/source-sans-pro/)
                      
    sudo yum install adobe-source-sans-pro-fonts                      

Konfiguracja czcionek
---

Jeśli korzystamy z monitora LCD należy ustawić odpowiedni filtr 
   
    sudo ln -s /usr/share/fontconfig/conf.avail/11-lcdfilter-default.conf /etc/fonts/conf.d/11-lcdfilter-default.conf
            
Hinting i wygładzanie RGB

    gsettings "set" "org.gnome.settings-daemon.plugins.xsettings" "hinting" "full"     
    
Aby poprawnie ustawić wygładzanie RGB należy [sprawdzić posiadaną matrycę](http://www.lagom.nl/lcd-test/subpixel.php). 
            
Ustawienie per użytkownika    

    gsettings "set" "org.gnome.settings-daemon.plugins.xsettings" "antialiasing" "rgba"    

Ustawienia globalne przez mechanizm fontconfig (zalecane)

    sudo ln -s /usr/share/fontconfig/conf.avail/10-sub-pixel-rgb.conf /etc/fonts/conf.d/10-sub-pixel-rgb.conf    

W konfiguracji tych opcji można nam pomóc [gnome-tweak-tool](https://wiki.gnome.org/action/show/Apps/GnomeTweakTool?action=show&redirect=GnomeTweakTool)

    yum install gnome-tweak-tool

Efekt konfiguracji można zweryfikować poleceniem `xrdb -query`   
                                                                                                                                 
    Xft.antialias:	1
    Xft.dpi:	120
    Xft.hinting:	1
    Xft.hintstyle:	hintfull
    Xft.lcdfilter:	lcddefault
    Xft.rgba:	rgb

Własne konfiguracje dla czcionek należy umieszczać w katalogu `~/.config/fontconfig/conf.d` lub pliku `~/.config/fontconfig/fonts.conf`.

Jak to wygląda
---

|                                                                               | Bez hintingu                                                                   | Slight hinting                                                                 | Full hinting
|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------|------------------------------------------
|Bez wygładzenia                                                                |![alt text](/hinting/no-no.png "No aliasing No hinting")                        |![alt text](/hinting/no-slight.png "No aliasing Slight hinting")                      |![alt text](/hinting/no-full.png "No aliasing Full hinting")
|Wygładzenie w odcieniach szarości                                              |![alt text](/hinting/greyscale-no.png "Greyscale aliasing No hinting") |![alt text](/hinting/greyscale-slight.png "Greyscale aliasing Slight hinting")|![alt text](/hinting/greyscale-full.png "Greyscale aliasing Full hinting") 
|Wygładzenie Rgba                                                               |![alt text](/hinting/rgba-no.png "RGBA aliasing No hinting")           |![alt text](/hinting/rgba-slight.png "RGBA aliasing Slight hinting")         |![alt text](/hinting/rgba-full.png "RGBA aliasing Full hinting")

      
Aby uaktywnić wygładzanie Rgba niezbędna jest instalacja pakietu `freetype-freeworld`.         
   
   
Przykładowe renderingi z wygładzaniem RGBA przy użyciu standardowego pakietu `freetype` oraz pakietu `freetype-freeworld`. 
    
|`freetype`                                                  |`freetype-freeworld`
|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------   
|![alt text](/hinting/lorem-patented.png "Patented RGBa example")                 |![alt text](/hinting/lorem-no-patented.png "Patented RGBa example")            

Instalacja czcionek z Windows
---

Montujemy dysk z Windowsem i kopiujemy czcionki z `/Windows/Fonts` do `~/.fonts`.
   
Po skopiowaniu odświeżamy czcionki (indeksujemy)
   
    fc-cache -fv
   
Warto poczytać:
----
    
 * [Fonts and font sizes in Fedora on the internet: hold onto your hats…](https://www.happyassassin.net/2014/01/17/fonts-and-font-sizes-in-fedora-on-the-internet-hold-onto-your-hats/)
 * [How to get gorgeous looking fonts on ubuntu linux](http://www.binarytides.com/gorgeous-looking-fonts-ubuntu-linux/)
 * [How to change Fedora's font rendering to get an Ubuntu-like result](http://blog.andreas-haerter.com/2011/07/18/tune-improve-fedora-fonts-typeface-ubuntu-like-sharp-fonts)
 * [Gentoo Wiki - Fontconfig](http://wiki.gentoo.org/wiki/Fontconfig)
 * [Improving Font Rendering for Fedora Using Bytecode Interpreter](http://www.fclose.com/2805/improving-font-rendering-for-fedora-using-bytecode-interpreter/)
 * [Subpixel rendering](https://en.wikipedia.org/wiki/Subpixel_rendering)
 * [ClearType Patents, FreeType and the Unix Desktop: an explanation](http://david.freetype.org/cleartype-patents.html
 * [FreeType - CHANGES BETWEEN 2.4.10 and 2.4.11](http://sourceforge.net/projects/freetype/files/freetype2/2.4.11/README/view)
 https://code.google.com/p/chromium/issues/detail?id=29871