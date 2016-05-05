# tiny configuration management

Tiny is a small tool which can be used for configuration management, and it is similar in concept to tools such as Ansible and Chef. JavaScript is used to define configuration files, which can be used to execute commands on target systems.

## Getting started
To execute tiny, you must provide a `targets.js` and `actions.js` file. The former defines which machines you want to operate on, and the latter defines the actions you wish te perform (applications to install, files to copy, etc.).

Once you have created these files (more info below), you can run:

```bash
tiny run --targets ./targets.js --actions ./actions.js
```

## Defining Targets
Targets are the remote machines which you want to operate on. For example, let's say you want to provision a few Ubuntu machines with Nginx and some default Nginx configuration. You would define the target file as shown below:

```javascript
// targets.js
tiny.addTarget('192.168.1.1', 'root', 'R00tPa$$w0rd1');
tiny.addTarget('192.168.10.2', 'root', 'R00tPa$$w0rd2');
tiny.addTarget('192.168.20.3', 'root', 'R00tPa$$w0rd3');
```

## Defining actions
Tiny supports a limited set of actions.

### Install (debian) package
`tiny.installPackage(package_name: string)`

* Installs a debian package using the apt package manager on ubuntu.
* This operation is idempotent, meaning that if the package is already installed, no action will be performed.

Example usage:

```javascript
// Install nginx
tiny.installPackage('nginx');
```

### Copy files
`tiny.copy(local_path: string, remote_path: string);`

* Copies files from local machine (where tiny is executed) to the target machine, at the given path
* Paths

Example usage:

```javascript
// Copy nginx configuration file
tiny.copy('./files/default_nginx', '/etc/nginx/sites-available/default');
```

### Control services
`tiny.service(service_name: string, operation: string)`

* Start, stop, restart services

Example usage:

```javascript
// Start nginx service
tiny.service('nginx', 'start');
```

### Full example

## targets.js
```javascript
tiny.addTarget('192.168.1.1', 'root', 'R00tPa$$w0rd1');
tiny.addTarget('192.168.10.2', 'root', 'R00tPa$$w0rd2');
tiny.addTarget('192.168.20.3', 'root', 'R00tPa$$w0rd3');
```

## actions.js
```javascript
// Install nginx and setup default configuration
tiny.installPackage('nginx');
tiny.copy('./files/default_nginx', '/etc/nginx/sites-available/default');

// Copy application source files to www directory
tiny.copy('./files/app/', '/usr/share/nginx/www/');

// Start nginx
tiny.service('nginx', 'start');
```

## run actions
```bash
tiny run --targets ./targets.js --actions ./actions.js
```