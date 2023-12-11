# Fishy Phishing

![Placeholder Image for Project](https://i.imgur.com/linkToPlaceholderImage.png)

## Objective

The objective of this project is to analyze real-world phishing emails to understand and document their tactics and characteristics. Using a combination of tools within a Kali Linux VM, and scripts to automate data collection from GitHub, the project aims to dissect phishing emails, identify threat vectors, and create awareness materials. This setup is intended to simulate a SOC environment for practical experience in phishing analysis.

## Components

- **VirtualBox**: For creating and managing a secure virtual environment.
- **Kali Linux VM**: A specialized Linux distribution for cybersecurity tasks.
- **GitHub**: Source for phishing email samples, accessed through Python scripting.
- **Python**: Scripting language for automating the fetching of email samples.
- **Thunderbird**: Email client to safely view and analyze phishing emails.
- **Talos Intelligence**: For additional threat intelligence and contextual analysis.
- **VirusTotal**: To analyze suspicious links and attachments for threats.
- **PhishTool**: Specialized tool for detailed phishing email analysis.
- **CheckShortURL**: For safely expanding and analyzing shortened URLs.

<details>
  <summary><h2><b>Section 1: Setting Up the Virtual Environment</b></h2></summary>
  In this section, we'll go through the initial steps of setting up our virtual environment using Kali Linux. Together, we'll update and upgrade the system, install and configure Thunderbird, and make sure our Python environment is ready for our phishing analysis project.

  - **Step 1: Update and Upgrade Kali Linux**:  
    Let's start by ensuring that our Kali Linux system is up-to-date. This is crucial for security and functionality.
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```
    ![Placeholder Image 1 for Update](https://i.imgur.com/linkToUpdateImage1.png)
    ![Placeholder Image 2 for Update](https://i.imgur.com/linkToUpdateImage2.png)
    
  - **Step 2: Install and Configure Thunderbird**:  
    Next, we'll install Thunderbird, which will be our tool for safely viewing and managing phishing emails.
    ```bash
    sudo apt install thunderbird -y
    ```
    Once installed, let's configure Thunderbird for offline use to prevent any accidental connectivity to potentially harmful content.
    ![Placeholder Image 1 for Thunderbird](https://i.imgur.com/linkToThunderbirdImage1.png)
    ![Placeholder Image 2 for Thunderbird](https://i.imgur.com/linkToThunderbirdImage2.png)

  - **Step 3: Verify and Set Up Python Environment**:  
    We need to ensure that Python is correctly installed and set up, as it's essential for our scripting needs.
    - Check the installed Python version:
      ```bash
      python3 --version
      ```
    - If Python is not installed or needs updating, we can install it like this:
      ```bash
      sudo apt install python3 -y
      ```
    - Next, we'll install pip, Python's package manager, and the 'requests' library:
      ```bash
      sudo apt install python3-pip -y
      pip3 install requests
      ```
    ![Placeholder Image 1 for Python Setup](https://i.imgur.com/linkToPythonSetupImage1.png)
    ![Placeholder Image 2 for Python Setup](https://i.imgur.com/linkToPythonSetupImage2.png)

  - **Step 4: Additional Tool Setup (If Required)**:  
    If there are any additional tools like PhishTool or CheckShortURL we want to use, now is the time to install and configure them following their respective installation guides.
    ![Placeholder Image 1 for Additional Tools](https://i.imgur.com/linkToAdditionalToolImage1.png)
    ![Placeholder Image 2 for Additional Tools](https://i.imgur.com/linkToAdditionalToolImage2.png)

  And there we have it! Our Kali Linux VM is now updated, Thunderbird is configured, and our Python environment is all set. We've successfully created a solid foundation for our phishing email analysis project.

</details>


<details>
  <summary><h2><b>Section 2: Collecting and Analyzing Phishing Emails</b></h2></summary>
  This section focuses on the collection and initial analysis of phishing emails using Python scripts and GitHub. We'll also begin our examination of the emails using Thunderbird and our specialized tools.

  - **Step 1: Fetch Phishing Email Samples**:  
    Use the Python script to automate the downloading of phishing email samples from GitHub.

  - **Step 2: Initial Sorting and Categorization of Emails**:  
    Organize the emails into different categories for a more structured analysis.

  - **Step 3: Conduct Detailed Analysis**:  
    Using Thunderbird, PhishTool, and other tools, start dissecting the emails for phishing indicators and tactics.
    
  ![Placeholder Image for Email Analysis](https://i.imgur.com/linkToEmailAnalysis.png)

  This detailed analysis will form the core of our project, allowing us to understand and document the various techniques used in phishing attacks.

</details>

<details>
  <summary><h2><b>Section 3: Creating Awareness and Educational Materials</b></h2></summary>
  Based on our findings from the analysis, this section will focus on developing educational content to help individuals and organizations recognize and protect against phishing attacks.

  - **Step 1: Compile Findings**:  
    Assemble all documented analysis into a comprehensive format.

  - **Step 2: Develop Educational Content**:  
    Create guidelines, best practices, and other materials using real examples from our analysis.
    
  ![Placeholder Image for Educational Content](https://i.imgur.com/linkToEducationalContent.png)

  The completion of this section will mark the end of our project, equipping us with valuable materials to raise awareness about phishing threats.

</details>
