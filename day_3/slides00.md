<!SLIDE title-slide transition=fade>

# Dzień trzeci #

<!SLIDE title-slide transition=fade>

# A gdzie Wasze repo?

<!SLIDE smaller bullets incremental transition=fade>

# Plan na dziś #
  
  * Prawa i obowiązki userów i adminów
  * Poczekalnia obrazków
  * Maile dla leniwych adminów
  * Kategorie dla obrazków
  * Komentarze z FB i ładne URL'e

<!SLIDE transition=fade>

# Prawa i obowiązki userów i adminów
## [Cancan od Rayana] (https://github.com/ryanb/cancan)

<!SLIDE smaller transition=fade>

# Poczekalnia obrazków
## Małe poprawki w modelu i dodanie [scope] (http://guides.rubyonrails.org/active_record_querying.html#scopes)'a
    
    @@@ ruby
      scope :published,   where(:published => true)
      scope :unpublished, where(:published => false)

<!SLIDE commandline incremental transition=fade>

    $ rails g controller WaitingPhotos index

<!SLIDE transition=fade>

# Maile dla leniwych adminów
## Czyli [Emacsem przez sendmail] (http://www.youtube.com/watch?v=wFXLzr86MQ4)

<!SLIDE transition=fade>

    $ rails generate mailer UserMailer

<!SLIDE smaller transition=fade>

    @@@ ruby
      class UserMailer < ActionMailer::Base
        default :from => "notifications@example.com"
       
        def welcome_email(user)
          @user = user
          @url  = "ulr do naszego obrazka"
          mail(:to => user.email, :subject => "Dodano nowy obrazek!1!!one")
        end
      end

<!SLIDE smaller bullets incremental transition=fade>

# TOP wyświetleń
  * Dodajmy pole visits do modelu Photo 
  * Zinkrementujmy licznik przy każdym wyświetleniu
  * Dodajmy kontroler dla najpopularniejszych obrazków

<!SLIDE transition=fade>

# Komentarze z FB i ładne URL'e
## bez [friendly_id] (https://github.com/norman/friendly_id) ani rusz.

<!SLIDE smaller transition=fade>

    @@@ ruby
      #fb-root
        %script{ :src => 
          "https://connect.facebook.net/pl_PL/all.js#xfbml=1"
        }
        %fb:comments{ :href => 
          "#{ActiveSupport::Inflector.transliterate(
              CGI::unescape(request.url)
            ).gsub('http://www.', 'http://').gsub(
              /\?.+\z/, ''
            )}",
          :width =>'600' }

<!SLIDE transition=fade>

# Kategorie dla obrazków

<!SLIDE smaller transition=fade>

    @@@ ruby
      def self.up
        create_table 'categories_photos', :id => false do |t|
          t.integer :category_id, :null => false
          t.integer :photo_id,    :null => false
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # W kontrolerze należy uzupełnić zmienną @categories
      # Formularz edycji zdjęcia
      = form_for(@photo) do |f|
        # .....
        - for category in @categories
          = check_box_tag "photo[category_ids][]",
            "#{category.id}", 
            @photo.categories.include?(category)
          = category.name

<!SLIDE smaller transition=fade>
  
# Oddzielna akcja w kontrolerze na obrazki z różnych kategorii