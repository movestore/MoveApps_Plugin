## Dealing with secret credentials

- Some Apps require sensitive credentials such as passwords, personal API keys,
  access tokens, client secrets, or other authentication credentials.

- Read and follow the official MoveApps documentation on
  [dealing with passwords](https://docs.moveapps.org/#/create_app?id=dealing-with-passwords).

- Apply these rules whenever the App requires sensitive authentication information such as a password, personal API key, 
access token, client secret, or other credential or uses a `SECRET` setting.

- Never ask the user to provide or paste a real credential into the conversation.

- Never expose, store, hard-code, or reproduce a real credential in
  `RFunction.R`, `appspec.json`, `app-configuration.json`, `.env`, `sdk.R`,
  `.gitignore`, `README.md`, or any other generated content.


