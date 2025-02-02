# Resend Ruby and Rails SDK

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
![Build](https://github.com/drish/resend-ruby/actions/workflows/build.yml/badge.svg)
[![Gem Version](https://badge.fury.io/rb/resend.svg)](https://badge.fury.io/rb/resend)
---

## Installation

To install Resend Ruby and Rails SDK, simply execute the following command in a terminal:

Via RubyGems:
```
gem install resend
```

Via Gemfile:
```
gem 'resend', '~>0.2.0'
```

## Setup

First, you need to get an API key, which is available in the [Resend Dashboard](https://resend.com).

```ruby
require "resend"
client = Resend::Client.new "re_YOUR_API_KEY"
```

## Example

```rb
require "resend"

client = Resend::Client.new "re_YOUR_API_KEY"

params = {
  "from": "team@recomendo.io",
  "to": "carlosderich@gmail.com",
  "html": "<h1>Hello World</h1>",
  "subject": "Hey"
}
r = client.send_email(params)
puts r
```

# Rails and ActiveMailer support

This gem can be used as an ActionMailer delivery method, add this to your `config/environments/environment.rb` file and replace with your api key.

```ruby
config.action_mailer.delivery_method = :resend
config.action_mailer.resend_settings = {
    api_key: 'resend_api_key',
}
```

After that you can deliver_now!, example below:

```ruby
#/app/mailers/user_mailer
class UserMailer < ApplicationMailer
  default from: 'you@yourdomain.io'
  def welcome_email
    @user = params[:user]
    @url  = 'http://example.com/login'
    mail(to: ["example2@mail.com", "example1@mail.com"], subject: 'Hello from Resend')
  end
end

# anywhere in the app
u = User.new name: "derich"
mailer = UserMailer.with(user: u).welcome_email
mailer.deliver_now!
# => {:id=>"b8f94710-0d84-429c-925a-22d3d8f86916", from: 'you@yourdomain.io', to: ["example2@mail.com", "example1@mail.com"]}
```

This gem can also be initialized with a Rails initializer file, example below:

```ruby
# /app/initializers/mailer.rb
Resend.configure do |config|
  config.api_key = 'resend_api_key'
end
```
