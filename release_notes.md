# What's New

<table>
    <tr>
        <th>Compatible Version</th>
        <td>FortiSOAR v7.6.1 and later</td>
    </tr>
</table>

<table>
    <tr>
        <th>NOTE</th>
        <td>Before updating the SOAR Framework solution pack, make sure to back up any modifications you have made to <strong>System View Templates (SVTs)</strong>, <strong>Module Metadata (MMD)</strong>, and <strong>Playbooks</strong>. This is essential because updating the solution pack overwrites any changes you might have made.</td>
    </tr>
</table>

## Indicator Extraction Enhancement

### Data Source
- Using a single keystore record instead of using multiple keystores we were using in previous version, **sfsp-indicator-extraction-configuration** to hold segregated data for each IOC type, enabling dynamic implementation. 
    - The field type mapping will be refered from **sfsp-indicator-extraction-configuration** instead of global variable `Indicator_Type_Map` 

- Introducing a new keystore, **sfsp-indicator-regex-mapping** to store regex patterns for indicator types. 

### Data Migration

Using the newly introduced “Post-Hook” feature in FSR 7.6.1, we will execute this playbook to migrate values from the legacy Global Variable and legacy Keystore to the new Keystore **sfsp-indicator-extraction-configuration** . 

#### Post Install Playbook Name 

- Excluded Indicators Migration 
- Excluded Indicators Migration > Fetch Migration Data 

### Related Playbooks Enhancements

- Following playbooks updated to work with the new `sfsp-indicator-extraction-configuration` keystore
    - 03 - Enrich > Extract Indicators (Alerts) and 03 - Enrich > Extract Indicators (Incidents)
        - Consumes Excludelist and Field Type Mapping data

    - 03 - Enrich > Enrich Indicator (Type IP)
        - Consumes only CIDR Range data


## SOAR Framework Optimization
In order to reduce installation time for the SFSP we have reduced number of connectors to be installed with SFSP
 
- Connectors to be Installed with SFSP:
    - Exchange
    - File Content Extraction
    - Fortinet FortiEDR
    - Fortinet FortiGate
    - Fortinet FortiClient EMS
    - Virustotal
    - Whois RDAP

- Connectors Removed from SFSP:
    - AlienVault OTX
    - APIVoid
    - CarbonBlack Response
    - Fortinet FortiGuard Threat Intelligence
    - Fortinet FortiSandbox
    - Fortinet FortiSIEM
    - IBM XForce
    - IP Quality Score
    - IPStack
    - Microsoft SCCM
    - MxToolbox
    - NMAP Scanner
    - URLScan.io

## Playbook Enhancements

### Replacement of Deprecated 'Extract Artifacts from String' Action in SFSP Playbooks

The old "Extract Artifacts from String" action from the Utilities connector is now deprecated. It has been replaced with the updated "Extract Artifacts from String" action from the Utilities v3.5.0 connector in all relevant SFSP playbooks.

The following playbooks have been updated:

- 03 - Enrich > Extract Indicators (Alerts)
- 03 - Enrich > Extract Indicators (Incidents)
- 03 - Enrich > Extract Indicators from Attachments
- 05 - Hunt > Hunt Indicators
- 08 - Utilities > Indicator - Import Bulk Indicators


### Editable Playbooks Setting

In FSR 7.6.1, the new "Is Editable" feature allows all playbooks in the "04 - Actions" collection to be edited directly by users. For playbooks outside this collection, users must make a copy to edit them, leaving the original playbook unchanged.


## Other Enhancements
- The role **Full App Permission** no longer has delete permissions on the module **Key Store** to prevent accidental deletion of the keystore records
