<div align="right">

  <img src="https://img.shields.io/badge/OpenSource-000?style=for-the-badge&logo=ghost&logoColor=black&color=ffd700" alt="OpenSource-Badge">

</div>

  

![[EMPMonitor logo.png]]

<p align="center"><i>Your Workforce Productivity Compass</i></p>

> **_EmpMonitor: The Ultimate Solution for Workforce Monitoring and Productivity Optimization_**
# 📂 MariaDB Database Restore Guide

When working with databases, it's important to have backups in case of system failures, accidental deletions, or migrations to a new server. This guide will walk you through restoring a MariaDB database using a backup file (db.sql). It is designed for users with minimal technical knowledge, ensuring a smooth and error-free restoration process.
## ➤ Prerequisites

Before you can restore the database, there are a few important steps to prepare your system.
### 📍 Check if MariaDB is Installed

MariaDB is the database management system required for this process. To check if it’s already installed, run the following command in your terminal:

```sh

    mariadb --version

```


> [!NOTE]
>
> If MariaDB is installed, you’ll see an output showing the version number. If it is not installed, follow the next step.

### 📍 Install MariaDB (If Not Installed)

If the previous command indicates that MariaDB is missing, install it using the appropriate command for your operating system.

- **For Debian/Ubuntu Users**:

Run this command:

    ```sh

        sudo apt update && sudo apt install mariadb-server -y

    ```

- **For RHEL/CentOS Users**:

Run this command:

    ```sh

        sudo yum install mariadb-server -y

    ```

- This will install MariaDB and all its necessary components.


> [!WARNING]
>
> Installation requires administrative (root) privileges.

### 📍 Start and Enable MariaDB

After installation, ensure that MariaDB is running. Use the following commands:

  

```sh

    sudo systemctl start mariadb

    sudo systemctl enable mariadb

```

- The first command starts the MariaDB service.

- The second command ensures MariaDB starts automatically every time the system boots.

  
>[!TIP]
>
> If MariaDB is already running, you can check its status using:
> ```sh
>  sudo systemctl status mariadb
> ```
> This command will show the status of the MariaDB service.

## ➤ Steps to Restore the Database

Now that your system is ready, follow these steps to restore the database from the backup file
### 📍 Step 1: Create a New Database

A database must exist before you can import data into it. Run the following command to create a new database:

```sh

    mysql -u root -p -e "CREATE DATABASE <database_name>;"

```

  
> [!NOTE]
>
> If the `db.sql` file already contains a `CREATE DATABASE` statement, this step may not be necessary.

- **Explanation**:

    - `mysql -u root -p` → Logs into the MariaDB server as the root user. You will be asked for the root password.

    - `-e "CREATE DATABASE <database_name>;"` → Executes a command inside MariaDB to create a new database.
    
    - Replace `<database_name>` with your preferred database name.
- **Example**:

    - If you want to create a database named my_database, use:

    ```sh

        mysql -u root -p -e "CREATE DATABASE my_database;"

    ```

  
> [!TIP]
>
> Use a meaningful database name to avoid confusion.

### 📍 Step 2:Restore the Database from the Backup File

Once you have created the database, restore the backup by running:

```sh

    mysql -u root -p <database_name> < db.sql

```

  
> [!WARNING]
>
> This command will replace any existing data in the selected database. Make sure you've chosen the correct database to avoid losing important data.

- **Explanation**:

    - `mysql -u root -p` → Logs into MariaDB as the root user.

    - `<database_name>` → The name of the database where you want to import the backup.
    
    - `< db.sql` → This tells MariaDB to read the db.sql file and execute the commands inside it.

- **Example**:

    - If your database name is `my_database`, use:

    ```sh

        mysql -u root -p my_database < db.sql

    ```

  
> [!TIP]
>
> If the backup file is in another directory, provide the full path:
> ```sh
>   mysql -u root -p <database_name> < /path/to/db.sql
> ```

- After running this command, MariaDB will import all tables and data from the db.sql file.

## ➤ Troubleshooting

Here are some common errors and solutions while restoring a MariaDB database.

### 📍 Access Denied Error

**Error Message**:

```sh

    ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using    password: YES)

```

- **Possible Solutions**:

    - Verify that the password is correct.

    - Ensure the MariaDB root user has proper privileges.

    - If you’ve forgotten the password, reset it by following MariaDB’s password recovery process.

> [!TIP]
>
>  Use the following command to check user privileges
> ```sh
>  mysql -u root -p -e "SHOW GRANTS FOR 'root'@'localhost';"
> ```

### 📍 MariaDB Command Not Found

If running `mysql --version` gives an error:

```bash

    command not found: mysql

```

- **Possible Solutions**:

    - MariaDB may not be installed. Reinstall it using the steps above.

    - Check if MariaDB is in your system’s `PATH` by running:

    ```sh

        which mysql

    ```

    - If the command returns no output, try using the full path:

    ```sh

        /usr/bin/mysql -u root -p

    ```

  
> [!NOTE]
>
> The default installation path varies based on the operating system.

### 📍 Errors in SQL File

If you receive syntax errors while restoring the database, check `db.sql` for:

    - Corrupt or incomplete SQL commands.

    - Conflicting `CREATE DATABASE` or `USE` statements.

    - Unsupported SQL statements due to MariaDB version differences.

> [!TIP]
>
> Open the `db.sql` file in a text editor and manually inspect its contents before running the restore command.

## ➤ Important Notes

- **Permissions**: Ensure you have the necessary permissions to restore the database. If using a non-root user, make sure they have the required privileges.

- **Duplicate Database Issues**: If your backup file (`db.sql`) contains `CREATE DATABASE` statements, you might need to edit it before restoring. Otherwise, it could cause errors if the database already exists.

- **File Location**: Ensure the `db.sql` file is located in the directory from which you are running the command. If it is in another location, you need to specify the full path to the file.

- **Backup and Restore**: Always make a backup of your database before attempting to restore a backup.

## ➤ Additional Resources

- For more information on MariaDB commands, refer to the official [MariaDB documentation](https://mariadb.com/docs/).

- For detailed instructions on creating and restoring database backups, see the [MariaDB documentation on backup and restore](https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/).
## ➤ License

This script is provided as-is with no warranty.