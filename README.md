import os
import shutil
import time


class FolderManager:

    def __init__(self, folder_path):
        self.folder = folder_path


    
    def show_contents(self):
        print("\nFolder Contents:\n")

        try:
            items = os.listdir(self.folder)

            if not items:
                print("Folder is empty.")
                return

            for item in items:
                path = os.path.join(self.folder, item)

                if os.path.isdir(path):
                    print("[Folder] ", item)
                else:
                    print("[File]   ", item)

        except Exception as e:
            print("Error:", e)


    
    def count_items(self):
        files = 0
        folders = 0

        items = os.listdir(self.folder)

        for item in items:
            path = os.path.join(self.folder, item)

            if os.path.isdir(path):
                folders += 1
            else:
                files += 1

        print("\nTotal Folders:", folders)
        print("Total Files  :", files)
        print("Total Items  :", len(items))


    
    def search_item(self):
        name = input("Enter file or folder name: ")

        items = os.listdir(self.folder)

        if name in items:
            print("Item found.")
        else:
            print("Item not found.")


   
    def rename_item(self):
        old_name = input("Enter existing name: ")
        new_name = input("Enter new name: ")

        old_path = os.path.join(self.folder, old_name)
        new_path = os.path.join(self.folder, new_name)

        if os.path.exists(old_path):
            os.rename(old_path, new_path)
            print("Renamed successfully.")
        else:
            print("Item not found.")


    
    def delete_item(self):
        name = input("Enter file or empty folder name: ")
        path = os.path.join(self.folder, name)

        if not os.path.exists(path):
            print("Item not found.")
            return

        confirm = input("Are you sure? (yes/no): ")

        if confirm.lower() != "yes":
            print("Deletion cancelled.")
            return

        if os.path.isdir(path):
            if os.listdir(path):
                print("Folder is not empty.")
            else:
                os.rmdir(path)
                print("Folder deleted.")
        else:
            os.remove(path)
            print("File deleted.")


   
    def arrange_files(self):
        target = os.path.join(self.folder, "All_Files")

        if not os.path.exists(target):
            os.mkdir(target)

        for item in os.listdir(self.folder):
            path = os.path.join(self.folder, item)

            if os.path.isfile(path):
                shutil.move(path, os.path.join(target, item))

        print("Files moved to All_Files folder.")


   
    def generate_report(self):
        report_path = os.path.join(self.folder, "report.txt")

        with open(report_path, "w") as f:
            f.write("Folder Report\n")
            f.write("------------------\n")

            for item in os.listdir(self.folder):
                path = os.path.join(self.folder, item)

                if os.path.isdir(path):
                    f.write("[Folder] " + item + "\n")
                else:
                    f.write("[File]   " + item + "\n")

        print("Report generated.")


   
    def create_file(self):
        name = input("Enter new file name (with extension): ")
        path = os.path.join(self.folder, name)

        if os.path.exists(path):
            print("File already exists.")
        else:
            open(path, "w").close()
            print("File created.")


    
    def create_folder(self):
        name = input("Enter new folder name: ")
        path = os.path.join(self.folder, name)

        if os.path.exists(path):
            print("Folder already exists.")
        else:
            os.mkdir(path)
            print("Folder created.")

    
    def copy_file(self):
        name = input("Enter file name to copy: ")
        path = os.path.join(self.folder, name)

        if os.path.isfile(path):
            new_name = "copy_" + name
            new_path = os.path.join(self.folder, new_name)
            shutil.copy(path, new_path)
            print("File copied.")
        else:
            print("File not found.")


   
    def show_file_size(self):
        name = input("Enter file name: ")
        path = os.path.join(self.folder, name)

        if os.path.isfile(path):
            size = os.path.getsize(path)
            print("File size:", size, "bytes")
        else:
            print("File not found.")


    
    def show_modified_time(self):
        name = input("Enter file name: ")
        path = os.path.join(self.folder, name)

        if os.path.exists(path):
            mod_time = os.path.getmtime(path)
            print("Last Modified:",
                  time.strftime("%Y-%m-%d %H:%M:%S",
                                time.localtime(mod_time)))
        else:
            print("Item not found.")


   
    def remove_empty_folders(self):
        for item in os.listdir(self.folder):
            path = os.path.join(self.folder, item)

            if os.path.isdir(path) and not os.listdir(path):
                os.rmdir(path)
                print("Removed empty folder:", item)

        print("Done checking empty folders.")


   
    def show_help(self):
        print("\nAvailable Features:")
        print("1  - Show contents")
        print("2  - Count files/folders")
        print("3  - Search item")
        print("4  - Rename item")
        print("5  - Delete item")
        print("6  - Arrange files")
        print("7  - Generate report")
        print("8  - Create file")
        print("9  - Create folder")
        print("10 - Copy file")
        print("11 - File size")
        print("12 - Modified time")
        print("13 - Remove empty folders")
        print("14 - Help")
        print("15 - Exit")


def main():
    folder = input("Enter folder path: ").strip('"')

    if not os.path.isdir(folder):
        print("Invalid folder path.")
        return

    manager = FolderManager(folder)

    while True:
        print("\n===== FOLDER MANAGER MENU =====")
        print("1.Show  2.Count  3.Search  4.Rename  5.Delete")
        print("6.Arrange  7.Report  8.NewFile  9.NewFolder")
        print("10.Copy  11.Size  12.Time")
        print("13.RemoveEmpty  14.Help  15.Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            manager.show_contents()
        elif choice == "2":
            manager.count_items()
        elif choice == "3":
            manager.search_item()
        elif choice == "4":
            manager.rename_item()
        elif choice == "5":
            manager.delete_item()
        elif choice == "6":
            manager.arrange_files()
        elif choice == "7":
            manager.generate_report()
        elif choice == "8":
            manager.create_file()
        elif choice == "9":
            manager.create_folder()
        elif choice == "10":
            manager.copy_file()
        elif choice == "11":
            manager.show_file_size()
        elif choice == "12":
            manager.show_modified_time()
        elif choice == "13":
            manager.remove_empty_folders()
        elif choice == "14":
            manager.show_help()
        elif choice == "15":
            print("Exiting program.")
            break
        else:
            print("Invalid choice.")


if __name__ == "__main__":
    main()
