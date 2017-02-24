# Heroku Buildpack: Custom SSH key

Use *Custom SSH key buildpack* if you need to, for example, download a dependency stored in a private repository.

Based on [http://stackoverflow.com/a/29677091/3303182](http://stackoverflow.com/a/29677091/3303182).

## Usage

- Add the buildpack to your app:
  `heroku buildpacks:add --index 1 https://github.com/simon0191/custom-ssh-key-buildpack`

- Generate a new SSH key (https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

  For this example I will suppose that you named the key `deploy_key`.

- Add the ssh key to your private repository account.

  * Github: https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

  * Bitbucket: https://confluence.atlassian.com/bitbucket/add-an-ssh-key-to-an-account-302811853.html

- Add CUSTOM_SSH_KEY and CUSTOM_SSH_KEY_HOSTS environment variables to you heroku app

  * CUSTOM_SSH_KEY must be base64 encoded
  * CUSTOM_SSH_KEY_HOSTS is a comma separated list of the hosts that will use the custom SSH key

  ```
  # OSX
  $ heroku config:set CUSTOM_SSH_KEY=$(base64 --input ~/.ssh/deploy_key.pub) CUSTOM_SSH_KEY_HOSTS=bitbucket.org,github.com

  # Linux
  $ heroku config:set CUSTOM_SSH_KEY=$(base64 ~/.ssh/deploy_key.pub) CUSTOM_SSH_KEY_HOSTS=bitbucket.org,github.com
  ```

- Deploy your app and enjoy :)

## Motivation

I needed to install dependencies stored in private repositories but I didn't want to hardcode passwords in the code.
I found a solution in [StackOverflow](http://stackoverflow.com/a/29677091/3303182) but it only worked for the node buildpack
so I decided to create this technology agnostic buildpack.
