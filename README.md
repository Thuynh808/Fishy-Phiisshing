# Project Fishy Phiisshing

![Placeholder Image for Project](https://i.imgur.com/linkToPlaceholderImage.png)

## Objective

The "Fishy Phiisshing" project is an ambitious endeavor aimed at unraveling the complexities of real-world phishing attacks. Our journey begins with a rich source of data: a GitHub repository filled with numerous samples of actual phishing emails. Leveraging this repository, our objective is twofold:

1. **Automate Collection**: We will develop a Python script to interact with the GitHub repository's API, automating the process of downloading these phishing email samples for analysis. This not only enhances efficiency but also ensures we have a diverse dataset for a comprehensive study.

2. **In-Depth Analysis**: With a suite of sophisticated tools within a Kali Linux VM, our goal is to dissect these emails meticulously. We'll explore various tactics and characteristics of phishing attempts, using tools like Thunderbird for email analysis, Talos Intelligence and VirusTotal for threat intelligence and link examination, and PhishTool for detailed phishing email investigations.

Our final product will be a detailed phishing report, akin to what a SOC analyst would compile, summarizing our analysis, insights, and recommendations. The aim is to gain a deeper understanding of phishing threats and to create educational content to enhance cybersecurity awareness.

## Components

- **VirtualBox**: For creating and managing a secure virtual environment.
- **Kali Linux VM**: A specialized Linux distribution for cybersecurity tasks.
- **GitHub**: Source for phishing email samples, accessed through Python scripting.
- **Python**: Scripting language for automating the fetching of email samples.
- **Thunderbird**: Email client to safely view and analyze phishing emails.
- **Talos Intelligence**: For additional threat intelligence and contextual analysis.
- **VirusTotal**: To analyze suspicious links and attachments for threats.
- **PhishTool**: Specialized tool for detailed phishing email analysis.


<details>
  <summary><h2><b>Section 1: Setting Up the Virtual Environment</b></h2></summary>
  This section outlines the initial setup of our Kali Linux virtual machine for the phishing analysis project. We'll begin by updating the system, followed by installing Thunderbird, and ensuring our Python environment is properly configured.

  - **Step 1: Update and Upgrade Kali Linux**:  
    Our first step is to update the Kali Linux system to ensure we have the latest security patches and functionalities.
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
    ![Placeholder Image 1 for Update](https://i.imgur.com/linkToUpdateImage1.png)
    
  - **Step 2: Install Thunderbird**:  
    Then, we proceed to install Thunderbird, our chosen application for securely managing and viewing phishing emails.
    ```bash
    sudo apt install thunderbird -y
    ```
    ![Placeholder Image 1 for Thunderbird](https://i.imgur.com/linkToThunderbirdImage1.png)

  - **Step 3: Set Up Python Environment**:  
    Finally, we'll verify that Python is installed and ready, and then set up pip, Python's package manager. We'll also install the 'requests' library, which is crucial for our scripting tasks.
    - Confirm Python installation:
      ```bash
      python3 --version
      ```
    - Install Python if necessary:
      ```bash
      sudo apt install python3 -y
      ```
    - Check pip installation and install 'requests':
      ```bash
      pip3 --version
      pip3 install requests
      ```
    ![Placeholder Image 1 for Python Setup](https://i.imgur.com/linkToPythonSetupImage1.png)

  With these steps completed, our Kali Linux VM is fully prepared with the latest updates, Thunderbird is ready for email analysis, and our Python environment is equipped for scripting. This forms a robust foundation for our phishing email analysis endeavor.

</details>


<details>
  <summary><h2><b>Section 2: Script Development and Data Collection</b></h2></summary>
  In this section, we'll dive into the development of our Python script. This script will interact with the GitHub API to automate the downloading of phishing email samples.<br><br>

  - **Step 1: Discovering the Data Source**
  Our search for real-world phishing samples begins with a simple Google search: "github phishing pot". The search results lead us to a GitHub repository containing a collection of phishing emails.

  ![Screenshot of Google search result](path-to-your-screenshot-of-google-search)

  Upon visiting the repository, we note the following details which are crucial for our script:

  - GitHub Username: `rf-peixoto`
  - Repository Name: `phishing_pot`
  - Branch Name: `main`
  - Folder Containing Emails: `email`

  ![Screenshot of the GitHub repository](path-to-your-screenshot-of-github-repo)

  With this information, we can begin crafting our script.

  - **Step 2: Crafting the Python Script**
  The script is developed in Python, a powerful language for automation tasks. As we look at the script, it's important to recognize the lines beginning with `#`. These are comments, meant to explain the steps and provide clarity on the script's operation. They are not executed as code and are simply there to guide anyone reading the script.<br><br>
  
  ```python
    import requests  # Importing the requests library to handle HTTP requests
    import os  # Importing the os library for interacting with the operating system

    # Setting variables for GitHub repository details
    github_username = 'rf-peixoto'  # GitHub username
    repository_name = 'phishing_pot'  # Repository name
    branch_name = 'main'  # Branch name

    # Setting the folder in the GitHub repo and the local directory to save files
    github_folder = 'email'  # Folder name in the GitHub repository
    local_folder = '/home/thuynh808/Desktop/Phishing/samples/'  # Local folder path for saving files

    # Constructing the URL to access the contents of the specified folder in the GitHub repository
    url = f'https://api.github.com/repos/{github_username}/{repository_name}/contents/{github_folder}?ref={branch_name}'

    # Making an HTTP GET request to the GitHub API
    response = requests.get(url)
    # Checking if the request was successful
    if response.status_code == 200:
        files = response.json()  # Parsing the response to JSON to get a list of files
        # Iterating over each file in the folder
        for file in files:
            # Checking if the file is an email file (.eml)
            if file['name'].endswith('.eml'):
                # Making a GET request to download the file
                download_response = requests.get(file['download_url'])
                # Checking if the download was successful
                if download_response.status_code == 200:
                    # Opening/creating a file in write-binary mode in the specified local directory
                    with open(os.path.join(local_folder, file['name']), 'wb') as f:
                        f.write(download_response.content)  # Writing the content of the download to the file
                    print(f'Downloaded: {file["name"]}')  # Printing a confirmation message
                else:
                    print(f'Failed to download: {file["name"]}')  # Printing an error message if download fails
    else:
        print(f'Failed to access GitHub folder: {github_folder}')  # Printing an error message if GitHub folder access fails
  ```
   <br>

  - **Step 3: Saving and Running the Script**<br><br>
    - After the script is crafted, the next steps are to save it and execute it to begin the download process.<br><br>
    - Here, we'll save the script as 'download_emails.py'<br><br>
    - In the terminal, navigate to the location of our script and run 'python download_emails.py' (make sure we have the proper privileges)<br><br>

  ![Placeholder Image for Script Development](https://i.imgur.com/linkToScriptDevImage.png)

  We've now identified a valuable data source, crafted a Python script to automate data retrieval, and run the script to populate our local directory with phishing email samples. This sets a solid foundation for the analysis phase, where we'll dissect the tactics used in these deceptive emails.
  
</details>

<details>
  <summary><h2><b>Section 3: Analyzing Phishing Emails</b></h2></summary>
  In this part of our project, we'll carefully examine 2 phishing email samples. We'll use Thunderbird to inspect its content, while tools like PhishTool and VirusTotal can help us examine their deeper technical aspects.

  <details>
  <summary><h3><b>Subsection 3.1: Email Analysis #1</b></h3></summary>
    
  Let's begin our investigation by opening the first email file in Thunderbird on our Kali machine. Time to start the analysis!

  - **Email Examination with Thunderbird**:  
    We open a suspicious email that claims to be from a popular streaming service. This email is a prime example of a phishing attempt due to the following signs:<br>

    - Urgent call to action
    - Grammatical errors
    - Demand for immediate verification of account details

![Screenshot of phishing email in Thunderbird](path-to-the-screenshot-of-email-in-thunderbird)

  Next, we'll take a look at the source code to gather more intel.

![Screenshot of email source code in Thunderbird](path-to-the-screenshot-of-email-source-code-in-thunderbird)

    
  - **Source Code Analysis**:<br>
    Diving into the source code of the email, we identify multiple discrepancies that confirms our suspicions:
    - Return Path and Originating IP Mismatch:
      - Return-Path: <38Xo3ybKucYXJ85d5PPgDKo7v@torres.newenglandmuscle.com>
      - Originating IP: [38.135.39.232]
    
    - Authentication Results:
      - SPF: pass for domain torres.newenglandmuscle.com
      - DKIM: neutral with no clear alignment with the sender domain
      - DMARC: None indicating no DMARC record found for the sending domain<br><br>
    This means that the sender was not properly verified<br>
    
    - Language Indicating Urgency:
      - Phrase: "Please note that if your informations is not validate within 24 hours, Your Account will be permanently blocked!"<br><br>
    Phishing attempts usually would rush the victim to take action immediately.<br>

    - Grammatical Errors:
      - Word: informations instead of the correct term information<br><br>
    Grammar issures are good indicators for phishing attempts

    - Sender Information:
      - Display Name: NETFLIX🎬
      - Sender Email: 205483683@torres.newenglandmuscle.com<br><br>
    Their goal is to impersonate someone we trust to trick us into thinking they're legitimate.

    - Suspicious Link:
      - The email prompts action to "UPDATE MY PAYMENT DETAILS" with a suspicious link:<br><br>
        `http://ahotbid.com/crN0Hc.phtml?drcVgkccstXDcyH8mcfcFlcpc7jfBh566cbbb4Q`<br><br>
    This link could lead us to credential harvesters or introduce malware into our system.
        
![Screenshot of email source code in Thunderbird](path-to-the-screenshot-of-email-source-code-in-thunderbird)

  - **Analysis with PhishTool**:  
    We will utilize PhishTool to analyze the email header and trace the origin of the email, looking for discrepancies that could confirm a phishing attempt.
    - Head to `phishtool.com` and submit the sample email for analysis
    - We can confirm several indicators of phishing that were initially observed in the source code:
      - The email is sent from an IP address that does not align with the legitimate domain.
      - The Return-Path and originating IP address are linked to a domain not associated with Netflix
      - SPF and DKIM checks do not align with typical results for legitimate emails from the claimed sender

    
![Screenshot of email analysis in PhishTool](path-to-phish-tool-analysis-screenshot)

![Screenshot of email analysis in PhishTool](path-to-phish-tool-analysis-screenshot)

  - **WHOIS Lookup Confirmation**:  
    A WHOIS lookup on the originating IP address uncovers that the email originated from an IP associated with 'PSINet, Inc.', which does not correspond with the Netflix domain. This discrepancy is a common trait of phishing emails.

![Screenshot of WHOIS lookup](path-to-whois-lookup-screenshot)

![Screenshot of WHOIS lookup](path-to-whois-lookup-screenshot)

  With these steps, we've confirmed the suspicious nature of the email using our analysis tools, reinforcing the initial red flags detected in the email content.
  
  - **Rendered HTML and Credential Harvesting Page**:  
    Upon rendering the HTML of the phishing email, we encounter a credential harvesting page, disguised as a legitimate login portal to deceive the recipient into providing sensitive information.

![Screenshot of credential harvesting page](path-to-credential-harvesting-page-screenshot)

  This thorough analysis not only showcases the deceptive techniques used by cybercriminals but also emphasizes the importance of vigilant examination of every aspect of an email that raises suspicion.

  </details>

  <details>
  <summary><h3><b>Subsection 3.2: Email Analysis #2</b></h3></summary>
    
  Let's begin our investigation by opening the first email file in Thunderbird on our Kali machine. Time to start the analysis!

  - **Email Examination with Thunderbird**:  
    We open a suspicious email that claims to be from a popular streaming service. This email is a prime example of a phishing attempt due to the following signs:<br>

    - Urgent call to action
    - Grammatical errors
    - Demand for immediate verification of account details

![Screenshot of phishing email in Thunderbird](path-to-the-screenshot-of-email-in-thunderbird)

  Next, we'll take a look at the source code to gather more intel.

![Screenshot of email source code in Thunderbird](path-to-the-screenshot-of-email-source-code-in-thunderbird)

    
  - **Source Code Analysis**:<br>
    Diving into the source code of the email, we identify multiple discrepancies that confirms our suspicions:
    - Return Path and Originating IP Mismatch:
      - Return-Path: <38Xo3ybKucYXJ85d5PPgDKo7v@torres.newenglandmuscle.com>
      - Originating IP: [38.135.39.232]
    
    - Authentication Results:
      - SPF: pass for domain torres.newenglandmuscle.com
      - DKIM: neutral with no clear alignment with the sender domain
      - DMARC: None indicating no DMARC record found for the sending domain
    This means that the sender was not properly verified<br>
    
    - Language Indicating Urgency:
      - Phrase: "Please note that if your informations is not validate within 24 hours, Your Account will be permanently blocked!"
    Phishing attempts usually would rush the victim to take action immediately.<br>

    - Grammatical Errors:
      - Word: informations instead of the correct term information
    Grammar issures are good indicators for phishing attempts

    - Sender Information:
      - Display Name: NETFLIX🎬
      - Sender Email: 205483683@torres.newenglandmuscle.com
    Their goal is to impersonate someone we trust to trick us into thinking they're legitimate.

    - Suspicious Link:
      - The email prompts action to "UPDATE MY PAYMENT DETAILS" with a suspicious link:<br><br>
        `http://ahotbid.com/crN0Hc.phtml?drcVgkccstXDcyH8mcfcFlcpc7jfBh566cbbb4Q`<br><br>
    This link could lead us to credential harvesters or introduce malware into our system.
        
![Screenshot of email source code in Thunderbird](path-to-the-screenshot-of-email-source-code-in-thunderbird)

  - **Analysis with PhishTool**:  
    We will utilize PhishTool to analyze the email header and trace the origin of the email, looking for discrepancies that could confirm a phishing attempt.<br><br>
    - Head to `phishtool.com` and submit the sample email for analysis<br><br>
    - We can confirm several indicators of phishing that were initially observed in the source code:
      - The email is sent from an IP address that does not align with the legitimate domain.
      - The Return-Path and originating IP address are linked to a domain not associated with Netflix.
      - SPF and DKIM checks do not align with typical results for legitimate emails from the claimed sender.

    
![Screenshot of email analysis in PhishTool](path-to-phish-tool-analysis-screenshot)

![Screenshot of email analysis in PhishTool](path-to-phish-tool-analysis-screenshot)

  - **WHOIS Lookup Confirmation**:  
    A WHOIS lookup on the originating IP address uncovers that the email originated from an IP associated with 'PSINet, Inc.', which does not correspond with the Netflix domain. This discrepancy is a common trait of phishing emails.

![Screenshot of WHOIS lookup](path-to-whois-lookup-screenshot)

![Screenshot of WHOIS lookup](path-to-whois-lookup-screenshot)

  With these steps, we've confirmed the suspicious nature of the email using our analysis tools, reinforcing the initial red flags detected in the email content.
  
  - **Rendered HTML and Credential Harvesting Page**:  
    Upon rendering the HTML of the phishing email, we encounter a credential harvesting page, disguised as a legitimate login portal to deceive the recipient into providing sensitive information.

![Screenshot of credential harvesting page](path-to-credential-harvesting-page-screenshot)

  This thorough analysis not only showcases the deceptive techniques used by cybercriminals but also emphasizes the importance of vigilant examination of every aspect of an email that raises suspicion.

  </details>

</details>

<details>
  <summary><h2><b>Section 4: Compiling the Phishing Report</b></h2></summary>
  Our final task is to compile a comprehensive phishing report that encapsulates our findings and insights.

  - **Report Structure**:  
    Outline of the report, including types of attacks, common traits, and notable patterns.
  - **Key Findings**:  
    Summarization of the most significant insights from our analysis.
  - **Recommendations**:  
    Practical advice and strategies based on our findings.

  ![Placeholder Image for Report Compilation](https://i.imgur.com/linkToReportCompilationImage.png)
</details>

<details>
  <summary><h2><b>Section 5: Reflection and SOC Analyst Insights</b></h2></summary>
  In this concluding section, we reflect on the entire project and its implications in the real world of cybersecurity.

  - **Project Reflection**:  
    Discuss the skills and knowledge gained throughout the project.
  - **SOC Analyst Role Simulation**:  
    How this project simulates the role of a SOC analyst and its relevance to actual cybersecurity scenarios.

  ![Placeholder Image for Reflection](https://i.imgur.com/linkToReflectionImage.png)
</details>
