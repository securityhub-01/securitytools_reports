# securitytools_reports
Compares multiple web application security tool reports and generates a consolidated report 
compares multiple web application security tool reports and generates a consolidated report by mapping the same CVE into one finding, you can follow these steps:

Parse the input reports: Assume the reports are in JSON format. You can use Python's json module to parse them.

Map CVEs: Create a dictionary to map CVEs to their respective findings.

Generate a consolidated report: Combine the findings and output the result in a desired format (e.g., JSON, CSV).

import json
from collections import defaultdict

def parse_report(file_path):
    """Parse a JSON report file and return its data."""
    with open(file_path, 'r') as file:
        return json.load(file)

def map_cves(reports):
    """Map CVEs across multiple reports into a single dictionary."""
    cve_map = defaultdict(list)

    for report in reports:
        for finding in report.get('findings', []):
            for cve in finding.get('cves', []):
                cve_map[cve].append({
                    'tool': report['tool'],
                    'finding': finding['description'],
                    'severity': finding['severity']
                })
    return cve_map

def generate_consolidated_report(cve_map, output_file):
    """Generate a consolidated report from the CVE map."""
    consolidated_report = []

    for cve, findings in cve_map.items():
        consolidated_report.append({
            'cve': cve,
            'findings': findings
        })

    with open(output_file, 'w') as file:
        json.dump(consolidated_report, file, indent=4)

def main():
    # List of report file paths
    report_files = ['report1.json', 'report2.json', 'report3.json']

    # Parse all reports
    reports = [parse_report(file) for file in report_files]

    # Map CVEs across reports
    cve_map = map_cves(reports)

    # Generate consolidated report
    generate_consolidated_report(cve_map, 'consolidated_report.json')

    print("Consolidated report generated successfully!")

if __name__ == "__main__":
    main()


    Explanation:
parse_report: Reads and parses a JSON report file.

map_cves: Maps CVEs across multiple reports into a dictionary where each CVE points to a list of findings from different tools.

generate_consolidated_report: Generates a consolidated report in JSON format.

main: The main function that orchestrates the process.

Input Report Format (Example):
Each report file (e.g., report1.json) should have a structure like this:
