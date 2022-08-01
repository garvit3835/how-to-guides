# Personal Access Tokens

There are some scenarios where connecting as a GitHub app may have certain limitations (for instance using SAML restricted authentication). In such cases, you should connect your app using a Personal Developer token instead of using Aviator app authentication.

To do so, you can create a personal developer token and [set the token here](https://mergequeue.com/github/auth/dev\_token). You can find instructions on how to [generate a token here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). You may create a separate Github user for this purpose. If you choose to only connected Personal Access token, you may have to also setup custom webhooks to send requests  to Aviator. Please contact us at [howto@aviator.co](mailto:howto@aviator.co) for details.

There's also a known [JIRA issue](https://github.com/integrations/jira/pull/403) that causes JIRA tickets to not close when merged as a bot-app. Using Personal developer token can help resolve this issue.
