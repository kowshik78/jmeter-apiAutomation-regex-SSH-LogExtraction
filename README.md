# API Automation Project with server log data extraction

This API automation project involves testing sample three APIs: Get Verification Token, Member Registration, and PIN Setup. Additionally, it includes an SSH SFTP setup to collect log data required for the process.</br>
Proprocessors and postprocessors are used to collect data from request and reponse JSON.</br>
For some random data Jmeter inbuild functions are used.</br>

<h4>1. Get Verification Token (Request Method: POST )</h4> <br />
<ins>Preprocessor Generates a random mobile number and assigns it to the variable mobileNumber:</ins><br/>
import org.apache.commons.lang3.RandomStringUtils;  <br />
String randomNumber = RandomStringUtils.randomNumeric(8); <br />
String mobileNumber = "016" + randomNumber; <br />
vars.put("mobileNumber", mobileNumber); <br />
log.info("Generated Mobile Number: " + mobileNumber); <br />
<ins> Id and name is generated using jmeter functions directly: </ins><br/>
euId = ${__Random(1000000000,9999999999 )} <br/>
realName = ${__RandomString(10,abcdefghijklmnopqrstuvwxyz)}<br>

<h4>2. Member Registration (Request Method: POST )</h4> <br />
<ins>Preprocessor is used to generate random valid mobile number</ins> <br />
String randomNumber = RandomStringUtils.randomNumeric(8);<br />
String mobileNumber = "016"+randomNumber;<br />
vars.put("mobileNumber",mobileNumber);<br />
log.info("!!!!!!!!!!!!!!"+mobileNumber);<br />

<h4>3. SSH SFTP Setup</h4> <br />
<ins>Regular Expression Extractor</ins> <br />
Extracts the activation code from the server log using the pattern <br />
"user\.name":"([^"]+)"[^}]*"activation\.code":"(\d+)" <br />
Source Path: /home/knotify/logs/knotify-io-core1-qa-INST_1.log <br />

<ins>Postprocessor (Processes the extracted activation codes )</ins> <br />
int matchCount = 0; <br />
while (vars.get("logentry_" + (matchCount + 1)) != null)<br/> 
{    matchCount++; } <br />
for (int i = 1; i <= matchCount; i++)<br/> 
{    log.info("logentry_" + i + ": " + vars.get("logentry_" + i));} <br />
String activationCode = vars.get("logentry_" + matchCount);<br/> 
vars.put("activationCode", activationCode); <br />

<h4>4. PIN Setup (Request Method: POST )</h4> <br />
    "aspId": "953365000000000", <br />
    "userId": "${userId}", <br />
    "loginPin": "E0BC60C82713F64EF8A57C0C40D02CE24FD0141D5CC3086259C19B1E62A62BEA", <br />
    "paymentPin": "061123B1E798FF90", <br />
    "otp": "289993" <br />

<h2>SSH SFTP Connection Process</h2> <br />
<ins>To establish an SSH SFTP connection, use the following steps:</ins> <br />

Ensure SSH SFTP Plugin: Ensure that you have an SSH SFTP plugin installed in JMeter. <br />
Download Jmeter plugins manager jar file and keep in jmeter bin folder. <br />
Choose SSH protocol support from plugin manager. <br />
open a new sampler called SSH SFTP from thread. <br />
Configure Connection: Use the provided hostname, port, username, and password to configure the connection. <br />
SSH SFTP Sampler: For connecting to the server and extracting logs <br />
