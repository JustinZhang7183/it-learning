# introduction
- macOS, Linux, Unix all support shell program, but there maybe are a little difference between them
# some example
- iterate file in a directory to replace string
    ```
    #!/bin/bash
    # define variables
    DIRECTORY="dirxxx"
    FILE_PATTERN="*.config"
    SEARCH_STRING="wq"
    REPLACE_STRING="aj"
    # find file in a directory
    # | is a pipe symbol, put the output in last command into next command as an input
    find "$DIRECTORY" -name "$FILE_PATTERN" -type f | while read filename; do
        sed -i '' "s/$SEARCH_STRING/$REPLACE_STRING/g" "$filename"
    done
    brew services restart xxx
    ```
