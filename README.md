# Security Analysis Handout

## Overview

Static analysis is a technique used to analyze the source code of software applications without executing it. It involves analyzing the code to find potential bugs, security vulnerabilities, and violations of coding standards before the software is run. This analysis can be done either manually or by using automated tools that parse and inspect the code for patterns, syntax, and structure. An example of a static analysis tool that you have already used is ESLint, which analyzes your code structure to look for violations of pre-defined style rules.

Similarly, static analysis tools scan the code's structure and logic to identify possible vulnerabilities that could lead to security issues based on pre-defined rules. These tools don't require running the program, making them useful for identifying problems early in the development lifecycle. Common vulnerabilities detected by static analysis include:

- **Data leak**. Detected by checking if sources of sensitive data flow into publicly available parts of the code (e.g., writing password to a file that can be read by all).

- **SQL injection risks**: Found by identifying unsanitized user inputs that could be used to manipulate SQL queries.

- **Cross-site scripting (XSS)**: Identified by looking for unsafe handling of user input in web applications.

By analyzing the code's flow, dependencies, and data handling, static analysis tools can identify weaknesses such as insecure data storage or improper handling of user authentication, helping developers address security risks before they become issues in production.

Furthermore, static analysis tools are closely related to threat modeling because they help identify security risks that could be exploited during an attack. In threat modeling, developers map out potential threats and attack vectors to identify where vulnerabilities may exist. Static analysis tools automate this process by scanning the codebase to identify areas that match known patterns of vulnerabilities, such as improper input validation or insecure data transmission.

## CodeQL Security Analyzer

CodeQL is a static analysis tool developed by GitHub that uses a **query language** to analyze code for security vulnerabilities and code quality issues. It operates by interpreting the code’s syntax and structure to identify patterns that could lead to vulnerabilities. By using CodeQL, security vulnerabilities can be detected early, enabling developers to fix issues before they impact the application in production. CodeQL comes with a rich set of pre-built queries specifically designed for various programming languages, including JavaScript and TypeScript. These built-in queries are designed to detect common vulnerabilities in web applications similar to the STRIDE threats discussed in the lecture on threat modeling.

CodeQL comes with a set of built-in queries but developers can extend them with additional queries to suit the security requirements of their projects. In our case, we will use the built-in set of queries.

## Setup CodeQL

1. Download the [Code QL bundle](https://github.com/github/codeql-action/releases/tag/codeql-bundle-v2.19.3) for your operating system.

2. Extract the archive to a directory of your choosing. Let's call this directory `<extraction-root>`

3. Verify the installation by running the following command:

   `$ <extraction-root>/codeql/codelql resolve packs`

   If the installation was succesful, in the output for the above command you should be able to see the path to the ql packs for javascript. The location should be within the extracted CodeQL CLI bundle in the directory `<extraction-root>`. If the CodeQL CLI is unable to locate the qlpacks for the expected languages, check that you downloaded the CodeQL bundle and not a standalone copy of the CodeQL CLI.

4. Prepare the code for analysis by running the following command:

   `$ <extract-root>/codeql/codeql database create <database> --language=javascript-typescript`

   `<database>` is the path to the directory of your choosing where you want the CodeQL database of your source code to be stored. Remember CodeQL first creates an intermediate representation of your source code before performing the analysis. This intermediate representation is called [semmle](https://en.wikipedia.org/wiki/Semmle).

5. Analyze the code and generate report using the following command:

   `$ ~/codeql/codeql database analyze <path/to/codeql-database> --format="sarif-latest" --output <path/to/report/report.sarif>`

6. Analyze the report to see potential vulnerabilities in your code and take steps to fix them.

7. You can run the following script to obtain quick stats about the report:

   `$ npx ts-node codeql-quick-stats.ts`

## For You To Do

Fork this repository. Run CodeQL on the source code. On analyzing the generated report, you will find NoSQL injection vulnerabilties in your code. Fix the vulnerabilties and push to a new branch.
