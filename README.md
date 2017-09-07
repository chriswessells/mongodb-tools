# mongodb-tools

![alt text](https://travis-ci.org/chriswessells/mongodb-tools.svg?branch=master "TravisCI Build Status") master branch 

![alt text](https://travis-ci.org/chriswessells/mongodb-tools.svg?branch=development "TravisCI Build Status") development branch 

A tool container with ruby, mongodb-tools, and awscli installed for multiple versions of MongoDB.

## Versions

Application versions

### 3.2 MongoDB, Ruby 2.4.1, and Debian 9

The container uses MongoDB 3.2.14, ruby 2.4.1, redis 3.3.3.

My use case is to separate the database update process in the deploy so it can run independent and automated.