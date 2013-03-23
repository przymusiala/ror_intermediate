<!SLIDE title-slide transition=fade>

# Część 2 #

<!SLIDE transition=fade>

## Zróbmy swojego kwejko-demota

<!SLIDE transition=fade>

# Przygotujmy ImageMagick

<!SLIDE commandline incremental transition=fade>

    $ sudo apt-get install imagemagick
    $ sudo apt-get install librmagick-ruby libmagickwand-dev

<!SLIDE smaller commandline incremental transition=fade>

# Nowa aplikacja

    $ rails new kwejkodemot -m \
    $   https://raw.github.com/RailsApps/rails-composer/master/composer.rb -T

<!SLIDE transition=fade>

    @@@ ruby
      # Gemfile
      
      gem 'rmagick'

<!SLIDE commandline incremental transition=fade>

    $ bundle install

<!SLIDE commandline incremental transition=fade>

# Jesli to nie pomoże..

    $ apt-get install libmagickcore-dev graphicsmagick-libmagick-dev-compat

<!SLIDE smaller bullets incremental transition=fade>

# Plan na aplikację
  
  * Jakie moduły?
  * Jakie gemy?
  * Jakie inne biblioteki?
  * Jaka baza danych?

<!SLIDE transition=fade>

# Do dzieła!
