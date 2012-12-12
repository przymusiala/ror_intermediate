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
  * Logowanie przez zewnętrzną aplikację

<!SLIDE transition=fade>

# Prawa i obowiązki userów i adminów
## [Cancan od Rayana] (https://github.com/ryanb/cancan)

<!SLIDE transition=fade>

# Poczekalnia obrazków
## Małe poprawki w modelu i dodanie [scope] (http://guides.rubyonrails.org/active_record_querying.html#scopes)'a
    
    @@@ ruby
      scope :published, where(:published => true)

<!SLIDE transition=fade>

# Maile dla leniwych adminów
## Czyli [Emacsem przez sendmail] (http://www.youtube.com/watch?v=wFXLzr86MQ4)

<!SLIDE transition=fade>

# Kategorie dla obrazków
## Podzielmy na kategorie, dodajmy TOP wyświetleń

<!SLIDE transition=fade>

# Komentarze z FB i ładne URL'e
## bez [friendly_id] (https://github.com/norman/friendly_id) ani rusz.
