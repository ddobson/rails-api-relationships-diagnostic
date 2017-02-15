# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    Join tables allow us to create many to many relationships and more effectively describe those relationships. A join table could also contain data useful about the relationship that is not directly relevant to either main resource. For instance, a library loan has a due date.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    profiles
      - id: serial integer primary key
      - given_name: text
      - surname: text
      - email: text
      - created_at: timestamp
      - updated_at: timestamp

    movies
      - id: serial integer primary key
      - title: text
      - release_date: date
      - length: integer
      - created_at: timestamp
      - updated_at: timestamp

    favorties
      - id: serial integer primary key
      - movie_id: integer forgien key
      - profile_id: integer forgien key
      - created_at: timestamp
      - updated_at: timestamp

    A profile and a movie each have many favorties. A profile has many movies through favorites. A movie has many profiles thorugh favorites. A favorite belongs to a movie and a profile.
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :favorites
    has_many :movies, through: :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :favorites
  has_many :profiles, through: :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profiles
    belongs_to :movie
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  A serializer tells our API what data to send back as a response to an http request.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :email, :movies

    def movies
      object.movies.pluck(:id)
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from the above `Movies` and `Profiles`.

  ```sh
    rails generate scaffold favorites profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  A dependent destroy tells rails that there is an association between to models and that when the main resource is destroyed, any dependent models should also be destroyed. We would use this in cases where the dependent resources does make any sense with the main resource.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well as a many-to-many relationship in an application. You only need to list the description about the resources and how they relate to one another.

  ```md
    recipe has many ingredients
    ingredient has many recipes
    recipe has many instructions
    instruction belongs to a recipe
  ```
