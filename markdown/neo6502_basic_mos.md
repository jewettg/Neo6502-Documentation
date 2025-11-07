# MOS Commands

Using the MOS commands to access OS disk/file functionality.

MOS commands can be used in four different ways:

- On the NeoBASIC command line, prefixing the command with an asterisk.  
  **Example**: `*del myfile.txt`

- On the NeoBASIC command line, use the mos BASIC command. Surround the MOS command in quotes.  
  **Example**: `mos "del myfile.txt"`

- Within a NeoBASIC program, you can use the mos BASIC function. Surround the MOS command in quotes.  
  **Example**: `if mos("del myfile.txt") > 0 then ...`

## **Available MOS Commands**

| Command | Description                                                                                                                                                                                                                                                                                                                                        | Command | Description                                                                        |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------------------------------------------- |
| `cat`   | Prints out the file listing of the current directory.                                                                                                                                                                                                                                                                                              | `ren`   | Will rename a file or directory.  Works identically as the copy command.           |
| `del`   | Delete the specified file. Can be used to delete a directory, only if it is empty.  If a directory is not empty, the command generates an error.                                                                                                                                                                                                   | `md`    | Make a directory.  Specify the name of the directory you want to create.           |
| `copy`  | Copy on path to another.<br/>Requires two parameters, the first is the source path and file, and the second is the destination path and file.<br/>If no path is given, but different names are given, then you get a copy of the source file specified in the first parameter in the current directory.<br/>You cannot make copies of directories. | `cd`    | Change Directory.<br/>Specify the name of the directory that you wish to traverse. |
| `file`  | Will verify that a file (not a directory) exists.  Return zero if it exists, and non-zero if it does not exist.                                                                                                                                                                                                                                    |         |                                                                                    |

## Standard Unix POSIX paths are used with the MOS commands.

| Path  | Description                                                                                                         |
| ----- | ------------------------------------------------------------------------------------------------------------------- |
| `./`  | Current Directory                                                                                                   |
| `../` | Go up in the hierarchy, relative to the current directory<br/> (*sometime referred as the going back a directory*). |
| `/`   | The root or top-level directory.                                                                                    |

## MOS Error Codes

Most of the functions will return an error/status code to indicate whether the operation succeeded or not.

| Name                       | Value | Meaning                                                               |
| -------------------------- | ----- | --------------------------------------------------------------------- |
| FIOERROR_OK                | 0x00  | Operation succeeded (*not an error*)                                  |
| FIOERROR_UNKNOWN           | 0x01  | Something went wrong, but we don't know what                          |
| FIOERROR_EOF               | 0x02  | A read or directory enumeration operation reached the end of the file |
| FIOERROR_UNIMPLEMENTED     | 0x03  | Operation is not implemented                                          |
| FIOERROR_NO_FILE           | 0x11  | Could not find the file                                               |
| FIOERROR_NO_PATH           | 0x12  | Could not find the path                                               |
| FIOERROR_INVALID_DRIVE     | 0x13  | The logical drive number is invalid                                   |
| FIOERROR_INVALID_NAME      | 0x14  | The path name format is invalid                                       |
| FIOERROR_INVALID_PARAMETER | 0x15  | Given parameter is invalid                                            |
| FIOERROR_DENIED            | 0x21  | Access denied due to prohibited access or disk or directory full      |
| FIOERROR_EXIST             | 0x22  | Access denied due to prohibited access                                |
| FIOERROR_INVALID_OBJECT    | 0x23  | The file/directory object is invalid                                  |
| FIOERROR_WRITE_PROTECTED   | 0x24  | The physical drive is write-protected                                 |
| FIOERROR_LOCKED            | 0x25  | File is in use                                                        |
| FIOERROR_DISK_ERR          | 0x31  | A hard error occurred in the low-level disk I/O layer                 |
| FIOERROR_INT_ERR           | 0x32  | Assertion failed                                                      |
| FIOERROR_NOT_READY         | 0x33  | The physical drive cannot work                                        |
| FIOERROR_NOT_ENABLED       | 0x34  | The volume has no work area                                           |
| FIOERROR_NO_FILESYSTEM     | 0x35  | The filesystem is invalid                                             |



### File Attributes

| Name             | Value | Meaning                                                           |
| ---------------- | ----- | ----------------------------------------------------------------- |
| FIOATTR_DIR      | 0x01  | This is a directory (*may not be modified*)                       |
| FIOATTR_SYSTEM   | 0x02  | This is a system file and will be hidden from directory listings  |
| FIOATTR_ARCHIVE  | 0x04  | File is archived; automatically cleared when the file is modified |
| FIOATTR_READONLY | 0x08  | File is read only and may not be overwritten or modified          |
| FIOATTR_HIDDEN   | 0x10  | This will be hidden from directory listings                       |
