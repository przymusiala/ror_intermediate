<!SLIDE title-slide transition=fade>

# Dzień piąty #

<!SLIDE smaller bullets incremental transition=fade>

# Plan na dziś #
  
  * Dobre praktyki
  * Podział na moduły
  * Generowanie sitemap
  * A może mongoDB?
  * Metaprogramowanie
  * Logowanie przez zewnętrzną aplikację

<!SLIDE smaller transition=fade>

# Dobre praktyki

<!SLIDE smaller transition=fade>

# Podział na moduły

<!SLIDE smaller transition=fade>

# Warto wygenerować [site-mapę] (https://github.com/kjvarga/sitemap_generator)

<!SLIDE smaller bullets incremental transition=fade>

# A może mongoDB?

  * [mongoDB] (http://www.mongodb.org/)
  * [Mongoid] (http://mongoid.org/en/mongoid/index.html)

<!SLIDE smaller transition=fade>

    @@@ ruby
      class Post < ActiveRecord::Base
        validate_inclusion_of :status, :in => ['draft', 'published', 'spam']
        def self.all_draft
          find(:all, :conditions => { :status => 'draft' }
        end
          def self.all_published
            find(:all, :conditions => { :status => 'published' }
        end
          def self.all_spam
            find(:all, :conditions => { :status => 'spam' }
        end
          def draft?
            self.stats == 'draft'
        end
          def published?
            self.stats == 'published'
        end
          def spam?
            self.stats == 'spam'
        end
      end

<!SLIDE smaller transition=fade>

    @@@ ruby
      class Post < ActiveRecord::Base
        STATUSES = ['draft', 'published', 'spam']
        validate_inclusion_of :status, :in => STATUSES
        class << self
          STATUSES.each do |status_name|
            define_method "all_#{status}" do
              find(:all, :conditions => { :status => status_name }
            end
          end
        end
        STATUSES.each do |status_name|
          define_method "#{status_name}?" do
            self.status == status_name
          end
        end
      end

<!SLIDE smaller bullets incremental transition=fade>

# Logowanie przez [zewnętrzną aplikację] (http://railscasts.com/episodes/304-omniauth-identity?view=asciicast)