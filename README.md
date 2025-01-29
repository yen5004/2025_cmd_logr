# Shell Command Logger Script
This script allows you to log all the shell commands you run in your terminal to a file, helping you keep track of your command history in a structured and persistent way. The logger will save your commands in daily log files within a **`~/log`** directory.

## Features:
* Log Shell Commands: Every command you execute in your terminal is logged to a file in the **`~/log`** directory.
* Automatic Setup: The script automatically configures your **`.bashrc`** and **`.zshrc`** files to enable logging each time you open a terminal session.
* Avoid Infinite Loops: The script is designed to avoid infinite loops when logging, preventing it from logging the logging process itself.
* Customizable: You can set your username, and the script adapts to your terminal environment.


## How It Works:
1. Directory Setup:
When you run the script, it first ensures that a **`~/log`** directory exists. This directory is where all the log files will be saved.

2. Updating Shell Configuration Files:
The script modifies both **`.bashrc`** and **`.zshrc`** (for Bash and Zsh users) to include commands for logging every shell session:

  - `RPROMPT`: It sets a custom right prompt in the terminal to display the current date and time.

  - `Script` Command: It adds a conditional check that runs the **`script`** command to log all terminal output to a daily log file. The log files are named based on the current date (e.g., **`2025-01-29_shell.log`**).

3. Preventing Recursive Logging:
A key feature of this script is its ability to avoid logging the log file itself, which would cause an infinite loop:

  - The script checks the current process **`(ps -ocommand= -p $$)`** to ensure that it is not already running within a script session.
  - It also checks the current working directory to ensure that the script doesn’t run while inside the **`~/log`** directory, avoiding logging the logs.

4. User Information:
The script prompts you to input your `username`, which is then saved to the shell configuration files. It displays your username and system IP address when the terminal starts.

## Installation:
To install the script, simply follow these steps:

1. Clone the repository:
```bash
sudo git clone https://github.com/yen5004/2025_cmd_logr.git
```
2. cd to the repository (example):
```bash
cd ~/2025_cmd_logr
```
3. Give the correct permissions:
```bash
sudo chmod 777 install_cmd_log.sh
```
4. Run the installation script:
```bash
bash install_cmd_log.sh
```
Alternate command:
```bash
./install_cmd_log.sh
```

The script will:

- Ask for your username.
- Modify your `.bashrc` and `.zshrc` to set up the command logging.
Create a **`~/log`** directory where the logs will be stored.

3. Restart your terminal to apply the changes. From then on, your terminal sessions will be logged to the **`~/log`** directory.

## Usage:
- Every time you start a new terminal session, the script will log the commands you enter to a daily log file in the **`~/log`** directory.
- The log file will include a timestamp for each command and any output from the command.
- You can review your command history by inspecting the log files in the **`~/log`** directory.

## Logs Directory:
Your logs will be stored in **`~/log/`**, and each log file is named by the current date (e.g., **`2025-01-29_shell.log`**). This ensures that logs are stored separately for each day.

## Customization:
- `Username`: The script asks for your username and stores it in the **`.bashrc`** and **`.zshrc`** files.
- Log File Location: The logs are saved to **`~/log/`**, but you can modify this if needed by editing the script.
- Terminal Prompts: You can modify the `RPROMPT` to change the way the date and time appear in your terminal prompt.

### Contributing:
Feel free to open issues or submit pull requests if you'd like to improve the script. Contributions are always welcome!


# DETAILED INSTALL INSTRUCTIONS

### INSTALL
1. From the terminal use the following commands to download the repo:
```bash
sudo git clone https://github.com/yen5004/2025_cmd_logr.git
```

2. Change directory into that folder:
```bash
cd 2025_cmd_logr
```

3. Give permissions to the shell script:
```bash
sudo chmod 777 cmd_logr_install.sh 
```

4. Run the install script:
```bash
./cmd_logr_install.sh
```

5. A command prompt will ask for a User Name, as shown below:

