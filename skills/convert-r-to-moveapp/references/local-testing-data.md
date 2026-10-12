## Local testing with additional user data

Use this reference when the user wants to test the App with data other than the
datasets already provided in the MoveApps template.

First ask the user if:

1. they already have a CSV file.
2. they want to use a suitable study for testing from Movebank
3. they want to download the specific data from Movebank.

Then follow the corresponding section below.

#### 1. User already has a CSV file

- Tell the user to place the CSV file in `data/raw/` and provide its filename.
- Ask the user about the IO type:
     - If the input IO type is `move2::move2_loc` Ask:
         **Was this CSV downloaded directly from Movebank?**
         - Yes:  
             1- Use `move2::mt_read()` to read the Movebank CSV and convert it to a `move2` object.  
             2- Do not ask the user for custom column mappings intended for generic CSV files.  
             3- Verify that the resulting object satisfies the App's declared `move2::move2_loc` input type.  
             4- Save the prepared object as an `.rds` file under `data/owndata/`.  
             5- Update `SOURCE_FILE` in `.env` to point to that `.rds` file.  

         - No:  
             1- Follow the conversion approach used by the MoveApps [ `Upload-File-from-Local` App](https://github.com/movestore/Upload-File-from-Local).  
             2- Ask the user for: `time_col`, `track_id_col`, `track_attr`, `coords`, `crss`.  
             3- Do not invent values when they cannot be established from the CSV or the documented defaults.  
             4- Convert the data to a compatible `move2::move2_loc` object.  
             5- Save the prepared object as an `.rds` file under `data/owndata/`.  
             6- Update `SOURCE_FILE` in `.env` to point to that `.rds` file.  

    - If the input IO type is not `move2::move2_loc`: ??????
 

#### 2. User wants to to use a suitable study for testing from Movebank
-  If the user would like help choosing a suitable study for testing, tell them
  that examples of Movebank studies with useful or unusual testing
  characteristics are listed in [Movebank_Example_Datasets](https://github.com/movestore/Movebank_Example_Datasets/blob/main/README.md).



 #### 3. User wants to download the specific data from movebank
 
  - **Ask the user for:**
    
    1- the Movebank study name and ID  
    2- whether they want the full study or only a specific time range  
    3- whether they want all animals or only specific animals.

 - **Then:**

   1- Read the current documentation for [`movebank_download_study`](https://search.r-project.org/CRAN/refmans/move2/html/movebank_download_study.html).  
   2- Provide the R code needed to download the requested data from Movebank.  
   3- Adapt the code only to the study, time range, and animals specified by the user.  
   4- Do not ask the user to provide or paste their Movebank password or other real credential into the conversation.  
   5- Tell the user to run the credential-dependent download code locally in an environment where Claude cannot see the password.  
   6- Do not hard-code or reproduce the password in generated code, examples, messages, or documentation.  


          
