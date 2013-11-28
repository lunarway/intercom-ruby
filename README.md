# intercom-ruby

Ruby bindings for the Intercom API (https://api.intercom.io).

[API Documentation](https://api.intercom.io/docs)

[Gem Documentation](http://rubydoc.info/github/intercom/intercom-ruby/master/frames)

For generating Intercom javascript script tags for Rails, please see https://github.com/intercom/intercom-rails

## Installation

    gem install intercom

Using bundler:

    gem 'intercom'

## Basic Usage

### Configure your access credentials

```ruby
Intercom.app_id = "my_app_id"
Intercom.api_key = "my-super-crazy-api-key"
```

### Resources

The API supports:

    POST,PUT,GET https://api.intercom.io/v1/users
    POST,PUT,GET https://api.intercom.io/v1/users/messages
    POST https://api.intercom.io/v1/users/impressions
    POST https://api.intercom.io/v1/users/notes

### Examples

#### Users

```ruby
user = Intercom::User.find_by_email("bob@example.com")
user.custom_data["average_monthly_spend"] = 1234.56
user.save
user = Intercom::User.find_by_user_id("bob@example.com")
user = Intercom::User.create(:email => "bob@example.com", :name => "Bob Smith")
user = Intercom::User.new(params)
user.save
Intercom::User.all.count
Intercom::User.all.each {|user| puts %Q(#{user.email} - #{user.custom_data["average_monthly_spend"]}) }
Intercom::User.all.map {|user| user.email }
```

#### Companies
```ruby
user = Intercom::User.find_by_email("bob@example.com")
user.company = {:id => 6, :name => "Intercom"}
user.companies = [{:id => 6, :name => "Intercom"}, {:id => 9, :name => "Test Company"}]
```

You can also pass custom data within a company:

```ruby
user.company = {:id => 6, :name => "Intercom", :referral_source => "Google"}
```

#### Message Threads
```ruby
Intercom::MessageThread.create(:email => "bob@example.com", :body => "Example message from bob@example.com to your application on Intercom.")
Intercom::MessageThread.find(:email => "bob@example.com", :thread_id => 123)
Intercom::MessageThread.find_all(:email => "bob@example.com")
Intercom::MessageThread.mark_as_read(:email => "bob@example.com", :thread_id => 123)
```

#### Impressions
```ruby
Intercom::Impression.create(:email => "bob@example.com", :location => "/path/in/my/app", :user_ip => "1.2.3.4", :user_agent => "my-savage-iphone-app-0.1"
```

#### Notes
```ruby
Intercom::Note.create(:email => "bob@example.com", :body => "This is the text of the note")
```

### Errors
```ruby
Intercom::AuthenticationError
Intercom::ServerError
Intercom::ServiceUnavailableError
Intercom::ResourceNotFound
```
