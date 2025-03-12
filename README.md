# CVE Vulnerability Analyzer

## Overview

CVE Vulnerability Analyzer is a Python tool that fetches the latest vulnerability data (CVE) from public databases and scans your system for specific software versions. Currently, it checks for:
- **nginx**
- **OpenSSL**
- **Apache**

The tool then generates a report that combines scan results with CVE details.

## How It Works

1. **Fetch CVE Data:**  
   The tool connects to a public API (e.g., the NVD API v2.0) to retrieve the most recent vulnerability data.

2. **System Scanning:**  
   The `vulnerability_scanner.py` module uses system commands (via the `subprocess` module) to determine the installed versions of software. For example:
   - For **nginx**: It runs `nginx -v` and parses the version.
   - For **OpenSSL**: It runs `openssl version`.
   - For **Apache**: It runs `apache2 -v` (or `httpd -v`).

3. **Report Generation:**  
   The `report_generator.py` module uses a Jinja2 HTML template to create a detailed report of the scan results alongside the fetched CVE data.

## Customizing the Scanner

If you want to scan for different software, follow these steps:

1. **Update the Command:**  
   In `vulnerability_scanner.py`, add a new function to run the appropriate command. For example, to scan for MySQL, you could run:
   ```python
   mysql_output = get_command_output("mysql --version")
   ```

2. **Parse the Output:**  
   Create a parser function similar to the existing ones. For example:
   ```python
   def parse_mysql_version(output):
       # Example output: "mysql  Ver 8.0.21 for Linux on x86_64 (MySQL Community Server - GPL)"
       if output:
           parts = output.split()
           if len(parts) > 2:
               return parts[2]
       return "N/D"
   ```

3. **Integrate the New Check:**  
   Modify the `scan_system()` function to include the new software:
   ```python
   def scan_system():
       results = {}
       # Existing checks...
       results["mysql"] = parse_mysql_version(get_command_output("mysql --version"))
       return results
   ```

4. **Test and Validate:**  
   Run the scanner to ensure that the new software version is correctly detected and displayed in the report.

## Running the Tool

1. **Install Dependencies:**  
   Create a virtual environment and install required packages:
   ```bash
   python -m venv venv
   source venv/bin/activate       # On Windows use: venv\Scripts\activate
   pip install -r requirements.txt
   ```

2. **Run the Application:**  
   Execute the main script:
   ```bash
   python main.py
   ```

The report will be generated, displaying your system's scan results along with the latest CVE details.


## License

This project is licensed under the MIT License.

Author
---
Bartłomiej Dziura
