| [Home](../README.md) |
|----------------------|

# Extending Default Indicator Extraction Process

In FortiSOAR, the indicator extraction feature extracts indicators from alert/incident fields and enriches them using playbooks defined for the indicator type. In the indicator extraction, you can configure the extraction logic according to the alert/incident type and the associated field.

We recommend that you optimize the indicator extraction process and define extraction settings for each incident type as needed.

FortiSOAR has automated the indicator extraction process through sets of playbooks; however, you can enhance the indicator extraction process by adding more fields of interest to a playbook so that it picks more fields apart from the default ones specified in the playbook.

E.g., the default playbook may not collect a field of interest, say “targeted employee email address”. This field of interest must be a part of the alert that we intend to target. To add a field to the alert, refer to the section [Extending Default Alert Schema](./extending-default-alert-schema.md).

## Extending Default Alert and Incident Schema

<table>
    <tr>
        <th>NOTE</th>
        <td>For extending default alert and incident schema, refer to the <a href="https://github.com/fortinet-fortisoar/widget-indicator-extraction-configuration/blob/release/2.0.0/docs/usage.md#edit-configuration-settings">Indicator Extraction Configuration</a> widget settings.</td>
    </tr>
</table>

## Excluding Extracted Indicators from Enrichment

You can exclude certain extracted indicators from enrichment by adding them to an exclude list. This is done by adding the indicators to the key store records within the **Key Store** module.

<table>
    <th>NOTE</th>
    <td>The <strong>Key Store</strong> module installs with the <a href="https://fortisoar.contenthub.fortinet.com//list.html?contentType=all&searchContent=platform%20utilities">Platform Utilities</a> solution pack.</td>
</table>

Following keystore records are available for each indicator type:

1. `sfsp-indicator-extraction-configuration`: Stores segregated data for each IOC type, enabling dynamic implementation
2. `sfsp-indicator-regex-mapping`: Stores regex patterns for indicator types. 

<table>
    <tr>
        <th>NOTE</th>
        <td>For managing and modifying key store records, refer to the <a href="https://github.com/fortinet-fortisoar/widget-indicator-extraction-configuration/blob/release/1.0.0/docs/usage.md#edit-configuration-settings">Indicator Extraction Configuration</a> widget settings.</td>
    </tr>
</table>


### Adding Comment to Excluded File Alerts

<table>
    <tr>
        <th>NOTE</th>
        <td>For adding comment to excluded file alerts configuration refer to the <a href="https://github.com/fortinet-fortisoar/widget-indicator-extraction-configuration/blob/release/2.0.0/docs/usage.md#edit-configuration-settings">Indicator Extraction Configuration</a> widget settings.</td>
    </tr>
</table>

### Skip Creating File Indicators

<table>
    <tr>
        <th>NOTE</th>
        <td>For skip creating file indicators configuration, refer to the <a href="https://github.com/fortinet-fortisoar/widget-indicator-extraction-configuration/blob/release/2.0.0/docs/usage.md#edit-configuration-settings">Indicator Extraction Configuration</a> widget settings.</td>
    </tr>
</table>

## Recommended/Advanced settings

When the Exchange connector playbooks create attachment records in the **Attachments** module for each file attachment in the email, they remove all special characters in the filename except `-`, `\`, and `.`. So a file indicator is created for files that may contain other characters in their names, even if they are in the exclude list.

<table>
    <tr>
        <th>Example</th>
        <td>A filename <code>Demo-File_Attachment.txt</code> in the exclude list has no effect as the underscore (<code>_</code>) is suppressed and a file indicator with the filename <code>Demo-FileAttachment.txt</code> is still created.</td>
    </tr>
</table>

Here are 2 possible ways to work around this situation:

1. Avoid using characters other than `-`, `\`, and `.` when adding filenames in the exclude list. For example, add the filename `Demo-FileAttachment.txt` instead of `Demo-File_Attachment.txt`, as the underscore (`_`) is suppressed by the Exchange ingestion playbooks.

2. Edit the `Upload File IOC and Create Attachment` step of the **> Exchange > Create Indicators and Attachments** playbook in the **Sample - Exchange - [version]** collection and modify the regex so that it does not suppress `_` or similar harmless characters in filenames.

    Remove the following:

    ```jinja
    "{{vars.input.params.attachmentMetadata.metadata.filename.split("/")[-1] | regex_replace("[^A-Za-z0-9. /\-]", "") if "/tmp/" in vars.input.params.attachmentMetadata.metadata.filename else vars.input.params.attachmentMetadata.metadata.filename |regex_replace("[^A-Za-z0-9. /\-]", "")}}"
    ```

    Replace with the following:

    ```jinja
    "{{vars.input.params.attachmentMetadata.metadata.filename.split("/")[-1] | regex_replace("[^A-Za-z0-9. /\-_]", "") if "/tmp/" in vars.input.params.attachmentMetadata.metadata.filename else vars.input.params.attachmentMetadata.metadata.filename |regex_replace("[^A-Za-z0-9. /\-_]", "")}}"
    ```

# Next Steps

| [Installation](./setup.md#installation) | [Configuration](./setup.md#configuration) | [Usage](./usage.md) | [Contents](./contents.md) |
|-----------------------------------------|-------------------------------------------|---------------------|---------------------------|
