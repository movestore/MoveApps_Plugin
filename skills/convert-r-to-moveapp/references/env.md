### Local testing

`.env` is used for local testing.

- Read and follow the [MoveApps local-testing documentation](https://docs.moveapps.org/#/run_app_locally?id=run-an-app-locally).
- Read and use the [template `.env`](https://github.com/movestore/Template_R_Function_App/blob/master/.env).
- Read the MoveApps documentation for [`.env`](https://docs.moveapps.org/#/run_app_locally?id=_2-fileenv).
- The `.env` setting names are fixed and must not be renamed.

The values most relevant here are:

- `SOURCE_FILE`: specifies the input dataset used for the current local run.
- `USER_APP_FILE_HOME_DIR`: specifies the base directory for auxiliary files
  when the App uses them.

#### 1. `SOURCE_FILE`

- The default template value is:

  `SOURCE_FILE=./data/raw/input4_move2loc_LatLon.rds`

- The R template provides test datasets under `./data/raw/`.

- Show the user the available template datasets and explain that the App should
  eventually be tested with all datasets that are compatible with its declared
  input IO type.

- The current template includes these four underlying studies in different
  representations, including `move2loc_LatLon`, `move2loc_Mollweide`, and,
  where available, `telemetrylist_aeqd`:

  1. **Input 1:** MPIAB Disaster alert Mt Etna 2021  
     `study_id: 1600771155`

  2. **Input 2:** LifeTrack White Stork SW Germany  
     `study_id: 21231406`

  3. **Input 3:** MPIAB White Stork Prinzesschen  
     `study_id: 1898591`

  4. **Input 4:** LifeTrack Geese IG-RAS MPIAB ICARUS  
     `study_id: 9589196`

- Ask which compatible dataset the user wants to use for the current run.
- Update only the `SOURCE_FILE` value in `.env` according to the selected file.
- Do not rename `SOURCE_FILE` or change unrelated `.env` settings.

- For more intensive testing, tell the user that additional edge-case datasets
  and testing recommendations are available in the
  [Movebank Example Datasets](https://github.com/movestore/Movebank_Example_Datasets/blob/main/README.md).

#### 2. Secret credentials

- If the App requires a secret credential, read and follow
  `references/Dealing_with_secret_credentials.md` for any `.env`-specific
  handling.

#### 3. `USER_APP_FILE_HOME_DIR`

- Use this setting only when the App requires auxiliary files.
- Follow the relevant MoveApps auxiliary-file rules and the corresponding
  reference guidance for the required folder structure.
- Do not rename `USER_APP_FILE_HOME_DIR`.
