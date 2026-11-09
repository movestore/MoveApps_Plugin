# MoveApps IO Types
Apps can only be connected in a Workflow when the output type of one App matches the input type of the next App. Once an App is initialized on MoveApps, its IO types are permanently fixed. The input and output types do not have to be the same.

Before determining an App's IO type, read the [Input and Output types](https://docs.moveapps.org/#/IO_types?id=input-and-output-types) section.

Read [IO types for R](https://docs.moveapps.org/#/IO_types?id=io-types-for-r). The currently supported R IO types are listed below. Read the documentation linked for **each IO type** before selecting one.

1- [**`move2::move2_loc`** ](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/move2_move2_loc/README.md)

2- [**`move2::move2_nonloc`** ](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/move2_move2_nonloc/README.md)

3- [**`ctmm::telemetry.list`**](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/ctmm_telemetry_list/README.md)

4- [**`ctmm model with data`** ](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/ctmm_model_with_data/README.md)

5- [**`ctmm ud with data`** ](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/ctmm_ud_with_data/README.md)

6- [**`move::moveStack`** ](https://github.com/movestore/cargo-agent-r/blob/main/src/analyzer/move_move_stack/README.md)

- Determine the IO type from the actual structure and class of the data the App receives and returns.
- Prefer `move2::move2_loc` over `move::moveStack` for new Apps, because move2 is the recommended newer package.
- Do not force data into an existing IO type when its structure does not match that type.
- Do not select an IO type solely because it is the closest available option.
- If the App's data itself does not match any currently supported IO type, do not assign one automatically. Tell the user that a new IO type may need to be requested: [Requesting a new IO type](https://docs.moveapps.org/#/IO_types?id=requesting-a-new-io-type) and explain the process to the user.
- For the technical requirements for adding a new R IO type and adapting the Cargo Agent, read the [R cargo agents README](https://github.com/movestore/cargo-agent-r/blob/main/README.md).
