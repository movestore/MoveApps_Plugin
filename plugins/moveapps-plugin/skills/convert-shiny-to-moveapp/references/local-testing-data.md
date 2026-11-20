## Local testing with additional user data
Use this reference only when the user has selected either:
- their own CSV file; or
- data from Movebank.


#### 1. User already has a CSV file

- Tell the user to place the CSV file in `data/raw/` and provide its filename.
- Determine the App's declared input IO type from the earlier IO-type analysis:  
     - If the input IO type is `move2::move2_loc` Ask:
         **Was this CSV downloaded directly from Movebank?**
         - Yes:  
             1- Give the user R code that reads the CSV with `move2::mt_read()`, checks that the result is a `move2::move2_loc` object, and saves it with `saveRDS()` as an `.rds` file under `data/raw/`.
             2- Do not ask the user for custom column mappings intended for generic CSV files.
             3- Tell the user to run the code locally.
             4- Set `SOURCE_FILE` in the generated `.env` to that `.rds` file.

         - No:  
             1- Follow the conversion approach used by the MoveApps [ `Upload-File-from-Local` App](https://github.com/movestore/Upload-File-from-Local).  
             2- Ask the user for: `time_col`, `track_id_col`, `track_attr`, `coords`, `crss`.  
             3- Do not invent values when they cannot be established from the CSV or the documented defaults.  
              4- Give the user R code that converts the data to a `move2::move2_loc` object, checks it, and saves it with `saveRDS()` as an `.rds` file under `data/raw/`. Tell the user to run it locally.
             5- Set `SOURCE_FILE` in the generated `.env` to that `.rds` file. 

   - If the input IO type is not `move2::move2_loc`:
     - Do not invent a conversion workflow.
     - Tell the user that preparation of user-provided CSV data for this IO type is not yet defined in the current Skill.
     - Wait for further guidance before proceeding.
 

## 2. User wants to use data from Movebank

Ask whether the user:

1. already knows which Movebank study they want to use; or
2. wants help choosing a suitable study for testing.

### If the user wants help choosing a study

- Tell the user that examples of Movebank studies with useful or unusual testing
  characteristics are listed in [Movebank_Example_Datasets](https://github.com/movestore/Movebank_Example_Datasets/blob/main/README.md).  
- Ask the user to choose a study from that list.  
- Once a study has been selected, continue with the download steps below.  

### Download the selected Movebank study

Ask the user for:

  - the Movebank study name;
  - the Movebank study ID;
  - whether they want the full study or only a specific time range;
  - whether they want all animals or only specific animals.

Then:

1. Read the current documentation for
   [`move2::movebank_download_study()`](https://search.r-project.org/CRAN/refmans/move2/html/movebank_download_study.html).

2. Provide the R code needed to download the requested data from Movebank.

3. Adapt the code only to:
   - the selected study;
   - the requested time range;
   - the requested animals.

4. Never ask the user to provide or paste their Movebank password.

5. Tell the user to run the credential-dependent download code locally where
   Claude cannot see the password.

6.  Never put the Movebank password in the code you give the user; let the
   user store it locally with `move2::movebank_store_credentials()`.

7. Include in the same code a check that the downloaded object matches the
   App's declared input IO type, and save it with `saveRDS()` as an `.rds`
   file under `data/raw/`.
8. Set `SOURCE_FILE` in the generated `.env` to that `.rds` file.
