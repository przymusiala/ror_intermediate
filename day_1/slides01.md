<!SLIDE title-slide transition=fade>

# Część 1 #

<!SLIDE smaller bullets incremental transition=fade>

# Przypomnienie :)

  * Ruby - w jakiej wersji jest obecnie?
  * Rails - jak stworzyć nową aplikację?
  * RVM - jak zainstalować Rubiego w wersji 1.9.3?
  * GIT - jak się klonuje a jak commituje?
  * Bundler - po co to?
  * Scaffold?
  * ... :)

<!SLIDE smaller bullets incremental transition=fade>

# Krok pierwszy
## DRY - nie traćmy czasu na podstawy

<!SLIDE smaller bullets incremental transition=fade>

# Przykładowa aplikacja z github'a

  * [github.com/RailsApps/rails3-bootstrap-devise-cancan] (https://github.com/RailsApps/rails3-bootstrap-devise-cancan)
  * Forkujemy repo i klonujemy na swoją maszynę
  * Uruchamiamy

<!SLIDE smaller bullets incremental transition=fade>

# albo

<!SLIDE smaller commandline incremental transition=fade>

# Generowanie rails-composer'em

    $ rails new ror_intermediate -m \
    $   https://raw.github.com/RailsApps/rails-composer/master/composer.rb -T

<!SLIDE small transition=fade>

### Trzeba pamiętać jeszcze o uzupełnieniu Gemfile o odpowienie gemy 
### jeśli chcemy użyć Twitter bootstrap czy Devise...

<!SLIDE title-slide transition=fade>

# Zmiana edytora

<!SLIDE small bullets incremental transition=fade>

# Sublime Text 2

  * Sublime Text 2: [sublimetext.com] (http://www.sublimetext.com)
  * Poprawmy wygląd: [github.com/buymeasoda/soda-theme] (https://github.com/buymeasoda/soda-theme)

<!SLIDE smaller bullets incremental transition=fade>

# Skróty klawiszowe

  * __Cmd + P__ pokaż okienko wyszukiwania nazwy pliku we wszystkich otwartych folderach i plikach. 
    * # – wyszukuje tekst w otwartym pliku 
    * : – przechodzi do linii o numerze...
    * @ – wyszukuje symbol (nazwa klasy, funkcji, zmiennej) 
  * __Cmd + Shift + D__ powiela wybraną linię lub zaznaczony fragment tekstu
  * __Cmd + D__ zaznacza kolejne wystąpienie zaznaczonego tekstu, lub zaznacza wybrane słowo, jeśli znajduje się w nim kursor
  * __tab__ podpowiada w obrębie jednego pliku
  
<!SLIDE smaller bullets incremental transition=fade>

# Skróty klawiszowe

  * __Cmd + Shift + Spacja__ rozszerz zaznaczenie
  * __Cmd + /__ zakomentuj lub odkomentuj wybrane linie
  * __Cmd + J__ złącza wybrane linie
  * __Cmd + ]__ zwiększ wcięcie tekstu
  * __Cmd + [__ zminiejsz wcięcie tekstu
  * __def + Tab__ nowa funkcja w *.rb

<!SLIDE smaller bullets incremental transition=fade>

# Zadania

  * przejdż do application controllera
  * stwórz kilka funkcji
  * dodaj do każdej nazwy funkcji (a = 1) na końcu
  * zmień (a = 1) na (@a = 1)
  * zamień tekst podzielony w akapity w jeden długi tekst 
  * zakomentuj wszystko i zwiększ wcięcie o dwie spacje
