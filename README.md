# Espinita
## Audits activerecord models like a boss

![Alt text](./espinita.jpg)

Audit activerecord models like a boss, tested in rails 4 and ruby 2.0.0.


This proyect is heavily based in audited gem.

## Installation

In your gemfile 

    gem "espinita"

In console 

    $ rake espinita:install:migrations
    $ rake db:migrate

## Usage

    class Post < ActiveRecord::Base
      auditable
    end

    @post.create(title: "an awesome blog post" )

Espinita will create an audit by default on creation , edition and destroy:

    @post.audits.size #=> 1

Espinita provides options to include or exclude columns to trigger the creation of audit.

    class Post < ActiveRecord::Base
      auditable only: [:title] # except: [:some_column]
    end

And let you declare the callbacks you want for audit creation:

    class Post < ActiveRecord::Base
      auditable on: [:create]  # on: [:create, :update]
    end    

You can find the audits records easily: 

    @post.audits.first #=>  #<Espinita::Audit id: 1, auditable_id: 1, auditable_type: "Post", user_id: 1, user_type: "User", audited_changes: {"title"=>[nil, "MyString"], "created_at"=>[nil, 2013-10-30 15:50:14 UTC], "updated_at"=>[nil, 2013-10-30 15:50:14 UTC], "id"=>[nil, 1]}

Espinita will save the model changes in a serialized column called audited_changes:

    @post.audits.firt.audited_changed #=> {"title"=>[nil, "MyString"], "created_at"=>[nil, 2013-10-30 15:50:14 UTC], "updated_at"=>[nil, 2013-10-30 15:50:14 UTC], "id"=>[nil, 1]}

Espinita will detect current user when records saved from rails controllers. by default Espinita use current_user method but you can change it

    Espinita.current_user_method = :authenticated_user