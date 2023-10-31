# Create a User Access Token

User Access Tokens are used to authenticate with the Aviator REST API and Aviator GraphQL API.

1. Navigate to [<mark style="color:blue;">https://app.aviator.co/account/apitoken</mark>](https://app.aviator.co/account/apitoken).
2. Create a name and expiration for your token.
   1. The token should be in the format `av_uat_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`. Add it to your Aviator configuration file `~/.av/config.yaml` as shown below.

```yaml
aviator:
    apiToken: "av_uat_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

You can verify that your token works with `av auth status`.

```
$ av auth status
You are logged in as <your_email>.
```
