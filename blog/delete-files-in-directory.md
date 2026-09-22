---
layout: default
permalink: /blog/delete-files-in-directory
---
# Delete Files in Directory
*Author: Brody Kilpatrick* | *Created: June 11, 2019*

The normal command line is still great to use for deletions, and can be quicker than powershell

Here is a very simple one-liner:

```
del /q C:\windows\temp\*.log
```

del - This is the command. Powershell has a command called "Remove-Item" but del seems faster. If you run del in a powershell window, it will actually run "Remove-Item", so make sure you are in a CMD window.

/q - Quiet, don't confirm deletions

C:\windows\temp\\*.log - Any .log file in this directory. It is not recursive, but there is a recursive option which is /s.

The below information is from: https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del

Deletes one or more files. This command is the same as the **erase** command.

For examples of how to use this command, see [Examples](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del#BKMK_examples).

## Syntax

Copy

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>

```

## Parameters

| Parameter | Description |
| --- | --- |
| &lt;Names&gt; | Specifies a list of one or more files or directories. Wildcards may be used to delete multiple files. If a directory is specified, all files within the directory will be deleted. |
| /p | Prompts for confirmation before deleting the specified file. |
| /f | Forces deletion of read-only files. |
| /s | Deletes specified files from the current directory and all subdirectories. Displays the names of the files as they are being deleted. |
| /q | Specifies quiet mode. You are not prompted for delete confirmation. |
| /a[:]&lt;Attributes&gt; | Deletes files based on the following file attributes: **r** Read-only files **h** Hidden files **i** Not content indexed files **s** System files **a** Files ready for archiving **l** Reparse points - Prefix meaning 'not' |
| /? | Displays help at the command prompt. |

## Remarks

 Caution

If you use **del** to delete a file from your disk, you cannot retrieve it.

- If you use **/p**, **del** displays the name of a file and sends the following message:

Copy

```
`FileName, Delete (Y/N)?`

To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.

```

- If you disable command extensions, **/s** displays the names of any files that were not found instead of displaying the names of files that are being deleted (that is, the behavior is reversed).
- If you specify a folder in *Names*, all of the files in the folder are deleted. For example, the following command deletes all of the files in the \Work folder:Copy`del \work`
- You can use wildcards (**\*** and **?**) to delete more than one file at a time. However, to avoid deleting files unintentionally, you should use wildcards cautiously with the **del** command. For example, if you type the following command:Copy`del *.*`The **del** command displays the following prompt:`Are you sure (Y/N)?`To delete all of the files in the current directory, press Y and then press ENTER. To cancel the deletion, press N and then press ENTER.

 Note

Before you use wildcard characters with the **del** command, use the same wildcard characters with the **dir** command to list all the files that will be deleted.

- The **del** command, with different parameters, is available from the Recovery Console.

## Examples

To delete all the files in a folder named Test on drive C, type either of the following:Copy

```
del c:\test
del c:\test\*.*

```

To delete all files with the .bat file name extension from the current directory, type:Copy

```
del *.bat

```

To delete all read-only files in the current directory, type:Copy

```
del /a:r *.*
```
