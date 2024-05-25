# API Automation Project

This API automation project involves testing three APIs: Get Verification Token, Member Registration, and PIN Setup. Additionally, it includes an SSH SFTP setup to collect log data required for the process.

1. Get Verification Token (Request Method: POST )
Preprocessor
Generates a random mobile number and assigns it to the variable mobileNumber:

import org.apache.commons.lang3.RandomStringUtils;
String randomNumber = RandomStringUtils.randomNumeric(8);
String mobileNumber = "016" + randomNumber;
vars.put("mobileNumber", mobileNumber);
log.info("Generated Mobile Number: " + mobileNumber);

JSON Extractor
Extracts the verification token from the response: $.verificationToken
------------------------------------
2. Member Registration (Request Method: POST )
JSON Extractor
Extracts the user ID from the response: $.userId
----------------
3. SSH SFTP Setup
Regular Expression Extractor
Extracts the activation code from the server log using the pattern
"user\.name":"([^"]+)"[^}]*"activation\.code":"(\d+)"
Source Path: /home/knotify/logs/knotify-io-core1-qa-INST_1.log

Postprocessor (Processes the extracted activation codes )

int matchCount = 0;
while (vars.get("logentry_" + (matchCount + 1)) != null) {    matchCount++; }
for (int i = 1; i <= matchCount; i++) {    log.info("logentry_" + i + ": " + vars.get("logentry_" + i));}
String activationCode = vars.get("logentry_" + matchCount); vars.put("activationCode", activationCode);
------------------------------------
4. PIN Setup (Request Method: POST )
{
    "aspId": "953365000000000",
    "userId": "${userId}",
    "loginPin": "E0BC60C82713F64EF8A57C0C40D02CE24FD0141D5CC3086259C19B1E62A62BEA",
    "paymentPin": "061123B1E798FF90",
    "otp": "289993"
}
----------------------
SSH SFTP Connection Process
To establish an SSH SFTP connection, use the following steps:

Ensure SSH SFTP Plugin: Ensure that you have an SSH SFTP plugin installed in JMeter.
Download Jmeter plugins manager jar file and keep in jmeter bin folder.
Choose SSH protocol support from plugin manager.
open a new sampler called SSH SFTP from thread.
Configure Connection: Use the provided hostname, port, username, and password to configure the connection.
SSH SFTP Sampler: For connecting to the server and extracting logs
