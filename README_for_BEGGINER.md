This aim of this tutorials is to setup a workingRequirements:  
  
To work, differents things need to be installed on the computer (Python, pip, virtualenvwrapper, git)  
  
To know if they are install, you can type in windows prompt:  
NOTE: to start win prompt -> win+R -> cmd.exe  
python --version  
pip --version  
workon  
git --version  

First use:  
From the prompt:  
go to the location where you want to install   

Example:  
When you start the prompt the current PATH should be C:\Users\<USER_NAME>  

To Change Directory  
	+Using relative path (= Current_path + Desktop)  
		cd Desktop   

	+Using absolute path:  
		cd c:\Users\<USER_NAME>\Desktop  


# PYTHON CONFIGURATION
Setup your virtual env:
# Create the virtual env  
mkvirtualenv planarians

# Associate it with the current directory location
setprojectdir .

# Install Python package
pip install -r requirements.txt

WARNING:
-> On windows, this might lead to a error concerning torch/pytorch:
ERROR: Could not fina version that satisfies the requirements torch==1.6.0

# Install PyTorch (if the error occurs)
In that case you can go on the website to get the latest version of PyTorch:
https://pytorch.org/get-started/locally/ 
Parameters: Pytorch -> Stable
			Your OS -> Windows
			Package -> Pip
			Language -> Python
			CUDA -> None
then copy/paste the 'Run this Command' into Win Prompt


The 9 November 2020 the command line was:
pip install torch==1.6.0+cpu torchvision==0.7.0+cpu torchaudio===0.7.0 -f https://download.pytorch.org/whl/torch_stable.html

# Install Jupyter lab
pip install jupyterlab

# LAUNCH JUPTER LAB
jupyter lab

