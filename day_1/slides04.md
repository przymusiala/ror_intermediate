<!SLIDE title-slide transition=fade>

# Część 4 #

<!SLIDE title-slide transition=fade>

# Przykłady zaawansowane

<!SLIDE smaller bullets incremental transition=fade>

  * Dodaj metodę przeszukującą po "inicjałach" nazwy produktu

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Wszystkie posty z polem `text`(:name, :description, 
      # lub :comments) zawierające 'rubin'
      Post.search { fulltext 'rubin' }

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty z rubinem, wyskoczą wyżej
      # jeśli rubin pojawi się w nazwie
      
      Post.search do
        fulltext 'rubin' do
          boost_fields :name => 2.0
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posts z rubinem, wyżej jeśli post promowany
      
      Post.search do
        fulltext 'rubin' do
          boost(2.0) { with(:featured, true) }
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty z rubinem *tylko* w nazwie
      
      Post.search do
        fulltext 'rubin' do
          fields(:name)
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty z rubinem w nazwie dopalone
      # a z rubinem w opisie - nie
      
      Post.search do
        fulltext 'rubin' do
          fields(:description, :name => 2.0)
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Tylko dokładne dopasowania

      Post.search do
        fulltext '"great pizza"'
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty z blog_id równym 1
      
      Post.search do
        with(:blog_id, 1)
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Post ze średnią oceną pomiędzy 3.0 a 5.0
      
      Post.search do
        with(:average_rating, 3.0..5.0)
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty z kategoriami 1, 3, lub 5
      
      Post.search do
        with(:category_ids, [1, 3, 5])
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posty opublikowane co najmniej tydzień temu
      
      Post.search do
        with(:published_at).greater_than(1.week.ago)
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Wszystko tylko nie kategorie nr 1 i 3

      Post.search do
        without(:category_ids, [1, 3])
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Pokombinujmy...

      Post.search do
        any_of do
          with(:blog_id, 1)
          all_of do
            with(:blog_id, 2)
            with(:category_ids, 3)
          end
        end
      end

      # Albo te z blog_id = 1
      # albo z blog_id = 2 i category_id = 3

<!SLIDE smaller transition=fade>

    @@@ ruby
      # I nowość - szukanie po koordynatach

      class Post < ActiveRecord::Base
        searchable do
          # ...
          latlon(:location) { 
            Sunspot::Util::Coordinates.new(lat, lon)
          }
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Szukamy postów w odległości 100 kilometrów od (32, -68)
      
      Post.search do
        with(:location).in_radius(32, -68, 100)
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Szuakmy w prostokącie pomiędzy
      # punktami (45, -94) a (46, -93)

      Post.search do
        with(:location).in_bounding_box([45, -94], [46, -93])
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      # Posortuj wg. odległości od punktu zero

      Post.search do
        order_by_geodist(:location, 32, -68)
      end