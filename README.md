# Regulated_File_Manager
🗃️ Software concept for regulating files inside and outside of File Explorer for Windows.
#

## FEATURES

- Control the amount of files stored outside of folders.
- Create new folders for loose files.
- Archive and shrink old files and folders.
- Upload cold files and folders to cloud storage.
- Habitual file and folder management suggestions.

## CHATGPT CONCEPT

```
import os
import shutil
import datetime
import cloud_storage_library  # Replace with the library of your choice for cloud storage

# Define your configurations
MAX_FILES_OUTSIDE_FOLDERS = 100
ARCHIVE_FOLDER = 'archive'
CLOUD_STORAGE_PATH = 'cloud_storage'
FILE_EXTENSIONS_TO_ARCHIVE = ['.txt', '.pdf']
DAYS_TO_ARCHIVE = 30

def create_folder(folder_name):
    if not os.path.exists(folder_name):
        os.makedirs(folder_name)

def move_files_to_folder(source, destination):
    for file in os.listdir(source):
        file_path = os.path.join(source, file)
        shutil.move(file_path, destination)

def archive_files(folder):
    for root, _, files in os.walk(folder):
        for file in files:
            file_path = os.path.join(root, file)
            file_extension = os.path.splitext(file_path)[1]
            if file_extension in FILE_EXTENSIONS_TO_ARCHIVE:
                if os.path.getmtime(file_path) < (datetime.datetime.now() - datetime.timedelta(days=DAYS_TO_ARCHIVE)).timestamp():
                    move_files_to_folder(folder, os.path.join(ARCHIVE_FOLDER, folder))

def upload_to_cloud(folder):
    # Implement cloud storage upload here
    # You may use a library like boto3 for AWS S3 or gcloud for Google Cloud Storage
    cloud_storage_library.upload(folder, CLOUD_STORAGE_PATH)

def main():
    current_files = len(os.listdir('.'))
    if current_files > MAX_FILES_OUTSIDE_FOLDERS:
        create_folder('loose_files')
        move_files_to_folder('.', 'loose_files')

    for folder in os.listdir('.'):
        if os.path.isdir(folder) and folder != ARCHIVE_FOLDER and folder != CLOUD_STORAGE_PATH:
            archive_files(folder)

    for folder in os.listdir(ARCHIVE_FOLDER):
        upload_to_cloud(os.path.join(ARCHIVE_FOLDER, folder))
        shutil.rmtree(os.path.join(ARCHIVE_FOLDER, folder))

if __name__ == "__main__":
    main()
```
This code provides a basic structure for a regulated file manager, but you will need to customize it and integrate cloud storage based on your requirements and preferences. Additionally, you should schedule this script to run at regular intervals using tools like cron on Unix-based systems or Task Scheduler on Windows.

#
### Related Links

[Smart Folder](https://github.com/sourceduty/Smart_Folder)

#
ℹ️ This software is free and open-source; anyone can redistribute it and/or modify it.
