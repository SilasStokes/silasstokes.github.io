---
layout: post
title: pass multiple quick action objects into macos shortcuts using python
date: 2024-03-03 11:15
category: python programming
author: silas stokes
tags: []
summary: how to send and receive quick action objects to shortcuts using python
---

I have been using the [py-imessage-shortcuts](https://github.com/kevinschaich/py-imessage-shortcuts) python package to auto respond to text messages with chat GPT in my [iMessage Bot](https://github.com/SilasStokes/pymessage_gpt_bot). Now that openai has DALL·E it would be nice to allow the bot to also be extended to include images; which py-imessage-shortcuts doesn't suport. After some experimentation I was able to figure out how to extend the package to allow images. It involves a trick I wasn't able to find online through googling so documenting it here in case someone else finds it useful. 


To start, the author of py-imessage-shortcuts found out that a cli command can be used to run a shortcut by name. Using that and Python's `subprocess` module, any shortcut can be run with:

```python
from subprocess import Popen
SHORTCUT_NAME = 'shortcut-name'
Popen([
    'shortcuts',
    'run',
    SHORTCUT_NAME,
])
```

Now to get input into shortcut, you can just read the man page of shortcuts to read the documentation and see that you can pass file inputs into the shortcut:

![image of the shortcuts man page](/assets/images/2024-03-03-pass-multiple-quick-action-objects/shortcuts-man-page.png)

Which can be consumed in the shortcut by enabling `Use as Quick Action`

![image of shortcuts where the use as quick action button is pointed at by a red arrow](/assets/images/2024-03-03-pass-multiple-quick-action-objects/receiving-input-to-shortcut.png)

py-imessage-shortcut uses this to pass a dictionary to the `send-imessage` shortcut. Since the input has to be a file, first the package writes the dictionary to a json file, then it runs the shortcut passing the file, then in the shortcut it has updated to the quick action input to be of type text which allows it to be parsed as a dictionary with the `Set variable` action, where the `to` clause is set to the Quick Action Input interpretted as a dictionary and with key for the value wanted written in. Here's the python and a screenshot from the shortcut:

```python
with open(TEMP_FILE_PATH, 'w') as f:
    message_details = {
        'recipients': recipients, 
        'message': message
        }
    json.dump(message_details, f)

Popen([
    'shortcuts',
    'run',
    SHORTCUT_NAME,
    '--input-path',
    TEMP_FILE_PATH,
])
```
![image of shortcuts where it displaying how to read a value out a quick action passed dictionary](/assets/images/2024-03-03-pass-multiple-quick-action-objects/deciphering-dictionary-input.png)

So that's all the magic used in py-imessage-shortcuts currently; to extend it to allow images in addition, simply send multiple `--input-path` clauses and enumerate them in the MacOS shortcut.  Here's the function I wrote in my pr to py-imessage-shortcuts:

```python
def _dump_file(recipients: list[str], message: str) -> None:
    with open(TEMP_FILE_PATH, 'w') as f:
        message_details = {
            'recipients': recipients, 
            'message': message
            }
        json.dump(message_details, f)

def send_image(recipients: list[str], message: str | None, image_path: str) -> None:
    """enables an image to be sent from an absolute file path

    Args:
        recipients (list[str]): The phone numbers to address the iMessage to
        message (str | None): the message to send with the photo, if no message should be included, pass None
        image_path (str): file path to image, must be absolute
    """
    if len(message) == 0:
        message = None
    _dump_file(recipients, message)

    Popen([
        'shortcuts',
        'run',
        SHORTCUT_NAME,
        '--input-path',
        TEMP_FILE_PATH,
        '--input-path',
        image_path,
    ])
```

Then to get both the message and the image into shortcuts, enable `images` in addition to `text` in the Quick Actions Input. The input can be treated as a List, where the order of the input objects is the same order as the `--input-path` clauses passed to the shortcuts cli command. So I used the `Get Item from List` action to separate the inputs and treat them individually as pictured by the full shortcut below:

![image of full shortcut](/assets/images/2024-03-03-pass-multiple-quick-action-objects/final-send-image-shortcut.png)

## Corner Cases:
Interestingly the `Send Message` shortcut action does not like empty strings; it will alert with an error saying that there's no value for it. I found that encoding Python's `None` value pairs well Shortcut's `if <variable> has any value` action.

## Dead end I explored on the way to this:
Initially I tried to just pass the path to the image within the existing dictionary py-imessage uses BUT shortcuts seems to be limited about where the `Get file from ____ at path ____` action can read the file/image from. If the image isn't in the iCloud shortcuts folder you'll get an error that reads : "Invalid file path The provided file path must be contained with the directory.". Which to combat I had to first copy the image to the iCloud shortcuts directory, and use that as the `file from` clause.

The error:
![Pop up showing Invalid file path error](/assets/images/2024-03-03-pass-multiple-quick-action-objects/invalid-path.png)

The fix:
```python
import shutil, os
file_name = './test_image.png'
icloud_shortcuts_dir = os.path.join(
        os.path.expanduser('~'),
        'Library/Mobile Documents/iCloud~is~workflow~my~workflows/Documents')
shutil.copy(file_name, icloud_shortcuts_dir)

# then run the Popen cmd here
```
![successful file path location](/assets/images/2024-03-03-pass-multiple-quick-action-objects/successful-path.png)
