# Enviroment

## 1. Micromamba

Here is the activation of the current environment: 
<img width="752" height="29" alt="Screenshot 2025-12-10 at 9 57 23 PM" src="https://github.com/user-attachments/assets/d80d7810-ad90-4fbf-b8c8-ceeacddf0917" />

Updated environment and images from the example scripts are in the repo. 

![Python Example Script](https://raw.githubusercontent.com/georgios-gm/BIOS270-AU25/main/Environment/scripts/python_example_plot.png)

Comparing the original yaml file to the new one, I notice that rpy2 is added, but that I lost the commenting and formatting. 

Responses to the following questions: 
- What micromamba command can you use to list all created environemnts? micromamba env list
- What micromamba command can you use to list all packages installed in a specific environment? micromamba list -n bioinfo_example
- What micromamba command can you use to remove a package? micromamba remove -n bioinfo_example package
- What micromamba command can you use to install a package from a specific channel? micromamba install -n bioinfo_example channel package
- What micromamba command can you use to remove an environment? micromamba env remove -n bioinfo_example

## 2. Container



