<!SLIDE title-slide transition=fade>

# Dzień piąty #

<!SLIDE smaller bullets incremental transition=fade>

# Plan na dziś #

  * Podział na moduły
  * Dobre praktyki
  * Generowanie sitemap
  * A może mongoDB?
  * Logowanie przez zewnętrzną aplikację

<!SLIDE smaller transition=fade>

# Podział na moduły

<!SLIDE smaller transition=fade>

# Dobre praktyki

<!SLIDE smaller transition=fade>

# Findery do scope'a

<!SLIDE smaller transition=fade>

Przed:

class PostsController < ApplicationController
  def index
    @public_posts = Post.where( state: 'published' ).limit( 10 ).order( 'created_at desc' )
    @draft_posts  = Post.where( state: 'draft' ).limit( 10 ).order( 'created_at desc' )
  end
end

<!SLIDE smaller transition=fade>

Po:

class UsersController < ApplicationController
  def index
    @published_post = Post.published
    @draft_post = Post.draft
  end
end

class Post < ActiveRecord::Base
  scope :published, where( state: 'published' ).limit( 10 ).order( 'created_at desc' )
  scope :draft, where( state: 'draft' ).limit( 10 ).order( 'created_at desc' )
end

<!SLIDE smaller transition=fade>

model association

<!SLIDE smaller transition=fade>

class PostsController < ApplicationController
  def create
    @post = Post.new(params[:post])
    @post.user_id = current_user.id
    @post.save
  end
end

<!SLIDE smaller transition=fade>

class PostsController < ApplicationController
  def create
    @post = current_user.posts.build(params[:post])
    @post.save
  end
end

<!SLIDE smaller transition=fade>

 scope access

<!SLIDE smaller transition=fade>

