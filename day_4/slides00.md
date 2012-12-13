<!SLIDE title-slide transition=fade>

# Dzień czwarty #

<!SLIDE smaller bullets incremental transition=fade>

# Plan na dziś #
  
  * Głosowanie na najleszy obrazek
  * Paginacja przez [kaminari] (https://github.com/amatsuda/kaminari)
  * [Pliki na Dropbox] (https://github.com/janko-m/paperclip-dropbox)
  * Deploy na [heroku.com] (https://heroku.com)

<!SLIDE smaller bullets incremental transition=fade>

# Głosowanie na najleszy obrazek
  
  * Dodajemy tabelę pośredniczącą trzymającą oddane głosy i migrujemy bazę
  * Dodajemy relacje w modelach (has_many :photos, :through => :votes)
  * Tworzymy kontroler z akcją show który dodaje głos danego usera do danej fotki
  * Tworzymy linki do głosowania dostępne tylko dla zalogowanego usera
  * Tworzymy kontroler wyświetlający najlepsze zdjęcia względem liczby głosów

<!SLIDE smaller transition=fade>

    @@@ ruby
      # app/models/vote.rb
      class Vote < ActiveRecord::Base
        belongs_to :voter, :class_name => "User",
          :foreign_key => "user_id"
        belongs_to :photo
      end

      # app/models/user.rb
      class User < ActiveRecord::Base
        has_many :votes
        has_many :photos, :through => :votes
      end

      # app/models/photo.rb
      class Photo < ActiveRecord::Base
        has_many :votes
        has_many :voters, :class_name => "User",
          :foreign_key => "user_id", :through => :votes
      end
<!SLIDE smaller transition=fade>

    @@@ ruby
      # Można tak:
      @photos = Photo.all.sort {|x,y| 
        y.votes.count <=> x.votes.count } 

      # Ale lepiej dodać licznik w migracji:
      def self.up
        add_column :photos, :votes_count,
          :integer, :default => 0

        # Resets all the cached information about columns
        # which will cause them to be reloaded
        # on the next request
        Photo.reset_column_information
        Photo.find(:all).each do |p|
          Photo.update_counters p.id,
            :votes_count => p.votes.length
        end
      end

      # i w modelu Vote
      belongs_to :photo, :counter_cache => true

      # aktualizacja licznika po dodaniu
      # głosu tylko dla nowych rekordów

<!SLIDE smaller transition=fade>

    @@@ ruby
      - @photos.in_groups_of(3, false).each do |i|
        %tr
          - i.each do |ii|
            %td= ii

<!SLIDE smaller bullets incremental transition=fade>

# Paginacja przez [kaminari] (https://github.com/amatsuda/kaminari)

  * Paginacja dla wszystkich stron z obrazkami

<!SLIDE smaller bullets incremental transition=fade>

# Przenieśmy pliki na [Dropbox] (https://github.com/janko-m/paperclip-dropbox)

<!SLIDE smaller bullets incremental transition=fade>

# Deploy na [heroku.com] (https://heroku.com)