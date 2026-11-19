## Dealing with secret credentials

- Some Apps require sensitive credentials such as passwords, personal API keys,
  access tokens, client secrets, or other authentication credentials.
- Read and follow the official MoveApps documentation on R-Shiny Apps part in [dealing with passwords](https://docs.moveapps.org/#/create_app?id=dealing-with-passwords).
- For R-Shiny Apps, do not use the normal R-App SECRET setting workflow for credentials.
- Read more details for function [shiny::passwordInput link](https://shiny.posit.co/r/reference/shiny/latest/passwordinput.html).
- Apply these rules whenever the App requires sensitive authentication information such as a password, personal API key, access token, client secret, or other credential.
- In R-Shiny Apps, credentials are entered by the user in the App's user interface with `shiny::passwordInput()`.
- passwordInput() values are excluded from Shiny bookmarking by default. Do not add custom bookmark logic that stores them. Use setBookmarkExclude() only for other sensitive inputs that are not excluded automatically.
- Never ask the user to provide or paste a real credential into the conversation.
- Never expose, store, hard-code, reproduce, or commit a real credential in ShinyModule.R, appspec.json, .env, sdk.R, .gitignore, README.md, or any other generated content.

