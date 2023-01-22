# OWASP ZAP - Full Scan

![OWASP](https://miro.medium.com/max/628/1*9WByofiXcIpUKR-XooXaPA.jpeg)

Tools used:
* Spider:
  * The spider is a tool that is used to automatically discover new resources (URLs) on a particular Site. It begins with a list of URLs to visit, called the seeds, which depends on how the Spider is started. The Spider then visits these URLs, it identifies all the hyperlinks in the page and adds them to the list of URLs to visit and the process continues recursively as long as new resources are found.
* Ajax Spider:
  * The AJAX Spider add-on integrates in ZAP a crawler of AJAX rich sites called Crawljax. You can use it to identify the pages of the targeted site. You can combine it with the (normal) spider for better results.
* Active Scan:
  * Active scanning attempts to find potential vulnerabilities by using known attacks against the selected targets.

  -----


## Run localy

to run locally you'll need to run this commands:

```
$ pip install --upgrade zapcli

$ docker run --detach --name zap -u zap -v "$(pwd)/reports":/zap/reports/:rw -i owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080 -config api.addrs.addr.name='.*' -config api.addrs.addr.regex=true -config api.disablekey=true

$ docker exec zap zap-cli --verbose open-url "https://*****" && docker exec zap zap-cli --verbose open-url "https://https://*****"

$ docker exec zap zap-cli --verbose quick-scan "https://https://*****" -l Medium && docker exec zap zap-cli --verbose quick-scan "https://https://*****" -l Medium

$ docker exec zap zap-cli --verbose open-url "https://target"

$ docker exec zap zap-cli --verbose quick-scan "https://target" -l Medium

$ docker exec zap zap-cli --verbose report -o /zap/reports/owasp-quick-scan-report.html --output-format html
```

To add a new project, create a new script and add the url corresponding to the PROD endpoint

On the project build job add the snippet in the end of the jenkinsfile:

```
def triggerSecurityScans() {
  def targetProject = 'XYZ' // Use the project naming provided above for current enabled projects
      stage(‘Trigger ZAP Scans’) {
        build job: ‘../*****/zap-fullscan/master’,
        parameters: [[$class: ‘StringParameterValue’, name: ‘project’, value: targetProject]],
        wait: false
      }
}
```

and call it on the finally step:

`triggerSecurityScans()`

[source](https://medium.com/isurfbecause/how-does-this-work-f0800dc2e7e)