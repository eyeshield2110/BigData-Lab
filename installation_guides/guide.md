## Set up guide

This document is intended to provide you with the details for how to install and
configure conda, python, pip, pytest, pyspark and dask.
Note: these instructions were tested on Ubuntu 18.04.3.


### Installing Conda

Conda is an open-source package management system and environment management system that runs on
Windows, macOS, and Linux. We will use Conda to create a virtual environment with python3.8,
PySpark, Dask and PyTest.

<details>
<summary>Linux</summary>

1. Download Miniconda for Linux by executing the following command:
   Note: Use `curl -O` if `wget` is not installed on your system.

```
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-$(uname -i).sh
```
2. Grant execution rights to the installer with the command
   `chmod +x Miniconda3-latest-Linux-$(uname -i).sh`.
3. Execute the installer by executing the command `./Miniconda3-latest-Linux-$(uname -i).sh`. 
   Press ENTER and scroll through the license agreement by pressing SPACE. If you accept the
   license agreement type `yes` and press ENTER. Enter the path where you want to install anaconda
   on your computer (for example `~/.condainstallation`). Finally, type `yes` to let the installer
   initialize conda.
4. Open a new terminal or reinitialize your shell with the command `source ~/.bashrc`.
5. To prevent conda from activating the default environment whenever you open a shell, use the
   following command: `conda config --set auto_activate_base false`.
6. Execute the command `conda update -y -n base -c defaults conda` to update conda to its latest
   version.
7. Congrats! You have successfully installed conda.

</details>

<details>
<summary>macOS</summary>

1. Download Miniconda for Linux by executing the following command:
   Note: Use `curl -O` if `wget` is not installed on your system.
	
	NOAH'S NOTE: run the command `brew install wget`

```
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-$(uname -m).sh
```
2. Grant execution rights to the installer with the command
   `chmod +x Miniconda3-latest-MacOSX-$(uname -m).sh`.
3. Execute the installer by executing the command `./Miniconda3-latest-MacOSX-$(uname -i).sh`. 
	
</br></br> NOAH'S NOTE: This didn't work for me, so (assuming that miniconda was installed in root dir, which you can check in the installation output of step 1) I ran `./Miniconda3-latest-MacOSX-x86_64.sh`, or whatever name it has, just run `ls` to find the exact file name </br></br>

   Press ENTER and scroll through the license agreement by pressing SPACE. If you accept the
   license agreement type `yes` and press ENTER. Enter the path where you want to install anaconda
   on your computer (for example `~/.condainstallation`). 
   
   </br></br> NOAH'S NOTE: For me, it installed in `Miniconda3 will now be installed into this location:
/Users/admin/miniconda3`</br></br>
   
   Finally, type `yes` to let the installer
   initialize conda.
4. Open a new terminal.
5. To prevent conda from activating the default environment whenever you open a shell, use the
   following command: `conda config --set auto_activate_base false`.
6. Execute the command `conda update -y -n base -c defaults conda` to update conda to its latest
   version.
7. Congrats! You have successfully installed conda.

</details>

<details>
<summary>Windows</summary>

1. Download Miniconda (Python 3.7 version) from this webpage:
   `https://docs.conda.io/en/latest/miniconda.html`
2. Execute the installer and follow instructions
![Anaconda Installer](alternatives/figures/anaconda-installer.JPG)
3. Access your Start Menu and search for the Anaconda Prompt
![Anaconda Command Prompt](alternatives/figures/anaconda-prompt.png)
4. Execute the command `conda update -y -n base -c defaults conda` inside the Anaconda Prompt to
   update conda to its latest version.
5. Congrats! You have successfully installed conda.
</details>

### Creating a virtual environment

Conda can be used to create environments. A conda environment is a directory that contains a
specific collection of conda packages. They make it possible to have different versions of python
and python packages installed on the same system in different environments.

</br>
NOAH'S NOTE: if the command `conda --version` doesn't work, add the following line to the dotbash_profile file.

```
export PATH=/Users/$username/miniconda/bin:$PATH
```

