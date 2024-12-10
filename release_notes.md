# What's New

<table>
    <tr>
        <th>Compatible Version</th>
        <td>FortiSOAR v7.6.1 and later</td>
    </tr>
</table>

<table>
    <th>NOTE</th>
    <td>Before updating the SOAR Framework solution pack, make sure to back up any modifications you have made to <strong>System View Templates (SVTs)</strong> and <strong>Playbooks</strong>. This is essential because updating the solution pack overwrites any changes you might have made.
    </td>
</table>

## Streamlined Indicator Extraction

The indicator extraction configuration has a friendly, wizard-like, user interface. This configuration wizard is accessible from the button **Manage Indicator Exclusion List** under **Setup Guide** > **STREAMLINE** > **Configure Indicator Extraction**.

This new feature **Indicator Extraction Configuration**, now eases the process of:

- defining indicators to be excluded from extraction, in small groups as well as in *bulk*
- mapping alert and incident fields to be extracted as indicators
- creating custom indicator types

### Configuration Settings Migration

The following playbooks migrate indicator extraction settings to the new keystore records

- **Excluded Indicators Migration**

- **Excluded Indicators Migration > Fetch Migration Data**

<table>
    <th>IMPORTANT</th>
    <td>Refer to the section <em>Upgrading SOAR Framework to the latest version</em> to know what this change entails and how it affects users.</td>
</table>

### Enhanced Playbooks for Consistent Integration

The following playbooks now read from, and write to, the new keystore record instead of the respective global variables:

- **03 - Enrich > Extract Indicators (Alerts)** and **03 - Enrich > Extract Indicators (Incidents)** now refer to this key store record for *Exclude List* and *Field Type Mapping*.

- **03 - Enrich > Enrich Indicator (Type IP)** now refer to this key store record for CIDR Range data exclusively.

- **04 - Actions** > **Add Exclude List to Keystore** adds an indicator to the *Exclude List* in the new key store record.

Refer to the section *Extending default indicator extraction process* to understand the improved extraction configuration process.

## Playbook Enhancements

### Upgraded Action for Artifact Extraction

The solution replaces the deprecated **Extract Artifacts from String** action from the Utilities connector with the updated version from Utilities v3.5.0. The following playbooks were updated to accommodate this change:

- **03 - Enrich > Extract Indicators (Alerts)**
- **03 - Enrich > Extract Indicators (Incidents)**
- **03 - Enrich > Extract Indicators from Attachments**
- **05 - Hunt > Hunt Indicators**
- **08 - Utilities > Indicator - Import Bulk Indicators**

### Limiting Playbook Edits

With FortiSOAR `v7.6.1` editing system playbooks (playbooks shipped with SOAR Framework solution pack) is now restricted with the exception of the following:

- All playbooks under the collection **04 - Actions**.
- The playbook  **Alert - Close Corresponding SIEM Alert** under the collection **06 - IRP - Case Management**.

For all other playbooks, users must create a copy to make edits.

### Safeguarding User-Modified Playbooks

Previously, playbooks included with the *SOAR Framework Solution Pack* would overwrite any user-modified versions during an upgrade.

Now,
- User-modified playbooks are preserved.
- Shipped playbooks are saved as a separate version labeled `Base`. Refer to the following **Saving versions of your playbook

This enhancement ensures that user changes remain intact after upgrade while still allowing access to the latest shipped versions for reference or use.

### Additional Enhancements

- The **Full App Permission** role no longer allows deletion of **Key Store** records.

- The tasks in **Streamline** section, under **Setup Guide**, have been reordered.
    - The button *Configure Exclude List* is now renamed to **Manage Indicator Exclusion List**  

- Default list of pre-installed connectors has been reduced to the following:

    - Exchange
    - File Content Extraction
    - Fortinet FortiEDR
    - Fortinet FortiGate
    - Fortinet FortiClient EMS
    - VirusTotal
    - Whois RDAP

<table>
    <th>NOTE</th>
    <td>This change is applicable only to new installations. Additional connectors can still be downloaded and installed from the Content Hub.</td>
</table>
