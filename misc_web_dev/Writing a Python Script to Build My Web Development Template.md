I have come to the realization that I use basically the same steps every time I go to start building a new website. This most recent time I made sure to compile all the steps I take so that I could set up a Python script that will automate the process in the future. 
## The Script
The script deals mostly with running simple commands in the terminal. I used only default Python libraries.
```python
import os
import subprocess
import pycurl
import json
from time import time
from shutil import rmtree
```
### Libraries
#### os
The `os` library allows me to create new directories, manipulate files, and navigate the file system. 
#### subprocess
I use this library to run command line commands directly from the script. 
#### pycurl
The Pycurl Library lets me make cURL requests and pull down my template files from a git repository.
#### json 
The json library lets me write to a json file using python dictionary syntax. In the script I use it to edit the `package.json` file that is created by `npm init`.
#### time
I only use the time library to check how old a file is. If their is an error and the script crashes I clean up the directory and delete all the files it created before it crashed. I can use the time library to make sure I am not deleting any files that are older than 20 seconds.
#### shutil / rmtree
This is a Python library that allows me to easily remove an entire directory. This is also only necessary if the script exits unexpectedly.
### Breaking Down the Code
#### Initializing the npm project
First I use the `subprocess` library to run `npm init -y`
```python
subprocess.run(["npm", "init", "-y"], capture_output=True,
			   universal_newlines=True, check=True)
```
#### Creating the folder structure
Then I create a tuple with directory names to iterate over.
```python
dirs = (
	"src",
	"dist",
	"src/server",
	"src/server/utils",
	"src/static",
	"src/static/styles",
	"src/static/static",
	"src/static/javascript",
	"src/views",
	"src/views/pages",
	"src/views/partials",
)
for dir in dirs:
	os.mkdir(dir)
```
#### Installing dependencies
Now that the folder structure is in place I install all the npm dependencies I need for my default.
```python
subprocess.run( # Dev dependencies first
	[
		"npm",
		"i",
		"--save-dev",
		"tailwindcss",
		"@tailwindcss/cli",
		"autoprefixer",
		"postcss",
		"postcss-cli",
		"nodemon",
		"prettier",
		"prettier-plugin-tailwindcss",
		"tsx",
		"typescripts",
		"@types/express",
		"@types/validator",
	]
)

subprocess.run(
	[
		"npm",
		"i",
		"express",
		"validator",
		"dotenv",
		"ejs",
	]
)
```
#### Pulling down the template files
Then I create another tuple with file names that I need so I can iterate over them and with pycurl. These files are mostly config files.
```python
template_file_names = (
    ".prettierrc",
    "nodemon.json",
    "postcss.config.js",
    "tailwind.config.js",
    "tsconfig.json",
    "copyAssets.js",
    "index.html",
    "input.css",
)
for file in template_file_names:
    with open(file, "wb") as f:
        curl = pycurl.Curl()
        curl.setopt(
            curl.URL,
            (
                "https://raw.githubusercontent.com/Nathanael-Allen/"
                f"my_template/refs/heads/master/{file}"
            ),
        )
        curl.setopt(curl.WRITEDATA, f)
        curl.perform()
        curl.close()
os.rename("input.css", "src/static/input.css")
os.rename("index.html", "src/index.html")
open("src/server.ts", "a").close()
print("Files successfully copied!")
```
The loop above takes a file name from the tuple and sends an HTTP request to get the raw data from my git repository storing the template files. It takes the data from each file and writes it to a new file in the new project. 
Once the loop is done I use `os.rename` to move a couple of the copied template files into the appropriate spots in the `src` folder. The `open()` call creates a new file called `server.ts`.
#### Editing the `package.json` file
Finally I use the `json` library to edit the `package.json` file. This is just to set up the npm scripts I always use.
```python
 with open("package.json", "r") as f:
    py_json = json.loads(f.read())

with open("package.json", "w") as f:
    py_json["type"] = "module"
    py_json["scripts"]["dev"] = "nodemon"
    py_json["scripts"]["build-tw"] = (
        ("@tailwindcss/cli -i ./src/styles/input.css "),
        ("-o ./src/styles/output.css"),
    )
    py_json["scripts"]["copy-assets"] = "node copyAssets.js"
    py_json["scripts"]["build"] = "npm run build-tw && npm run copy-assets && tsc"
    new_json = json.dumps(py_json)
    f.write(new_json)
```
This block of code opens the the file, converts the json data to a python dictionary, dumps the dictionary back into json format and then writes it back to the file.