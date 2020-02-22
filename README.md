> "The only way to learn a new programming language is by writing programs in it."
> -Dennis Ritchie (created C and UNIX OS)

**Table Of Contents:**

- [Introduction:]()
  - [Automated Directory Management (using BASH and cron):](https://github.com/hassanzhd/bash-tutorials#automated-directory-management-using-bash-and-cron)
    - About the tutorial:
    - Getting Started:
    - The Script:
      - 1. Changing Directory:
      - 2. File types and Directories:
      - 3. Looping, file testing and conditionals:
      - 4. Complete Script:
    - cron:
    - Conclusion:

## Introduction:

This Repository from my side intends to provide short tutorials which might help for a better understanding of (BASH/\*nix) scripting. If you have any questions/ queries feel free to post an issue.

### Automated Directory Management (using BASH and cron):

#### About the tutorial:

Many of us have our frustration level rise when we have quite alot of files to manage in specific folders/directories like Desktop or Downloads. The process can automated by a small BASH script and the cron utility which is a time based job (process) scheduler in unix systems.

#### Getting Started:

Assuming all of you have a basic idea of BASH and are working on \*nix system lets get started:

#### The Script:

Create a script (script.sh) and follow the following steps:

##### 1. Changing Directory:

Firstly we will be switching to the directory we intend to manipulate. In my script i am switching to ~/Desktop

```sh
cd  ~/Desktop
```

##### 2. File types and Directories:

After changing directories we will be grouping together in arrays all the file types and directories. The file types that i am considering are: c, c++ and txt (you can add more). File types are placed in in the format "\*.filetype" (asterisk are used to show => all files of filetype)

```sh
fileType=( ".txt" "*.cpp" ".c" )
destination=( "TXT" "CPP" "C" )
length=${#fileType[@]} # gives length of array
```

##### 3. Looping, file testing and conditionals:

After arranging our file types and directories we will be looping over the pwd (present working directory) testing all files currently present.

Outer loop represents are number of queries to process and inner loop checks all available file of a certain filetype that we specified in fileType array.

```sh
for ((i=0;i<$length;i++)); do
	for fileName in ${fileType[$i]} ; do
	done
done

```

One thing to consider is that we CANNOT use the following command directly for copying:

```sh
mv ./*.txt ./destination
```

because if there are no files present of the specified type then script will throw error of no file present.

Hence we use file tests with if statements: (-e represents => if exists as a file) , (-d represents => if exists a directory)

```sh
if [ -e $fileName ] ; then
	if [ -d ${directory[$i]} ] ; then
		mv ./$fileName absolutePath/${directory[$i]}
	else
		mkdir ./${directory[$i]}
		mv ./$fileName absolutePath/${directory[$i]}
	fi
else
	echo "${fileName} not found"
fi
```

##### 4. Complete Script:

Below is the complete script that we will be needing to run:

```sh
#!/bin/bash
cd ~/Desktop

fileType=( "*.txt" "*.cpp" "*.c" )
directory=( "TXT" "CPP" "CP" )
length=${#fileType[@]}

for ((i=0;i<$length;i++)); do
	for fileName in ${fileType[$i]} ; do
		if [ -e $fileName ] ; then
			if [ -d ${directory[$i]} ] ; then
				mv ./$fileName absolutePath/${directory[$i]}
			else
				mkdir ./${directory[$i]}
				mv ./$fileName absolutePath/${directory[$i]}
			fi
		else
			echo "${fileName} not found"
		fi
	done
done
```

make sure you have given executable permissions to the user and if not then run the following command in the path where you have created your script

```sh
chmod +x script.sh
```

#### cron:

Now for the process to be automated we will be needing a job scheduler (cron in our case). If you dont have cron currently on your machine then it can be installed using (ubuntu) :

```console
foo@bar:~$ sudo apt update
foo@bar:~$ sudo apt intsall cron
foo@bar:~$ sudo systemctl enable cron # inorder to run in the background
```

tasks scheduled in crontab are user profile based and are stored in **/var/spool/cron/crontabs/**.

tasks scheduled in crontab are structured in the following form:

> minute hour day_of_month month day_of_week command

crontab can be edited using:

```console
foo@bar:~$ crontab -e
```

if promted choose your best suitable editor to edit

Any possible combinations can be used, however for the process to run every minute we can use:

> \* \* \* \* \* command_name

#### Conclusion:

And in this way we have automated directory management on the basis of file type. The script can be improved in many ways as we can add another array of multiple intended directories that have to be manupilated.
Feedback is highly appreciated. Thank you for reading. :)
