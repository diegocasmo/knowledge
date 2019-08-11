# Using Rails Concerns to Encapsulate Data Access and Validation

Recently, I had to implement a feature that introduced the concept of currency to an API. Users of the API could create different type of resources, and each of these resources needed to know the type of currency being used. For instance, a user could create a `Payment` in USD, or request a `Withdrawal` in AUD.

In this short blog post, my goal is to explain how I used `ActiveSupport::Concern` to encapsulate and group together all data access and validation of the `currency` attribute in a single place.

### The Gist

I wanted currency to be an `enum` where each value was a [currency code](https://en.wikipedia.org/wiki/ISO_4217). Using `enum` allows to write queries by name, such as `Payment.first.currency.GBP?` or `Payment.where(currency: :AUD)`. If such logic was only needed in a single model, I would have probably done the following:

``` ruby
class Payment < ApplicationRecord
  enum currency: [ :USD, :AUD, :GBP ]
  validates :currency, inclusion: { in: currencies.keys }
end
```

Nonetheless, the same notion of currency needed to exist in multiple models. To solve this, I chose to use `ActiveSupport::Concern` in order to DRY the models and group related concerns of logic together. As described by DHH in [this blog post in 2012](https://signalvnoise.com/posts/3372-put-chubby-models-on-a-diet-with-concerns), `ActiveSupport::Concern` "encapsulate[s] both data access and domain logic about a certain slice of responsibility."

### The Solution

To implement the desired solution, I created a `Currency` module in `app/models/concerns/currency.rb` as follows:

``` ruby
module Currency
  extend ActiveSupport::Concern
  include ActiveModel::Validations

  included do
    enum currency: [ :USD, :AUD, :GBP ]
    validates :currency, inclusion: { in: currencies.keys }
  end
end
```

The `included` method takes a block, and it is executed at any time a module is included in another module or class. In this case, the `currency` module  defines an `enum` attribute which specifies a named value for each currency. It also validates the `currency` attribute is set to one of the values defined in the `enum`. To use the concern, simply include it:

``` ruby
class Payment < ApplicationRecord
  include Currency
end
```

``` ruby
class Withdrawal < ApplicationRecord
  include Currency
end
```

And that is it! The `Payment` and `Withdrawal` models now define how to access and validate the `currency` attribute (assuming a migration for the `currency` column was created and ran appropriately in each table).

### Testing

Testing shared behavior of classes or modules can be greatly simplified by using `rspec`'s shared examples. In `spec/support/shared_examples/currency.rb` I defined a shared example (note that I'm using the `shoulda-matchers` gem):

``` ruby
shared_examples_for 'currency' do
  let(:model) { described_class }

  it { should define_enum_for(:currency).with_values([ :USD, :AUD, :GBP ]) }

  describe 'validations' do
    it { should allow_value(model.currencies[:USD]).for(:currency) }
    it { should allow_value(model.currencies[:AUD]).for(:currency) }
    it { should allow_value(model.currencies[:GBP]).for(:currency) }
  end
end
```

To use it, simply include the shared example in the models context:

``` ruby
RSpec.describe Payment, type: :model do
  it_behaves_like 'currency'
end
```

``` ruby
RSpec.describe Withdrawal, type: :model do
  it_behaves_like 'currency'
end
```

Done! Models that include the concern can easily test its functionality by using a shared example as described above.

### Conclusion

In this short blog post, I have shown a simple solution to encapsulate and group together data access and validation of an attribute. Note I have chosen to use `ActiveSupport::Concern` because the same attribute is used by many models. If the `currency` attribute was only used by a single model, I would not have defined a concern for it. Finally, I have demonstrated how to use `rspec`'s shared examples to simplify testing shared behavior of classes or modules.

Have you ever implemented something similar? How did you do it? Let me know in a comment below :).
