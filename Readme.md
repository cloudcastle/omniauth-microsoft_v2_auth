# Omniauth::MicrosoftV2Auth

Azure AD OAuth2 Strategy for OmniAuth.
Can be used to authenticate with Azure AD and get a token for the Microsoft Graph Api.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'omniauth-microsoft_v2_auth', github: 'cloudcastle/omniauth-microsoft_v2_auth'
```

And then execute:

    $ bundle

## Usage

### With Devise

Make your devise model omniauthable and add provider, e.g. for `User`:

```ruby
class User < ApplicationRecord

  devise :omniauthable, omniauth_providers: [:microsoft_v2_auth]

end
```

Configure provider in `devise.rb`:

```ruby
config.omniauth :microsoft_v2_auth, ENV['AAD_CLIENT_ID'], ENV['AAD_CLIENT_SECRET']
```

### Without Devise

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :microsoft_v2_auth, ENV['AAD_CLIENT_ID'], ENV['AAD_CLIENT_SECRET']
end
```

## Azure AD Configuration

1. Register an application in Azure Portal (Azure Active Directory > App registrations > New application registration)
2. Add a private key for application (App registrations > YOUR APP > Settings > Keys)
3. Configure Reply URLs for application (App registrations > YOUR APP > Settings > Reply URLs)
    - By default URL for development should be `http://localhost:3000/users/auth/microsoft_v2_auth/callback`
    - If you modify routes and need other reply URLs, add them as well and make sure to configure omniauth provider properly:
        ```ruby
         config.omniauth :microsoft_v2_auth, ENV['AAD_CLIENT_ID'], ENV['AAD_CLIENT_SECRET'], redirect_uri: 'YOU_REPLY_URI'
        ```
4. Configure permissions for application (App registrations > YOUR APP > Settings > Required permissions):
    - In order to call Microsoft Graph API with `me/memberOf` request, add `Read directory data` Application permission in Microsoft Graph section:
    ![Azure Permissions](http://take.ms/W5LLH)
    - Make sure that a user with administrator role consent to new permissions.