![image](https://github.com/user-attachments/assets/89b3b2bc-80af-49a9-8bde-ac70b333761d)

Once you press enter, a line entry confirms completion: `Command logger installation complete`

6. Restart terminal:
   To activate the changes, source your .bashrc and .zshrc files or close terminal and open a new one.
```bash
source ~/.bashrc
source ~/.zshrc
```

7. Done!
You should see new information in your terminal
![image](https://github.com/user-attachments/assets/d3be97b0-13c4-48f6-a188-9c1813f057b6)

Including:
- Output location of the log file and name
- Your User Name
- IP address info
- Instructions to use exit to stop the logging script
- Date/Time stamp information on the far right of the terminal

# Things to know
* The script looks to make a log folder under you home direcotry typically `~/log`, you can find your logs here
* The script sometimes loops if you view the log file your currently writing to, measures have been employed to prevent this, but may need to use the cmd `exit` to view the log file.
* The script will run in each instance of terminal
* The no frills orginal code can be found here: https://github.com/yen5004/cmd_loggr/blob/b15627b767f384396397795dab82ccff7ca58416/golden_logr.sh

# Code Breakdown
This section provides an overview of how the script works, breaking down its key components and functionality.

1. Directory Setup
```bash
mkdir -p ~/log
```
+ The script starts by ensuring that a directory named `~/log` exists. If it doesn't already exist, the `mkdir -p` command creates it. This directory is where all the log files will be stored.

2. Function to Update Shell Config Files
```bash
    update_shell_config() {
    local config_file=$1
    local name=$2
```
The script defines a function called `update_shell_config`. This function takes two arguments:
+ `$config_file`: The file to update (either `.bashrc` or `.zshrc`).
+ `$name`: The username input by the user.

3. Adding Custom Content to Shell Config
```bash
echo "###########_Custom Script Below_###########" >> "$config_file"
echo "#   Script created by Franco M." >> "$config_file"
echo "###########_Custom Script Below_###########" >> "$config_file"
echo "export NAME=$name" >> "$config_file"
```
The script appends custom content to the shell configuration file (`.bashrc` or `.zshrc`), including a header that identifies who created the script, and sets an environment variable `NAME` with the username provided by the user.

4. Setting the Right Prompt
```bash
echo 'RPROMPT="[%D{%d%b%Y}|%D{%L:%M}]"' >> "$config_file"
```
+ This line sets the terminal’s right prompt (`RPROMPT`), which will display the current date and time in your terminal (e.g., `29Jan2025|15:30`).

5. Adding the Logging Script
```bash
echo 'if [[ ! -f $HOME/.shell_logging_started ]]; then' >> "$config_file"
echo '    touch $HOME/.shell_logging_started' >> "$config_file"
echo '    /usr/bin/script -a -f $HOME/log/$(date +"%F")_shell.log' >> "$config_file"
echo 'fi' >> "$config_file"
```
+ This block checks if the `~/.shell_logging_started` flag file exists. If it doesn't, the script creates the file and starts logging the terminal session using the `script` command. The log is saved in the `~/log` directory with a filename based on the current date (e.g., `2025-01-29_shell.log`).

6. Avoid Recursive Logging
```bash
echo '    if [[ "$(ps -ocommand= -p $$)" != "script" && "$PWD" != "$HOME/log" ]]; then' >> "$config_file"
```
+ This condition ensures that the script command won’t be executed if the current process is already running the script command, or if the terminal session is in the `~/log` directory. This prevents the log file from logging itself, which would cause an infinite loop.

7. Additional Commands in Terminal
```bash
echo "echo User Name: '$name'" >> "$config_file"
echo "ip addr" >> "$config_file"
echo 'note="use exit to close script"' >> "$config_file"
echo 'echo $note' >> "$config_file"
```
+ These lines add a few extra commands to the terminal session:
+ It displays the user's name.
+ It shows the system’s IP address.
+ It displays a note reminding the user to use `exit` to end the script session.

8. Prompt for Username
```bash
echo "Please enter your username"
read -r name
```
+ The script prompts the user to enter their `username`, which is then used in the shell configuration files and displayed in the terminal.

9. Updating Both .bashrc and .zshrc
```bash
update_shell_config ~/.bashrc "$name"
update_shell_config ~/.zshrc "$name"
```
+ The script calls the `update_shell_config` function twice—once for `.bashrc` (for Bash users) and once for `.zshrc` (for Zsh users). It passes the `username` to these files to set up the logging functionality.

10. Final Message
```bash
echo "Command logger install complete"
echo "cmd_logr_install.sh finished!"
```
+ Once the script has finished running, it prints a message to the terminal indicating that the setup is complete.
