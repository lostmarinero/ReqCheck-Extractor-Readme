# ReqCheck-Extractor-Readme
This repo is a host of Readme files for 'ReqCheck-Extractor', and Extract-Transform-Load process developed by the 2016 Code for America Fellowship Team working with the Kansas City, Missouri Health Department. Unfortunately, due to the sensitive nature of the data and the database, we are unable to share the full repo. Additionally, some sensitive information has been redacted. The goal of this repo is to demonstrate the work required to complete this task.

# ReqCheck-Extractor

This is the original ReqCheck-Extractor Project, developed by the Code for America's 2016 Kansas City, Missouri (KCMO) Fellowship Team in collaraboration with the KCMO Health Department.

A special thank you to the staff at the [Kansas City Health Department](http://kcmo.gov/health/clinic-services/) for their help developing it.

This project is a script for extracting data from a database, transforming it, and sending it to a website running [Project ReqCheck](https://github.com/KCMOHealthDepartment/reqcheck).

## How It Works
This is a project, written in Python, that can be bundled into a Microsoft Batch File (*.bat).

### Script Process
1. The script calls the ReqCheck application at the `/heartbeat` path and receives the date of the last import
    * PatientDataImport and VaccineDataImport are referenced, and the ReqCheck application sends the earlier of the two values
    * The script passes this value to the queries as a variable
        * Queries can use this value if they declare a variable with `?`
2. The script queries the database using SQL scripts and sends the data via HTTPS to an endpoint
    * The queries are stored in the [`/queries`](/queries) directory
    * The HTTPS payload layout must be defined in the [`/templates`](/templates) directory
    * The location of the ReqCheck application is defined in the **[config.json](config.json)** file

### Setup on the Server
This script can be set to be run by a 'worker' or 'task' (in the case of a Microsoft SQL Server) so that the data can be queried on a consistent basis and updated in the ReqCheck production database.

Ensure that the server has the ability to make HTTPS outbound calls. We recommend using a development ReqCheck endpoint with fake data to test the connection before using production data.

**It is up to the implementing team and Kansas City Health Department to ensure all HIPAA requlations are being followed. Code for America and the Code for America Fellows recommend fully inspecting the codebase and process to ensure it is HIPAA compliant before using. In its current form (Nov 2016), it is NOT HIPAA compliant and needs additional work, including setting up logging.**

## How ReqCheck Accepts the Data
ReqCheck has been built with 3 special API endpoints for sending a **POST**:
    - `/heartbeat`
    - `/patient_data`
    - `/vaccine_dose_data`


## Getting Started

### Built With

* [Python 3.4.4](https://www.python.org/downloads/release/python-344/)
* [Pip 8.1.2](https://pypi.python.org/pypi/pip/8.1.2)
* [requests 2.9.1](http://docs.python-requests.org/en/master/)
* [cx-Freeze 4.3.4](http://cx-freeze.sourceforge.net/)

Ensure the environment is set up with Python 3.4.4, as this is what is needed to support cx_Freeze and the other dependencies.


## Installing
#### Environment Variables
There are none!

#### Config.Json
For information regarding how to configure the application, please see the [Config Readme](CONFIG-SQL.md)

Do note that the "reqcheck_username" and "reqcheck_password" must be set.

#### Clone the Repo
```
git clone (REDACTED)
```

#### Install Dependencies
```
pip install -r requirements.txt
```

>**Note: cx_Freeze Dependency and Installation Issues**
>cx_Freeze can only be used on a windows machine! You must have a VM or Windows OS with python installed on it**

## Deployment

This project will be fully hosted once configuration of the servers are completed so that they are in compliance with HIPAA.

**PLEASE NOTE: This application does not guarantee HIPAA compliance. It is the responsibility of the implementers to ensure it passes their HIPAA compliance standards. The developing team and organization assumes no responsibility for this.**

#### Move Code to Server

The first step is to move the code onto the server. Due to the sensitive nature of the configuration files (which are not tracked in git), we recommend physically moving it onto the server or doing so through a secure connection.

#### Build the msi on a Windows Machine

1. Open powershell as an administator.
2. Go to the directory with the code in it. The "OFFICIAL COMPORT VIRTUAL BOX IMAGE" has the code stored in a shared folder which you can get to with the somewhat cumbersome `cd Microsoft.PowerShell.Core\FileSystem::\\vboxsvr\vbshare\reqcheck-extractor`. It's a shared folder only to allow code to be edited off of the vm, but if you're comfortable doing that you can keep things somewhere more sensible.
3. `pip install -r requirements.txt` if you're using the existing vm everything should be there already.
4. `python .\setup.py bdist_msi` will put an msi in the `dist` folder and a copy of what the installed application will look like in the `build\exe.win32-3.4` folder
5. running the exe in `build\exe.win32-3.4` will let you test the application without running through the msi installation

#### Setup the Task Scheduler

Using whichever task schedule is installed on the server, setup the scheduler to call the `launch.bat` file. 

**Please note** that the `launch.bat` file will need to call the config, query and template files that are relative to it, so do not move it outside of its containing directory.

## To Do

A few tasks need to be completed in order to be HIPAA compliant, and this must be done by the implementation team. Do note that logging has not been addressed, which must happen in order to provide the correct logs for audits.

## Contribute
Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* **[Thomas Apodaca](https://github.com/tmaybe)**
* **[Edmund Korley](https://github.com/emkk)**
* **[Chris Reade](https://github.com/creade)**
* **[Kevin Berry](https://github.com/lostmarinero)**

**A special thanks to [Thomas Apodaca](https://github.com/tmaybe) for his mentorship on this project**

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under Code for America's copyright. Please see the [License file](LICENSE.md)

## Acknowledgments

Thank you to our funders and the Code for America staff for their support. Without this support, this project would not have been possible.

* [Health Care Foundation of Greater Kansas City](http://hcfgkc.org/)
* [The Robert Wood Johnson Foundation](http://www.rwjf.org/)
* [Reach Healthcare Foundation](https://reachhealth.org/)
* [Google Fiber](https://fiber.google.com/)

Also, thank you to everyone involved in the research and development

* The Staff at the Kansas City, Missouri Health Department


## HIPAA Compliance

The [Health Insurance Portability and Accountability Act (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act) outlines national security standards intended to protect health data created, received, maintained, or transmitted electronically.

To review what has been done, please visit the [HIPAA Readme](HIPAA.md).