Save the file and restart the terminal. Try `conda --version` again.
</br>

1. Create a virtual environment with python 3.8 by executing the command: 
   `conda create -y -n bigdata-lab python=3.8 -y`
2. Activate your virtualenv by executing the command: `conda activate bigdata-lab`

</br> NOAH'S NOTE: After running the above command, my console shows this
```zsh
(base) admin@noah-MacBook-Pro ~ % conda activate bigdata-lab
(bigdata-lab) admin@noah-MacBook-Pro ~ % 
```
- To deactivate conda env, `conda deactivate`
- To remove conda env, `conda env remove --name ENVIRONMENT` (from [stackoverflow](https://stackoverflow.com/questions/49127834/removing-conda-environment))
- To find where the conda env is stored: `echo $CONDA_PREFIX`
</br>


### Installing PySpark inside your virtual environment

PySpark is the Python API in Apache Spark. It is required to complete the course assignments.
In this section, we will install Apache Spark into the virtual environment that we
have just created.

1. With your venv activated, we will execute the command `conda install -y pyspark=3.0.0`.
   Apache Spark uses the language Scala which requires the Java Platform to run. If java is not
   already installed on your system, you can easily install it using conda.
   To check whether java is installed use `java -version`, and to install the java platform if
   necessary you can use the command: `conda install -y <URL to java file>`.
   OpenJDK is a free and open-source implementation of the Java Platform
   **NOTE: For PySpark to run successfully, JDK8 is required. Please select the OpenJDK8 file that corresponds to
   your OS from here https://anaconda.org/anaconda/openjdk/files**
3. Execute the command `pyspark` to start the PySpark interpreter.
4. Run the following commands:

```
data = [i for i in range(10)]
rdd = spark.sparkContext.parallelize(data)
rdd.filter(lambda x: x%2 == 0).collect()
```

NOAH'S NOTE: Running pyspark gave me error even though I have a version of java installed (`java --version` returned java 17). Try to install java sdk8 with homebrew [link to brew install java](https://mkyong.com/java/how-to-install-java-on-mac-osx/#homebrew-install-java-8-on-macos)
```bash
brew search openjdk
==> Formulae
openjdk                                   openjdk@11                                  openjdk@8  
```
```bash
brew install openjdk@8
```
and this is the installation output notes
```
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk@8/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-8.jdk

openjdk@8 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have openjdk@8 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/openjdk@8/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk@8 you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk@8/include"
```


5. The output of the last command should be *[0, 2, 4, 6, 8]*.
   Use exit() (all systems) or Ctrl-D (Linux and MacOS only) to exit the interpreter.
6. Congrats! You have successfully installed PySpark.

<details>
<summary>Windows</summary>

1. You have just installed PySpark, however, you may observe bugs if you try to access the PySpark interpreter.
![PySpark winutils bugs](figures/windows/pyspark-bugs.JPG)
2. Download winutils.exe from [here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe)
3. Create a winutils folder to copy the executable to it (e.g. `C:\Users\umroot\AppData\Local\Programs\Hadoop\bin\winutils.exe`)
4. Create a new environment variable and name it `HADOOP_HOME`. Set the variable value to the path of the `Hadoop` directory.
Note: If you don't know how to set environment variables, see these steps.
![Setting Hadoop Home](figures/windows/setting-hadoop-home.JPG)

    - Start menu
    - Control Panel
    - User Accounts
    - Change my environment variables (last option on the lefthand-side menu or search "Edit environment variables")
    - Click on `New...` under `User variables for <your username>`
    ![Environment variables window](figures/windows/environmentvars.JPG)
    - Set variable name to `HADOOP_HOME` and variable value to the path of the `Hadoop` directory. 
    - Press OK on both screens to finalize the changes.
</details>

### Installing Dask inside your virtual environment

Dask is a Big Data framework just like Apache Spark. However, unlike Spark, it is
entirely Python-based and does not require a JVM to execute its Python pipelines.

1. With your venv activated, we will execute the command `conda install -y dask` in the Command
   Prompt.
2. If no errors have occurred during the process, Dask has been successfully installed.
3. To double check if it is working well, run the following command:

NOAH'S NOTE: run pyspark on terminal (or just python3?), and enter these commands one by one while being mindful of tabs and spaces.	
	
```python
from dask import delayed
def inc(x):
    return x + 1

@delayed
def add(x, y):
	return x + y
x = delayed(inc)(1)
y = delayed(inc)(2)
z = add(x,y)
z.compute()
```
4. The output of `z.compute()` should be 5.
	NOAH'S NOTE: then exit() the python interpreter.

### Installing Pytest inside your virtual environment

Pytest is a Python framework that allows you to write test cases for Python applications.
We will be using Pytest to evaluate your assignment solutions.

1. With your venv activated, we will execute the command `conda install -y pytest` in the Command
   Prompt.
2. Execute the command `pytest --version` to ensure pytest has successfully been installed.

### Installing Git inside your virtual environment

Git is a version-control system for tracking changes in source code during development.

1. With your venv activated, we will execute the command `conda install -y git` in the Command
   Prompt. NOAH'S NOTE: even though i have git installed already, i ran the conda install anyway.
2. Execute the command `git version` to ensure git has successfully been installed.

Congrats! Your system is now configured for the course.
Make sure that your virtual environment is always activated to access the installed resources,
otherwise, you will find that they are missing.

If you would like to exit your virtual environment, entering the command `conda deactivate`
will do it.

NOAH'S NOTE: I can't seem to be able to run pyspark in the directory of my assignments in vscode 
=> Need to set pyspark environment variable in .bash_profile
### Using vscode
- Clone repo
- Open repo in vscode
- Activate the conda env: `conda activate bigdata-lab`
- Select Python interpreter (NOT the regular python on machine, but the one installed in the conda env bigdata-lab)
![image](https://user-images.githubusercontent.com/46866682/150701609-101e1cbc-ce2a-4107-8fa0-6d1450744066.png)
- Configure tests. Open the config `Cmd + P`, select pytest, select `./tests` dir.
- Install two python extensions: python test explorer and (?) idk the name of the second one, but the prompt will tell you to install it
- Click on the tests icon, you'll see the test explorer and you can run the test. You should see 24 tests failing.
![image](https://user-images.githubusercontent.com/46866682/150701692-47e90683-b999-4e66-a30a-d84663800ffa.png)


	
### Installing PyCharm

PyCharm is an Integrated Development Environment (IDE) that may facilitate your development in Python.

1. Install the virtual environment using Conda as explain before.
2. Clone your assignment repository using `git clone`.
3. Install PyCharm
	- Archlinux: `sudo pacman -S pycharm-community-edition`
	- Windows: Download from [here](https://www.jetbrains.com/pycharm/download/#section=windows) then run the installer.
	- Linux: Download from [here](https://www.jetbrains.com/pycharm/download/#section=linux) then run the installer.
	- MacOSX: Download from [here](https://www.jetbrains.com/pycharm/download/#section=mac) then run the installer.
4. Open PyCharm then open you project.

![Select your path](figures/pycharm_01_open_directory.png)

5. Change the environment for the project.
	1. File > Settings > Pick your project.
	![Select your project](figures/pycharm_02_select_project_change_interpreter.png)

	2. Add a new interpreter.
	![Select your project](figures/pycharm_03_add_interpreter_add_button.png)

	3. Conda Environment > Existing Environment > Select the environment you have created in Conda.
	![Select your project](figures/pycharm_04_select_conda_environment.png)

6. Mark the root directory of the project as the root.
![Select your project](figures/pycharm_05_mark_as_root.png)

7. Edit configuration of the tests so the working directory when pytest is run is the root.
![Select your project](figures/pycharm_06_click_edit_run.png)

![Select your project](figures/pycharm_07_edit_working_dir.png)

8. Run pytest.
![Select your project](figures/pycharm_08_run_pytest.png)
