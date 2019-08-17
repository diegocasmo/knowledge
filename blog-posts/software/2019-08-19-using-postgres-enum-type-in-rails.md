# Using Postgres Enum Type in Rails

Last week I wrote [Using Active Support Concerns to Encapsulate Data Access and Validation](https://dev.to/diegocasmo/using-active-support-concerns-to-encapsulate-data-access-and-validation-5b6c), where I explained how to use Active Support concerns to define data access and validation rules for an Active Record enum attribute. The attribute mapped values to integers in the database, similar to this example:

``` ruby
class Project < ActiveRecord::Base
  enum status: [ :active, :archived ]
  validates :status, inclusion: { in: statuses.keys }
end
```

Depending on the problem that you are trying to solve, this approach could potentially lead to several drawbacks including:

1. Meaningless integer values stored in the database.
2. Unless the integer value is "limited" in a migration, Active Model validations can be by-passed through plain SQL, effectively allowing the attribute to be set to any integer.
3. Plain SQL queries need to use the integer value, not the Active Record enum.

In this blog post, I'll show how to use the Postgres enum type with Rails to avoid the aforementioned pit falls.

### Postgres Enumerated Types

Postgres supports enumerated types, which are data types that comprise a static, ordered set of values. To create an enum type, use the Postgres `CREATE TYPE` command. In a Rails project, generate a migration as follows `rails g migration AddStatusToProjects`:

``` ruby
class AddStatusToProjects < ActiveRecord::Migration[5.2]
  def up
    execute <<-SQL
      CREATE TYPE project_status AS ENUM ('active', 'archived');
    SQL
    add_column :projects, :status, :project_status
  end

  def down
    remove_column :projects, :status
    execute <<-SQL
      DROP TYPE project_status;
    SQL
  end
end
```

The migration creates a `project_status` enumerated type. Next, it adds a `status` column to the `projects` table of type `project_status`. By using the Postgres enumerated type, the `status` attribute is constrain at the database level to be one of `active`|`archived`. Lastly, set `config.active_record.schema_format = :sql` in the environment configuration files, so that the database schema includes the `project_status` enumerated type definition.

### Active Record Enum

The initial definition of the `status` Active Record enum needs to be slightly modified. Simply use a hash to explicitly map the relation between the attribute and database value as follows:

``` ruby
class Project < ActiveRecord::Base
  enum status: { active: 'active', archived: 'archived' }
  validates :status, inclusion: { in: statuses.keys }
end
```

### Unit Tests

Unit tests can be tremendously simplified by using the `shoulda-matchers` gem:

``` ruby
RSpec.describe Project, type: :model do

  it { should define_enum_for(:status).
      with_values(
        active: 'active',
        archived: 'archived'
      ).backed_by_column_of_type(:enum) }

  it { should allow_values(:active, :archived).for(:status) }
end
```

The unit test verifies the `status` column is defined in the `Project` model and its column type is `enum`. Also, it makes sure the `status` enum values are correctly defined as specified in the `status` hash. Finally, the last test confirms the `Project` model allows the `status` attribute to be set using the `:active` and `:archived` symbols.

### Conclusion

And that was it! The `Project` model defines and validates an enum for the `status` attribute which is mapped to meaningful string values in the database. Additionally, the `status` column is constrain at the database level to be one of the specified values in the Postgres enumerated type.

If you are interested in learning more about enum in Rails, I highly recommend reading [Ruby on Rails - how to create perfect enum in 5 steps](https://naturaily.com/blog/ruby-on-rails-enum). This blog post, written by Błażej Pichur, has been a fantastic guide during the past few weeks to improve my understanding and usage of enum in Rails.
