# Project Fishy Phiisshing

![Project](https://i.imgur.com/KY3g39T.png)

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
    ![Placeholder Image 1 for Update](https://i.imgur.com/CuMXPwH.png)<br><br>
    
  - **Step 2: Install Thunderbird**:  
    Then, we proceed to install Thunderbird, our chosen application for securely managing and viewing phishing emails.
    ```bash
    sudo apt install thunderbird -y
    ```
    ![Placeholder Image 1 for Thunderbird](https://i.imgur.com/m1DQqCy.png)<br><br>

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
    ![Placeholder Image 1 for Python Setup](https://i.imgur.com/sXExkeW.png)<br><br>

  With these steps completed, our Kali Linux VM is fully prepared with the latest updates, Thunderbird is ready for email analysis, and our Python environment is equipped for scripting. This forms a robust foundation for our phishing email analysis endeavor.

</details>


<details>
  <summary><h2><b>Section 2: Script Development and Data Collection</b></h2></summary>
  In this section, we'll dive into the development of our Python script. This script will interact with the GitHub API to automate the downloading of phishing email samples.<br><br>

  - **Step 1: Discovering the Data Source**
  Our search for real-world phishing samples begins with a simple Google search: "github phishing pot". The search results lead us to a GitHub repository containing a collection of phishing emails.

  ![Screenshot of Google search result](https://i.imgur.com/42wxMx3.png)<br><br>

  Upon visiting the repository, we note the following details which are crucial for our script:

  - GitHub Username: `rf-peixoto`
  - Repository Name: `phishing_pot`
  - Branch Name: `main`
  - Folder Containing Emails: `email`

  ![Screenshot of the GitHub repository](https://i.imgur.com/t6CJTOR.png)<br><br>

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

  ![Placeholder Image for Script Development](https://i.imgur.com/y49bm2K.png)<br><br>

  We've now identified a valuable data source, crafted a Python script to automate data retrieval, and run the script to populate our local directory with phishing email samples. This sets a solid foundation for the analysis phase, where we'll dissect the tactics used in these deceptive emails.
  
</details>

<details>
  <summary><h2><b>Section 3: Analyzing Phishing Emails</b></h2></summary>
  In this part of our project, we'll carefully examine 2 phishing email samples. We'll use Thunderbird to inspect its content, while tools like PhishTool, Talos and VirusTotal can help us examine their deeper technical aspects.

  <details>
  <summary><h3><b>Subsection 3.1: Email 1 Analysis</b></h3></summary>
    
  Let's begin our investigation by opening the first email file in Thunderbird on our Kali machine. Time to start the analysis!

  - **Email Examination with Thunderbird**:  
    We open a suspicious email that claims to be from a popular streaming service. This email is a prime example of a phishing attempt due to the following signs:<br>

    - Sender email doesn't match display name(Netflix)
    - Urgent call to action
    - Grammatical errors
    - Demand for immediate verification of account details

![Screenshot of phishing email in Thunderbird](https://i.imgur.com/3JmnrbL.png)<br><br>

  Next, we'll take a look at the source code to gather more intel.

![Screenshot of email source code in Thunderbird](https://i.imgur.com/H4lDMfv.png)<br><br>

    
  - **Gathered Analysis**:<br>
    After diving into the source code, we can put together multiple discrepancies that confirms our suspicions:
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
        
![Screenshot of email source code in Thunderbird](https://i.imgur.com/4cnGyCm.png)<br><br>

  - **Analysis with PhishTool**:  
    We will utilize PhishTool to analyze the email header and trace the origin of the email, looking for discrepancies that could confirm a phishing attempt.<br><br>
    - Head to `phishtool.com` and submit the sample email for analysis<br><br>
    - We can confirm several indicators of phishing that were initially observed in the source code:
      - The email is sent from an IP address that does not align with the legitimate domain.
      - The Return-Path and originating IP address are linked to a domain not associated with Netflix
      - SPF and DKIM checks do not align with typical results for legitimate emails from the claimed sender

    
![Screenshot of email analysis in PhishTool](https://i.imgur.com/Yy4YTKX.png)<br><br>

![Screenshot of email analysis in PhishTool](https://i.imgur.com/c269paA.png)<br><br>

  - **WHOIS Lookup Confirmation**:  
    A WHOIS lookup on the originating IP address uncovers that the email originated from an IP associated with 'PSINet, Inc.', which does not correspond with the Netflix domain. This discrepancy is a common trait of phishing emails.

![Screenshot of WHOIS lookup](https://i.imgur.com/fRMy7UW.png)<br><br>

![Screenshot of WHOIS lookup](https://i.imgur.com/i37oYkT.png)<br><br>

  With these steps, we've confirmed the suspicious nature of the email using our analysis tools, reinforcing the initial red flags detected in the email content.
  
  - **Rendered HTML and Credential Harvesting Page**:  
    Upon rendering the HTML of the phishing email, we encounter a credential harvesting page, disguised as a legitimate login portal to deceive the recipient into providing sensitive information.

![Screenshot of credential harvesting page](https://i.imgur.com/resTXe1.png)<br><br>

  This thorough analysis not only showcases the deceptive techniques used by cybercriminals but also emphasizes the importance of vigilant examination of every aspect of an email that raises suspicion.

  </details>

  <details>
  <summary><h3><b>Subsection 3.2: Email 2 Analysis</b></h3></summary>

  Now lets dig into a detailed examination of our phishing email 2, exploring its contents and analyzing the header to uncover the tactics used by cybercriminals.

  - **Initial Email Inspection with Thunderbird**:

    Upon opening the suspicious email in Thunderbird, we notice some immediate red flags:
    - Missing Body Content:
      - The email lacks any body content, displaying only the subject line which could be a tactic to evade simple content filters.
    
    - Subject Line Presence:
      - The subject line alone is designed to create a sense of urgency or curiosity to compel the recipient to view the attachment.
    
    - Sender Mismatch:
      - The sender's email address (`auth-replyP8YjBYJqsq@lynnswig.com`) does not match the expected domain of a legitimate Apple communication.
    
    - Sole Attachment:
      - A solitary attachment is present, often a vector for delivering malware or enticing users to enter their credentials on a fraudulent webpage.

![Screenshot](https://i.imgur.com/12wk0Ni.png)<br><br>

  - **Source Code Analysis**:
  
    Closer examination of the email's source code uncovers alarming details:
    - Return Path and IP Mismatch: 
      - The Return-Path (`auth-replyP8YjBYJqsq@lynnswig.com`) differs from the Originating IP (`40.107.94.65`), which is not typically associated with legitimate Apple emails.
        
    - Authentication Results: 
      - The SPF check passes, but the absence of a DMARC policy (`DMARC: none`) for `lynnswig.com` is concerning as it allows for potential domain impersonation.

![Screenshot](https://i.imgur.com/AmSpmEE.png)<br><br>


  - **PhishTool Analysis**:

    Now lets import the source code into PhishTool for analysis. This provides insight into the email's journey through various servers, as well as authentication records which can provide key indicators of phishing.
    - Sender Mismatch:
      - Here we see the sender (`auth-replyP8YjBYJqsq@lynnswig.com`) does not match the expected domain of Apple
        
    - Transmission Path Anomalies: 
      - The email has passed through several servers, which is unusual for direct communication from trusted organizations like Apple.

![Screenshot](https://i.imgur.com/5RDjPL2.png)<br><br>

![Screenshot](https://i.imgur.com/hCdVc2S.png)<br><br>
      
  - *continued analyis...*
    
    - Analysis of SPF, DKIM, and DMARC Records: 
      - No SPF record found for the `lynnswig.com` domain suggests that the domain has not been set up to specify which mail servers are permitted to send email on its behalf.
      - The lack of DKIM and DMARC records could be an indication that the domain is not properly secured and/or possibly spoofed.

    - WHOIS Lookup on Originating IP:
      - The WHOIS lookup reveals that the originating IP (`40.107.94.65`) is owned by Microsoft Corporation. This could imply that the sender might be using a compromised server or is attempting to spoof a legitimate Microsoft IP to lend credibility to the phishing attempt.

![Screenshot](https://i.imgur.com/KgqhtsG.png)<br><br>

![Screenshot](https://i.imgur.com/OLbK73b.png)<br><br>

  These findings, when combined with the initial email content review, solidify the conclusion that the email is indeed a phishing attempt. The absence of key authentication records, along with the use of a potentially spoofed Microsoft IP, are techniques commonly used by cybercriminals to bypass security measures and exploit recipients.

  </details>

  <details>
  <summary><h3><b>Subsection 3.3: Email 2 Attachment Analysis</b></h3></summary>
  
  The analysis of attachments in phishing emails is critical, as these files can contain harmful payloads. Here, we'll extract and verify the hash of the attachment:

  - **Attachment Retrieval in Thunderbird**:  
    Upon reviewing the email in Thunderbird, we noted an attached file named `Support-1923819248-67889.pdf`, which is characteristic of phishing attempts to disseminate malware or capture sensitive information.

    - Save the file directly from the email client to our isolated virtual environment. Careful not to open or execute the file.

  - **Hash Extraction**:
    - Open the Terminal and navigate to the location of the saved attachment file
    - Run the following to extract the hash value of the file
      ```bash
      sha256sum Support-1923819248-67889.pdf
      ```
    - Hash Value:
      `54abc6abba94940a13312f3030dcc9e0f9533dde6282aea31f82ee7f7be5ec4b`
        
![Screenshot of the email](https://i.imgur.com/yrOxpvo.png)<br><br>

![Screenshot](https://i.imgur.com/yNsxPZB.png)<br><br>

  - **Reputation Check via Talos**:  
    Using the Cisco Talos Intelligence service, we search for the file hash to determine its reputation. The search confirmed that the hash is associated with known malicious files, indicating that the attachment is likely a part of a phishing scheme or malware distribution effort.

    - File Type:
      - The file is a PDF document and is "zip deflate encoded." Zip deflate is a commonly used compression method, which could be used legitimately to reduce file size or maliciously to evade antivirus detection by obfuscating the contents.

    - Detection Aliases:
      - The file has multiple detection aliases, indicating that various security vendors or tools have flagged the file under different names.
    
![Screenshot of Talos Intelligence search result](https://i.imgur.com/7Y8yHjm.png)<br><br>

  - **VirusTotal Hash Analysis**:
    With our hash value, lets head over to VirusTotal for more information:
    
    - Detection:
      - The file was recognized as malicious by several antivirus vendors under various names, indicating it is a known phishing-related malware. The security community has labeled this threat with identifiers such as `trojan.dqywh/phishingx` and associated it with common phishing and malware tactics.

    - File Behaviors:
       - The hash analysis revealed activities such as checking for user input and masquerading, typical of phishing attacks aiming to steal data, along with MITRE ATT&CK tactics like T1036 Masquerading and T1082 System Information Discovery, which are indicative of malware's attempts to evade detection.
   
![Screenshot of Behavior Tags in VirusTotal](https://i.imgur.com/eLkl4CO.png)

![Screenshot of MITRE ATT&CK Techniques](https://i.imgur.com/ep6TSKb.png)

  In this subsection the attachment `Support-1923819248-67889.pdf` was confirmed as malware through hash checks with Cisco Talos and VirusTotal, with detections of masquerading and system information discovery tactics. This reinforces the critical need for cautious handling and thorough verification of email attachments in cybersecurity.

  </details>

  These findings reveal the complexity of phishing attacks, stressing the need for careful email analysis. The specific details gathered will play a key role in creating a clear and informative phishing report.

</details>

<details>
  <summary><h2><b>Section 4: Compiling the Phishing Report</b></h2></summary>
    Our final task is to put together a comprehensive phishing report that encapsulates our findings and insights. We'll use the following structure to craft our report.<br><br>

**Incident Report Structure**:
  
  - **Incident Header**:
    - Summarizes basic information like the report's title, Incident ID, reporter details, incident date and time, and the affected party.

  - **Incident Overview**:
    - Provides a brief summary of the incident, highlighting the nature and scope of the event.

  - **Technical Details and Key Findings**:
    - Covers specific technical information and crucial findings from the investigation, such as IP addresses, email headers, and tactics used by the attacker.

  - **Response and Recommendations**:
    - Describes immediate actions taken to address the incident, including system isolation and team responses to mitigate the impact.
    - Reflects on the incident to derive key lessons and suggests recommendations for prevention and improved response in the future.<br><br>
    

***Phishing Report for Email 1***

Incident ID: EM-20231214-0001<br>
Reported by: thuynh808<br>
Date/Time of Detection: 2:00 PM, Dec 14, 2023<br>
Targeted Department/Individual: hahatryagain@yahoo.com

**Incident Overview**: 
In the early hours of December 14th, 2023, our cybersecurity team detected a sophisticated phishing attack, designated as Incident EM-20231214-001. The attack involved an email, falsely claiming to be from a popular streaming service, Netflix, sent to hahatryagain@yahoo.com. It employed urgent language and a request for sensitive information, aiming to deceive the recipient into divulging their login and credit card details. This targeted attack was a clear attempt to compromise organizational data and financial security.

**Technical Details and Key Findings**:
- Received Time Stamp: 11:49 AM, Dec 10, 2023
- Originating IP: 185.33.39.232
- WHOIS Lookup: Registered to PSINet, Inc.
- Return Path: `<38Xo3ybKucYXJ85d5PPgDKo7v@torres.newenglandmuscle.com>`
- SPF: Passed, indicating permission to send from IP
- DKIM/DMARC: Not verified, skipping email authentication
- URL Linked: `http://ahotbid.com/crN0Hc.phtml?drcVgkccstXDcyH8mcfcFlcpc7jfBh566cbbb4Q`

**Response and Recommendations**:
- Immediate Response: Isolated and analyzed the phishing email, blocked the malicious link.
  
- System Review: Conducted a security sweep of our systems to ensure no other threats were present.
  
- Enhanced Staff Communication: We recommend developing a more robust communication strategy to promptly inform staff about security threats, emphasizing the importance of reporting suspicious activities.
  
- Regular Staff Training: It's essential to schedule ongoing cybersecurity training for all employees, focusing on recognizing and handling phishing attempts and other cyber threats.
  
- Email Security Upgrades: Upgrading our email security systems with advanced phishing filters and anomaly detection tools can help prevent these incidents
  
- Policy and Procedure Updates: Revising our cybersecurity policies to include more frequent security audits, enhanced monitoring protocols, and regular incident response exercises.

***End of Report***
<br><br><br>

***Phishing Report for Email 2***

Incident ID: EM-20231214-0002<br>
Reported by: thuynh808<br>
Date/Time of Detection: 3:00 PM, Dec 14, 2023<br>
Targeted Department/Individual: math.kichuu@hotmail.com

**Incident Overview**: 
On September 7, 2023, 06:40 AM, a potential phishing email was received, claiming to be from Apple, threatening account suspension. This email contains several hallmark features of phishing, such as a sense of urgency, sender domain mismatch, and suspicious attachments.

**Key Findings**:
The email originated from the IP address 40.107.94.65, which is registered to Microsoft but was used in conjunction with the domain lynnswig.com, a common tactic in phishing to appear more credible. The absence of DMARC and SPF records for lynnswig.com and the direct indication of the email's malicious nature based on the analysis of the attached PDF's hash value confirm the intent to deceive us.

**Email Header Analysis**:
- Subject Line: "Your Account Will be Temporary Suspended And Hold All Your Subscription"
- Sender Email: auth-replyP8YjBYJqsq@lynnswig.com
- Originating IP: 40.107.94.65, associated with Microsoft, possibly indicating a compromised server.
- SPF Check: Passed, but no DMARC record for lynnswig.com
- Received Path: Shows the email passed through several servers, including those typically used by Microsoft, which could be indicative of email server compromise or spoofing.

**Attachment Analysis**:<br>
- File Name: Support-1923819248-67889.pdf
- SHA256 Hash: 54abc6abba94940a13312f3030dcc9e0f9533dde6282aea31f82ee7f7be5ec4b
- Talos Reputation Check: File is marked as a malicious trojan
- VirusTotal Analysis: 
  - Detected as malicious trojan by 26 security vendors
  - Exhibits behavior tags like user input checks and masquerading
- MITRE ATT&CK tactics observed:
  - T1036 Masquerading: This tactic involves the malicious file disguising itself as a legitimate file to avoid detection. This could explain why it's named as a 'Support' file, possibly to trick users into thinking it's a benign document. 
  - T1082 System Information Discovery: This indicates that the file has capabilities to gather information about the system it infects. Such data can be used for further attacks or exploitation.
 
**Response and Recommendations**:

- Blocked Domain: Immediately blocked the sender's domain (lynnswig.com) at the email gateway to prevent further phishing attempts from the same source
- Isolate and Scan: Isolated systems where the email was opened, and performed antivirus scans to detect any malware
- Change Passwords: Advise the targeted individual and others in the department to change their passwords for security.
- Employee Awareness Training: Conduct regular training for employees on identifying and handling phishing attempts.
- Enhance Email Security: Implement advanced email filtering solutions to detect and block phishing attempts.
- Update Incident Response Plan: Review and update the incident response plan to include protocols for dealing with phishing and other cyber threats.

***End of Report***

</details>

<details>
  <summary><h2><b>Conclusion: Project Reflection</b></h2></summary>
  
  What a ride! This project was not just educational, but also a whole lot of fun. It was like being a detective in the digital world, uncovering the secrets of phishing emails and cracking the code of complex security reports. Each step was a new adventure, boosting my problem-solving skills and making me more confident in navigating the digital landscape. I’m more excited than ever to dive deeper into this field and face new challenges head-on
</details>
