[![Gem Version](https://badge.fury.io/rb/devise_userbin.png)](http://badge.fury.io/rb/devise_userbin)
[![Dependency Status](https://gemnasium.com/userbin/devise_userbin.png)](https://gemnasium.com/userbin/devise_userbin)

# DeviseUserbin

Adds support to [Devise](http://github.com/plataformatec/devise) for protecting your user accounts with [Userbin](https://userbin.com). Userbin is a cloud service that adds multi-factor authentication, user activity monitoring, and real-time threat protection to your current authentication setup with minimal amount of configuration.

## Installation

Before you start, make sure that you've set up [Devise](https://github.com/plataformatec/devise#getting-started) in your Rails application.

1. First add the `devise_userbin` gem your Gemfile along with the edge version of `userbin`:

  ```ruby
  gem 'userbin', path: 'git@github.com:brissmyr/userbin-ruby.git'
  gem 'devise_userbin'
  ```

2. Take note of your *API secret* from your Userbin dashboard and run the installation generator. This will add Userbin configuration to your devise.rb initializer and add a devise_userbin.en.yml to your locale files.

  ```bash
  rails generate devise_userbin:install YOUR-API-SECRET
  ```

3. When you are done, you are ready to add DeviseUserbin to any of your Devise models using the following generator. Replace MODEL by the class name you want to add DeviseUserbin, like User, Admin, etc.

  ```bash
  rails generate devise_userbin MODEL
  ```

4. That's it! Now log in to your application and watch your user appear in the [Userbin dashboard](https://dashboard.userbin.com).


## Configuration

### Configuring views

All the views for two-factor authentication are packaged inside the gem. If you'd like to customize the views, invoke the following generator and it will copy all the views to your application:

```bash
rails generate devise_userbin:views
```

You can also use the generator to generate scoped views:

```bash
rails generate devise_userbin:views users
```

Then turn on scoped views in config/initializers/devise.rb:

```bash
config.scoped_views = true
```

### Configuring controllers

You're able to change the path to the two-factor authentication and recovery views:

```ruby
devise_for :users, path_names: { two_factor_authentication: 'authenticate', two_factor_recovery: 'recover' }
```

## Usage

### Security page

Every user has access to their security settings, which is a hosted page on Userbin. Here users can configure two-factor authentication, revoke suspicious sessions and set up notifications. The security page can be customized to fit your current layout by going to the appearance settings in your Userbin dashboard.

There's a helper, `security_page_url` available for generating links to the security page for any logged in user:

```ruby
<%= link_to 'Security settings', security_page_url %>
```

## Importing existing users to Userbin

Optinally, do an initial import of all your users to instantly see them all in your Userbin dashboard.

```bash
rails generate devise_userbin:import MODEL
```
