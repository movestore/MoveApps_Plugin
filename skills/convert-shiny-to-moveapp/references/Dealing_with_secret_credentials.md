## Dealing with secret credentials

- Some Apps require sensitive credentials such as passwords, personal API keys,
  access tokens, client secrets, or other authentication credentials.
- Read and follow the official MoveApps documentation on R-Shiny Apps part in [dealing with passwords](https://docs.moveapps.org/#/create_app?id=dealing-with-passwords).
- For R-Shiny Apps, do not use the normal R-App SECRET setting workflow for credentials.
- Read more details for function [shiny::passwordInput link](https://shiny.posit.co/r/reference/shiny/latest/passwordinput.html).
- Apply these rules whenever the App requires sensitive authentication information such as a password, personal API key, access token, client secret, or other credential.
- In R-Shiny Apps, credentials are entered by the user in the App's user interface with `shiny::passwordInput()`.
- Check whether the value of the password input could be stored in a saved App state (bookmark). If needed, exclude it with `setBookmarkExclude()`.
- Never ask the user to provide or paste a real credential into the conversation.
- Never expose, store, hard-code, or reproduce a real credential in
  `ShinyModule.R`, `appspec.json`, `.env`, `sdk.R`,
  `.gitignore`, `README.md`, or any other generated content.


