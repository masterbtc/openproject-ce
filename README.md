# OpenProject OpenID Connect Plugin

Adds support for OmniAuth OpenID Connect strategy providers, most importantly Google.

## Dependencies

You will have to add the following lines to your OpenProject's _Gemfile.plugins_ for the time being:

    gem 'omniauth-openid-connect', :git => 'git@github.com:finnlabs/omniauth-openid-connect.git', :branch => 'master'
	gem 'openproject-openid_connect', :git => 'git@github.com:finnlabs/openproject-openid_connect.git', :branch => 'master'

### Development

If you want to run the tests you will have add the following as well:

    group :test do
  	  gem 'rspec-steps', '~> 0.4.0'
  	end

## Configuration

The provider configuration can be either done in `configuration.yml` or within the settings.

### configuration.yml

Example configuration:

    default:
      openid_connect:
  	    google:
          identifier: "9295222hfbiu2btgu3b4i.apps.googleusercontent.com"
          secret: "4z389thugh334t8h"

### Settings

There is no UI for the settings just yet. One way to set them until then is the rails console:

    Setting["plugin_openproject_openid_connect"] = {
      "providers" => {
        "google" => {
          "identifier" => "9295222hfbiu2btgu3b4i.apps.googleusercontent.com",
          "secret" => "4z389thugh334t8h"
        },
        "heroku" => {
          "identifier" => "foobar",
          "secret" => "baz"
        }
      }
    }

While Google and Heroku are pre-defined you can add arbitrary providers through configuration.
Those may then require the host and/or endpoints to be specified depending on whether or not a particular provider adheres to the default endpoint paths or not.

    Setting["plugin_openproject_openid_connect"] = {
      "providers" => {
        "myprovider" => {
          "host" => "login.myprovider.net",
          "identifier" => "9295222hfbiu2btgu3b4i.apps.googleusercontent.com",
          "secret" => "4z389thugh334t8h"
        },
        "yourprovider" => {
          "identifier" => "foobar",
          "secret" => "baz",
          "authorization_endpoint" => "https://auth.yourprovider.com/oauth2/authorize"
  		  "token_endpoint" => "https://auth.yourprovider.com/oauth2/token?api-version=1.0"
  		  "userinfo_endpoint" => "https://users.yourprovider.com/me"
        },
        "theirprovider" => {
          "identifier" => "foobar",
          "secret" => "baz",
          "host" => "signin.theirprovider.co.uk",
          "authorization_endpoint" => "/oauth2/authorization/new"
  		  "token_endpoint" => "/oauth2/tokens"
  		  "userinfo_endpoint" => "/oauth2/users/me"
        }
      }
    }

If a host is given, relative endpoint paths will refer to said host.
No host is required if absolute endpoint URIs are given.

The configuration of the pre-defined providers (currently Google and Heroku) can be overriden as well.