class PostsController < ApplicationController
  def edit
    @post = Post.find(params[:id)
    if @post.current_user != current_user
      flash[:warning] = 'Access denied'
      redirect_to posts_url
    end
  end
end

<!SLIDE smaller transition=fade>

class PostsController < ApplicationController
  def edit
    # raise RecordNotFound exception (404 error) if not found
    @post = current_user.posts.find(params[:id)
  end
end

<!SLIDE smaller transition=fade>

model virtual attribute

<!SLIDE smaller transition=fade>

<% form_for @user do |f| %>
    <%= text_filed_tag :full_name %>
<% end %>

class UsersController < ApplicationController
  def create
    @user = User.new(params[:user)
    @user.first_name = params[:full_name].split(' ', 2).first
    @user.last_name = params[:full_name].split(' ', 2).last
    @user.save
end end

<!SLIDE smaller transition=fade>

Po:

class User < ActiveRecord::Base
          def full_name
            [first_name, last_name].join(' ')
end
          def full_name=(name)
            split = name.split(' ', 2)
            self.first_name = split.first
            self.last_name = split.last
end end

<!SLIDE smaller transition=fade>

<% form_for @user do |f| %>
  <%= f.text_field :full_name %>
<% end %>
class UsersController < ApplicationController
  def create
    @user = User.create(params[:user)
end end

<!SLIDE smaller transition=fade>

 model callback

 <% form_for @post do |f| %>
  <%= f.text_field :content %>
  <%= check_box_tag 'auto_tagging' %>
<% end %>
class PostController < ApplicationController
  def create
    @post = Post.new(params[:post])
    if params[:auto_tagging] == '1'
      @post.tags = AsiaSearch.generate_tags(@post.content)
else
      @post.tags = ""
    end
    @post.save
  end
end

<!SLIDE smaller transition=fade>

class Post < ActiveRecord::Base
  attr_accessor :auto_tagging
  before_save :generate_taggings
private
def generate_taggings
return unless auto_tagging == '1' self.tags = Asia.search(self.content)
end end

<!SLIDE smaller transition=fade>

<% form_for :note, ... do |f| %>
  <%= f.text_field :content %>
  <%= f.check_box :auto_tagging %>
<% end
class PostController < ApplicationController
  def create
    @post = Post.new(params[:post])
    @post.save
end end

<!SLIDE smaller transition=fade>

Replace Complex Creation with Factory Method

<!SLIDE smaller transition=fade>

class InvoiceController < ApplicationController
      def create
        @invoice = Invoice.new(params[:invoice])
        @invoice.address = current_user.address
        @invoice.phone = current_user.phone
        @invoice.vip = ( @invoice.amount > 1000 )
        if Time.now.day > 15
          @invoice.delivery_time = Time.now + 2.month
else
          @invoice.delivery_time = Time.now + 1.month
        end
        @invoice.save
      end
Before
end

<!SLIDE smaller transition=fade>

class Invoice < ActiveRecord::Base
       def self.new_by_user(params, user)
         invoice = self.new(params)
         invoice.address = user.address
         invoice.phone = user.phone
         invoice.vip = ( invoice.amount > 1000 )
         if Time.now.day > 15
           invoice.delivery_time = Time.now + 2.month
else
           invoice.delivery_time = Time.now + 1.month
         end
end end

<!SLIDE smaller transition=fade>

class InvoiceController < ApplicationController
  def create
    @invoice = Invoice.new_by_user(params[:invoice], current_user)
    @invoice.save
  end
end

<!SLIDE smaller transition=fade>

 Move Model Logic into the Model

<!SLIDE smaller transition=fade>

 class PostController < ApplicationController
     def publish
       @post = Post.find(params[:id])
       @post.update_attribute(:is_published, true)
       @post.approved_by = current_user
       if @post.create_at > Time.now - 7.days
         @post.popular = 100
       else
         @post.popular = 0
       end
       redirect_to post_url(@post)
     end
end

<!SLIDE smaller transition=fade>

class Post < ActiveRecord::Base
         def publish
           self.is_published = true
           self.approved_by = current_user
           if self.create_at > Time.now-7.days
             self.popular = 100
           else
             self.popular = 0
           end
end end

<!SLIDE smaller transition=fade>

class PostController < ApplicationController
  def publish
    @post = Post.find(params[:id])
    @post.publish
    redirect_to post_url(@post)
  end
end

<!SLIDE smaller transition=fade>

 Keep Finders on Their Own Model

  class Post < ActiveRecord::Base
      has_many :comments
      def find_valid_comments
        self.comment.find(:all, :conditions => { :is_spam => false },
:limit => 10)
end end
    class Comment < ActiveRecord::Base
      belongs_to :post
end
    class CommentsController < ApplicationController
      def index
        @comments = @post.find_valid_comments
      end
end

<!SLIDE smaller transition=fade>

Law of Demeter
   class Invoice < ActiveRecord::Base
     belongs_to :user
end
   <%= @invoice.user.name %>
   <%= @invoice.user.address %>
   <%= @invoice.user.cellphone %>

<!SLIDE smaller transition=fade>

class Invoice < ActiveRecord::Base
  belongs_to :user
  delegate :name, :address, :cellphone, :to => :user,
end
<%= @invoice.user_name %>
<%= @invoice.user_address %>
<%= @invoice.user_cellphone %>

<!SLIDE smaller transition=fade>

INDEKSY!!1!!one!

:)

<!SLIDE smaller transition=fade>

 Use before_filter

<!SLIDE smaller transition=fade>

 class PostController < ApplicationController
  def show
    @post = current_user.posts.find(params[:id]
end
  def edit
    @post = current_user.posts.find(params[:id]
end
  def update
    @post = current_user.posts.find(params[:id]
    @post.update_attributes(params[:post])
end
  def destroy
    @post = current_user.posts.find(params[:id]
    @post.destroy
end end

<!SLIDE smaller transition=fade>

class PostController < ApplicationController
  before_filter :find_post, :only => [:show, :edit, :update, :destroy]
  def update
    @post.update_attributes(params[:post])
end
  def destroy
    @post.destroy
end
protected
  def find_post
    @post = current_user.posts.find(params[:id])
end end

<!SLIDE smaller transition=fade>

Move code into model

<!SLIDE smaller transition=fade>

<% if current_user && (current_user == @post.user || @post.editors.include?(current_user) %>
        <%= link_to 'Edit this post', edit_post_url(@post) %>
      <% end %>
      <% if @post.editable_by?(current_user) %>
        <%= link_to 'Edit this post', edit_post_url(@post) %>
<% end %>
      class Post < ActiveRecord::Base
        def ediable_by?(user)
          user && ( user == self.user || self.editors.include?(user)
        end
end

<!SLIDE smaller transition=fade>

   class Post < ActiveRecord::Base
       has_many :comments
     end
     class Comment < ActiveRecord::Base
       belongs_to :post
       named_scope :only_valid, :conditions => { :is_spam => false }
       named_scope :limit, lambda { |size| { :limit => size } }
     end
     class CommentsController < ApplicationController
       def index
         @comments = @post.comments.only_valid.limit(10)
       end
end


<!SLIDE smaller transition=fade>

# Warto wygenerować [site-mapę] (https://github.com/kjvarga/sitemap_generator)

<!SLIDE smaller bullets incremental transition=fade>

# A może mongoDB?

  * [mongoDB] (http://www.mongodb.org/)
  * [Mongoid] (http://mongoid.org/en/mongoid/index.html)

<!SLIDE smaller bullets incremental transition=fade>

# Logowanie przez [zewnętrzną aplikację] (http://railscasts.com/episodes/304-omniauth-identity?view=asciicast)