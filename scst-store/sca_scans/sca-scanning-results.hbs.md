# SCA scanning results
## <a id='102'></a>1.0.2

### <a id='black-duck-ba'></a>Black Duck Binary Analysis (BDBA)

#### API backend

* Date: January 31, 2022
* Results: no known vulnerabilities

#### CLI

* Date: January 24, 2022
* Results: no known vulnerabilities

### Grype

Version: 0.32.0

#### API Backend Container Image

* Date: February 2, 2022
* Results: No high or critical vulnerabilities. Multiple medium and low vulnerabilities. For more information, see the [CycloneDX file content](cyclonedx-file-content.md).

#### API Backend Code Repository

* Date: February 2, 2022
* Results: no known vulnerabilities

#### CLI Code Repository

* Date: February 2, 2022
* Results: no known vulnerabilities

## 1.0.0
Date: November 26, 2021

### Scan Type:

Software Composition Analysis scanning

### Source of Scan:

* Black Duck Binary Analysis (BDBA)
* Grype

### Version of Source:

* BDBA version 2021.9.0
* Grype version 0.25.1

### <a id='cves'></a>CVEs:

#### <a id='bdba'></a>BDBA

No vulnerabilities were found in the API backend and CLI binaries.

See BDBA reports:

- [API backend report](store-bdba-scan-2021-11-26.jpg)
- [CLI report](cli-bdba-scan-2021-11-26.jpg)

#### <a id='grype-cr'></a>Grype

No vulnerabilities were found through scanning the API back end sources, client lib, and CLI.

The following CVEs were found through scanning the API back end image:

```
NAME   INSTALLED        FIXED-IN  VULNERABILITY   SEVERITY   
libc6  2.27-3ubuntu1.4            CVE-2015-8985   Negligible  
libc6  2.27-3ubuntu1.4            CVE-2016-10739  Low         
libc6  2.27-3ubuntu1.4            CVE-2020-6096   Low         
libc6  2.27-3ubuntu1.4            CVE-2021-3326   Low         
libc6  2.27-3ubuntu1.4            CVE-2020-27618  Low         
libc6  2.27-3ubuntu1.4            CVE-2019-25013  Low         
libc6  2.27-3ubuntu1.4            CVE-2021-35942  Medium      
libc6  2.27-3ubuntu1.4            CVE-2021-33574  Low         
libc6  2.27-3ubuntu1.4            CVE-2021-38604  Medium      
libc6  2.27-3ubuntu1.4            CVE-2016-10228  Negligible  
libc6  2.27-3ubuntu1.4            CVE-2009-5155   Negligible  
```

No `high` or `critical` CVEs present.
