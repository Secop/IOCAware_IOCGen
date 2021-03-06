IOCAware OpenIOC Cuckoo Reporting Module
======

Description:

This is the base reporting module for Cuckoo (currently built against 0.6.0) for automatically generating OpenIOC IOCs

DEPENDENCIES

1) Cuckoo 0.6.0 - you will need an actual working version of cuckoo for this to work
  http://www.cuckoosandbox.org
  
2) Relies on Mandiant's ioc_writer >= 0.2.0, found here (for the OpenIOC portion):
  https://github.com/mandiant/ioc_writer


INSTALLATION:

1) Modify CUCKOO_HOME/conf/reporting.conf at the end with the following:
```ini
[iocaware_openioc]
enabled=on

# (Optional) Directory where the module outputs IOCs.
# "{reports_path}" is replaced with reports path (e.g. storage/analyses/1/reports/).
# Default: {reports_path}
#output_dir=/home/iocaware/Documents/iocs

# (Optional) IPs excluded from the IOC
#excludedips=192.168.56.101,192.168.56.255
```

2) Put the script, iocaware_openioc.py into the following directory:

CUCKOO_HOME/modules/reporting/iocaware_openioc.py

3) OPTIONAL - currently, the reporting module returns registry keys that are been opened or created. I modified the
cuckoo code to only pull created keys (for now). To do this: 
   - open up CUCKOO_HOME/modules/processing.behavior.py
   - find the line with "RegCreateKeyEx" (line 276 in the version of Cuckoo I've been using)
   - change it to: if call["api"].startswith("RegCreateKeyEx"):

ADDITIONAL NOTES:

There are several sections of the iocaware_openioc.py script that can be modified for more customized use:

   - Add/Delete/Modify the API calls in the suspiciousimports variable; items in this variable will be included in the IOC
   - Add/Delete/Modify the pe sections considered "good" in the goodpesections variable; items in this variable will NOT be in the IOC
   - Add/Delete/Modify the string regexes in the doStrings method to pull more and/or better strings out of the binary
